---
title: "AWS CloudWatch Terraform Module"
date: "2022-05-23"
---

Terraform simplifies the manner we deploy our infrastructure and allows us to maintain it as code.

Using our Terraform Modules, you can easily install and manage Coralogix integrations with AWS services as modules in your infrastructure code.

Our modules are open source and available on our [GitHub](https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudwatch-logs) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/aws/coralogix/latest).

## Installation

This module will be installing [our Cloudwatch collection lambda](https://coralogixstg.wpengine.com/docs/cloudwatch-logs/).

**STEP 1**. Add this declaration to your Terraform project. Input the following [parameters](https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudwatch-logs).

```vhdl
provider "aws" {
}
module "cloudwatch_logs" {
  source = "coralogix/aws/coralogix//modules/cloudwatch-logs"

  coralogix_region   = "Europe"
  private_key        = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXX"
  application_name   = "cloudwatch"
  subsystem_name     = "logs"
  log_groups         = ["test-log-group"]
}
```

**Notes**:

- Input your Coralogix [](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)[Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) as `private_key`.

- Input the [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/) (as `CustomDomain`) and [region](https://coralogixstg.wpengine.com/docs/coralogix-domain/) (as `coralogix_region`) associated with your Coralogix account.

**STEP 2**. Execute the following:

```
$ terraform init
$ terraform plan
$ terraform apply
```

**STEP 3**. Run `terraform destroy` when you no longer need these resources.

## AWS Secrets Manager

Deploy the [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/) Lambda layer for any of our AWS integrations. Find out more [here](https://coralogixstg.wpengine.com/docs/aws-secrets-manager-lambda-layer/).

## Additional Resources

<table><tbody><tr><td><strong>Documentation</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/" target="_blank" rel="noreferrer noopener">Coralogix Terraform Provider</a></td></tr><tr><td><strong>External Documentation</strong></td><td><a href="https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/cloudwatch-logs" target="_blank" rel="noreferrer noopener">GitHub</a><br><a href="https://registry.terraform.io/modules/coralogix/aws/coralogix/latest" target="_blank" rel="noreferrer noopener">Terraform Registry</a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
