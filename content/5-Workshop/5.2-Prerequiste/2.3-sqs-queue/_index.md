---
title : "Create SQS Queue"
date: ""
weight : 3
chapter : false
pre : " <b> 5.2.3. </b> "
---

#### Create Amazon SQS Queue

Amazon SQS is the backbone of the **Fan-Out architecture** in Pipeline B. The Lambda Producer pushes chunks of ticker lists into the Queue, and multiple Lambda Consumers run in parallel to process them.

![Amazon SQS Fan-Out Architecture](images/2.3/sqs-fanout-architecture.png)

*Figure 2.3. Amazon SQS Fan-Out architecture, where the Lambda Producer partitions stock symbols into multiple message batches, enabling the Lambda Consumer to process them concurrently for scalable and efficient data ingestion.*
---

#### Step 1: Create SQS Queue

```bash
aws sqs create-queue \
    --queue-name daily-collector-queue \
    --region ap-southeast-1 \
    --attributes '{
        "VisibilityTimeout": "300",
        "MessageRetentionPeriod": "86400",
        "ReceiveMessageWaitTimeSeconds": "20"
    }'
```

**Attribute explanation:**

| Attribute | Value | Explanation |
|:---|:---|:---|
| `VisibilityTimeout` | `300` seconds | Time Lambda Consumer holds the message before returning it to queue on failure |
| `MessageRetentionPeriod` | `86400` seconds (1 day) | How long messages are retained in the queue |
| `ReceiveMessageWaitTimeSeconds` | `20` seconds | Long-polling — reduces unnecessary API calls |

---

#### Step 2: Get Queue URL

```bash
aws sqs get-queue-url \
    --queue-name daily-collector-queue \
    --region ap-southeast-1
```

Expected result:
```json
{
    "QueueUrl": "https://sqs.ap-southeast-1.amazonaws.com/123456789012/daily-collector-queue"
}
```

Save this **Queue URL** — you will need it when configuring Lambda environment variables.

#### Step 3: Configure Environment Variables

Create or update the `.env` file in the project root directory:

```ini
# .env
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY_ID=YOUR_KEY
AWS_SECRET_ACCESS_KEY=YOUR_SECRET

# S3 Buckets
RAW_BUCKET=my-nasdaq-stock-market-raw-2026-ap-southeast-1
PROCESSED_BUCKET=my-nasdaq-stock-processed-2026-ap-southeast-1
MODEL_BUCKET=my-nasdaq-stock-models-2026-ap-southeast-1
SIM_BUCKET=my-nasdaq-stock-simulation-2026-ap-southeast-1

# SQS Queue URL
SQS_QUEUE_URL=https://sqs.ap-southeast-1.amazonaws.com/123456789012/daily-collector-queue

# Fan-Out Chunk Size (number of tickers per SQS message)
CHUNK_SIZE=100
```

{{% notice tip %}}
Setting `CHUNK_SIZE=100` means each SQS message contains 100 ticker symbols. For a universe of 3,000 tickers, the system generates 30 SQS messages, allowing 30 Lambda Consumer functions to run in parallel and collect market data concurrently.
{{% /notice %}}