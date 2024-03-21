---
title: "Webhooks API"
date: "2022-08-22"
---

Leverage our **Webhooks API** to seamlessly define, query, and manage your **outbound webhooks** with ease.

## API Key

This API uses the **Alert, Rules, and Tags API key**. Retrieve or generate this API key by following [these instructions](https://coralogixstg.wpengine.com/docs/alerts-rules-tags-api-key/).

## API Reference

The Webhooks API follows an HTTP-style method.

### Base URL

| Region | URL |
| --- | --- |
| US1  
EU1  
EU2  
AP2 (SG) | https://api.**<cx\_domain>**/api/v1/external/integrations/ |
| AP1 (IN) | https://api.app.**<cx\_domain>**/api/v1/external/integrations/ |

**<cx\_domain>**: Input the Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/) associated with your region.

### Endpoint Details

| HTTP Method | GET / POST |
| --- | --- |
| Content-Type | application/json |
| Header | Authorization: bearer <api\_key> |

### `curl` Command Example

Use this `curl` command with a placeholder for the token.

For regions **US1, EU1, EU2, AP2 (SG)**:

```
curl -X GET -H 'Authorization: bearer <api_key>' -H 'Content-Type: application/json' [https://api.<cx_domain>/api/v1/external/integrations/](<https://api.eu2.coralogixstg.wpengine.com/api/v1/external/integrations/>)

```

For region **API (IN)**:

```
curl -X GET -H 'Authorization: bearer <api_key>' -H 'Content-Type: application/json' [https://api.app.<cx_domain>/api/v1/external/integrations/](<https://api.eu2.coralogixstg.wpengine.com/api/v1/external/integrations/>)

```

## GET Request: Get Webhooks

Get all webhooks or a single webhook using a Webhook ID.

### Get All Webhooks

**Request:** GET https://api.<cx\_domain>/api/v1/external/integrations/

**Result**:

```
[
    {
        "alias": "string",
        "company_id": number,
        "created_at": "string - ISO format",
        "id": number,
        "url": "string",
        "integration_type_fields": "escaped json" or null,
        "integration_type_id": number,
        "integrationTypeId": number,
        "integration_type": {
            "label": "string",
            "icon": "string",
            "id": number
        },
        "updated_at": "string - ISO format"
    }, // array of webhooks
]

# RLRL - Example of a schema in the result with comments
[
  {
    # The name of the webhook
    "alias": string,
    # The company id
    "company_id": number
    # The webhook creation time in ISO8601 Format (e.g. 2021-01-01T12:32:23)
    "created_at": string
    ...
  },
  ...
]

```

### Get a Specific Webhook

**Request**: GET https://api.<cx\_domaijn>/api/v1/external/integrations/<webhook-id>

**Result**:

```
{
        "alias": "string",
        "company_id": number,
        "created_at": "string - ISO format",
        "id": number,
        "url": "string",
        "integration_type_fields": "escaped json" or null,
        "integration_type_id": number,
        "integrationTypeId": number,
        "integration_type": {
            "label": "string",
            "icon": "string",
            "id": number
        },
        "updated_at": "string - ISO format"
    }

```

## POST Request: Create a New Webhook

Create or update a **single** webhook.

### **Body Parameters**

| Parameter | Description | Type | Notes |
| --- | --- | --- | --- |
| alias | webhook name | string |  |
| integration\_type | webhook type | object | described below, must be a complete block from values as shown in Table A |
| integration\_type.label | webhook type name | string |  |
| integration\_type.icon | webhook icon | string |  |
| integration\_type.id | webhook type id | number |  |
| integration\_type\_id | webhook type id | number | must be from values shown in Table A |
| integration\_type\_fields | webhook additional fields | string | escaped json, an array of objects in the form of name + value. must comply with the structure shown in Table B |
| url | webhook url | string |  |

**Table A: `integration_type` Object**

| Type | JSON Object |
| --- | --- |
| slack | {“id”: 0, “name”: “Slack”, “icon”: “/assets/settings/slack-48.png”} |
| webhook | {“id”: 1, “name”: “WebHook”, “icon”: “/assets/webhook.png”} |
| pager\_duty | {“id”: 2, “name”: “PagerDuty”, “icon”: “/assets/settings/pagerDuty.png”} |
| sendlog | {“id”: 3, “name”: “SendLog”, “icon”: “/assets/invite.png”} |
| email\_group | {“id”: 4, “name”: “Email Group”, “icon”: “/assets/email-group.png”} |
| microsoft\_teams | {“id”: 5, “name”: “Microsoft Teams”, “icon”: “/assets/settings/teams.png”} |
| jira | {“id”: 6, “name”: “Jira”, “icon”: “/assets/settings/jira.png”} |
| opsgenie | {“id”: 7, “name”: “Opsgenie”, “icon”: “/assets/settings/opsgenie.png”} |
| demisto | {“id”: 8, “name”: “Demisto”, “icon”: “/assets/settings/demisto.png”} |

**Table B: `Integration_type_fields` String**

| Type | Field | value\_type | Notes |
| --- | --- | --- | --- |
| pager\_duty | serviceKey | string |  |
| jira | apiToken | string |  |
| jira | email | string |  |
| jira | projectKey | string |  |
| email\_group | payload | array of strings |  |
| webhook,demisto,sendlog | uuid | string | in UUID format |
| webhook,demisto,sendlog | method | string | must be one of \[“get”,”post”,”put”\] |
| webhook,demisto,sendlog | headers | object | a json object of headers |
| webhook,demisto,sendlog | payload | object | a json object of the webhook body |
|  |  |  |  |

### Create a Slack Webhook

**Request**: POST https://api.<cx\_domain>/api/v1/external/integrations/

**Result**:

```
{
        "alias": "slack-webhook",
        "url": "<slack-webhook-url>",
        "integration_type_fields": "[]",
        "integration_type_id": 0,
        "integration_type": {
            "label": "Slack",
            "icon": "/assets/settings/slack-48.png",
            "id": 0
        }
    }

```

### Create a ‘sendlog’ Webhook

**Request**: POST https://api.<cx\_domain>/api/v1/external/integrations/

```
{
    "alias": "sendlog-webhook",
    "integration_type": {
        "icon": "/assets/invite.png",
        "id": 3,
        "name": "Send Log"
    },
    "integration_type_id": 3,
    "integration_type_fields": "[{\\"name\\":\\"uuid\\",\\"value\\":\\"<uuid>\\"},{\\"name\\":\\"method\\",\\"value\\":\\"post\\"},{\\"name\\":\\"headers\\",\\"value\\":{\\"Content-Type\\":\\"application/json\\"}},{\\"name\\":\\"payload\\",\\"value\\":{\\"privateKey\\":\\"<send-your-data-api-key>\\",\\"applicationName\\":\\"$APPLICATION_NAME\\",\\"subsystemName\\":\\"$SUBSYSTEM_NAME\\",\\"computerName\\":\\"$COMPUTER_NAME\\",\\"logEntries\\":[{\\"severity\\":3,\\"timestamp\\":\\"$EVENT_TIMESTAMP_MS\\",\\"text\\":{\\"integration_text\\":\\"<Insert your desired integration description>\\",\\"alert_severity\\":\\"$EVENT_SEVERITY\\",\\"alert_id\\":\\"$ALERT_ID\\",\\"alert_name\\":\\"$ALERT_NAME\\",\\"alert_url\\":\\"$ALERT_URL\\",\\"hit_count\\":\\"$HIT_COUNT\\"}}],\\"uuid\\":\\"<same-uuid>\\"}}]",
    "url": "<https://api.coralogix.us/api/v1/logs>"
}

```

Inside `integration_type_fields` modify the following:

- <uuid> and <same-uuid> – with a newly generated uuid

- <send-your-data-api-key> – with your [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)

- <Insert your desired integration description>

**Result:**

```
{
    "id": 1051,
    "alias": "sendlog-webhook",
    "url": "<https://api.coralogix.us/api/v1/logs>",
    "integration_type_fields": "[{\\"name\\":\\"uuid\\",\\"value\\":\\"17a3b9e3-b0bc-4bc0-9e06-98d2a4c54ecb\\"},{\\"name\\":\\"method\\",\\"value\\":\\"post\\"},{\\"name\\":\\"headers\\",\\"value\\":{\\"Content-Type\\":\\"application/json\\"}},{\\"name\\":\\"payload\\",\\"value\\":{\\"privateKey\\":\\"5ef4a0d1-7e1f-47b2-ac0a-1282002aa2a1\\",\\"applicationName\\":\\"$APPLICATION_NAME\\",\\"subsystemName\\":\\"$SUBSYSTEM_NAME\\",\\"computerName\\":\\"$COMPUTER_NAME\\",\\"logEntries\\":[{\\"severity\\":3,\\"timestamp\\":\\"$EVENT_TIMESTAMP_MS\\",\\"text\\":{\\"integration_text\\":\\"Insert your desired integration description\\",\\"alert_severity\\":\\"$EVENT_SEVERITY\\",\\"alert_id\\":\\"$ALERT_ID\\",\\"alert_name\\":\\"$ALERT_NAME\\",\\"alert_url\\":\\"$ALERT_URL\\",\\"hit_count\\":\\"$HIT_COUNT\\"}}],\\"uuid\\":\\"17a3b9e3-b0bc-4bc0-9e06-98d2a4c54ecb\\"}}]",
    "integration_type_id": 3,
    "integrationTypeId": 3,
    "company_id": 12345,
    "updated_at": "2022-08-22T07:32:09.348Z",
    "created_at": "2022-08-22T07:32:09.348Z",
    "companyId": 12345
}

```

## POST Request: Create New bulk Webhooks

Create webhooks in **bulk**.

To create several webhooks in one request, please send an **array of objects**, where each object is a different webhook.

### Create 2 Webhooks in the Same Request

**Request**: POST https://api.coralogixstg.wpengine.com/api/v1/external/integrations-bulk

```
[
       {
        "alias": "slack-webhook",
        "url": "<slack-webhook-url>",
        "integration_type_fields": "[]",
        "integration_type_id": 0,
        "integration_type": {
            "label": "Slack",
            "icon": "/assets/settings/slack-48.png",
            "id": 0
        }
    },
 {
    "alias": "sendlog-webhook",
    "integration_type": {
        "icon": "/assets/invite.png",
        "id": 3,
        "name": "Send Log"
    },
    "integration_type_id": 3,
    "integration_type_fields": "[{\\"name\\":\\"uuid\\",\\"value\\":\\"<uuid>\\"},{\\"name\\":\\"method\\",\\"value\\":\\"post\\"},{\\"name\\":\\"headers\\",\\"value\\":{\\"Content-Type\\":\\"application/json\\"}},{\\"name\\":\\"payload\\",\\"value\\":{\\"privateKey\\":\\"<send-your-data-api-key>\\",\\"applicationName\\":\\"$APPLICATION_NAME\\",\\"subsystemName\\":\\"$SUBSYSTEM_NAME\\",\\"computerName\\":\\"$COMPUTER_NAME\\",\\"logEntries\\":[{\\"severity\\":3,\\"timestamp\\":\\"$EVENT_TIMESTAMP_MS\\",\\"text\\":{\\"integration_text\\":\\"<Insert your desired integration description>\\",\\"alert_severity\\":\\"$EVENT_SEVERITY\\",\\"alert_id\\":\\"$ALERT_ID\\",\\"alert_name\\":\\"$ALERT_NAME\\",\\"alert_url\\":\\"$ALERT_URL\\",\\"hit_count\\":\\"$HIT_COUNT\\"}}],\\"uuid\\":\\"<same-uuid>\\"}}]",
    "url": "<https://api.coralogix.us/api/v1/logs>"
 }
]

```

**Result:**

```
[
 {
    "id": 1050,
    "alias": "slack-webhook",
    "url": "<slack-webhook-url>",
    "integration_type_fields": "[]",
    "integration_type_id": 0,
    "integrationTypeId": 0,
    "company_id": 12345,
    "updated_at": "2022-08-22T07:32:09.348Z",
    "created_at": "2022-08-22T07:32:09.348Z",
    "companyId": 12345
},
{
    "id": 1051,
    "alias": "sendlog-webhook",
    "url": "<https://api.coralogix.us/api/v1/logs>",
    "integration_type_fields": "[{\\"name\\":\\"uuid\\",\\"value\\":\\"17a3b9e3-b0bc-4bc0-9e06-98d2a4c54ecb\\"},{\\"name\\":\\"method\\",\\"value\\":\\"post\\"},{\\"name\\":\\"headers\\",\\"value\\":{\\"Content-Type\\":\\"application/json\\"}},{\\"name\\":\\"payload\\",\\"value\\":{\\"privateKey\\":\\"5ef4a0d1-7e1f-47b2-ac0a-1282002aa2a1\\",\\"applicationName\\":\\"$APPLICATION_NAME\\",\\"subsystemName\\":\\"$SUBSYSTEM_NAME\\",\\"computerName\\":\\"$COMPUTER_NAME\\",\\"logEntries\\":[{\\"severity\\":3,\\"timestamp\\":\\"$EVENT_TIMESTAMP_MS\\",\\"text\\":{\\"integration_text\\":\\"Insert your desired integration description\\",\\"alert_severity\\":\\"$EVENT_SEVERITY\\",\\"alert_id\\":\\"$ALERT_ID\\",\\"alert_name\\":\\"$ALERT_NAME\\",\\"alert_url\\":\\"$ALERT_URL\\",\\"hit_count\\":\\"$HIT_COUNT\\"}}],\\"uuid\\":\\"17a3b9e3-b0bc-4bc0-9e06-98d2a4c54ecb\\"}}]",
    "integration_type_id": 3,
    "integrationTypeId": 3,
    "company_id": 12345,
    "updated_at": "2022-08-22T07:32:09.348Z",
    "created_at": "2022-08-22T07:32:09.348Z",
    "companyId": 12345
 }
]

```

## POST Request: Update a Single Webhook or Bulk Webhooks

To update an existing webhook, send a POST request with all of the usual values. Add an ID field with the webhook ID as the value.

To update a group of webhooks, make sure your POST request consists of an array of objects and that your URL ends with `/integrations-bulk`.

## Copying Webhooks Between Accounts

To copy webhooks from one account to another:

- Create a GET request with the API key of the origin account and GET all webhooks.

- Copy the full response and remove the ID of the webhook. Do **not** remove the ID under `integration_type`.

- Copy the edited array to a new POST request:
    - Make sure to modify the API key to the API key of the destination account.
    
    - Make sure your URL ends with `/integrations-bulk`.

## DELETE Request – Delete a Webhook

### **Delete a Webhook**

**Request**: DELETE https://api.<cx\_domain>/api/v1/external/integrations/<webhook-id>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
