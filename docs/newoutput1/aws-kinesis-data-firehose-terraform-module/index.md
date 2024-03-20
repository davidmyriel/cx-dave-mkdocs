---
title: "AWS Kinesis Data Firehose Terraform Module"
date: "2023-11-29"
---

Using **Coralogix Terraform modules**, you can easily install and manage Coralogix integrations with AWS services as modules in your infrastructure code. Our modules are open source and available on our [GitHub](https://github.com/coralogix/terraform-coralogix-aws/) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/aws/coralogix/latest).

This tutorial demonstrates how to install **AWS Kinesis Data Firehose** for [logs](https://coralogixstg.wpengine.com/docs/aws-firehose/) and [metrics](https://coralogixstg.wpengine.com/docs/amazon-kinesis-data-firehose-metrics/)**.**

## Installation

### Logs

For logs, install [AWS Kinesis Data Firehose](https://coralogixstg.wpengine.com/docs/aws-firehose/) by adding this declaration to your Terraform project:

```
module "cloudwatch_firehose_coralogix_logs" {
  source                         = "coralogix/aws/coralogix//modules/firehose-logs"
  private_key                    = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXX"
  firehose_stream                = "coralogix-firehose-logs"
  coralogix_region               = "Europe"
  integration_type_logs          = "Default"
  source_type_logs               = "DirectPut"
}

```

### Metrics

For metrics, install [AWS Kinesis Data Firehose](https://coralogixstg.wpengine.com/docs/amazon-kinesis-data-firehose-metrics/) by also adding this declaration to your Terraform project:

```
module "cloudwatch_firehose_coralogix_metrics" {
  source           = "coralogix/aws/coralogix//modules/firehose-metrics"
  private_key      = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXX"
  firehose_stream  = "coralogix-firehose-metrics"
  coralogix_region = "Europe"
}

```

## Additional Resources

<table><tbody><tr><td><strong>Documentation</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/" target="_blank" rel="noreferrer noopener"><strong>Coralogix Terraform Provider</strong></a></td></tr><tr><td><strong>External Documentation</strong></td><td><strong><a href="https://github.com/coralogix/terraform-coralogix-aws/">GitHub</a><br><a href="https://registry.terraform.io/modules/coralogix/aws/coralogix/latest">Terraform Registry</a></strong></td></tr></tbody></table>

## S**upport**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
