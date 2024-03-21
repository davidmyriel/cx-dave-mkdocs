---
title: "Coralogix Endpoints"
date: "2023-02-22"
---

Coralogix offers a regional endpoint for sending data from all observability sources into the Coralogix platform.

This document provides generally available endpoints. [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/what-is-privatelink.html) endpoints can be found [here](https://coralogixstg.wpengine.com/docs/coralogix-amazon-web-services-aws-privatelink-endpoints/).

**Notes**:

- The tables below provide the most up-to-date endpoints provided by Coralogix. Other endpoints utilized in Coralogix integrations remain available.

- Find out more about your Coralogix domain [here](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

## Endpoints

### OpenTelemetry

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>ingress.coralogixstg.wpengine.com:443</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>ingress.coralogix.in:443</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>ingress.coralogix.us:443</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>ingress.eu2.coralogixstg.wpengine.com:443</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>ingress.coralogixsg.com:443</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>ingress.cx498-aws-us-west-2.coralogixstg.wpengine.com:443</td></tr></tbody></table>

### Coralogix Logs

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://ingress.coralogixstg.wpengine.com/logs/v1/singles</td></tr><tr><td><br>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://ingress.coralogix.in/logs/v1/singles</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://ingress.coralogix.us/logs/v1/singles</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://ingress.eu2.coralogixstg.wpengine.com/logs/v1/singles<br></td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://ingress.coralogixsg.com/logs/v1/singles</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://ingress.cx498-aws-us-west-2.coralogixstg.wpengine.com/logs/v1/singles</td></tr></tbody></table>

### Coralogix REST API Bulk

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://ingress.coralogixstg.wpengine.com/logs/v1/bulk<br></td></tr><tr><td><br>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://ingress.coralogix.in/logs/v1/bulk</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://ingress.coralogix.us/logs/v1/bulk</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://ingress.eu2.coralogixstg.wpengine.com/logs/v1/bulk<br></td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://ingress.coralogixsg.com/logs/v1/bulk</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://ingress.cx498.coralogixstg.wpengine.com/logs/v1/bulk</td></tr></tbody></table>

### Coralogix REST API Singles

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://ingress.coralogixstg.wpengine.com/logs/v1/singles<br></td></tr><tr><td><br>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://ingress.coralogix.in/logs/v1/singles</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://ingress.coralogix.us/logs/v1/singles</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://ingress.eu2.coralogixstg.wpengine.com/logs/v1/singles<br></td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://ingress.coralogixsg.com/logs/v1/singles</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://ingress.cx498.coralogixstg.wpengine.com/logs/v1/singles</td></tr></tbody></table>

### Prometheus RemoteWrite

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://ingress.coralogixstg.wpengine.com/prometheus/v1</td></tr><tr><td><br>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://ingress.coralogix.in/prometheus/v1</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://ingress.coralogix.us/prometheus/v1</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://ingress.eu2.coralogixstg.wpengine.com/prometheus/v1<br></td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://ingress.coralogixsg.com/prometheus/v1</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://ingress.cx498-aws-us-west-2.coralogixstg.wpengine.com/prometheus/v1</td></tr></tbody></table>

### TLS / TCP Syslog

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]<br></td><td>syslog.coralogixstg.wpengine.com:6514</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>syslog.coralogix.in:6514</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>syslog.coralogix.us:6514</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>syslog.eu2.coralogixstg.wpengine.com:6514</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]<br></td><td>syslog.coralogixsg.com:6514<br></td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>syslog.cx498.coralogix.com:6514</td></tr></tbody></table>

### Management

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]<br></td><td>https://ng-api-http.coralogixstg.wpengine.com/api/v1/dataprime/query</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://ng-api-http.app.coralogix.in/api/v1/dataprime/query</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://ng-api-http.coralogix.us/api/v1/dataprime/query</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://ng-api-http.eu2.coralogixstg.wpengine.com/api/v1/dataprime/query</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]<br></td><td>https://ng-api-http.coralogixsg.com/api/v1/dataprime/query</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://ng-api-http.cx498.coralogixstg.wpengine.com/api/v1/dataprime/query</td></tr></tbody></table>

### External Alerts

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]<br></td><td>https://api.coralogixstg.wpengine.com/api/v1/external/alerts</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://api.coralogix.in/api/v1/external/alerts</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://api.coralogix.us/api/v1/external/alerts</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://api.eu2.coralogixstg.wpengine.com/api/v1/external/alerts</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]<br></td><td>https://api.coralogixsg.com/api/v1/external/alerts</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://api.cx498.coralogixstg.wpengine.com/api/v1/external/alerts</td></tr></tbody></table>

### Firehose

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://firehose-ingress.coralogixstg.wpengine.com/firehose</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://firehose-ingress.coralogix.in/firehose</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://firehose-ingress.coralogix.us/firehose</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://firehose-ingress.eu2.coralogixstg.wpengine.com/firehose</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://firehose-ingress.coralogixsg.com/firehose<br></td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://firehose-ingress.cx498.coralogixstg.wpengine.com/firehose</td></tr></tbody></table>

### gRPC

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://ng-api-grpc.coralogixstg.wpengine.com</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://ng-api-grpc.app.coralogix.in</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://ng-api-grpc.coralogix.us</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://ng-api-grpc.eu2.coralogixstg.wpengine.com</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://ng-api-grpc.coralogixsg.com<br></td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://ng-api-grpc.cx498.coralogixstg.wpengine.com</td></tr></tbody></table>

### SCIM

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://ng-api-http.coralogixstg.wpengine.com/scim</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://ng-api-http.app.coralogix.in/scim</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://ng-api-http.coralogix.us/scim</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://ng-api-http.eu2.coralogixstg.wpengine.com/scim</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://ng-api-http.coralogixsg.com/scim<br></td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://ng-api-http.cx498.coralogixstg.wpengine.com/scim</td></tr></tbody></table>

### OpenSearch

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://coralogix-esapi.coralogixstg.wpengine.com:9443/*</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://es-api.app.coralogix.in:9443/*</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://esapi.coralogix.us:9443/*</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://es-api.eu2.coralogixstg.wpengine.com:9443/*</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://es-api.coralogixsg.com:9443/*</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://es-api.cx498.coralogixstg.wpengine.com:9443/*</td></tr></tbody></table>

### Metrics Cardinality

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>Endpoint</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]</td><td>https://ng-api-http.coralogixstg.wpengine.com/api/metrics/cardinality</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>https://ng-api-http.coralogix.in/api/metrics/cardinality</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>https://ng-api-http.coralogix.us/api/metrics/cardinality</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>https://ng-api-http.eu2.coralogixstg.wpengine.com/api/metrics/cardinality</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]</td><td>https://ng-api-http.coralogixsg.com/api/metrics/cardinality</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>https://ng-api-http.cx498.coralogixstg.wpengine.com/api/metrics/cardinality</td></tr></tbody></table>

## LoadBalancer Stats IPs

<table><tbody><tr><td><strong>Coralogix Domain</strong></td><td><strong>Coralogix AWS Region</strong></td><td><strong>LoadBalancer<br>Stats IPs</strong></td></tr><tr><td>coralogixstg.wpengine.com</td><td>eu-west-1<br>[EU1 - Ireland]<br></td><td>34.253.20.0<br>18.203.53.231<br>52.212.0.182</td></tr><tr><td>coralogix.in</td><td>ap-south1<br>[AP1 - India]</td><td>3.109.88.8<br>13.127.95.184<br>43.204.151.45</td></tr><tr><td>coralogix.us</td><td>us-east2<br>[US1 - Ohio]</td><td>18.119.84.198<br>3.22.174.3<br>3.139.112.94</td></tr><tr><td>eu2.coralogixstg.wpengine.com</td><td>eu-north-1<br>[EU2 - Stockholm]</td><td>13.53.114.41<br>16.170.111.131<br>13.50.102.213</td></tr><tr><td>coralogixsg.com</td><td>ap-southeast-1<br>[AP2 - Singapore]<br></td><td>52.77.57.115<br>52.221.106.207<br>18.142.194.181</td></tr><tr><td>cx498.coralogixstg.wpengine.com</td><td>us-west-2<br>[US2 - Oregon]</td><td>44.228.187.149</td></tr></tbody></table>

## Limits & Quotas

Coralogix places the following limits on endpoints:

- A hard limit of 10MB of data to our OpenTelemetry endpoint, with a recommendation of 2MB

- A hard limit of 2411724 bytes of data to our Prometheus RemoteWrite endpoint, with a recommendation for any amount less than this limit

Limits apply to single requests, regardless of timespan.

## Additional Resources

<table><tbody><tr><td><strong>GitHub</strong></td><td><a href="https://github.com/coralogix/telemetry-shippers"><strong>Coralogix Telemetry Shippers</strong></a><br><a href="https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter/coralogixexporter"><strong>Coralogix OpenTelemetry Exporter</strong></a></td></tr><tr><td><strong>Docs</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-domain/"><strong>Coralogix Domain</strong></a><br><a href="https://coralogixstg.wpengine.com/docs/coralogix-aws-lambda-telemetry-exporter/"><strong>Coralogix Lambda Telemetry</strong></a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [**support@coralogixstg.wpengine.com**](mailto:support@coralogixstg.wpengine.com).
