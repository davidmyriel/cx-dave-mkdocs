---
title: "Kubernetes Complete Observability: Advanced Configuration"
date: "2023-12-20"
---

Coralogix offers [Kubernetes Observability using OpenTelemetry](https://coralogixstg.wpengine.com/docs/kubernetes-observability-opentelemetry/) for comprehensive Kubernetes and application observability. This tutorial will guide you through **advanced configuration options** for Kubernetes clusters.

For **basic configuration**, view our tutorial [here](https://coralogixstg.wpengine.com/docs/otel-collector-for-k8s/).

## **Prerequisites**

- [Kubernetes](https://kubernetes.io/) v1.24+ installed, with the command-line tool [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)

- [Helm](https://helm.sh/) v3.9+ installed and configured

## Overview

The [OpenTelemetry Integration Chart](https://github.com/coralogix/telemetry-shippers/tree/master/otel-integration/k8s-helm) utilises the [values.yaml](https://github.com/coralogix/telemetry-shippers/blob/master/otel-agent/k8s-helm/values.yaml) file as its default configuration. This configuration is based on the [OpenTelemetry Collector Configuration](https://opentelemetry.io/docs/collector/configuration/) for both the **OpenTelemetry Agent Collector** and **OpenTelemetry Cluster Collector**.

## Default Configuration in 3 Easy Steps

**STEP 1**. Create a new YAML-formatted override file that defines certain values for the [OpenTelemetry Integration Chart](https://github.com/coralogix/telemetry-shippers/tree/master/otel-integration/k8s-helm).

The following global values are the minimum required configurations to getting the chart working:

```yaml
# values.yaml
global:
  domain: "<coralogix-endpoint>"
  clusterName: "<k8s-cluster-name>"

```

- Input the following values:
    - `domain`: Choose the **[OpenTelemetry endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/)** for the **domain** associated with your Coralogix account.
    
    - `clusterName`: You are also **required** to specify as a cluster identifier.

- You also may copy all or any other configurations from the repository [`values.yaml`](https://github.com/coralogix/telemetry-shippers/blob/master/otel-agent/k8s-helm/values.yaml) file.

- If you'd like to provide your own overrides for an array values such as `extraEnvs`, `extraVolumes` or `extraVolumeMounts`, be aware that Helm does not support merging arrays. Instead, arrays [are nulled out](https://github.com/helm/helm/issues/3486).

- In case you'd like to provide your own values for these arrays, first **copy over any existing array values** from the provided [`values.yaml`](https://github.com/coralogix/telemetry-shippers/blob/master/otel-agent/k8s-helm/values.yaml) file.

**STEP 2**. Save this file as `values.yaml`

**STEP 3**. Install with the `helm upgrade --install` command

```
helm upgrade --install otel-integration coralogix-charts-virtual/otel-integration -f values.yaml -n $NAMESPACE

```

## Optional Configurations

### Enabling Dependent Charts

The **OpenTelemetry Agent** is primarily used for collecting application telemetry, while the **OpenTelemetry Cluster Collector** is primarily used to collect cluster-level data. Depending on your requirements, you can either use the default configuration that enables both components, or you can choose to disable either of them by modifying the `enabled` flag in the `values.yaml` file under the `opentelemetry-agent` or `opentelemetry-cluster-collector` section as shown below:

```
...
opentelemetry-agent:
  enabled: true
  mode: daemonset
...
opentelemetry-cluster-collector:
  enabled: true
  mode: deployment

```

### Installing the Chart on Clusters with Mixed Operating Systems (Linux and Windows)

Installing `otel-integration` is also possible on clusters that support running Windows workloads on Windows node alongside Linux nodes (such as [EKS](https://docs.aws.amazon.com/eks/latest/userguide/windows-support.html), [AKS](https://learn.microsoft.com/en-us/azure/aks/windows-faq?tabs=azure-cli) or [GKE](https://cloud.google.com/kubernetes-engine/docs/how-to/creating-a-cluster-windows)). The `kube-state-metrics` and collector will be installed on Linux nodes, as these components are supported only on Linux operating systems. Conversely, the agent will be installed on both Linux and Windows nodes as a daemonset, in order to collect metrics for both operating systems. In order to do so, the chart needs to be installed with few adjustments.

Adjust the Helm command in **STEP 10** of the [basic configuration](https://coralogixstg.wpengine.com/docs/otel-collector-for-k8s/) to use the `values-windows.yaml` file as follows:

```
helm upgrade --install otel-coralogix-integration coralogix/otel-integration -n $NAMESPACE -f values-windows.yaml --set global.domain="coralogixstg.wpengine.com" --set global.clusterName="<cluster name>"

```

### **Service Pipelines**

The [OpenTelemetry Collector Configuration](https://opentelemetry.io/docs/collector/configuration/) guides you to initialise components and then add them to the pipelines in the `service` section. It is important to ensure that the telemetry type is supported. For example, the `[prometheus](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/prometheusreceiver#prometheus-receiver)` receiver documentation in the [README](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/prometheusreceiver#prometheus-receiver) states that it only supports `metrics`. Therefore, the following `prometheus` receiver can only be defined under `receivers` and added to the `metrics` pipelines in the `service` block to enable it.

```
opentelemetry-agent:
...
	config:
		receivers:
			prometheus:
        config:
          scrape_configs:
            - job_name: opentelemetry-infrastructure-collector
              scrape_interval: 30s
              static_configs:
                - targets:
                    - ${MY_POD_IP}:8888
  
		...
	  service:
	    pipelines:
				logs:
					...
				metrics:
					receivers:
					- prometheus
	      traces:
						...

```

### **Coralogix Exporter**

In both charts, you have the option to configure the sending of logs, metrics, and / or traces to Coralogix. This can be done by configuring the [Coralogix Exporter](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter/coralogixexporter) for different pipelines. The default `values.yaml` file includes all three options, but you can customize it by removing the `coralogix` exporter from the `pipelines` configuration for either `logs`, `metrics`, or `traces`.

The following `opentelemetry-agent` exporter configuration also applies to the `opentelemetry-cluster-collector`:

```
global:
  domain: "<coralogix-domain>"
  clusterName: "<cluster-name>"
  defaultApplicationName: "otel"
  defaultSubsystemName: "integration"
...
opentelemetry-agent:
...
  config:
	...
    exporters:
      coralogix:
        timeout: "30s"
        private_key: "${CORALOGIX_PRIVATE_KEY}"
				## Values set in "global" section
        domain: "{{ .Values.global.domain }}"
				application_name: "{{ .Values.global.defaultApplicationName }}"
        subsystem_name: "{{ .Values.global.defaultSubsystemName }}"
    service:
      pipelines:
        metrics:
          exporters:
            - coralogix
							...
        traces:
          exporters:
            - coralogix
							...
        logs:
          exporters:
            - coralogix

```

## **OpenTelemetry Agent**

The OpenTelemetry Agent is enabled and deployed as a `daemonset` by default. This creates an Agent pod per node. Allowing the collection of logs, metrics, and traces from application pods to be sent to OpenTelemetry pods hosted on the same node and spreads the ingestion load across the cluster. Be aware that the OpenTelemetry Agent pods consumes resources (e.g., CPU & memory) from each node on which it runs.

```
opentelemetry-agent:
  enabled: true
  mode: daemonset

```

**Notes:**

- If there are nodes without a running OpenTelemetry Agent pod, the hosted pods of applications may be missing metadata attributes (e.g. node info and host name) in the telemetry sent.

### Agent **Presets**

The multi-instanced OpenTelemetry Agent can be deployed across multiple nodes as a `daemonset`. It provides presets for collecting host metrics, Kubernetes attributes, and Kubelet metrics. When logs, metrics, and traces are generated from a pod, the collector enriches them with the metadata associated with the hosting machine. This metadata is very useful for linking infrastructure issues with performance degradation in services.

For more information on presets, refer to the [Configuration of OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-helm-charts/tree/main/charts/opentelemetry-collector#configuration).

```
# example
opentelemetry-agent:
...	
  presets:
    logsCollection:
      enabled: true
    kubernetesAttributes:
      enabled: true
    hostMetrics:
      enabled: true
    kubeletMetrics:
      enabled: true

```

For example, enabling the `kubeletMetrics` preset to `true` will add configuration `kubeletstats` receiver that will pull node, pod, container, and volume metrics from the API server of the host’s kubelet. It will send it down metric pipeline.

```
# example
receivers:
	kubeletstats:
		auth_type: serviceAccount
		collection_interval: 20s
		endpoint: ${K8S_NODE_NAME}:10250

```

### **Receivers**

Once configured, you will be able to send logs, metrics, and traces to be collected in the **OpenTelemetry Agent** pods before exporting them to Coralogix.

To achieve this, you need to first [instrument your application](https://opentelemetry.io/docs/concepts/instrumenting/) with OpenTelemetry SDKs and expose the Collector to a corresponding [receiver](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver). It is recommended to use the [OTLP receiver](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/otlpreceiver) (OpenTelemetry protocol) for transmission over gRPC or HTTP endpoints.

The `daemonset` deployment of the OpenTelemetry Agent also uses `hostPort` for the `otlp` port, allowing agent pod IPs to be reachable via node IPs, as follows:

```
# K8s daemonset otlp port config
ports:
- containerPort: 4317
  hostPort: 4317
  name: otlp
  protocol: TCP

```

The following examples demonstrate how to configure an [Auto-Instrumented JavaScript application](https://opentelemetry.io/docs/instrumentation/js/automatic/) to send traces to the agent pod’s gRPC receiver.

**STEP 1**. Set the Kubernetes environment variables of the JavaScript application’s deployment/pod as in the example below. Define the `OTEL_EXPORTER_OTLP_ENDPOINT` as the configured `NODE_IP` and `OTLP_PORT`. Configure `OTEL_TRACES_EXPORTER` to send in the `otlp` format. Choose `OTEL_EXPORTER_OTLP_PRO` as `grpc`.

```
# kubernetes deployment manifest's env section
spec:
  containers:
		...	
	env:
  - name: NODE_IP
    valueFrom:
      fieldRef:
        fieldPath: status.hostIP
  - name: OTLP_PORT
    value: "4317"
  - name: OTEL_EXPORTER_OTLP_ENDPOINT
    value: "http://$(NODE_IP):$(OTLP_PORT)"
  - name: OTEL_TRACES_EXPORTER
    value: "otlp"
	- name: OTEL_EXPORTER_OTLP_PROTOCOL
    value: "grpc"

```

**STEP 2**. By default the agent has the otlp receiver configured as follows:

```
# collector config
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: ${MY_POD_IP}:4317
      http:
        endpoint: ${MY_POD_IP}:4318

```

**Notes**:

- `${MY_POD_IP}` is a container environment variable that is mapped to the pod's IP address.

- The agent is also preconfigured to collect data from `jaeger`.

### **Processors**

Processors are generally used to process logs, metrics, and traces before the data is exported. This may include, for example, modifying or altering attributes or sampling traces.

In the example below, a `k8sattributes` processor is used to automatically discovers k8s resources (pods), extract metadata from them and add the extracted metadata to the relevant logs, metrics and spans as resource attributes.

```
# default in values.yaml
processors:
	k8sattributes:
    filter:
      node_from_env_var: KUBE_NODE_NAME
    extract:
      metadata:
        - "k8s.namespace.name"
        - "k8s.deployment.name"
        - "k8s.statefulset.name"
        - "k8s.daemonset.name"
        - "k8s.cronjob.name"
        - "k8s.job.name"
        - "k8s.pod.name"
        - "k8s.node.name"

```

**Notes:**

- The `k8sattributes` processor is enabled by default at the `preset` level as `kubernetesAttributes` and further extended in the default [`values.yaml`](https://github.com/coralogix/telemetry-shippers/blob/master/otel-agent/k8s-helm/values.yaml).

- More information can be found in the [Kubernetes Attributes Processor README](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/processor/k8sattributesprocessor/README.md).

## OpenTelemetry Cluster Collector

Enable the `opentelemetry-cluster-collector` by setting `enabled` to `true`.

```
opentelemetry-cluster-collector:
  enabled: true
  mode: deployment

```

**Notes**:

- The cluster collector operates as a `deployment` workload with a minimal replica of 1 to avoid duplication of telemetry data.

### **Cluster Collector Presets**

The cluster collector is best suited to enable [presets](https://github.com/open-telemetry/opentelemetry-helm-charts/tree/main/charts/opentelemetry-collector#configuration) such as Kubernetes Events and Cluster Metrics. A smaller instance count of the `deployment` is sufficient to query the Kubernetes API.

```
	presets:
    clusterMetrics:
      enabled: true
    kubernetesEvents:
      enabled: true
    kubernetesExtraMetrics:
      enabled: true

```

For example, if you enable the `kubernetesEvents` preset, the [Kubernetes objects receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/k8sobjectsreceiver) configuration will be added dynamically during the Helm installation. This configuration enables the collection of `events.k8s.io` objects from the Kubernetes API server.

## Kubernetes Events: Reducing the Amount of Collected Data

When collecting Kubernetes events using the cluster collector, it is common for the number of events to reach millions, especially in large clusters with numerous nodes and constantly scaling applications. To collect only the relevant data, you can use the following settings.

### Cleaning Data

By default, a transform processor named `transform/kube-events` is configured to remove some unneeded fields from Kubernetes events collected. You may override this or alter the fields as desired.

```
processors:
	transform/kube-events:
	  log_statements:
	    - context: log
	      statements:
	        - keep_keys(body["object"], ["type", "eventTime", "reason", "regarding", "note", "metadata", "deprecatedFirstTimestamp", "deprecatedLastTimestamp"])
	        - keep_keys(body["object"]["metadata"], ["creationTimestamp"])
	        - keep_keys(body["object"]["regarding"], ["kind", "name", "namespace"])

```

### Filtering Kubernetes Events

In large-scale environments, where there are numerous events occurring per hour, it may not be necessary to process all of them. In such cases, you can use an additional OpenTelemetry processor to filter out the events that do not need to be sent to Coralogix.

Below is a sample configuration for reference. This configuration filters out any event that has the field `reason` with one of those values `BackoffLimitExceeded|FailedScheduling|Unhealthy`.

```
processors:
  filter/kube-events:
    logs:
      log_record:
        - 'IsMatch(body["reason"], "(BackoffLimitExceeded|FailedScheduling|Unhealthy)") == true'

```

### Collecting Only Warning Events

Currently, Kubernetes has two different types of events: `Normal` and `Warning`. As we have the ability to filter events according to their type, you may choose to collect only `Warning` events, as these events are key to troubleshooting. One example could be the use of a filter processor to drop all unwanted `Normal` typed events.

```
processors:
  filter/kube-events:
    logs:
      log_record:
        - 'IsMatch(body["object"]["type"], "Normal")'

```

## Kubernetes Infrastructure Monitoring

If you already have an existing log shipper (e.g. [Fluentd](https://coralogixstg.wpengine.com/docs/fluentd-helm-chart-for-kubernetes/), [Filebeat](https://coralogixstg.wpengine.com/docs/filebeat/)) in place and your goal is to monitor all Kubernetes elements of your cluster, follow these steps to enable only the necessary collection of metrics and Kubernetes events to be sent to Coralogix.

**STEP 1**. Copy the following into a YAML-formatted override file and save as `values.yaml`.

```
global:
  domain: "<coralogix-endpoint>"
  clusterName: "<k8s-cluster-name>"

opentelemetry-agent:
  presets:
    logsCollection:
      enabled: false
  config:
    exporters:
      logging: {}
    receivers:
      zipkin: null
      jaeger: null

    service:
      pipelines:
        traces: 
          exporters:
            - logging
          receivers:
            - otlp
        logs:
          exporters: 
            - logging
          receivers:
            - otlp

```

**STEP 2**. Install with the `helm upgrade --install` command.

```
helm upgrade --install otel-integration coralogix-charts-virtual/otel-integration -f values.yaml -n $NAMESPACE

```

## Next Steps

**Validation** instructions can be found [here](https://coralogixstg.wpengine.com/docs/validation-kubernetes-observability-using-opentelemetry/).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://github.com/coralogix/telemetry-shippers/tree/master/otel-integration/k8s-helm#prerequisites"><strong>GitHub Repository</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)
