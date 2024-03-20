---
title: "Service Removal gRPC API"
date: "2024-01-23"
---

## Overview

We are introducing the APM Service Removal API for customers who want to regularly maintain their APM Service Catalog. Even if services no longer exist on your side, the catalog in Coralogix lists all previously imported services indefinitely. With the Service Removal API, you can manually remove one or more unused services from your Coralogix subscription.

Here is what you can expect after a service is deleted:

- The service is removed from the service Catalog list.

- The service is removed from the dropdown in the New SLO modal.

- Dimensions based on the service’s span tags/process tags are removed from the ‘Dimensions’ dropdown.

- The service is removed from the service map, and any connections on the map are adjusted accordingly.

- The Dependency map is updated. If other services had dependencies on the deleted service, these connections are also adjusted in the UI.

### Prerequisites

- [API Key for Alerts, Rules & Tags to successfully authenticate.](https://coralogixstg.wpengine.com/docs/alerts-rules-tags-api-key/)

- [Management API Endpoint that corresponds with your Coralogix subscription.](https://coralogixstg.wpengine.com/docs/management-api-endpoints/)

- Administrator permissions to manage your services.

## API Endpoints

This reference document lists example requests and responses using [gRPCurl](https://github.com/fullstorydev/grpcurl). The following calls accept arguments as JSON in the request body and return results as JSON in the response body. A complete [list of Management Endpoints](https://coralogixstg.wpengine.com/docs/management-api-endpoints/) is available here.

### Authentication

Coralogix API uses API keys to authenticate requests. You can view and [manage your API keys](https://coralogixstg.wpengine.com/docs/alerts-rules-tags-api-key/) from the Data Flow tab in Coralogix. You need to use this API key in the Authorization request header to successfully connect.

Example:

```
grpcurl -H "Authorization: Bearer API_KEY_HERE"

```

Then, use one of our designated **[Management Endpoints](https://coralogixstg.wpengine.com/docs/management-api-endpoints/)** to structure your header.

```
-d @ ng-api-grpc.coralogixstg.wpengine.com:443

```

For the Service Removal API, the service name will be `ApmServiceService`.

```
com.coralogixapis.apm.services.v1.ApmServiceService/

```

The complete request header should look like this:

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ApmServiceService/

```

### ListApmServices

Lists all the services available in the APM Service Catalog. In this example, you are listing information for the `browser-api-service` , `browser-ingress`, and `cdn-ingress` services. Here you will find the `id` field associated with each service.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ApmServiceService/ListApmServices <<EOF
{
}
EOF

```

RESPONSE

```
{
  "services": [
    {
      "id": "5ae06ace-6643-4ad9-80e5-e2de3875cc99",
      "name": "browser-api-service",
      "type": "web",
      "workloads": [
        "browser-api-service"
      ],
      "sloStatusCount": {},
      "technology": ""
    },
    {
      "id": "2937b289-9e8e-422f-8958-676d75e50f7a",
      "name": "browser-ingress",
      "type": "web",
      "workloads": [
        "browser-ingress"
      ],
      "sloStatusCount": {},
      "technology": "rust"
    },
    {
      "id": "c2192929-683d-405a-866e-e4e922137d36",
      "name": "cdn-ingress",
      "type": "web",
      "workloads": [
        "cdn-ingress"
      ],
      "sloStatusCount": {},
      "technology": "rust"
    }
  ]
}

```

### GetApmService

Retrieves specific information for a given service from the catalog. In this example, you are retrieving information pertaining the `ws-tracing` service from the catalog list. You need to provide the service `id`.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ApmServiceService/GetApmService <<EOF
{
    "id": "46f9cc9c-cf83-4575-adb7-3c62345bb499"
}
EOF

```

RESPONSE

```
{
  "service": {
    "id": "46f9cc9c-cf83-4575-adb7-3c62345bb499",
    "name": "ws-tracing",
    "type": "web",
    "workloads": [
      "ws-tracing",
      "ws-tracing-graph-test"
    ],
    "sloStatusCount": {
      "ok": "0",
      "breach": "0",
      "notAvailable": "1"
    },
    "technology": "mysql"
  }
}

```

### BatchGetApmServices

Batch retrieves multiple services from the catalog. In this example, you are retrieving information on three specified services from the catalog.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ApmServiceService/BatchGetApmServices <<EOF
{
  "ids": [
    "0e8e7662-d47a-441f-8f0c-f4d1ac952b48",
    "ce970d0a-85d9-4fcc-9872-c5ae8033c0a4",
    "46f9cc9c-cf83-4575-adb7-3c62345bb499"
  ]
}
EOF

```

RESPONSE

```
{
  "services": {
    "0e8e7662-d47a-441f-8f0c-f4d1ac952b48": {
      "id": "0e8e7662-d47a-441f-8f0c-f4d1ac952b48",
      "name": "ws-service-catalog",
      "type": "web",
      "workloads": [
        "ws-service-catalog"
      ],
      "sloStatusCount": {},
      "technology": "mysql"
    },
    "46f9cc9c-cf83-4575-adb7-3c62345bb499": {
      "id": "46f9cc9c-cf83-4575-adb7-3c62345bb499",
      "name": "ws-tracing",
      "type": "web",
      "workloads": [
        "ws-tracing",
        "ws-tracing-graph-test"
      ],
      "sloStatusCount": {
        "ok": "0",
        "breach": "0",
        "notAvailable": "1"
      },
      "technology": "mysql"
    },
    "ce970d0a-85d9-4fcc-9872-c5ae8033c0a4": {
      "id": "ce970d0a-85d9-4fcc-9872-c5ae8033c0a4",
      "name": "ws-statistics",
      "type": "web",
      "workloads": [
        "ws-statistics"
      ],
      "sloStatusCount": {},
      "technology": "mysql"
    }
  }
}

```

### DeleteApmService

Deletes the specified service from the catalog. In this example, you are deleting the `ws-tracing` service from your subscription.

REQUEST

```
grpcurl -H "Authorization: Bearer API_KEY_HERE" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 com.coralogixapis.apm.services.v1.ApmServiceService/DeleteApmService <<EOF
{
    "id": "46f9cc9c-cf83-4575-adb7-3c62345bb499"
}
EOF

```

RESPONSE

```
{}

```

## Body Parameters

| Field | Type | Description |
| --- | --- | --- |
| id | int32 | The unique identifier for the APM service. |
| name | string | The name of the specified APM service. |
| type | string | The type of the APM service. e.g. “web” |
| workloads | array | An array of other service names associated with the service. |
| technology | string | The technology associated with the APM service. e.g. “mysql” |
| sloStatusCount | array | Object containing counts for SLO statuses. |
| └ ok | string | Count of services with SLO status “ok”. |
| └ breach | string | Count of services with SLO status “breach”. |
| └ notAvailable | string | Count of services with SLO status “notAvailable”. |
