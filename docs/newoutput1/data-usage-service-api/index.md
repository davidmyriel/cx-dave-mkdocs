---
title: "Data Usage Service API"
date: "2023-04-02"
---

Coralogix provides an API in support of our [Detailed Data Usage Report](https://coralogixstg.wpengine.com/docs/data-usage/), which presents you with all data sent, per policy, for either the current month or retroactively 30 or 90 days. The API allows you to query your data consumption in given a time period.

## Prerequisites

- Coralogix **Alerts, Rules and Tags AP Key**. Access this in your navigation pane by clicking **Data Flow** > API Keys.

- [**Management API Endpoint**](https://coralogixstg.wpengine.com/docs/management-api-endpoints/)

## Fetch Detailed Data Usage Group by Application and Subsystem Name

```
grpcurl -H "Authorization: Bearer <YOUR-API-KEY>" -d @ <CORALOGIX-DOMAIN> com.coralogix.datausage.v2.DataUsageService/GetTeamDetailedDataUsage <<EOF
{
  "resolution": "6h",
  "date_range": {
    "from_date": "2023-03-20T01:30:15.01Z",
	  "to_date": "2023-03-21T01:30:15.01Z"
  }
}
EOF

```

### Request args

| Field | Description |
| --- | --- |
| Resolution | Describes the precision by which to group your data. In this example, the response contains the data usage per [Application and Subsystem](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) every 6h |
| Team Id | ID of the team you are seeking |
| Date Range | Date range of the requested data in ISO 8601 format |

### Response

The response will be a list of data, such as the following:

```
[{
  "timestamp": "2023-03-20T16:00:00Z",
  "sizeGb": 0.000011989847,
  "units": 0.000004795939,
  "dimensions": [
    {
      "tier": "TCO_TIER_HIGH"
    },
    {
      "genericDimension": {
        "key": "subsystem_name",
        "value": "vzmgr-server"
      }
    },
    {
      "pillar": "PILLAR_LOGS"
    },
    {
      "severity": "SEVERITY_CRITICAL"
    },
    {
      "genericDimension": {
        "key": "application_name",
        "value": "staging"
      }
    }
  ]
}]

```

| Field | Description | Field | Description |
| --- | --- | --- | --- |
| timestamp | Date of the sample |  |  |
| sizeGb | Size in GB of the processed data |  |  |
| units | Amount in units |  |  |
| dimension | List of dimensions by which data has been grouped | genericDimension | Generic label of the data. Example: application\_name and subsystem\_name. |
|  |  | tier | Data priority label: TCO\_TIER\_HIGH, TCO\_TIER\_MEDIUM, TCO\_TIER\_LOW, TCO\_TIER\_BLOCKED |
|  |  | pillar | Pillar information: PILLAR\_LOGS, PILLAR\_METRICS, PILLAR\_SPANS |
|  |  | severity | Severity just for PILLAR\_LOGS: SEVERITY\_UNSPECIFIED, SEVERITY\_DEBUG, SEVERITY\_VERBOSE, SEVERITY\_INFO, SEVERITY\_WARNING,SEVERITY\_ERROR,SEVERITY\_CRITICAL |

## Additional Resources

[Data Usage](https://coralogixstg.wpengine.com/docs/data-usage/)

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
