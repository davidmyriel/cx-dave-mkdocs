---
title: "AWS Terraform Module"
date: "2023-02-15"
---

Terraform simplifies the way we deploy our infrastructure and allows us to maintain it as code.

Using our Terraform Modules, you can easily install and manage Coralogix integrations with AWS services as modules in your infrastructure code.

Our modules are open source and available on our [Github](https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudtrail) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/aws/coralogix/latest).

## Prerequisites

**STEP 1**. [Sign up](https://signup.coralogixstg.wpengine.com/#/) for a Coralogix account. Set up your account on the Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/) corresponding to the region within which you would like your data stored.

**STEP 2**. Access your Coralogix API key:

- In your Coralogix navigation bar, click **Data Flow** > **API Keys** > **Alerts, Rules and Tags API Key**.

- Copy this information.

**STEP 3**. [Install](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) Terraform.

## Installation

Install the AWS service of your choice:

- [CloudTrail](https://coralogixstg.wpengine.com/docs/terraform-modules-for-aws-cloudtrail/)

- [S3 Log Collection](https://coralogixstg.wpengine.com/docs/terraform-modules-aws-s3-logs/)

- [CloudWatch](https://coralogixstg.wpengine.com/docs/terraform-modules/)

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
