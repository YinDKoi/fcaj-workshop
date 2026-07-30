---
title: "Week 3 Worklog"
date: 2026-06-22
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Develop AWS Lambda functions for stock data collection.
* Build the automated serverless data collection workflow.
* Integrate Amazon EventBridge and Amazon SQS into the pipeline.
* Test the complete stock data collection process.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ------------------ |
| 1 | Develop an AWS Lambda function to collect stock market data from Yahoo Finance. | 06/22/2026 | 06/22/2026 | AWS Lambda Documentation |
| 2 | Develop the Producer Lambda function to publish messages to Amazon SQS for parallel processing. | 06/23/2026 | 06/23/2026 | Amazon SQS Documentation |
| 3 | Configure Amazon EventBridge to automatically trigger the data collection workflow on a scheduled basis. | 06/24/2026 | 06/24/2026 | Amazon EventBridge Documentation |
| 4 | Implement the Consumer Lambda function to receive SQS messages and process collected stock data. | 06/25/2026 | 06/25/2026 | AWS Lambda Documentation |
| 5 | Store collected stock data in Amazon S3 following the predefined storage structure. | 06/26/2026 | 06/26/2026 | Amazon S3 Documentation |
| 6 | Perform end-to-end testing of the EventBridge → Lambda → Amazon SQS → Lambda → Amazon S3 workflow. | 06/27/2026 | 06/27/2026 | Internal Testing |
| 7 | Fix implementation issues, optimize Lambda execution time, and verify data stored in Amazon S3. | 06/28/2026 | 06/28/2026 | Testing Report |

---

### Week 3 Achievements:

* Successfully developed AWS Lambda functions for automated stock data collection.

* Configured Amazon EventBridge to trigger the serverless workflow automatically based on a predefined schedule.

* Implemented asynchronous processing using Amazon SQS to improve the scalability of the system.

* Successfully collected stock market data from Yahoo Finance and stored it in Amazon S3 following the designed storage architecture.

* Completed end-to-end testing of the EventBridge, AWS Lambda, Amazon SQS, and Amazon S3 workflow.

* Optimized Lambda execution performance and resolved issues identified during system testing.

* Successfully completed the Data Collection Pipeline and prepared the project for the ETL implementation phase in the following week.