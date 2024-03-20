---
title: "Incident Management API"
date: "2024-02-05"
---

## **Overview**

This document outlines the Incident Management API. It includes various methods for managing incidents, such as retrieving incident details, listing incidents, aggregating incidents, assigning and unassigning incidents and acknowledging or resolving incidents. The `IncidentsService` is designed to handle all operations related to incident management within Coralogix.

### **Prerequisites**

Before you begin, please make sure you have the following:

- [API Key for Alerts, Rules & Tags to successfully authenticate.](https://coralogixstg.wpengine.com/docs/alerts-rules-tags-api-key/)

- [Management API Endpoint that corresponds with your Coralogix subscription.](https://coralogixstg.wpengine.com/docs/management-api-endpoints/)

- Administrator permissions to manage your services.

## **Authentication**

Coralogix API uses API keys to authenticate requests. You can view and [manage your API keys](https://coralogixstg.wpengine.com/docs/alerts-rules-tags-api-key/) from the Data Flow tab in Coralogix. You need to use this API key in the Authorization request header to successfully connect.

### Example:

`grpcurl -H "Authorization: Bearer API_KEY_HERE"`

Then, use one of our designated **[Management Endpoints](https://coralogixstg.wpengine.com/docs/management-api-endpoints/)** to structure your header.

`d @ ng-api-grpc.coralogixstg.wpengine.com:443`

For the Incidents Service API, the service name will be `IncidentsService`.

`com.coralogixapis.incidents.v1.IncidentsService/`

The complete request header should look like this:

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.incidents.v1.IncidentsService/

```

### **Sample Request**

Lists all available incidents based on specified filters and order. In this case, incidents are shown per `assignee`. The list is ordered in an unspecified direction and sorted by time created.

```
grpcurl -H "Authorization: Bearer APY_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.incidents.v1.IncidentsService/ListIncidents <<EOF 
{
    "filter": {
        "assignee": [
            {
                "value": "assignee@coralogixstg.wpengine.com"
            }
        ]},
    "order_bys": [
        {
            "direction": "ORDER_BY_DIRECTION_UNSPECIFIED",
            "incident_field": "INCIDENTS_FIELDS_CREATED_TIME"
        }
    ]
}
EOF

```

### **Sample Response**

```
{
    "incidents": [
        {
            "assignments": [
                {
                    "assigned_to": {
                        "user_id": {
                            "value": "assignee@coralogixstg.wpengine.com"
                        }
                    },
                    "assigned_by": {
                        "user_id": {
                            "value": "assignee@coralogixstg.wpengine.com"
                        }
                    }
                }
            ],
            "events": [],
            "contextual_labels": [
                {
                    "key": "alert_id",
                    "value": "e2e1e00f-552f-4dfc-9d24-ab9d21d4979c"
                },
                {
                    "key": "alert_name",
                    "value": "inalert"
                },
                {
                    "key": "alert_type",
                    "value": "Standard"
                },
                {
                    "key": "alert_severity",
                    "value": "Info"
                },
                {
                    "key": "alert_group_by_fields",
                    "value": "coralogix.metadata.applicationName , coralogix.metadata.subsystemName"
                },
                {
                    "key": "alert_notification_group_id",
                    "value": "ab8dee0e-063b-43c2-89a3-bdbb068ff851"
                },
                {
                    "key": "alert_notification_group_grouping_fields",
                    "value": "coralogix.metadata.applicationName , coralogix.metadata.subsystemName"
                },
                {
                    "key": "alert_notification_group_integrations_ids",
                    "value": ""
                }
            ],
            "display_labels": [
                {
                    "key": "coralogix.metadata.subsystemName",
                    "value": "coralogix-operator"
                },
                {
                    "key": "coralogix.metadata.applicationName",
                    "value": "staging"
                }
            ],
            "id": {
                "value": "cdfaf78b-27ee-401f-8d13-ebd2daa08232"
            },
            "name": null,
            "state": "INCIDENT_STATE_TRIGGERED",
            "status": "INCIDENT_STATUS_TRIGGERED",
            "description": null,
            "severity": "INCIDENT_SEVERITY_INFO",
            "created_at": {
                "seconds": "1703677320",
                "nanos": 0
            },
            "closed_at": null,
            "last_state_update_time": {
                "seconds": "1706088981",
                "nanos": 286000000
            },
            "last_state_update_key": {
                "value": "8cde7807-dedc-418b-b542-62d78fead629"
            },
            "is_muted": {
                "value": false
            }
        }
    ]
}

```

## API Endpoints

⚠️ This is only a list of endpoints. For detailed schema, please consult the whole [specification file in GitHub](https://github.com/coralogix/cx-api-incidents/blob/master/docs/incidents-api-reference.md).

### IncidentsService

The IncidentsService is designed to provide a set of functionalities and operations to facilitate the effective management, monitoring, and resolution of incidents. Here are some key methods within the IncidentsService:

| Method Name | Description |
| --- | --- |
| ListIncidents | Lists incidents based on filters. This method is used to retrieve a collection of incidents that match certain criteria. |
| ListIncidentAggregations | Lists incident aggregations. This method is used to retrieve aggregated information about incidents, grouped by specific parameters. |
| GetIncident | Retrieves detailed information about a specific incident. This method is used to get comprehensive details regarding a single incident. |
| GetIncidentEvents | Retrieves events associated with a particular incident. This method is used to obtain a chronological sequence of events related to an incident. |
| BatchGetIncident | Retrieves details for multiple incidents. This method is designed to efficiently retrieve information for a batch of incidents in a single request. |
| AssignIncidents | Assigns incidents to specific users or teams. This method is used for managing the assignment of incidents to responsible parties. |
| UnassignIncidents | Unassigns incidents from users or teams. This method is used to remove assignment associations for incidents. |
| AcknowledgeIncidents | Acknowledges incidents. |
| PaginationRequest | Retrieves pagination information for incidents. |
| CloseIncidents | Closes incidents. |
| DeleteIncidents | Deletes incidents. |
| ResolveIncidents | Resolves incidents. |

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email to [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
