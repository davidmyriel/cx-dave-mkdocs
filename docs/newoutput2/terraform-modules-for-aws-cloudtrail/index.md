---
title: "AWS CloudTrail Terraform Module"
date: "2022-06-09"
---

Using Coralogix Terraform Modules, you can easily install and manage Coralogix integrations with AWS services as modules in your infrastructure code. This tutorial demonstrates how to install our [CloudTrail collection Lambda](https://coralogixstg.wpengine.com/docs/aws-cloudtrail/).

Our modules are open source and available on our [Github](https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudtrail) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/aws/coralogix/latest).

## Installation

Install our [CloudTrail collection Lambda](https://coralogixstg.wpengine.com/docs/aws-cloudtrail/) by adding this declaration to your Terraform project:

```vhdl
module "coralogix-shipper-cloudtrail" {
  source = "coralogix/aws/coralogix//modules/s3"

  coralogix_region   = "Europe"
  private_key        = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXX"
  application_name   = "cloudtrail"
  subsystem_name     = "logs"
  s3_bucket_name     = "test-bucket-name"
  integration_type   = "cloudtrail"
}
```

## Best Practices

Customers should add environment variable `CORALOGIX_BUFFER_SIZE` with value `268435456`.

## AWS Secrets Manager

Deploy the [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/) Lambda layer for any of our AWS integrations. Find out more [here](https://coralogixstg.wpengine.com/docs/aws-secrets-manager-lambda-layer/).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/" target="_blank" rel="noreferrer noopener"><strong>Coralogix Terraform Provider</strong></a></td></tr><tr><td>External Documentation</td><td><a href="https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudtrail" target="_blank" rel="noreferrer noopener"><strong>GitHub</strong></a><br><a href="https://registry.terraform.io/modules/coralogix/aws/coralogix/latest" target="_blank" rel="noreferrer noopener"><strong>Terraform Registry</strong></a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
