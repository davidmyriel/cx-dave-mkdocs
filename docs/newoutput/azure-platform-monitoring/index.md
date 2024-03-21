---
title: "Azure Platform Monitoring"
date: "2023-09-13"
---

[Microsoft Azure](https://coralogixstg.wpengine.com/docs/introduction-to-microsoft-azure/) platform monitoring focuses on capturing [platform logs](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/platform-logs-overview) - **Microsoft Entra ID, Activity, and Resource logs** - from various components within your environment.

## Overview

Azure [platform logs](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/platform-logs-overview), automatically generated, provide detailed diagnostic and auditing information for Azure resources and the Azure platform they depend on. They are divided as follows:

- **Microsoft Entra ID** **logs**, which capture activity at the authentication layer

- **Azure Activity logs**, which capture activity at the subscription layer

- **Resource logs**, which capture activity at the resource layer

## **Microsoft Entra ID** Logs

**Microsoft Entra ID logs** (previously **Azure Active Directory logs**) capture all the actions performed against your Active Directory domain. This includes the history of sign-in activity and an audit trail of changes made in Azure AD for a particular tenant. This type of log is focused on the Authentication layer of Azure. Find out more [here](https://coralogixstg.wpengine.com/docs/azure-active-directory-logs/).

## Activity Logs

**Activity logs** provide insight into the operations performed _on_ each Azure resource in the subscription from the outside, known as the management plane. This can include the creation or deletion of resources or updates to existing services. There's a single activity log for each Azure subscription. Find out more [here](https://coralogixstg.wpengine.com/docs/azure-activity-logs/).

## Resource Logs

**Resource logs** \[previously referred to as diagnostic logs\] capture resource-specific audit information, providing insight into operations performed within an Azure resource. This is known as the data plane. Examples include a connection made to a PostgreSQL server or when a Blob is created, read, or deleted from a storage account. The contents of resource logs vary according to the Azure service and resource type. Find out more [here](https://coralogixstg.wpengine.com/docs/azure-resource-logs/).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/introduction-to-microsoft-azure/">Introduction to Microsoft Azure</a></strong></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to answer any questions that may come up. Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
