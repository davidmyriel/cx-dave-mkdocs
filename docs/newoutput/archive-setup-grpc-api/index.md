---
title: "Archive Setup gRPC API"
date: "2023-05-15"
---

This tutorial will demonstrate how to use our Archive Setup API to:

- View bucket definitions and set a target bucket (Get target / Set target)

- Define archive retentions (Get, Update, Activate)

## Prerequisites

- Coralogix **Alerts, Rules & Tags API Key** (Access this in your navigation pane by clicking **Data Flow** > API Keys)

- **[Management API endpoint](https://coralogixstg.wpengine.com/docs/management-api-endpoints/)**

## Supported API Calls

The API supports the following gRPCs:

## Examples

rpc GetTarget(GetTargetRequest) returns (GetTargetResponse)

GetTarget**Request**

```
grpcurl -H "Authorization: Bearer <YOUR-API-KEY>" -d @ <CORALOGIX-DOMAIN> com.coralogix.archive.v1.TargetService/GetTarget <<EOF
{
}
EOF

```

GetTarget**Response**

```
{
  "target": {
    "archivingFormatId": "cx_data_v1",
    "isActive": true,
    "region": "eu-west-1",
    "bucket": "example-bucket",
    "enableTags": false
  }
}

```

| Field | Type | Description |
| --- | --- | --- |
| archivingFormatId | string | Format of the archive log files |
| isActive | boolean | Indicates whether archive is active |
| region | string | AWS region in which your s3 bucket is located |
| bucket | string | S3 bucket name |
| enableTags | boolean | Indicates whether retention tagging feature in enabled |

rpc SetTarget(SetTargetRequest) returns (SetTargetResponse)

SetTarget**Request**

```
grpcurl -H "Authorization: Bearer <YOUR-API-KEY>" -d @ <CORALOGIX-DOMAIN> com.coralogix.archive.v1.TargetService/SetTarget <<EOF
{
    "bucket": "example-bucket",
    "is_active": true
}
EOF

```

| Field | Type | Description |
| --- | --- | --- |
| bucket | string | S3 bucket name |
| is\_active | boolean | Activate / deactivate archive |

SetTarget**Response**

```
{
  "target": {
    "archivingFormatId": "cx_data_v1",
    "isActive": true,
    "region": "eu-west-1",
    "bucket": "example-bucket",
    "enableTags": false
  }
}

```

| Field | Type | Description |
| --- | --- | --- |
| archivingFormatId | string | Format of the archive log files |
| isActive | boolean | Indicates whether archive is active |
| region | string | AWS region in which your s3 bucket is located |
| bucket | string | S3 bucket name |
| enableTags | boolean | Indicates whether retention tagging feature in enabled |

rpc GetRetentionsEnabled(GetRetentionsEnabledRequest) returns (GetRetentionsEnabledResponse)

GetRetentionsEnabled**Request**

```
grpcurl -H "Authorization: Bearer <YOUR-API-KEY>" -d @ <CORALOGIX-DOMAIN> com.coralogix.archive.v1.RetentionsService/GetRetentionsEnabled <<EOF
{
}
EOF

```

GetRetentionsEnabled**Response**

```
{
  "enableTags": false
}

```

| Field | Type | Description |
| --- | --- | --- |
| enableTags | boolean | Indicates whether retention tagging feature in enabled |

rpc GetRetentions(GetRetentionsRequest) returns (GetRetentionsResponse)

```
grpcurl -H "Authorization: Bearer <YOUR-API-KEY>" -d @ <CORALOGIX-DOMAIN> com.coralogix.archive.v1.RetentionsService/GetRetentions <<EOF
{
}
EOF

```

GetRetentions**Request**

```
grpcurl -H "Authorization: Bearer <YOUR-API-KEY>" -d @ <CORALOGIX-DOMAIN> com.coralogix.archive.v1.RetentionsService/GetRetentions <<EOF
{
}
EOF

```

GetRetentions**Response**

```
{
  "retentions": [
    {
      "id": "c157476b-642d-400d-9acf-40f3ab02721a",
      "order": 1,
      "name": "Default",
      "editable": false
    },
    {
      "id": "485a2668-e815-4ec1-9b22-0228f5a27dc8",
      "order": 2,
      "name": "short",
      "editable": true
    },
    {
      "id": "3dacd60d-f83e-40f2-bf36-e7d2657a98d1",
      "order": 3,
      "name": "Intermediate",
      "editable": true
    },
    {
      "id": "ba29d452-966b-4ca7-8234-dc2d9bbe2d5e",
      "order": 4,
      "name": "Long",
      "editable": true
    }
  ]
}

```

| Field | Type | Description |
| --- | --- | --- |
| retentions | array | Array of retention elements |
| retentions\[\].id | string | Retention ID to be used in TCO policy API |
| retentions\[\].order | number | Has no significance. Currently only 4 retentions are supported |
| retentions\[\].name | string | Retention name, which can be changed (except 'default') |
| retentions\[\].editable | boolean | Whether retention is editable (All but 'default' are editable) |

rpc UpdateRetentions(UpdateRetentionsRequest) returns (UpdateRetentionsResponse)

UpdateRetentions**Request**

```
grpcurl -H "Authorization: Bearer <YOUR-API-KEY>" -d @ <CORALOGIX-DOMAIN> com.coralogix.archive.v1.RetentionsService/UpdateRetentions <<EOF
{
  "retention_update_elements": [
    {
      "id": "485a2668-e815-4ec1-9b22-0228f5a27dc8",
      "name": "1_week"
    },
    {
      "id": "e9ee9138-4533-463d-a93b-bb18692208a5",
      "name": "1_month"
    }
  ]
}
EOF

```

| Field | Type | Description |
| --- | --- | --- |
| retention\_update\_elements | Array | Array of retention elements |
| retention\_update\_elements\[\].id | string | ID of the retention to update |
| retention\_update\_elements\[\].name | string | Name of the retention to update |

UpdateRetentions**Response**

```
{
  "retentions": [
    {
      "id": "485a2668-e815-4ec1-9b22-0228f5a27dc8",
      "order": 2,
      "name": "1_week",
      "editable": true
    },
    {
      "id": "3dacd60d-f83e-40f2-bf36-e7d2657a98d1",
      "order": 3,
      "name": "1_month",
      "editable": true
    }
  ]
}

```

rpc ActivateRetentions(ActivateRetentionsRequest) returns (ActivateRetentionsResponse)

ActivateRetentions**Request**

```
grpcurl -H "Authorization: Bearer <YOUR-API-KEY>" -d @ <CORALOGIX-DOMAIN> com.coralogix.archive.v1.RetentionsService/ActivateRetentions <<EOF
{
}
EOF

```

ActivateRetentions**Response**

```
{
  "activateRetentions": true
}

```

| Field | Type | Description |
| --- | --- | --- |
| activateRetentions | boolean | True, if activated |

If missing tagging permissions on a bucket, response will be the following error:

```
ERROR:
  Code: FailedPrecondition
  Message: Failed to update archive retention policy. Please provide Coralogix with tagging permissions in your AWS dashboard and try again.

```

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/archive-retention-policy/">Archive Retention Policy</a><br><a href="https://coralogixstg.wpengine.com/docs/optimize-log-management-costs/">TCO Optimizer</a><br><a href="https://coralogixstg.wpengine.com/docs/tco-optimizer-api/">TCO Optimizer API</a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
