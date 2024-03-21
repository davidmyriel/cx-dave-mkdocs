---
title: "Azure Queue Storage Terraform Module"
date: "2023-05-29"
---

Using our Terraform modules, you can easily install and manage Coralogix integrations with Azure services as modules in your infrastructure code. This tutorial demonstrates how to install our [function app](https://github.com/coralogix/coralogix-azure-serverless/tree/master/StorageQueue) that connects to your storage queue and sends logs to Coralogix.

Our modules are open-source and available on [Github](https://github.com/coralogix/terraform-coralogix-azure) and in the [Terraform registry](https://registry.terraform.io/modules/coralogix/azure/coralogix/latest).

## Prerequisites

- A resource group and storage account to be used by your function app and provided as inputs in the Terraform module

- Preexisting storage queue

- Storage account associated with the storage queue configured for public access (Optional VNet support configuration available)

## Installation

To install our [function app](https://github.com/coralogix/coralogix-azure-serverless/tree/master/EventHub), which connects to your storage queue and sends logs to Coralogix, add this declaration to your Terraform project:

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

module "storagequeue" {
  source = "coralogix/azure/coralogix//modules/storagequeue"

  CoralogixRegion = "Europe"
  CustomDomain = < Custom FQDN if applicable >
  CoralogixPrivateKey = < Send Your Data - API Key >
  CoralogixApplication = "Azure"
  CoralogixSubsystem = "EventHub"
  FunctionResourceGroupName = < Function ResourceGroup Name >
  FunctionStorageAccountName = < Function StorageAccount Name >
  FunctionAppServicePlanType = "Consumption"
  StorageQueueName = < Name of the StorageQueue >
  StorageQueueStorageAccount = < Name of the StorageQueue Storage Account >
  StorageQueueResourceGroupName = < Name of the StorageQueue Resource Group >
}

```

**Notes**:

- Input the following variables:
    - `**CoralogixRegion**`: The [region](https://coralogixstg.wpengine.com/docs/coralogix-domain/) associated with your Coralogix account
    
    - **`CoralogixPrivateKey`:** Your [Coralogix Send-Your-Data API Key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)
    
    - `**CoralogixApplication**` & **`CoralogixSubsystem`**: [Coralogix application and subsystem names](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as they will appear in your UI

- Descriptions for other variables can be found [here](https://github.com/coralogix/terraform-coralogix-azure/blob/master/modules/blobstorage/README.md).

## Additional Resources

<table><tbody><tr><td><strong>Documentation</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/">Coralogix Terraform Provider</a></td></tr><tr><td><strong>External Documentation</strong></td><td><a href="https://github.com/coralogix/coralogix-azure-serverless/tree/master/DiagnosticData">Coralogix Function App</a><br><a href="https://github.com/coralogix/terraform-coralogix-azure">Github: Azure – Coralogix Terraform Module</a><br><a href="https://registry.terraform.io/modules/coralogix/azure/coralogix/latest">Terraform Registry</a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
