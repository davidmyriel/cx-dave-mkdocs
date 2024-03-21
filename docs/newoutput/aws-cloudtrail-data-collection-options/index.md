---
title: "AWS CloudTrail: Data Collection Options"
date: "2023-07-06"
---

**AWS CloudTrail** is an AWS service that helps you enable operational and risk auditing, governance, and compliance of your AWS account. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail.

Use any of our customized log collection options to allow Coralogix to ingest the logs stored in your Amazon S3 bucket and process them for further analysis and monitoring.

## Overview

Coralogix provides multiple methods in which you can collect logs from Amazon CloudTrail. Send us your CloudTrail data from your Amazon S3 bucket using an **AWS Lambda function**, with one of two event-driven design patterns:

- Invoke the Lambda **directly through an S3 event**

![](https://coralogixstg.wpengine.com/wp-content/uploads/2023/07/S3-frame.svg)

- Send the S3 event to a **Simple Notification Service (SNS)** queue, which in turn triggers the Lambda

![](https://coralogixstg.wpengine.com/wp-content/uploads/2023/08/AWS-3_diagram.svg)

## Customized Log Collection Options

Use any of our customized log collection options to allow Coralogix to ingest the logs stored in your Amazon S3 bucket and process them for further analysis and monitoring.

- **Automated Integration Packages (Recommended)**. For both S3 event and SNS design patterns, deploy the Coralogix Lambda function using our two-step, automated [integration packages](https://coralogixstg.wpengine.com/docs/integration-packages/).

- **Serverless Application Repository (SAR).** Deploy our Coralogix Lambda function via our AWS serverless application repository. Use this for [S3 event](https://coralogixstg.wpengine.com/docs/aws-cloudtrail/) or [SNS](https://coralogixstg.wpengine.com/docs/aws-coudtrail-log-collection-via-sns-trigger/) design patterns.

- **Terraform**. For both S3 event and SNS design patterns, install and manage the CloudTrail integration with AWS services as modules in your infrastructure code using our [AWS CloudTrail Terraform module](https://coralogixstg.wpengine.com/docs/terraform-modules-for-aws-cloudtrail/).

## Best Practices

For any customized data collection option, customers should add environment variable `CORALOGIX_BUFFER_SIZE` with value `268435456`.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
