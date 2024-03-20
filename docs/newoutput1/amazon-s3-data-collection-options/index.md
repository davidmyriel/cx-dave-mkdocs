---
title: "Amazon S3: Data Collection Options"
date: "2023-07-06"
---

**Amazon Simple Storage Service (Amazon S3)** is an object storage service that offers industry-leading scalability, data availability, security, and performance. You can record the actions that are taken by users, roles, or AWS services on Amazon S3 resources and maintain log records for auditing and compliance purposes.

Use any of our customized log collection options to allow Coralogix to ingest the logs stored in your Amazon S3 bucket and process them for further analysis and monitoring.

## Overview

Coralogix provides multiple methods in which you can collect logs from Amazon S3. Send us your CloudTrail data from your Amazon S3 bucket using an **AWS Lambda function**, with one of two event-driven design patterns:

- Invoke the Lambda **directly through an S3 event**

- Send the S3 event to a **Simple Notification Service (SNS)** queue, which in turn triggers the Lambda

## Customized Log Collection Options

Use any of our customized log collection options to allow Coralogix to ingest the logs stored in your Amazon S3 bucket and process them for further analysis and monitoring.

- **Automated Integration Packages (Recommended)**. For both S3 event and SNS design patterns, deploy the Coralogix Lambda function using our two-step, automated [integration packages](https://coralogixstg.wpengine.com/docs/integration-packages/).

- **Serverless Application Repository (SAR).** Deploy our Coralogix Lambda function via our AWS serverless application repository. Use this for [S3 event](https://coralogixstg.wpengine.com/docs/data-collection-s3/) or [SNS](https://coralogixstg.wpengine.com/docs/amazon-web-services-aws-s3-log-collection-via-sns-trigger/) design patterns.

- **Terraform**. For both S3 event and SNS design patterns, install and manage the Amazon S3 integration with AWS services as modules in your infrastructure code using our [AWS S3 Log Collection Terraform module](https://coralogixstg.wpengine.com/docs/terraform-modules-aws-s3-logs/).

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
