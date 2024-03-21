---
title: "Microsoft Azure Activity and Audit Logs with FileBeat"
date: "2021-08-27"
---

For us to be able to get audit logs from Azure, we are going to use the FileBeat Module.

Azure audit events are sent into an EventHub, from which FileBeat pulls the logs and sends them to Coralogix.

## Prerequisites

- The logs have to be exported first to the event hubs [https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create-kafka-enabled](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create-kafka-enabled)

- to export activity logs to event hubs users can follow the steps here [https://docs.microsoft.com/en-us/azure/azure-monitor/platform/activity-log-export](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/activity-log-export)

- to export audit and sign-in logs to event hubs users can follow the steps here [https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub)

## FileBeat Configuration

We need a Filebeat configured for using Coralogix as an output. Please follow this [documentation](https://coralogixstg.wpengine.com/integrations/filebeat/) if needed.

We will enable the Azure plugin in FileBeat:

```
filebeat modules enable azure 
```

The module contains the following filesets:

```
activitylogs
```

Will retrieve Azure activity logs. Control-plane events on Azure Resource Manager resources. Activity logs provide insight into the operations that were performed on resources in your subscription.

`platformlogs`

Will retrieve Azure platform logs. Platform logs provide detailed diagnostic and auditing information for Azure resources and the Azure platform they depend on.

`signinlogs`

Will retrieve Azure Active Directory sign-in logs. The sign-ins report provides information about the usage of managed applications and user sign-in activities.

`auditlogs`

Will retrieve Azure Active Directory audit logs. The audit logs provide traceability through logs for all changes done by various features within Azure AD. Examples of audit logs include changes made to any resources within Azure AD like adding or removing users, apps, groups, roles, and policies.

We will need to edit the configuration file, normally located in /etc/filebeat/modules.d/azure.yml

```
- module: azure
activitylogs:
enabled: true
var:
# eventhub name containing the activity logs, overwrite he default value if the logs are exported in a different eventhub
eventhub: "insights-operational-logs"
# consumer group name that has access to the event hub, we advise creating a dedicated consumer group for the azure module
consumer_group: "$Default"
# the connection string required to communicate with Event Hubs, steps to generate one here https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-get-connection-string
connection_string: ""
# the name of the storage account the state/offsets will be stored and updated
storage_account: ""
# the storage account key, this key will be used to authorize access to data in your storage account
storage_account_key: ""
platformlogs:
enabled: false
#  var:
#    eventhub: ""
#    consumer_group: "$Default"
#    connection_string: ""
#    storage_account: ""
#    storage_account_key: ""
auditlogs:
enabled: false
#   var:
#     eventhub: "insights-logs-auditlogs"
#     consumer_group: "$Default"
#     connection_string: ""
#     storage_account: ""
#     storage_account_key: ""
signinlogs:
enabled: false
#   var:
#     eventhub: "insights-logs-signinlogs"
#     consumer_group: "$Default"
#     connection_string: ""
#     storage_account: ""
#     storage_account_key: ""
```

We will need to create a storage account where Filebeat will save the position file. That Storage Account will be used in storage\_account.  
Also, we need to retrieve the access key.

Now we only need to restart the FileBeat service.

## Filebeat Azure input type

**Please note-** in case the module dosent work properly, please follow this guidance

First of all, please disable the Azure module by using the command:

```
filebeat modules disable azure 
```

After disabling the module, it's mandatory to specify the input type in the Filebeat.yml.  
The input type should look:

```
filebeat.inputs:
- type: azure-eventhub
  eventhub: "insights-operational-logs"
  consumer_group: "$Default"
  connection_string: "Endpoint=sb://....."
  storage_account: "..."
  storage_account_key: "....."
```

Fill in the values according to Azure, please note that after changing the config, its needed to restart the filebeat service.
