---
title: "Metrics API"
date: "2022-10-27"
---

Coralogix provides a Metrics API that lets you query your hosted metrics easily.

## Query Your Metrics

To use the Metrics API to query your metrics, first locate your:

- Coralogix Logs Query Key. In your Coralogix toolbar, navigate to **Data Flow** > **API Keys**.

- Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/)

Each HTTP request should add The Logs Query Key as a Coralogix token. Your Coralogix domain will be used to construct the Metrics API endpoint specific to your account.

The API request should contain the following:

- Headers:
    - 'Authorization: Bearer <TOKEN>'
    
    - ‘Content-type: application/json‘

- URL: Select the URL associated with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

<table><tbody><tr><td><strong>Domain</strong></td><td><strong>URL</strong></td></tr><tr><td>coralogix.us<br>cx498.coralogixstg.wpengine.com<br>coralogixstg.wpengine.com<br>eu2.coralogixstg.wpengine.com<br>coralogixsg.com</td><td>https://ng-api-http.&lt;domain&gt;/metrics/api/v1</td></tr><tr><td>coralogix.in</td><td>https://ng-api-http.app.&lt;domain&gt;/metrics/api/v1</td></tr></tbody></table>

## Examples

Be sure to update your **domain** and **Log Query Key** when using the following examples.

### Server Instant Metric Query

Evaluates a server's instant metric at a specific moment in time.

```
curl --location --request GET '/api/v1/query?query=<metric_name>' \\
--header 'Authorization: Bearer <TOKEN>' \\
--header 'Content-Type: application/json' \\
--data-raw ''
```

### Instant Metric Query

Evaluates an instant metric at a defined single point in time.

```
curl --location --request GET 'https://ng-api-http.app.<domain>/metrics/api/v1/query?query=<metric_name>&time=2022-10-26T12:10:51.781Z' \\
--header 'Authorization: Bearer <TOKEN>' \\
--header 'Content-Type: application/json' \\
--data-raw ''

```

### Range Query

Evaluates an instant metric over a range of time.

```
curl --location --request GET 'https://ng-api-http.app.<domain>/metrics/api/v1/query_range?query=<metric_name>&start=2022-10-26T10:10:51.781Z&end=2022-10-26T12:10:51.781Z' \\
--header 'Authorization: Bearer <TOKEN>' \\
--header 'Content-Type: application/json' \\
--data-raw ''

```

## **Additional Resources**

<table><tbody><tr><td>Documentation</td><td><a href="https://prometheus.io/docs/prometheus/latest/querying/api/" target="_blank" rel="noreferrer noopener"><strong>Prometheus API Documentation</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Contact us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
