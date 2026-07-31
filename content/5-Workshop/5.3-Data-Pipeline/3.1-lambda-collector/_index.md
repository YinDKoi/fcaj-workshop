---
title : "Lambda Daily Collector"
date: ""
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

#### Lambda Daily Collector — Fan-Out Producer

`lambda_daily_collector.py` is the first **Lambda Producer** in Pipeline B. It is triggered by **Amazon EventBridge** on a daily schedule and is responsible for:

1. Reading the `tickers.json` file from Amazon S3 to obtain the list of stock tickers to monitor.
2. Splitting the ticker list into chunks (`CHUNK_SIZE=100` tickers per chunk).
3. Sending each chunk as an SQS message to the `daily-collector-queue`.

![Eventbridge Lambda Sqs Flow](images/3.1/eventbridge-lambda-sqs-flow.png)

---

#### Step 1: Create the Lambda Function

Open the **AWS Lambda Console** and create a new Lambda function with the following configuration:

- **Function name:** `nasdaq-daily-collector`
- **Container image:** Select the ECR image `nasdaq-etl-lambda:latest`
- **CMD override:** `src.lambda_daily_collector.lambda_handler`
- **Execution role:** `nasdaq-etl-lambda-role`
- **Memory:** `512 MB`
- **Timeout:** `5 minutes`

![Lambda Create Function](images/3.1/lambda-create-function.png)

---

#### Step 2: Configure Lambda Environment Variables

In **Configuration → Environment variables**, add the following variables:

| Key | Value |
|:---|:---|
| `PROCESSED_BUCKET` | `my-nasdaq-stock-processed-2026-ap-southeast-1` |
| `SQS_QUEUE_URL` | `https://sqs.ap-southeast-1.amazonaws.com/123456789012/daily-collector-queue` |
| `TICKERS_S3_KEY` | `config/tickers.json` |
| `CHUNK_SIZE` | `100` |
| `AWS_REGION` | `ap-southeast-1` |

![Lambda Env Vars](images/3.1/lambda-env-vars.png)

---

#### Step 3: Configure the EventBridge Trigger

Create an **Amazon EventBridge Rule** to invoke the Lambda function automatically every business day:

```bash
aws events put-rule \
    --name "nasdaq-daily-pipeline-trigger" \
    --schedule-expression "cron(0 0 ? * MON-FRI *)" \
    --state ENABLED \
    --region ap-southeast-1
```

Then attach the Lambda function as the target:

```bash
aws events put-targets \
    --rule nasdaq-daily-pipeline-trigger \
    --targets "Id=lambda-target,Arn=arn:aws:lambda:ap-southeast-1:<ACCOUNT_ID>:function:nasdaq-daily-collector" \
    --region ap-southeast-1
```

{{% notice info %}}
The schedule `cron(0 0 ? * MON-FRI *)` runs at **00:00 UTC every Monday through Friday**, which corresponds to **7:00 AM (ICT)** after the U.S. stock market has closed.
{{% /notice %}}

![Eventbridge Rule](images/3.1/eventbridge-rule.png)

---

#### Step 4: Test the Lambda Function Manually

Create the following test event in the **AWS Lambda Console**:

```json
{
    "source": "manual-test",
    "detail-type": "Scheduled Event"
}
```

Check the **Amazon CloudWatch Logs** to verify that the Lambda function successfully reads the ticker list and sends SQS messages.

![Cloudwatch Logs](images/3.1/cloudwatch-logs.png)

---

#### Configure the Lambda Collector Consumer

`lambda_collector_producer.py` is the **Lambda Consumer** that receives SQS messages and retrieves stock market data from Yahoo Finance.

Configure the Lambda function with the following settings:

- **Function name:** `nasdaq-collector-producer`
- **CMD override:** `src.lambda_collector_producer.lambda_handler`
- **Memory:** `1024 MB`
- **Timeout:** `15 minutes`
- **SQS Trigger:** `daily-collector-queue` with a **Batch size** of `1`

{{% notice tip %}}
Setting the batch size to `1` ensures that each Lambda Consumer processes only one SQS message (100 tickers) at a time. This improves execution performance and helps prevent timeouts when requesting data from the Yahoo Finance API.
{{% /notice %}}