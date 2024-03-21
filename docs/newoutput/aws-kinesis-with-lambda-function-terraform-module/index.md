---
title: "AWS Kinesis with Lambda Function Terraform Module"
date: "2023-06-08"
---

Using Coralogix Terraform Modules, you can easily install and manage Coralogix integrations with AWS services as modules in your infrastructure code. This tutorial demonstrates how to install [AWS Kinesis with a Lambda Function](https://coralogixstg.wpengine.com/docs/aws-kinesis-with-lambda-function/).

Our modules are open source and available on our [GitHub](https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudtrail) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/aws/coralogix/latest).

## Installation

Install our [](https://coralogixstg.wpengine.com/docs/aws-vpc-flow-logs/)[](https://coralogixstg.wpengine.com/docs/aws-vpc-flow-logs/)[AWS Kinesis with a Lambda Function](https://coralogixstg.wpengine.com/docs/aws-kinesis-with-lambda-function/) by adding this declaration to your Terraform project:

```
module "kinesis" {
  source = "coralogix/aws/coralogix//modules/kinesis"

  coralogix_region    = "Europe"
  private_key         = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXX"
  ssm_enable          = "false"
  layer_arn           = "<your layer arn>"
  application_name    = "kinesis"
  subsystem_name      = "logs"
  kinesis_stream_name = "<your kinesis stream name>"
}
```

## Additional Resources

<table><tbody><tr><td><strong>Documentation</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/" target="_blank" rel="noreferrer noopener">Coralogix Terraform Provider</a></td></tr><tr><td><strong>External Documentation</strong></td><td><a href="https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudtrail" target="_blank" rel="noreferrer noopener">GitHub</a><br><a href="https://registry.terraform.io/modules/coralogix/aws/coralogix/latest" target="_blank" rel="noreferrer noopener">Terraform Registry</a></td></tr></tbody></table>

## S**upport**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
