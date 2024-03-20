---
title: "MongoDB Atlas"
date: "2023-03-28"
---

[MongoDB Atlas](https://www.mongodb.com/) is is a fully-managed cloud database that handles the complexity of deploying, managing, and healing your deployments on the cloud service provider of your choice.

Deploy this integration to send your MongoDB Atlas metrics to you Coralogix account using the OpenTelemetry Collector.

## Prerequisites

- Coralogix [account](https://signup.coralogixstg.wpengine.com/#/)

- MongoDB Atlas [project](https://www.mongodb.com/docs/atlas/reference/api/project-create-one/)

- Private and public keys created for your MongoDB Atlas [organization](https://docs.atlas.mongodb.com/tutorial/configure-api-access/organization/create-one-api-key/) or the [project](https://docs.atlas.mongodb.com/tutorial/configure-api-access/project/create-one-api-key/) from which to send the data

- [OpenTelemetry](https://coralogixstg.wpengine.com/docs/opentelemetry/) installed

## Configuration

**STEP 1**. Create a configuration file `/etc/otelcol-contrib/config.yaml` with the following parameters:

```
receivers:
  mongodbatlas:
    # You can obtain the public key from the API Keys tab of the MongoDB Atlas Project Access Manager.
    # This value is required.
    public_key: <<YOUR-MONGODB-ATLAS-PUBLIC-KEY>>
    # You can obtain the private key from the API Keys tab of the MongoDB Atlas Project Access Manager.
    # This value is required.
    private_key: <<YOUR-MONGODB-ATLAS-PRIVATE-KEY>>

exporters:
  logging:
  prometheusremotewrite:
    endpoint: <<coralogix_prometheusremotewrite_endpoint>>
    headers:
      Authorization: "Bearer <private key>"
    external_labels:
      environment: "QA"
processors:
  batch:

extensions:
  pprof:
    endpoint: :1777
  zpages:
    endpoint: :55679
  health_check:

service:
  extensions: [health_check, pprof, zpages]
  pipelines:
    metrics:
      receivers: [mongodbatlas]
      processors: [batch]
      exporters: [logging, prometheusremotewrite]

```

- Replace `<<YOUR-MONGODB-ATLAS-PUBLIC-KEY>>` with the public key to your MongoDB Atlas organization or project.

- Replace `<<YOUR-MONGODB-ATLAS-PRIVATE-KEY>>` with the private key to your MongoDB Atlas organization or project.

- Replace `<<coralogix_prometheusremotewrite_endpoint>>` with the **[Prometheus RemoteWrite endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/)** associated with your Coralogix domain.

**STEP 2**. Restart your collector and view your metrics in your Coralogix dashboard.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
