---
title: "Azure Blob Storage via Event Grid Terraform Module"
date: "2023-05-24"
---

Using our Terraform modules, you can easily install and manage Coralogix integrations with Azure services as modules in your infrastructure code. This tutorial demonstrates how to install the Coralogix [function app](https://github.com/coralogix/coralogix-azure-serverless/tree/master/BlobViaEventGrid), allowing you to connect your Blob Storage container and send logs to Coralogix.

Our modules are open-source and available on [Github](https://github.com/coralogix/terraform-coralogix-azure) and in the [Terraform registry](https://registry.terraform.io/modules/coralogix/azure/coralogix/latest).

## Prerequisites

- A resource group and storage account to be used by your function app

- Event Grid system topic configured for "Storage Accounts (Blob & GPv2)" aligned to the storage account containing the Blob Storage container
    - If it is a restricted storage account, review this [optional configuration](https://coralogixstg.wpengine.com/docs/optional-configurations-microsoft-azure) to learn about VNet support options.

## Installation

Install our [function app](https://github.com/coralogix/coralogix-azure-serverless/tree/master/BlobViaEventGrid) that connects to your Blob Storage container and sends logs to Coralogix.

Add this declaration to your Terraform project:

```
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      version = "~> 3.93"
    }
  }
}

provider "azurerm" {
  features {}
}

module "eventhub" {
  source = "coralogix/azure/coralogix//modules/eventhub"

  CoralogixRegion = "Europe"
  CustomDomain = < Custom FQDN if applicable >
  CoralogixPrivateKey = < Send Your Data - API Key >
  CoralogixApplication = "Azure"
  CoralogixSubsystem = "EventHub"
  FunctionResourceGroupName = < Function ResourceGroup Name >
  FunctionStorageAccountName = < Function StorageAccount Name >
  FunctionAppServicePlanType = "Consumption"
  EventhubInstanceName = < Name of EventHub Instance >
  EventhubNamespace = < Name of Eventhub Namespace >
  EventhubResourceGroupName = < Name of Eventhub ResourceGroup >
}

```

**Notes**:

- `**CoralogixRegion**`: The [region](https://coralogixstg.wpengine.com/docs/coralogix-domain/) associated with your Coralogix account

- **`CoralogixPrivateKey`:** Your [Coralogix Send Your Data - API Key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)

- `**CoralogixApplication**` & **`CoralogixSubsystem`**: [Application and subsystem names](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as they appear in your Coralogix UI

- Descriptions for other variables can be found [here](https://github.com/coralogix/terraform-coralogix-azure/blob/master/modules/blobstorage/README.md).

## Additional Resources

<table><tbody><tr><td><strong>Documentation</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/">Coralogix Terraform Provider</a></td></tr><tr><td><strong>External Documentation</strong></td><td><a href="https://github.com/coralogix/terraform-coralogix-azure">Github</a><br><a href="https://registry.terraform.io/modules/coralogix/azure/coralogix/latest">Terraform Registry</a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
