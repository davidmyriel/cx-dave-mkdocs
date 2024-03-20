---
title: "Fluentd Helm Chart for Kubernetes"
date: "2021-09-01"
---

Here at Coralogix, we love [Kubernetes](http://kubernetes.io/) and we love making things simple. To help streamline your Kubernetes monitoring, we created this chart to bootstrap [our optimized Fluentd image](https://github.com/coralogix/telemetry-shippers/tree/master/logs/fluentd/k8s-helm/http) to create a DaemonSet on your [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) Package Manager.

Our chart also supports metrics for Fluentd itself by default and is MultiArch.

Our Helm chart is open source and you are welcome to review and make suggestions for improvements in our [Integrations repository](https://github.com/coralogix/telemetry-shippers/blob/master/logs/fluentd/k8s-helm/http/README.md).

## Prerequisites

- `Kubernetes` 1.20+ with Beta APIs enabled.

- `Helm` 2.9+ Package Manager installed (For installation instructions please visit [Get Helm!](https://helm.sh/)).

**Kubernetes 1.25+**  
podsecuritypolicy has been deprecated for this version up.  
You can disable this by adding the following to your override file

```
podSecurityPolicy:
  create: false
```

## Installation

Create a Namespace for the daemonset (in our example we will use: **coralogix-logger**):

```shell
kubectl create namespace coralogix-logger
```

## Create Secret Key

Your [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) can be found in the Coralogix UI in the top of the screen under _Data Flow_ --> _API Keys_ --> _Send Your Data_

```shell
kubectl create secret generic coralogix-keys \
        -n coralogix-logger \
        --from-literal=PRIVATE_KEY=<Send-Your-Data API key>
```

## Add the Helm Chart Repo

(And run an update to fetch it)

```shell
helm repo add coralogix-charts-virtual https://cgx.jfrog.io/artifactory/coralogix-charts-virtual &&
helm repo update
```

## Create An override.yml File

This is where you can override settings like the HTTP endpoint to which we send logs (we added a table of endpoints at the bottom of this manual).

You can also change the dynamic field from which we extract the application and subsystem name or completely override the configuration.

```yaml
---
#override.yaml:
fluentd:
  configMapConfigs:
  - fluentd-prometheus-conf
  # - fluentd-systemd-conf
  env:
  - name: APP_NAME
    value: namespace_name
  - name: SUB_SYSTEM
    value: container_name
  - name: APP_NAME_SYSTEMD
    value: systemd
  - name: SUB_SYSTEM_SYSTEMD
    value: kubelet.service
  - name: ENDPOINT
    value: <put_your_coralogix_endpoint_here>
  - name: "FLUENTD_CONF"
    value: "../../etc/fluent/fluent.conf"
  - name: LOG_LEVEL
    value: error
  - name: K8S_NODE_NAME
    valueFrom:
      fieldRef:
        fieldPath: spec.nodeName
```

## Deploy the Chart

```
helm upgrade fluentd-http coralogix-charts-virtual/fluentd-http \
     --install --namespace=coralogix-logger \
     -f override.yaml
```

## Remove the Daemonset

```
helm uninstall fluentd-http \
     -n coralogix-logger
```

## Coralogix Endpoints

For a list of up to date Coralogix Endpoints, see the [Coralogix Endpoints List](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/).

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).

## [](https://github.com/coralogix/eng-integrations/tree/master/fluentd/http#disable-systemd-logs)
