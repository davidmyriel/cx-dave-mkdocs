---
title: "Prometheus Agent"
date: "2023-01-17"
---

Send your data to Coralogix using [Prometheus Agent](https://prometheus.io/blog/2021/11/16/agent/), a new operational mode of running [Prometheus](https://prometheus.io/), built directly into the Prometheus binary. The agent mode optimizes the remote write use case configuring the Prometheus instance while disabling some of Prometheus' usual features - querying and alerting.

Use agent mode to:

- Focus on scraping metrics and sending them to Coralogix, with querying and alerting disabled

- Enable easier horizontal scalability for ingestion compared to server-mode

- Increase efficiency by replacing local storage with a customized TSDB WAL

- Improve scalability by enabling horizontal scalability for ingestion

## Prerequisites

1\. [Sign up](https://signup.coralogixstg.wpengine.com/#/) for a Coralogix account. Set up your account on the Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/) corresponding to the region within which you would like your data stored.

2\. Access your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/).

3\. [Install and configure](https://coralogixstg.wpengine.com/docs/prometheus-operator/) Prometheus Operator. Prometheus Agent collects ServiceMonitors and PodMonitors, which are enabled only when using the Prometheus Operator.

**Note**:

- The Prometheus operator must be in **v0.59.0 at least** in order to support the agent mode.

## Configuration & Installation

**STEP 1. Create a secret private key.**

Create a Kubernetes `secret` for your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) called `coralogix-keys` with the value `PRIVATE_KEY`.

Take this step in order to ensure that your private key remains protected and unexposed. While other methods exist for protecting this private information, **we recommend** [creating a secret](https://jamesdefabia.github.io/docs/user-guide/kubectl/kubectl_create_secret_generic/) in this manner while running on a Kubernetes cluster.

**Your private key, as well as the Helm chart, should be saved in the same namespace**.

```
kubectl create secret generic coralogix-keys \\
  --from-literal=PRIVATE_KEY=<private-key>

```

The created secret should look like this:

```
apiVersion: v1
data:
  PRIVATE_KEY: <encrypted-private-key>
kind: Secret
metadata:
  name: coralogix-keys
  namespace: <the-release-namespace>
type: Opaque

```

**STEP 2.** **Configure your Coralogix endpoints**.

Select a **Prometheus RemoteWrite** [endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) for the [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/) associated with your Coralogix account.

**STEP 3. Add the Coralogix Helm charts repository.**

Add the Coralogix Helm charts repository to the local repos list with the following command:

```
helm repo add coralogix-charts-virtual https://cgx.jfrog.io/artifactory/coralogix-charts-virtual

```

To get the updated Helm charts from the added repository, run:

```
helm repo update

```

**STEP 4**. **Create an override.yaml file.**

Create an override.yaml file, which includes the following:

```
---
# override.yaml:
prometheus:
  prometheusSpec:
    remoteWrite:
    - authorization:
        credentials:
          name: coralogix-keys
          key: PRIVATE_KEY
      name: prometheus-agent-coralogix
      queueConfig:
        capacity: 2500
        maxSamplesPerSend: 1000
        maxShards: 200
      remoteTimeout: 120s
      url: <YOUR_ENDPOINT>


```

**STEP 5. Install the chart.**

```
helm upgrade prometheus-agent coralogix-charts-virtual/prometheus-agent-coralogix \
  --install \
  --namespace=<your-namespace> \
  --create-namespace \
  -f override.yaml

```

## Best Practices

By default, the Prometheus agent uses ephemeral volumes, which is not suitable for a production environment.

For a production environment, we **highly recommended** defining a persistent volume to avoid data loss between restarts. This is done by using the specification available on the values file.

**Note**:

If you do not specify the `storageClassName`, Kubernetes will use the default storage class available on the cluster.

```
prometheus:
  prometheusSpec:
    storageSpec:
      volumeClaimTemplate:
        spec:
          accessModes: ["ReadWriteOnce"]
          resources:
            requests:
              storage: 50Gi

```

## Limits & Quotas

Coralogix places the following limits on endpoints:

- A **hard limit of 10MB** of data to our **OpenTelemetry** **endpoint**, with a **recommendation of 2MB**

- A **hard limit of 2411724 bytes** of data to our **Prometheus RemoteWrite** **endpoint**, with a **recommendation** for any amount less than this limit.

Limits apply to single requests, regardless of timespan.

## Additional Resources

<table><tbody><tr><td><strong>Documentation</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/prometheus-operator/" target="_blank" rel="noreferrer noopener">Prometheus Operator</a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
