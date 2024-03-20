---
title: "Azure Event Hub Terraform Module"
date: "2022-06-27"
---

Using our Terraform modules, you can easily install and manage Coralogix integrations with Azure services as modules in your infrastructure code. This tutorial demonstrates how to install [our function app](https://github.com/coralogix/coralogix-azure-serverless/tree/master/EventHub) that connects to your Event Hub and sends logs to Coralogix.

Our modules are open-source and available on [Github](https://github.com/coralogix/terraform-coralogix-azure) and in the [Terraform registry](https://registry.terraform.io/modules/coralogix/azure/coralogix/latest).

## Prerequisites

- A resource group and storage account to be used by your function app must be provided as inputs in the Terraform module.

- The Event Hub namespace and instance must be preexisting.

## Installation

To install our [function app](https://github.com/coralogix/coralogix-azure-serverless/tree/master/EventHub), which connects to your Event Hub and sends logs to Coralogix, add this declaration to your Terraform project:

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

- Input the following variables:
    - `**CoralogixRegion**`: The [region](https://coralogixstg.wpengine.com/docs/coralogix-domain/) associated with your Coralogix account
    
    - **`CoralogixPrivateKey`:** Your [Coralogix Send Your Data - API Key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)
    
    - `**CoralogixApplication**` & **`CoralogixSubsystem`**: [Coralogix application and subsystem names](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as they will appear in your UI

- Descriptions for other variables can be found [here](https://github.com/coralogix/terraform-coralogix-azure/blob/master/modules/blobstorage/README.md).

- An SAS policy will be created by the Terraform module to allow listen access to the Event Hub instance by the function app.

## Additional Resources

<table><tbody><tr><td><strong>Documentation</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/">Coralogix Terraform Provider</a></td></tr><tr><td><strong>External Documentation</strong></td><td><a href="https://github.com/coralogix/coralogix-azure-serverless/tree/master/DiagnosticData">Coralogix Function App</a><br><a href="https://github.com/coralogix/terraform-coralogix-azure">Github: Azure – Coralogix Terraform Module</a><br><a href="https://registry.terraform.io/modules/coralogix/azure/coralogix/latest">Terraform Registry</a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
