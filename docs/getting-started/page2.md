# Coralogix Endpoints

Coralogix offers a regional endpoint for sending data from all observability sources into the Coralogix platform.

This document provides generally available endpoints. AWS PrivateLink endpoints can be found here.

Notes: The tables below provide the most up-to-date endpoints provided by Coralogix. Other endpoints utilized in Coralogix integrations remain available. Find out more about your Coralogix domain here.

## Endpoints

### OpenTelemetry

| Coralogix Domain | Coralogix AWS Region | Endpoint                                                   |
|------------------|----------------------|------------------------------------------------------------|
| coralogix.com    | eu-west-1            | [EU1 – Ireland] ingress.coralogix.com:443                 |
| coralogix.in     | ap-south1            | [AP1 – India] ingress.coralogix.in:443                     |
| coralogix.us     | us-east2             | [US1 – Ohio] ingress.coralogix.us:443                     |
| eu2.coralogix.com| eu-north-1           | [EU2 – Stockholm] ingress.eu2.coralogix.com:443            |
| coralogixsg.com  | ap-southeast-1       | [AP2 – Singapore] ingress.coralogixsg.com:443             |
| cx498.coralogix.com| us-west-2           | [US2 – Oregon] ingress.cx498-aws-us-west-2.coralogix.com:443 |

### Coralogix Logs

| Coralogix Domain | Coralogix AWS Region | Endpoint                                                     |
|------------------|----------------------|--------------------------------------------------------------|
| coralogix.com    | eu-west-1            | [EU1 – Ireland] https://ingress.coralogix.com/logs/v1/singles |
| coralogix.in     | ap-south1            | [AP1 – India] https://ingress.coralogix.in/logs/v1/singles    |
| coralogix.us     | us-east2             | [US1 – Ohio] https://ingress.coralogix.us/logs/v1/singles    |
| eu2.coralogix.com| eu-north-1           | [EU2 – Stockholm] https://ingress.eu2.coralogix.com/logs/v1/singles |
| coralogixsg.com  | ap-southeast-1       | [AP2 – Singapore] https://ingress.coralogixsg.com/logs/v1/singles |
| cx498.coralogix.com| us-west-2           | [US2 – Oregon] https://ingress.cx498-aws-us-west-2.coralogix.com/logs/v1/singles |

### Coralogix REST API Bulk

| Coralogix Domain | Coralogix AWS Region | Endpoint                                                  |
|------------------|----------------------|-----------------------------------------------------------|
| coralogix.com    | eu-west-1            | [EU1 – Ireland] https://ingress.coralogix.com/logs/v1/bulk |
| coralogix.in     | ap-south1            | [AP1 – India] https://ingress.coralogix.in/logs/v1/bulk    |
| coralogix.us     | us-east2             | [US1 – Ohio] https://ingress.coralogix.us/logs/v1/bulk    |
| eu2.coralogix.com| eu-north-1           | [EU2 – Stockholm] https://ingress.eu2.coralogix.com/logs/v1/bulk |
| coralogixsg.com  | ap-southeast-1       | [AP2 – Singapore] https://ingress.coralogixsg.com/logs/v1/bulk |
| cx498.coralogix.com| us-west-2           | [US2 – Oregon] https://ingress.cx498.coralogix.com/logs/v1/bulk |

### Coralogix REST API Singles

| Coralogix Domain | Coralogix AWS Region | Endpoint                                                     |
|------------------|----------------------|--------------------------------------------------------------|
| coralogix.com    | eu-west-1            | [EU1 – Ireland] https://ingress.coralogix.com/logs/v1/singles |
| coralogix.in     | ap-south1            | [AP1 – India] https://ingress.coralogix.in/logs/v1/singles    |
| coralogix.us     | us-east2             | [US1 – Ohio] https://ingress.coralogix.us/logs/v1/singles    |
| eu2.coralogix.com| eu-north-1           | [EU2 – Stockholm] https://ingress.eu2.coralogix.com/logs/v1/singles |
| coralogixsg.com  | ap-southeast-1       | [AP2 – Singapore] https://ingress.coralogixsg.com/logs/v1/singles |
| cx498.coralogix.com| us-west-2           | [US2 – Oregon] https://ingress.cx498.coralogix.com/logs/v1/singles |

### Prometheus RemoteWrite

| Coralogix Domain | Coralogix AWS Region | Endpoint                                                         |
|------------------|----------------------|------------------------------------------------------------------|
| coralogix.com    | eu-west-1            | [EU1 – Ireland] https://ingress.coralogix.com/prometheus/v1      |
| coralogix.in     | ap-south1            | [AP1 – India] https://ingress.coralogix.in/prometheus/v1         |
| coralogix.us     | us-east2             | [US1 – Ohio] https://ingress.coralogix.us/prometheus/v1         |
| eu2.coralogix.com| eu-north-1           | [EU2 – Stockholm] https://ingress.eu2.coralogix.com/prometheus/v1|
| coralogixsg.com  | ap-southeast-1       | [AP2 – Singapore] https://ingress.coralogixsg.com/prometheus/v1  |
| cx498.coralogix.com| us-west-2           | [US2 – Oregon] https://ingress.cx498-aws-us-west-2.coralogix.com/prometheus/v1 |

### TLS / TCP Syslog

| Coralogix Domain | Coralogix AWS Region | Endpoint                                                     |
|------------------|----------------------|--------------------------------------------------------------|
| coralogix.com    | eu-west-1            | [EU1 – Ireland] syslog.coralogix.com:6514                   |
| coralogix.in     | ap-south1            | [AP1 – India] syslog.coralogix.in:6514                      |
| coralogix.us     | us-east2             | [US1 – Ohio] syslog.coralogix.us:6514                      |
| eu2.coralogix.com| eu-north-1           | [EU2 – Stockholm] syslog.eu2.coralogix.com:6514             |
| coralogixsg.com  | ap-southeast-1       | [AP2 – Singapore] syslog.coralogixsg.com:6514               |
| cx498.coralogix.com| us-west-2           | [US2 – Oregon] syslog.cx498.coralogix.com:6514              |

### Management

| Coralogix Domain | Coralogix AWS Region | Endpoint                                                         |
|------------------|----------------------|------------------------------------------------------------------|
| coralogix.com    | eu-west-1            | [EU1 – Ireland] https://ng-api-http.coralogix.com/api/v1/dataprime/query |
| coralogix.in     | ap-south1            | AP1 |