---
title: "TCO Optimizer HTTP API"
date: "2021-08-26"
---

This tutorial demonstrates using our TCO Optimizer HTTP API to define, query, and manage your TCO policy overrides, which are used exclusively for logs.

Visit [this page](https://coralogixstg.wpengine.com/docs/tco-tracing-policy-grpc-api/) to learn how to use our **TCO Tracing gPRC API** to define, query, and manage your [TCO policy criteria](https://coralogixstg.wpengine.com/docs/optimize-log-management-costs/#policy-criteria), used both for spans and logs.

## Base URL

Select the base API endpoint associated with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

| Domain | Base API endpoint |
| --- | --- |
| coralogix.us (Ohio) | https://api.coralogix.us/api/v1/external/tco/ |
| cx498.coralogixstg.wpengine.com (Oregon) | https://api.app.cx498.coralogixstg.wpengine.com/api/v1/external/tco/ |
| coralogixstg.wpengine.com (Ireland) | https://api.coralogixstg.wpengine.com/api/v1/external/tco/ |
| eu2.coralogixstg.wpengine.com (Stockholm) | https://api.app.eu2.coralogixstg.wpengine.com/api/v1/external/tco/ |
| coralogix.in (Mumbai) | https://api.app.coralogix.in/api/v1/external/tco/ |
| coralogixsg.com (Singapore) | https://api.app.coralogixsg.com/api/v1/external/tco/ |

## Header

| Key | Value |
| --- | --- |
| Content-Type | application/json |
| Authorization | Bearer <API\_KEY> |

### API\_KEY

The TCO Optimizer API uses your **Alerts, Rules, and Tags API Key** to authenticate requests. To access this API key in your Coralogix navigation pane, click **Data Flow** **\>** **API Keys >** **Alerts, Rules, and Tags API Key.**

## Usage

### **Severity Options**

| Name | Value |
| --- | --- |
| debug | 1 |
| verbose | 2 |
| info | 3 |
| warning | 4 |
| error | 5 |
| critical | 6 |

### **Priority Options**

| Name | Value |
| --- | --- |
| block | block |
| low | low |
| medium | medium |
| high | high |

## Supported Endpoints

### Get all policy overrides

**GET** /overrides

**Route example:** [https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides](https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides)

**Response example:**

```
[
    {
        "id": "dd361b69-89c7-11ec-a5ad-0616c20b31c7",
        "name": "default|recommendationservice|INFO",
        "priority": "high",
        "severity": 3,
        "applicationName": "default",
        "subsystemName": "recommendationservice"
    },
    {
        "id": "61d551af-8f96-11ec-8bfb-02dd69f0920d",
        "name": "default|checkoutservice|DEBUG",
        "priority": "high",
        "severity": 1,
        "applicationName": "default",
        "subsystemName": "checkoutservice"
    }
]
```

### Get single policy override by ID

**GET** /overrides/{id}

**Route example:** [https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/\*972f6b98-343c-11ee-ac29-061115d0c307\*](https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/*972f6b98-343c-11ee-ac29-061115d0c307*)

**Response example:**

```
{
    "id": "dd361b69-89c7-11ec-a5ad-0616c20b31c7",
    "name": "default|recommendationservice|INFO",
    "priority": "high",
    "severity": 3,
    "applicationName": "default",
    "subsystemName": "recommendationservice"
}
```

### Create single policy override

**POST** /overrides

**Route example:** [https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides](https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides)

**Request example:**

```
{
    "priority": "high",
    "severity": 3,
    "applicationName": "default",
    "subsystemName": "blablabla123"
}
```

**Response example**:

```
{
    "priority": "high",
    "severity": 3,
    "applicationName": "default",
    "subsystemName": "blablabla123",
    "id": "972f6b98-343c-11ee-ac29-061115d0c307"
}
```

### Create multiple policy overrides

**POST** /overrides/bulk

**Route example:** [https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/bulk](https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/bulk)

**Request example:**

```
[
    {
        "priority": "high",
        "severity": 3,
        "applicationName": "default",
        "subsystemName": "blablabla1234"
    },
    {
        "priority": "high",
        "severity": 3,
        "applicationName": "default",
        "subsystemName": "blablabla12345"
    }
]
```

**Response example**:

```
[
    {
        "status": 200,
        "override": {
            "priority": "high",
            "severity": 3,
            "applicationName": "default",
            "subsystemName": "blablabla1234",
            "id": "2c42a7aa-343d-11ee-ac29-061115d0c307"
        }
    },
    {
        "status": 200,
        "override": {
            "priority": "high",
            "severity": 3,
            "applicationName": "default",
            "subsystemName": "blablabla12345",
            "id": "2c53d05f-343d-11ee-ac29-061115d0c307"
        }
    }
]
```

### Update multiple policy overrides

**PUT** /overrides/bulk

**Route example:** [https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/bulk](https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/bulk)

**Request example:**

```
[
    {
        "id": "2c42a7aa-343d-11ee-ac29-061115d0c307",
        "name": "default|blablabla1234|INFO",
        "priority": "high",
        "severity": 3,
        "applicationName": "default",
        "subsystemName": "blablabla1234"
    },
    {
        "id": "2c53d05f-343d-11ee-ac29-061115d0c307",
        "name": "default|blablabla12345|INFO",
        "priority": "high",
        "severity": 3,
        "applicationName": "default",
        "subsystemName": "blablabla12345"
    }
]
```

**Response example**:

```
[
    {
        "status": 200,
        "override": {
            "name": "default|blablabla1234|INFO",
            "priority": "high",
            "severity": 3,
            "applicationName": "default",
            "subsystemName": "blablabla1234",
            "id": "2c42a7aa-343d-11ee-ac29-061115d0c307"
        }
    },
    {
        "status": 200,
        "override": {
            "name": "default|blablabla12345|INFO",
            "priority": "high",
            "severity": 3,
            "applicationName": "default",
            "subsystemName": "blablabla12345",
            "id": "2c53d05f-343d-11ee-ac29-061115d0c307"
        }
    }
]
```

### Delete single policy override

**DELETE** /overrides/{id}

**Route example:** [https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/2c53d05f-343d-11ee-ac29-061115d0c307](https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/2c53d05f-343d-11ee-ac29-061115d0c307)

**Response example:**

```
{
    "id": "2c53d05f-343d-11ee-ac29-061115d0c307"
}
```

### Delete multiple policy overrides

**DELETE** /overrides/bulk

**Route example:** [https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/bulk](https://api.coralogixstg.wpengine.com/api/v1/external/tco/overrides/bulk)

**Request example:**

```
[
    {
        "id": "2c42a7aa-343d-11ee-ac29-061115d0c307"
    }
]
```

**Response example**:

```
[
    {
        "status": 200,
        "override": {
            "id": "2c42a7aa-343d-11ee-ac29-061115d0c307"
        }
    }
]
```

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/optimize-log-management-costs/"><strong>TCO Optimizer</strong></a><br><strong><a href="https://coralogixstg.wpengine.com/docs/tco-tracing-policy-grpc-api/">TCO Tracing gPRC API</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
