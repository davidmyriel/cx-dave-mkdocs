---
title: "Hosted Grafana API"
date: "2022-11-29"
---

Coralogix provides a secure **hosted Grafana API** for creating, editing, exporting, importing, querying, and other Grafana API operations. Through Grafana APIs, you can manage your hosted Grafana dashboards.

## Prerequisites

To use the Grafana API to query your dashboard, first locate your:

- Coralogix [Alerts, Rules and Tags API key](https://coralogixstg.wpengine.com/docs/alerts-rules-tags-api-key/)

- Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/)

## **HTTP Request for Your Hosted Grafana**

The Alerts, Rules and Tags API key should be added as Coralogix token with each HTTP request. Your Coralogix Domain will be used to construct the Grafana API endpoint specific to your account.

The API request should contain the following:

- Headers:
    - ‘token:<Alerts, Rules and Tags API Key>’
    
    - ‘Content-type: application/json‘

- URL: https://ng-api-http.<domain>/grafana/api/

## **Examples**

- GET Home Dashboard

```
curl --location --request GET 'https://ng-api-http.<DOMAIN>/grafana/api/dashboards/home' \
--header 'Content-type: application/json' \
--header 'Authorization: Bearer <Alerts, Rules and Tags API Key>' \
```

- Search all dashboards in your team:

```
curl --location --request GET 'https://ng-api-http.<DOMAIN>/grafana/api/search' \
--header 'Content-type: application/json' \
--header 'Authorization: Bearer <Alerts, Rules and Tags API Key>' \
```

- Get a dashboard and panels by uid:

```
curl --location --request GET 'https://ng-api-http.<DOMAIN>/grafana/api/dashboards/uid/<UID>' \
--header 'Content-type: application/json' \
--header 'Authorization: Bearer <Alerts, Rules and Tags API Key>' \
```

- Post Dashboard - create and update existing dashboards:

```
curl --location 'https://ng-api-http.<domain>/grafana/api/dashboards/db' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <Alerts, Rules and Tags API Key>' \
--data '<Dashboard JSON>'
```

- GET Annotations:

```
curl --location --request POST 'https://ng-api-http.coralogixstg.wpengine.com/grafana/api/dashboards/db' \
--header 'Content-type: application/json' \
--header 'Authorization: Bearer <Alerts, Rules and Tags API Key>' \
--data-binary "@path/to/file"
```

## Additional Resources

- [Grafana’s HTTP API Reference](https://grafana.com/docs/grafana/v9.0/developers/http_api/)

- Grafana API supports the Host Grafana features.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
