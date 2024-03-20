---
title: "Alert Suppression Rules API"
date: "2024-03-04"
---

## Overview

This document outlines the Alert Suppression Rules API. Alert Suppression Rules allow you to automatically mute alerts according to your specific parameters. You can set what to suppress (what group-by keys), when to suppress (specific times and dates, recurring times and dates, one-time suppressions), and which alerts to suppress during those times.

### Prerequisites

Before you begin, please make sure you have the following:

- [API Key for Alerts, Rules & Tags to successfully authenticate.](https://coralogix.com/docs/alerts-rules-tags-api-key/)

- [Management API Endpoint that corresponds with your Coralogix subscription.](https://coralogix.com/docs/management-api-endpoints/)

- Administrator permissions to manage your services.

## Authentication

Coralogix API uses API keys to authenticate requests. You can view and [manage your API keys](https://coralogix.com/docs/alerts-rules-tags-api-key/) from the Data Flow tab in Coralogix. You need to use this API key in the Authorization request header to successfully connect.

Example:

```
grpcurl -H "Authorization: Bearer API_KEY_HERE"
```

Then, use one of our designated **[Management Endpoints](https://coralogix.com/docs/management-api-endpoints/)** to structure your header.

```
-d @ ng-api-grpc.coralogix.com:443
```

For the Alert Suppression Rules API, the service name will be `AlertSchedulerRuleService`.

```
com.coralogixapis.alerting.alert_scheduler_rule_protobuf.v1.AlertSchedulerRuleService/
```

The complete request header should look like this:

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogix.com:443 com.coralogixapis.alerting.alert_scheduler_rule_protobuf.v1.AlertSchedulerRuleService/
```

## API Endpoints

### AlertSchedulerRuleService

| Method Name | Description |
| --- | --- |
| GetAlertSchedulerRule | Retrieves an alert suppression rule based on the provided rule ID. |
| CreateAlertSchedulerRule | Creates a new alert suppression rule according to the provided parameters. |
| UpdateAlertSchedulerRule | Updates an existing alert suppression rule with new settings and configurations. |
| DeleteAlertSchedulerRule | Deletes a specified alert suppression rule, removing it from the system. |
| GetBulkAlertSchedulerRule | Retrieves multiple alert suppression rules in bulk, potentially filtered by various criteria. |
| CreateBulkAlertSchedulerRule | Creates multiple alert suppression rules in bulk, based on the provided list of rule creation requests. |
| UpdateBulkAlertSchedulerRule | Updates multiple existing alert suppression rules with new settings and configurations in bulk. |

### GetAlertSchedulerRule

### Get Alert Scheduler Rule Request

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule\_id | string |  | The rule ID. |

### GetAlertSchedulerRuleResponse

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule | AlertSchedulerRule |  | Metadata from the alert rule. |

### CreateAlertSchedulerRule

### CreateAlertSchedulerRuleRequest

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule | AlertSchedulerRule |  | Metadata from the alert rule |

### CreateAlertSchedulerRuleResponse

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule | AlertSchedulerRule |  | Metadata from the alert rule |

### UpdateAlertSchedulerRule

### UpdateAlertSchedulerRuleRequest

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule | AlertSchedulerRule |  | Metadata from the alert rule |

### UpdateAlertSchedulerRuleResponse

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule | AlertSchedulerRule |  | Metadata from the alert rule |

### DeleteAlertSchedulerRule

### DeleteAlertSchedulerRuleRequest

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule\_id | string |  | The rule ID. |

### DeleteAlertSchedulerRuleResponse

| Field | Type | Label | Description |
| --- | --- | --- | --- |
|  |  |  |  |

### GetBulkAlertSchedulerRule

### GetBulkAlertSchedulerRuleRequest

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| active\_timeframe | ActiveTimeframe |  |  |
| enabled | bool | optional |  |
| alert\_scheduler\_rules\_ids | FilterByAlertSchedulerRuleIds |  |  |
| next\_page\_token | string | optional |  |

### GetBulkAlertSchedulerRuleResponse

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rules | AlertSchedulerRuleWithActiveTimeframe | repeated |  |
| next\_page\_token | string |  |  |

### CreateBulkAlertSchedulerRule

### CreateBulkAlertSchedulerRuleRequest

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| create\_alert\_scheduler\_rule\_requests | CreateAlertSchedulerRuleRequest | repeated |  |

### CreateBulkAlertSchedulerRuleResponse

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| create\_suppression\_responses | CreateAlertSchedulerRuleResponse | repeated |  |

### UpdateBulkAlertSchedulerRule

### UpdateBulkAlertSchedulerRuleRequest

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| update\_alert\_scheduler\_rule\_requests | UpdateAlertSchedulerRuleRequest | repeated |  |

### UpdateBulkAlertSchedulerRuleResponse

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| update\_suppression\_responses | UpdateAlertSchedulerRuleResponse | repeated |  |

### DeleteBulkAlertSchedulerRule

### DeleteBulkAlertSchedulerRuleRequest

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| delete\_alert\_scheduler\_rule\_requests | DeleteAlertSchedulerRuleRequest | repeated |  |

### DeleteBulkAlertSchedulerRuleResponse

| Field | Type | Label | Description |
| --- | --- | --- | --- |
|  |  |  |  |

### AlertSchedulerRule

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| unique\_identifier | string | optional | Rule unique\_identifier: The rule id. |
| id | string | optional | Rule id: The rule version id. |
| name | string |  | Rule name. |
| description | string | optional | Rule description. |
| meta\_labels | MetaLabel | repeated | Rule meta\_labels: Rule tags over the system. |
| filter | Filter |  | Rule filter: The rule filter definition over alert-events. |
| schedule | Schedule |  | Rule schedule: The schedule time definition, how often the rule will be active. |
| enabled | bool |  | Rule enabled: The rule activation mode. |
| created\_at | string | optional | Rule created\_at: The date-time when the rule was created. |
| updated\_at | string | optional | Rule updated\_at: The date-time when the rule was updated. |

### AlertSchedulerRuleWithActiveTimeframe

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule | AlertSchedulerRule |  |  |
| next\_active\_timeframes | ActiveTimeframe | repeated |  |

### ActiveTimeframe

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| start\_time | string |  | Timeframe start time: The point in the time(date-time) when the rule will start to be active. |
| end\_time | string |  | Timeframe end time: The point in the time(date-time) when the rule will finish to be active. |
| timezone | string |  | Timeframe timezone: The rule will be active according to a specific timezone. |

### Duration

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| for\_over | int32 |  | Duration for\_over: The duration interval. |
| frequency | DurationFrequency |  | Duration frequency: The duration frequency types (minute hour or day). |

### Timeframe

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| start\_time | string |  | Timeframe start time: The point in the time(date-time) when the rule will start to be active. |
| end\_time | string |  | Timeframe end time: The point in the time(date-time) when the rule will finish to be active. |
| duration | Duration |  | Timeframe duration: The duration interval of the rule activation. |
| timezone | string |  | Timeframe timezone: The rule will be active according to a specific timezone. |

### DurationFrequency

| Name | Number | Description |
| --- | --- | --- |
| DURATION\_FREQUENCY\_UNSPEC |  |  |

IFIED | 0 | | | DURATION\_FREQUENCY\_MINUTE | 1 | | | DURATION\_FREQUENCY\_HOUR | 2 | | | DURATION\_FREQUENCY\_DAY | 3 | |

### Recurring

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| always | Recurring.Always |  |  |
| dynamic | Recurring.Dynamic |  |  |

### Recurring.Always

| Field | Type | Label | Description |
| --- | --- | --- | --- |
|  |  |  |  |

### Recurring.Dynamic

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| repeat\_every | int32 |  | Recurring Dynamic repeat\_every: The rule will be activated in a recurring mode according to the interval. |
| daily | Daily |  |  |
| weekly | Weekly |  |  |
| monthly | Monthly |  |  |
| timeframe | Timeframe |  | Recurring Dynamic timeframe: The rule will be activated in a recurring mode according to the specific timeframe. |
| termination\_date | string | optional | Recurring Dynamic termination\_date: The rule will be terminated according to termination\_date. |

### Schedule

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| schedule\_operation | ScheduleOperation |  | Rule schedule\_operation: The rule operation mode (mute/active). |
| one\_time | OneTime |  | Schedule one\_time: The scheduling definition will activate the rule for a specific timeframe. |
| recurring | Recurring |  | Schedule recurring: The scheduling definition will activate the rule for a recurring timeframe. |

### Daily

| Field | Type | Label | Description |
| --- | --- | --- | --- |
|  |  |  |  |

### Weekly

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| days\_of\_week | int32 | repeated | Dynamic Weekly days\_of\_week: The rule will be activated at weekly intervals on specific days in a week. |

### Monthly

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| days\_of\_month | int32 | repeated | Dynamic Monthly days\_of\_month: The rule will be activated at monthly intervals on specific days in a month. |

### OneTime

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| timeframe | Timeframe |  |  |

### ScheduleOperation

| Name | Number | Description |
| --- | --- | --- |
| SCHEDULE\_OPERATION\_UNSPECIFIED | 0 |  |
| SCHEDULE\_OPERATION\_MUTE | 1 |  |
| SCHEDULE\_OPERATION\_ACTIVATE | 2 |  |

### FilterByAlertSchedulerRuleIds

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_ids | AlertSchedulerRuleIds |  |  |
| alert\_scheduler\_version\_ids | AlertSchedulerRuleVersionIds |  |  |

### AlertSchedulerRuleIds

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule\_ids | string | repeated |  |

### AlertSchedulerRuleVersionIds

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| alert\_scheduler\_rule\_version\_ids | string | repeated |  |

### Filter

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| what\_expression | string |  | Filter what\_expression: dataprime expression that filter the alerts group by values. |
| alert\_meta\_labels | MetaLabels |  | Filter alert\_meta\_labels: filter alerts by meta labels tagging. |
| alert\_unique\_ids | AlertUniqueIds |  | Filter alert\_unique\_ids: filter specific alerts (when alert\_unique\_ids is empty meaning it wil filter all alerts). |

### AlertUniqueIds

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| value | string | repeated |  |

### MetaLabels

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| value | MetaLabel | repeated |  |

### MetaLabel

| Field | Type | Label | Description |
| --- | --- | --- | --- |
| id | string | optional | MetaLabel id: The meta label id |
| key | string |  | MetaLabel key: The meta label key |
| value | string | optional | MetaLabel value: The meta label value |
