---
title : "Create IAM Roles"
date: ""
weight : 4
chapter : false
pre : " <b> 5.2.4. </b> "
---

#### Create IAM Roles for Lambda Functions

Each Lambda function requires an **IAM execution role** to access other AWS services, such as Amazon S3, Amazon SQS, and Amazon CloudWatch Logs.

---

#### Step 1: Create the Trust Policy Document

Create a file named `lambda-trust-policy.json` with the following content:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---

#### Step 2: Create the IAM Role

Run the following command to create the Lambda execution role:

```bash
aws iam create-role \
    --role-name nasdaq-etl-lambda-role \
    --assume-role-policy-document file://lambda-trust-policy.json
```

---

#### Step 3: Attach the Required IAM Policies

Attach the following managed IAM policies to the role:

```bash
# Amazon CloudWatch Logs permissions (required for all Lambda functions)
aws iam attach-role-policy \
    --role-name nasdaq-etl-lambda-role \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

# Amazon S3 Full Access
aws iam attach-role-policy \
    --role-name nasdaq-etl-lambda-role \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

# Amazon SQS Full Access
aws iam attach-role-policy \
    --role-name nasdaq-etl-lambda-role \
    --policy-arn arn:aws:iam::aws:policy/AmazonSQSFullAccess
```

{{% notice warning %}}
For production environments, always follow the **Principle of Least Privilege** by granting only the permissions required for each Lambda function instead of using broad `FullAccess` policies.
{{% /notice %}}

---

#### Step 4: Retrieve the Role ARN

Run the following command to obtain the IAM Role ARN:

```bash
aws iam get-role \
    --role-name nasdaq-etl-lambda-role \
    --query 'Role.Arn' \
    --output text
```

Save the returned **Role ARN** for later use.

Example:

```text
arn:aws:iam::123456789012:role/nasdaq-etl-lambda-role
```

![Iam Role Created](/images/2.4/iam-role-created.png)

---

#### Step 5: Build and Push the Docker Image to Amazon ECR

All Lambda functions in this project use **container images** to package Python dependencies such as **Polars**, **XGBoost**, and **yfinance**.

```bash
# 1. Authenticate Docker to Amazon ECR
aws ecr get-login-password --region ap-southeast-1 | \
    docker login --username AWS --password-stdin \
    <YOUR_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com

# 2. Create an Amazon ECR repository
aws ecr create-repository \
    --repository-name nasdaq-etl-lambda \
    --region ap-southeast-1

# 3. Build the Docker image
docker build -t nasdaq-etl-lambda:latest .

# 4. Tag the Docker image
docker tag nasdaq-etl-lambda:latest \
    <YOUR_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/nasdaq-etl-lambda:latest

# 5. Push the image to Amazon ECR
docker push \
    <YOUR_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/nasdaq-etl-lambda:latest
```

![Ecr Image Pushed](/images/2.4/ecr-image-pushed.png)