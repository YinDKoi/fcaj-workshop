---
title: "Week 3 Worklog"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Develop the automated stock data collection pipeline.
* Implement AWS Lambda to retrieve stock data from Yahoo Finance.
* Store raw datasets in Amazon S3.
* Validate the event-driven data ingestion workflow.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | -------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ---------------------- |
| 2 | - Develop the Lambda Function for collecting stock market data from Yahoo Finance.<br>- Study the response format returned by the API. | 06/29/2026 | 06/29/2026 | Yahoo Finance Documentation |
| 3 | - Process the collected data and convert it into JSON format.<br>- Design the storage structure in Amazon S3. | 06/30/2026 | 06/30/2026 | Amazon S3 Documentation |
| 4 | - Integrate Lambda with Amazon EventBridge to schedule daily data collection.<br>- Verify the upload process to Amazon S3. | 07/01/2026 | 07/01/2026 | AWS Lambda Documentation |
| 5 | - Integrate Amazon SQS for asynchronous message delivery.<br>- Test message publishing and consumption. | 07/02/2026 | 07/03/2026 | Amazon SQS Documentation |
| 6 | - Perform end-to-end testing of the data ingestion workflow.<br>- Monitor Lambda execution using Amazon CloudWatch and resolve identified issues. | 07/04/2026 | 07/05/2026 | Amazon CloudWatch Documentation |

### Week 3 Achievements:

* Successfully implemented the Lambda Function for collecting stock market data from Yahoo Finance.

* Built an automated data collection workflow using Amazon EventBridge.

* Stored raw datasets in Amazon S3 following the predefined folder structure.

* Integrated Amazon SQS to support asynchronous communication between system components.

* Monitored Lambda execution using Amazon CloudWatch to ensure stable and reliable data collection.

* Completed the Data Ingestion phase, providing the foundation for data validation and ETL processing in the following development stage.