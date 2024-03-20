---
title: "AWS VPC Flow Logs Terraform Module"
date: "2023-05-24"
---

Using Coralogix Terraform Modules, you can easily install and manage Coralogix integrations with AWS services as modules in your infrastructure code. This tutorial demonstrates how to install the [VPC Flow Logs collection Lambda](https://coralogixstg.wpengine.com/docs/aws-vpc-flow-logs/).

Our modules are open source and available on our [GitHub](https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudtrail) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/aws/coralogix/latest).

## Installation

Install our [](https://coralogixstg.wpengine.com/docs/aws-vpc-flow-logs/)[VPC Flow Logs collection Lambda](https://coralogixstg.wpengine.com/docs/aws-vpc-flow-logs/) by adding this declaration to your Terraform project:

```vhdl
module "coralogix-shipper-vpc-flow-logs" {
  source = "coralogix/aws/coralogix//modules/s3"

  coralogix_region   = "Europe"
  private_key        = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXX"
  application_name   = "vpc-flow-logs"
  subsystem_name     = "logs"
  s3_bucket_name     = "test-bucket-name"
  integration_type   = "vpc-flow-logs"
}
```

## AWS Secrets Manager

Deploy the [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/) Lambda layer for any of our AWS integrations. Find out more [here](https://coralogixstg.wpengine.com/docs/aws-secrets-manager-lambda-layer/).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/aws-vpc-flow-logs/"><strong>AWS VPC Flow Logs</strong></a><br><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/" target="_blank" rel="noreferrer noopener"><strong>Coralogix Terraform Provider</strong></a></td></tr><tr><td>External Documentation</td><td><a href="https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudtrail" target="_blank" rel="noreferrer noopener"><strong>GitHub</strong></a><br><a href="https://registry.terraform.io/modules/coralogix/aws/coralogix/latest" target="_blank" rel="noreferrer noopener"><strong>Terraform Registry</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
