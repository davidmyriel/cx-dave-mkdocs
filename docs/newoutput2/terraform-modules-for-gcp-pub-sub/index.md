---
title: "GCP Pub/Sub Terraform Module"
date: "2022-08-14"
---

Terraform simplifies the way we deploy our infrastructure and allows us to maintain it as code.

Using our Terraform Modules, you can easily install and manage Coralogix integrations with GCP services as modules in your infrastructure code.

Our modules are open source and available on our [GitHub](https://github.com/coralogix/terraform-coralogix-google) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/google/coralogix/latest).

## Installation

This module will be installing [our function app](https://github.com/coralogix/coralogix-gcp-serverless/tree/master/gcp-pub-sub) that gets messages from your Pub/Sub topic and sends logs to Coralogix.

For more information about the Function itself, dynamic applicationName and subsystemName and multiline support visit our [Pub/Sub guide.](https://coralogixstg.wpengine.com/docs/google-cloud-pub-sub/)

To use the module, first, add the provider configure block to your Terraform project:

```vhdl
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = ">= 4.31.0"
    }
  }
}

provider "google" {
  project = "your-project-id"
  region  = "your-region"
}
```

Then simply add this declaration to your Terraform project:

```vhdl
module "pubsub" {
  source = "coralogix/google/coralogix//modules/pubsub"

  coralogix_region = "Europe"
  private_key      = "2f55c873-c0cf-4523-82d4-c3b68ee6cb46"
  application_name = "Pub/Sub"
  subsystem_name   = "logs"
  topic           = "test-topic-name"
}
```

Important variables to change:

- coralogix\_region - The Coralogix location region, possible options are \[Europe, Europe2, India, Singapore, US\]

- private\_key - The Coralogix [private key](http://Settings Menu --> Send Your Logs tab) which is used to validate your authenticity.

- application\_name - The received logs applicationName.

- subsystem\_name - The received logs subsystemName.

- topic - Your GCP Pub/Sub topic name.

## Configuring a sink

When configuring a GCP logs router sink to catch logs from different resources andthem send to a pub/sub topic be sure to EXCLUDE the function created above from the sink.

**NOTE- not following the next step will result in an infinite loop in your GCP function, RESULTING IN HIGH GCP COST; please proceed with caution.**

```vhdl
resource "google_logging_project_sink" "logs-sink" {
  name        = "logs-sink"
  destination = "pubsub.googleapis.com/projects/<my-project-name>/topics/<my-pubsub-topic-name>"

  exclusions {
    name        = "pub-sub-func"
    description = "Exclude logs from the coralogix pub-sub function"
    filter      = "resource.labels.function_name=\"${module.pubsub.function}\""
  }

  unique_writer_identity = true
}
```

Note: replace <my-project-name> and <my-pubsub-topic-name> with your relevant values.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/">Coralogix Terraform Provider</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
