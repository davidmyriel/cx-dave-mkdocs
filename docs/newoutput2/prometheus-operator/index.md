---
title: "Prometheus Operator"
date: "2022-12-14"
---

Prometheus Operator provides easy way to operate end-to-end Kubernetes cluster monitoring with Prometheus and exporters. Use it to collect, process, and aggregate metrics from applications in your Kubernetes cluster and send them to Coralogix.  
This guide shows you how to run Prometheus Operator in Kubernetes to export your data to Coralogix.

Our Helm chart is open source and you are welcome to review and make suggestions for improvements in our [Integrations repository](https://github.com/coralogix/telemetry-shippers/blob/master/metrics/prometheus/operator/README.md).

## Prerequisites

- `Kubernetes` 1.20+ with Beta APIs enabled.

- `Helm` 2.9+ Package Manager installed (For installation instructions please visit [Get Helm!](https://helm.sh/)).

## Setup

### Create a Namespace (in our example, we will use: **monitoring**):

```
kubectl create namespace monitoring
```

### Create Secret

Your [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) can be found in the Coralogix UI in the top of the screen under _Data Flow_ –> _API Keys_ –> _Send Your Data_

```
kubectl create secret generic coralogix-keys \
        -n monitoring \
        --from-literal=PRIVATE_KEY=<send-your-data-API-key>
```

The created secret will look as such:

```
apiVersion: v1
data:
  PRIVATE_KEY: <encrypted-private-key>
kind: Secret
metadata:
  name: coralogix-keys
  namespace: monitoring
type: Opaque 
```

### Add the Helm Chart Repo

```
helm repo add coralogix-charts-virtual https://cgx.jfrog.io/artifactory/coralogix-charts-virtual &&
helm repo update
```

### Create An override.yml File

Choose the correct Coralogix Domain according to your account, the domain table can be found [here](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

```
#override.yaml:
---
global:
  endpoint: "https://ingress.<Coralogix Domain>/prometheus/v1"
```

### Deploy the Chart

```
helm upgrade --install prometheus-coralogix coralogix-charts-virtual/prometheus-operator-coralogix \
  --namespace=monitoring \
  -f override.yaml
```

### Delete the deployment

```
helm uninstall prometheus-coralogix \
--namespace=monitoring
```
