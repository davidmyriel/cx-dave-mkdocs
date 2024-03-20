---
title: "SLO Management API"
date: "2024-01-23"
---

## Overview

We are introducing the SLO API to enable teams, particularly those not utilizing a UI, to efficiently manage their SLOs programmatically. This API will let you retrieve, read, create, update and delete your SLOs.

### Prerequisites

- [API Key for Alerts, Rules & Tags to successfully authenticate.](https://coralogixstg.wpengine.com/docs/alerts-rules-tags-api-key/)

- [Management API Endpoint that corresponds with your Coralogix subscription.](https://coralogixstg.wpengine.com/docs/management-api-endpoints/)

- Administrator permissions to manage your SLOs.

## API Endpoints

This reference document lists example requests and responses using [gRPCurl](https://github.com/fullstorydev/grpcurl). The following calls accept arguments as JSON in the request body and return results as JSON in the response body. A complete [list of Management Endpoints](https://coralogixstg.wpengine.com/docs/management-api-endpoints/) is available here.

### Authentication

Coralogix API uses API keys to authenticate requests. You can view and [manage your API keys](https://coralogixstg.wpengine.com/docs/alerts-rules-tags-api-key/) from the Data Flow tab in Coralogix. You need to use an API key in the Authorization request header to successfully connect.

Example:

```
grpcurl -H "Authorization: Bearer API_KEY_HERE"

```

Then, use one of our designated **[Management Endpoints](https://coralogixstg.wpengine.com/docs/management-api-endpoints/)** to structure your header.

```
-d @ ng-api-grpc.coralogixstg.wpengine.com:443

```

For SLO API, the service name will be `ServiceSloService`.

```
com.coralogixapis.apm.services.v1.ServiceSloService/

```

The complete request header should look like this:

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ServiceSloService/

```

### ListServiceSlos

Lists all available SLOs from the entire service catalog. In this example, there are two available SLOs, `List_bigger_than_10ms` from `productcatalogservice` and `Latency_bigger_then_1ms` from the `frontend` service. Here you will find the `id` field associated with each SLO.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ServiceSloService/ListServiceSlos <<EOF
{
}
EOF

```

RESPONSE

```
{
  "slos": [
    {
      "id": "049b684b-4ef0-449e-8fad-33e5f90775b4",
      "name": "List_bigger_than_10ms",
      "serviceName": "productcatalogservice",
      "description": "Check if the operation of list products is bigger than 10ms for 50% of the spans",
      "targetPercentage": 50,
      "createdAt": "1970-01-20T11:38:56.476Z",
      "remainingErrorBudgetPercentage": 100,
      "latencySli": {
        "thresholdMicroseconds": "10000",
        "thresholdSymbol": "THRESHOLD_SYMBOL_GREATER_OR_EQUAL"
      },
      "filters": [
        {
          "field": "operationname",
          "compareType": "COMPARE_TYPE_IS",
          "fieldValues": [
            "hipstershop.ProductCatalogService/ListProducts"
          ]
        }
      ],
      "period": "SLO_PERIOD_7_DAYS"
    },
    {
      "id": "089d1831-7c40-4a31-bab8-0f64116c18b2",
      "name": "Latency_bigger_then_1ms",
      "serviceName": "frontend",
      "description": "Check if latency is bigger then 1ms ",
      "targetPercentage": 1,
      "createdAt": "1970-01-20T11:40:32.323Z",
      "remainingErrorBudgetPercentage": 100,
      "latencySli": {
        "thresholdMicroseconds": "1000",
        "thresholdSymbol": "THRESHOLD_SYMBOL_GREATER_OR_EQUAL"
      },
      "period": "SLO_PERIOD_7_DAYS"
    }
  ]
}

```

### GetServiceSlo

Retrieves specific SLO information for a given service. In this example, you are retrieving all SLOs for the `frontend`service. You need to provide the SLO `id`.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ServiceSloService/GetServiceSlo <<EOF
{
    "id": "049b684b-4ef0-449e-8fad-33e5f90775b4"
}
EOF

```

RESPONSE

```
{
  "slo": {
    "id": "049b684b-4ef0-449e-8fad-33e5f90775b4",
    "name": "List_bigger_than_10ms",
    "serviceName": "productcatalogservice",
    "description": "Check if the operation of list products is bigger than 10ms for 50% of the spans",
    "targetPercentage": 50,
    "createdAt": "1970-01-20T11:38:56.476Z",
    "remainingErrorBudgetPercentage": 100,
    "latencySli": {
      "thresholdMicroseconds": "10000",
      "thresholdSymbol": "THRESHOLD_SYMBOL_GREATER_OR_EQUAL"
    },
    "filters": [
      {
        "field": "operationname",
        "compareType": "COMPARE_TYPE_IS",
        "fieldValues": [
          "hipstershop.ProductCatalogService/ListProducts"
        ]
      }
    ],
    "period": "SLO_PERIOD_7_DAYS"
  }
}

```

### BatchGetServiceSlos

Retrieves specified SLOs from the list. You need to provide the `id` for each required SLO. In this example, we are retrieving `List_bigger_than_10ms` and `Latency_bigger_than_1ms`.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ServiceSloService/BatchGetServiceSlos <<EOF
{
  "ids": ["049b684b-4ef0-449e-8fad-33e5f90775b4", "089d1831-7c40-4a31-bab8-0f64116c18b2"]
}
EOF

```

RESPONSE

```
{
  "slos": {
    "049b684b-4ef0-449e-8fad-33e5f90775b4": {
      "id": "049b684b-4ef0-449e-8fad-33e5f90775b4",
      "name": "List_bigger_than_10ms",
      "serviceName": "productcatalogservice",
      "description": "Check if the operation of list products is bigger than 10ms for 50% of the spans",
      "targetPercentage": 50,
      "createdAt": "1970-01-20T11:38:56.476Z",
      "remainingErrorBudgetPercentage": 100,
      "latencySli": {
        "thresholdMicroseconds": "10000",
        "thresholdSymbol": "THRESHOLD_SYMBOL_GREATER_OR_EQUAL"
      },
      "filters": [
        {
          "field": "operationname",
          "compareType": "COMPARE_TYPE_IS",
          "fieldValues": [
            "hipstershop.ProductCatalogService/ListProducts"
          ]
        }
      ],
      "period": "SLO_PERIOD_7_DAYS"
    },
    "089d1831-7c40-4a31-bab8-0f64116c18b2": {
      "id": "089d1831-7c40-4a31-bab8-0f64116c18b2",
      "name": "Latency_bigger_then_1ms",
      "serviceName": "frontend",
      "description": "Check if latency is bigger then 1ms ",
      "targetPercentage": 1,
      "createdAt": "1970-01-20T11:40:32.323Z",
      "remainingErrorBudgetPercentage": 100,
      "latencySli": {
        "thresholdMicroseconds": "1000",
        "thresholdSymbol": "THRESHOLD_SYMBOL_GREATER_OR_EQUAL"
      },
      "period": "SLO_PERIOD_7_DAYS"
    }
  }
}

```

### CreateServiceSlo

Creates a new SLO with given configuration for a chosen service. In this example, you are creating the `frontendtest`SLO for a `frontend` service.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ServiceSloService/CreateServiceSlo <<EOF
{
  "slo": {
    "name": "frontendtest",
    "serviceName": "frontend",
    "description": "Sample description for your SLO",
    "targetPercentage": 95,
    "errorSli": {
    },
    "period": "SLO_PERIOD_7_DAYS"
  }
}
EOF

```

RESPONSE

```
{
  "slo": {
    "id": "0416396c-a1ad-46d8-a10c-a05ea15dc3af",
    "name": "frontendtest",
    "serviceName": "frontend",
    "description": "Sample description for your SLO",
    "targetPercentage": 95,
    "createdAt": "1970-01-20T17:43:33.485Z",
    "remainingErrorBudgetPercentage": 100,
    "errorSli": {},
    "period": "SLO_PERIOD_7_DAYS"
  }
}

```

### ReplaceServiceSlo

Updates an SLO with new field entries for a chosen service. In this example, you are replacing the `description` content for a frontend service SLO. The SLO is supposed to check for errors in the frontend.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ServiceSloService/ReplaceServiceSlo <<EOF
{
  "slo": {
    "id": "0416396c-a1ad-46d8-a10c-a05ea15dc3af",
    "name": "frontendtest",
    "serviceName": "frontend",
    "description": "Checks for frontend errors.",
    "targetPercentage": 95,
    "errorSli": {},
    "period": "SLO_PERIOD_7_DAYS"
  }
}
EOF

```

RESPONSE

```
"slo": {
    "id": "0416396c-a1ad-46d8-a10c-a05ea15dc3af",
    "name": "frontendtest",
    "serviceName": "frontend",
    "description": "Checks for frontend errors.",
    "targetPercentage": 95,
    "createdAt": "1970-01-20T17:43:33.485Z",
    "remainingErrorBudgetPercentage": 100,
    "errorSli": {},
    "period": "SLO_PERIOD_7_DAYS"
  }
}

```

### DeleteServiceSlo

Deletes a specified SLO for a given service. In this example, you are deleting the `frontendtest` SLO from the `frontend`service.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ServiceSloService/DeleteServiceSlo <<EOF
{
    "id": "0416396c-a1ad-46d8-a10c-a05ea15dc3af"
}
EOF

```

RESPONSE

```
{}

```

## Body Parameters

<table><tbody><tr><td>Field</td><td>Type</td><td>Definition</td></tr><tr><td>id</td><td>int32</td><td>Unique identifier for the Service Level Objective (SLO).</td></tr><tr><td>ids</td><td>array</td><td>List of service SLO IDs to retrieve.</td></tr><tr><td>slos</td><td>array</td><td>List of Service Level Objectives (SLOs) with their respective<br>details.</td></tr><tr><td>name</td><td>string</td><td>Name of the SLO.</td></tr><tr><td>serviceName</td><td>string</td><td>Service name associated with the SLO.</td></tr><tr><td>description</td><td>string</td><td>Description of the SLO.</td></tr><tr><td>targetPercentage</td><td>integer</td><td>Target percentage for the SLO.</td></tr><tr><td>createdAt</td><td>timestamp</td><td>Timestamp indicating when the SLO was created.</td></tr><tr><td>remainingErrorBudgetPercentage</td><td>integer</td><td>Remaining error budget percentage.</td></tr><tr><td>errorSli</td><td>boolean</td><td>Indicates the presence of an error SLI.</td></tr><tr><td>latencySli</td><td>array</td><td>Defines SLI threshold and relationship.</td></tr><tr><td>latencySli.thresholdMicroseconds</td><td>string</td><td>Threshold in microseconds for latency.</td></tr><tr><td>latencySli.thresholdSymbol</td><td>enum</td><td>Symbol indicating the threshold relationship (e.g.,<br>GREATER_OR_EQUAL).</td></tr><tr><td>filters.field</td><td>string</td><td>Field for the filter.</td></tr><tr><td>filters.compareType</td><td>enum</td><td>Type of comparison (e.g., IS, START_WITH).</td></tr><tr><td>filters.fieldValues</td><td>string</td><td>List of field values for the filter.</td></tr><tr><td>period</td><td>enum</td><td>Period for the SLO (e.g., 7 days, 14 days).</td></tr></tbody></table>

**CompareType**

<table><tbody><tr><td>Name</td><td>Number</td><td>Description</td></tr><tr><td>COMPARE_TYPE_UNSPECIFIED</td><td>0</td><td>Filter entry is unspecified</td></tr><tr><td>COMPARE_TYPE_IS</td><td>1</td><td>Filters for a specific entry.</td></tr><tr><td>COMPARE_TYPE_START_WITH</td><td>2</td><td>Filters for results that start with the entry.</td></tr><tr><td>COMPARE_TYPE_ENDS_WITH</td><td>3</td><td>Filters for results that end with the entry.</td></tr><tr><td>COMPARE_TYPE_INCLUDES</td><td>4</td><td>Filters for a result that includes the entry.</td></tr></tbody></table>

ThresholdSymbol

<table><tbody><tr><td>Name</td><td>Number</td><td>Description</td></tr><tr><td>THRESHOLD_SYMBOL_UNSPECIFIED</td><td>0</td><td>Threshold criterion is not given.</td></tr><tr><td>THRESHOLD_SYMBOL_GREATER</td><td>1</td><td>Threshold criterion is greater than the specified number.</td></tr><tr><td>THRESHOLD_SYMBOL_GREATER_OR_EQUAL</td><td>2</td><td>Threshold criterion is greater than or equal to the specified number.</td></tr><tr><td>THRESHOLD_SYMBOL_LESS</td><td>3</td><td>Threshold criterion is less than the specified number.</td></tr><tr><td>THRESHOLD_SYMBOL_LESS_OR_EQUAL</td><td>4</td><td>Threshold criterion is less than or equal to the specified number.</td></tr><tr><td>THRESHOLD_SYMBOL_EQUAL</td><td>5</td><td>Threshold criterion is equal to the specified number.</td></tr><tr><td>THRESHOLD_SYMBOL_NOT_EQUAL</td><td>6</td><td>Threshold criterion is not equal to the specified number.</td></tr></tbody></table>

SloPeriod

<table><tbody><tr><td>Name</td><td>Number</td><td>Description</td></tr><tr><td>SLO_PERIOD_UNSPECIFIED</td><td>0</td><td>Time period for which the SLO measures results is unspecified.</td></tr><tr><td>SLO_PERIOD_7_DAYS</td><td>1</td><td>Time period for which the SLO measures results is 7 days.</td></tr><tr><td>SLO_PERIOD_14_DAYS</td><td>2</td><td>Time period for which the SLO measures results is 14 days.</td></tr><tr><td>SLO_PERIOD_30_DAYS</td><td>3</td><td>Time period for which the SLO measures results is 30 days.</td></tr></tbody></table>
