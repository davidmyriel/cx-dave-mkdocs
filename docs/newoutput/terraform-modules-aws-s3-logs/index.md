---
title: "AWS S3 Logs Collection Terraform Module"
date: "2022-05-26"
---

Terraform simplifies the way we deploy our infrastructure and allows us to maintain it as code.

Using our Terraform Modules, you can easily install and manage Coralogix integrations with AWS services as modules in your infrastructure code.

Our modules are open source and available on our [Github](https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/s3) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/aws/coralogix/latest/submodules/s3)

## Installation

Install our [S3 Log Collection Lambda](https://coralogixstg.wpengine.com/docs/data-collection-s3/) by adding this declaration to your Terraform project.

```vhdl
module "coralogix-shipper-s3" {
  source = "coralogix/aws/coralogix//modules/s3"

  coralogix_region   = "Europe"
  private_key        = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXX"
  application_name   = "s3"
  subsystem_name     = "logs"
  s3_bucket_name     = "test-bucket-name"
  integration_type   = "s3"
}
```

## AWS Secrets Manager

Deploy the [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/) Lambda layer for any of our AWS integrations. Find out more [here](https://coralogixstg.wpengine.com/docs/aws-secrets-manager-lambda-layer/).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/" target="_blank" rel="noreferrer noopener"><strong>Coralogix Terraform Provider</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
