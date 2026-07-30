---
title: "Week 2 Worklog"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---



### Week 2 Objectives:

* Set up the AWS cloud environment for the project.
* Configure the core cloud resources required for the ETL pipeline.
* Prepare the storage structure and event-driven workflow.
* Establish a secure development environment using IAM and AWS CLI.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 1 | - Configure the AWS development environment.<br>- Verify AWS CLI credentials and default region settings.<br>- Create IAM users and roles for project development. | 06/22/2026 | 06/22/2026 | AWS IAM Documentation |
| 2 | - Create Amazon S3 buckets for storing raw, processed, and model-related data.<br>- Organize folder structures for different stages of the ETL pipeline. | 06/23/2026 | 06/23/2026 | Amazon S3 Documentation |
| 3 | - Configure Amazon EventBridge to automate the daily data collection process.<br>- Design the event scheduling strategy for Lambda execution. | 06/24/2026 | 06/24/2026 | Amazon EventBridge Documentation |
| 4 | - Create and configure Amazon SQS queues for asynchronous task processing.<br>- Review message flow and retry mechanisms. | 06/25/2026 | 06/25/2026 | Amazon SQS Documentation |
| 5 | - Configure project permissions between AWS services.<br>- Verify connectivity among EventBridge, Lambda, SQS, and S3 resources. | 06/26/2026 | 06/27/2026 | AWS Documentation |
| 6 | - Test the initial cloud infrastructure.<br>- Document the deployment process and resource configuration. | 06/28/2026 | 06/28/2026 | Project Documentation |

---

### Week 2 Achievements:

* Successfully configured the AWS development environment for the project.

* Created and organized Amazon S3 buckets to support multiple stages of the ETL workflow, including:
  * Raw data storage
  * Processed data storage
  * Model-related artifacts

* Configured Amazon EventBridge to support scheduled execution of serverless workflows.

* Established Amazon SQS as the messaging service for asynchronous communication between Lambda functions.

* Configured IAM roles and permissions to enable secure interaction among AWS services while following the principle of least privilege.

* Verified the connectivity between the primary cloud resources, ensuring that the event-driven architecture functioned as expected.

* Completed the initial cloud infrastructure required for implementing the serverless ETL pipeline in the following development phases.