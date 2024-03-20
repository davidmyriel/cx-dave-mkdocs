---
title: "Introduction to Microsoft Azure"
date: "2023-08-27"
---

The Coralogix Azure integrations enable the collection of logs and metrics from your [Azure environment](https://azure.microsoft.com/en-us/products/active-directory). Gain insights into role, user, group and directory management, successful and failed sign-in events, and application management data that helps you understand your users' experience and immediately troubleshoot any errors.

Connect to Microsoft Azure to:

- Visualize the performance of Event Hub, Blob and Queue Storage and correlate their performance with your applications

- Collect standard Azure Monitor metrics for all Azure services

- Tag your Azure metrics with Azure-specific information about the associated resource, such as region, resource group, and custom Azure tags

- Correlate data from your Azure applications across logs, metrics, APM tracing, user activity, and more within your Coralogix account

## Overview

Coralogix Azure integrations allow you to monitor activity in the [Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/fundamentals/whatis), a cloud-based identity and access management service. Here’s how it works:

1. Microsoft Entra ID sends [logs](https://learn.microsoft.com/en-us/azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub) and [metrics](https://learn.microsoft.com/en-us/azure/event-hubs/monitor-event-hubs-reference) to [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/overview).

3. Azure Monitor streams the data to [Azure Event Hub](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-about).

5. Once Azure Monitor receives the data, an event hub triggers its Azure function to send the data onto an HTTP source on a Coralogix-hosted collector. The HTTP source receives and ingests the monitoring data from the Azure function. It can be configured for logs or metrics.

7. After being triggered by its event hub, an Azure function sends the logs or metrics to a configured HTTP source on a hosted collector.

![](https://coralogixstg.wpengine.com/wp-content/uploads/2023/08/Azure_diagram-1.svg)

## Prerequisites

- An Azure subscription associated to the Microsoft Entra ID

- Microsoft Entra ID [requirements](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor) met in order to export logs to reports

- Azure [resource group](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal)

## Configuration

### **Basics**

Coralogix offers integrations for Azure [Event Hub](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-about), [Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction), [Queue Storage](https://learn.microsoft.com/en-us/azure/storage/queues/storage-queues-introduction), and [Diagnostic Metrics](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/metrics-instrumentation). Each integration provides instructions for setting up the ingestion pipeline from Microsoft Entra ID to Coralogix. This includes the creation of a storage account and storage, as well as configuration of Terraform and Azure Resource Manager (ARM) templates with the resource created.

### Azure Resource Manager (ARM) Deployments

Coralogix provides Azure Resource Manager (ARM) custom template deployments to build your log and metric pipelines. Each template creates an Event Hub to which Azure Monitor streams logs or metrics, an Azure function for sending monitoring data to Coralogix, and storage accounts to which the function writes its own log messages about successful and failed transmissions.

Select an automatic integration using ARM custom template deployments:

- [Event Hub](https://coralogixstg.wpengine.com/docs/azure-eventhub-trigger-function/)

- [Blob Storage](https://coralogixstg.wpengine.com/docs/blobstorage-microsoft-azure-functions/)

- [Queue Storage](https://coralogixstg.wpengine.com/docs/queue-storage-microsoft-azure-functions/)

- [Diagnostic Data](https://coralogixstg.wpengine.com/docs/diagnostic-data-microsoft-azure-resource-manager-arm/)

### Optional Configurations

Coralogix offers [optional configurations](https://coralogixstg.wpengine.com/docs/optional-configurations-microsoft-azure/) for particular use-cases utilizing our Azure deployments. If you require resource monitoring in Azure storage accounts or Event Hubs that cannot be made public, [deploy our function apps with virtual network (VNet) support](https://coralogixstg.wpengine.com/docs/optional-configurations-microsoft-azure/#storage-accounts--event-hubs-with-restricted-public-access).

### Terraform Modules

Create all of your resources in one Coralogix-provided file. Using our Terraform modules, you can easily install and manage Coralogix integrations with Azure services as modules in your infrastructure code:

- [Event Hub](https://coralogixstg.wpengine.com/docs/terraform-modules-for-azure-eventhub/)

- [Blob Storage via Event Grid](https://coralogixstg.wpengine.com/docs/terraform-modules-for-azure-blob-storage-via-event-grid/)

- [Queue Storage](https://coralogixstg.wpengine.com/docs/terraform-module-for-azure-queue-storage/)

- [Diagnostic Data](https://coralogixstg.wpengine.com/docs/terraform-modules-for-azure-diagnostic-data/)

## Azure Platform Monitoring

Each integration provides you with a pre-made package of extensions, allowing you easily and swiftly to monitor the data in your Coralogix dashboard. See our [Azure Platform Monitoring Guide](https://coralogixstg.wpengine.com/docs/azure-platform-monitoring/).

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
