---
title: "Fluent Bit Helm Chart for Kubernetes"
date: "2022-06-20"
---

Use our multi-arch [Helm chart](https://github.com/coralogix/telemetry-shippers/blob/master/logs/fluent-bit/k8s-helm/http/README.md) to streamline your Kubernetes monitoring by creating a DaemonSet on your [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager.

## Prerequisites

- Kubernetes 1.20+ with Beta APIs enabled

- Helm 2.9+ package manager [installed](https://helm.sh/)

## Installation

**STEP 1**. Create a namespace for the DaemonSet. The following example adopts the namespace **coralogix-logger**.

```shell
kubectl create namespace coralogix-logger
```

**STEP 2**. Create a secret key using your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/).

```shell
kubectl create secret generic coralogix-keys \
        -n coralogix-logger \
        --from-literal=PRIVATE_KEY=<Send-Your-Data API Key>
```

**STEP 3**. Add the Helm Chart Repo and run an update to fetch it.

```shell
helm repo add coralogix-charts-virtual https://cgx.jfrog.io/artifactory/coralogix-charts-virtual &&
helm repo update
```

**STEP 4**. Create an `override.yaml` to override particular settings.

- `endpoint`: Input the **[OpenTelemetry endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/)** associated with your Coralogix domain.

- `dynamic_metadata`: You may change the dynamic field from which we extract the [application and subsystem name](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) or a static value to overwrite these names.

```yaml
---
# fluentbit-override.yaml:
dynamic_metadata:
  app_name: kubernetes.namespace_name
  sub_system: kubernetes.container_name
fluent-bit:
  endpoint: <coralogix_endpoint>
  logLevel: error
```

```yaml
---
# fluentbit-override.yaml:
static_metadata:
  app_name: MyApplication
  sub_system: MySubsystem
fluent-bit:
  endpoint: <coralogix_endpoint>
  logLevel: error
```

- If a dynamic variable includes special characters, it **must** be declared in a different notation, such as the following. Failure to do so will result in LUA exceptions and failure to process properly.

```
  app_name: kubernetes.labels["k8s-app"]
  app_name: kubernetes.labels["app.kubernetes.io/name"]
```

**STEP 5**. Deploy the Helm Chart.

```
helm upgrade fluent-bit-http coralogix-charts-virtual/fluent-bit-http \
  --install \
  --namespace=coralogix-logger \
  -f fluentbit-override.yaml
```

## Remove the Daemonset

**STEP 6**. Remove the DaemonSet.

```
helm uninstall fluent-bit-http \
     -n coralogix-logger
```

**Notes**:

- `podsecuritypolicy` has been deprecated for **Kubernetes v1.25+**.

- Disable this by adding the following to your override file:

```
podSecurityPolicy:
  create: false
```

## Additional Resources

Review and make suggestions for improvement for our Helm chart in our [Integrations Repository](https://github.com/coralogix/telemetry-shippers/blob/master/logs/fluent-bit/k8s-helm/http/README.md).

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).

## [](https://github.com/coralogix/eng-integrations/tree/master/fluentd/http#disable-systemd-logs)
