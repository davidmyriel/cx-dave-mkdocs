---
title: "Kubernetes Observability using OpenTelemetry"
date: "2023-12-20"
---

Coralogix offers **Kubernetes Observability using OpenTelemetry** for comprehensive Kubernetes and application observability. Using our **OpenTelemetry Chart**, the integration enables you to simplify the collection of logs, metrics, and traces from the running application in your pods to the cluster-level components of your Kubernetes cluster, while enabling our [Kubernetes Dashboard](https://coralogixstg.wpengine.com/docs/kubernetes-dashboard/).

## Observability Explained

### Kubernetes Observability

Kubernetes observability is essential for monitoring a Kubernetes cluster's health, performance, resource utilization, and workloads. It involves collecting and analyzing metrics, logs and traces from the cluster and underlying machines to ensure the stability and optimal operation of the cluster.

When managing and monitoring Kubernetes components, consider these critical areas:

- **Cluster Health**. Monitoring the overall health of the Kubernetes cluster is crucial. This includes checking the status and availability of the master and worker nodes and the control plane components such as the API server, kube-proxy, and scheduler.

- **Resource Utilisation**. Observing the resource utilization of **cluster** **nodes** and **individual** **pods** is essential for identifying bottlenecks, optimizing resource allocation, and ensuring efficient utilization of cluster resources. Extracting metrics and metadata from the underlying components provides the CPU, memory consumption, system load, and file system activity.

- **Networking**. Monitoring Kubernetes networking is crucial for smooth pod and service communication. This involves observing network traffic, latency, and error rates to detect and troubleshoot connectivity issues, identify performance bottlenecks, and improve network configurations.

- **Application Performance**. Observing the performance of applications running on Kubernetes is essential for delivering a reliable and responsive user experience.

- **Logging and Tracing**. Logging and tracing play a vital role in understanding the behaviour and troubleshooting of Kubernetes components and applications. By collecting and analysing logs and traces, you can gain insights into system events, diagnose issues, and perform root cause analysis. Implementing effective logging and tracing strategies is important to capture relevant information for observability purposes.

### Application Observability

Application observability focuses on monitoring and understanding the behavior of applications running on the Kubernetes cluster. It includes collecting and analyzing metrics, logs, and traces specific to the applications to gain insights into their performance and identify any issues or bottlenecks. This includes monitoring response times, throughput, error rates, and other application-specific metrics.

## Integration Overview

Integrating OpenTelemetry with Kubernetes enables comprehensive Kubernetes and application observability. The OpenTelemetry Integration Chart is a solution that combines two dependent charts into a single Helm installation for Kubernetes clusters: the **OpenTelemetry Agent** and the **OpenTelemetry Cluster Collector**. Both are built on the [OpenTelemetry Collector Helm Chart](https://github.com/open-telemetry/opentelemetry-helm-charts/tree/main/charts/opentelemetry-collector), but are configured for optimal performance while collecting different data sources from Kubernetes. Together, they simplify the collection of logs, metrics, and traces from the running application in pods to the cluster-level components of your Kubernetes cluster.

![Kubernetes Observability OpenTelemetry Coralogix k8s otel](https://coralogixstg.wpengine.com/wp-content/uploads/2023/11/Excali-Diagram-1.svg)

Additionally, the OpenTelemetry Integration chart enables the collection of telemetry data needed for the [Kubernetes Dashboard](https://coralogixstg.wpengine.com/docs/kubernetes-dashboard/) setup. This dashboard is a powerful web-based interface for monitoring and managing Kubernetes clusters. It provides real-time CPU, memory, network, and disk usage metrics for nodes and pods. Users can track resource trends, optimize workload placement, and troubleshoot issues effectively. The dashboard also displays Kubernetes events for quick problem identification and resolution. Streamlining cluster management ensures efficient performance and smooth operation of applications.

### OpenTelemetry Agent

The OpenTelemetry Agent simplifies the collection of logs, metrics, and traces from applications running in your Kubernetes cluster. It is configured to deploy as a `daemonset` and runs on every node in the cluster. The agent maps metadata - such as Kubernetes attributes, Kubelet metrics, and host data - to the collected telemetry. This is particularly beneficial for high-traffic clusters or when utilizing our [APM capabilities](https://coralogixstg.wpengine.com/docs/apm-kubernetes/).

The agent comes with several pre-configured processors and receivers:

- [Kubernetes Attributes Processor](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor/k8sattributesprocessor). This processor enriches data with Kubernetes metadata, such as pod and deployment information.

- [Kubernetes Log Collection](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/filelogreceiver). Enables native Kubernetes log collection with OpenTelemetry Collector, eliminating the need for multiple agents like Fluentd, Fluent Bit, or Filebeat.

- [Host Metrics](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/hostmetricsreceiver). Collects native Linux and Windows node resource data.

- [Kubelet Metrics](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/kubeletstatsreceiver). Fetches running container metrics from the local Kubelet.

- Traces. Collects data in various formats such as [Jaeger](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/jaegerreceiver), [OpenTelemetry Protocol](https://github.com/open-telemetry/opentelemetry-collector/blob/main/receiver/otlpreceiver/README.md), or [Zipkin](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/zipkinreceiver).

- Metrics. Collects application metrics via the [OpenTelemetry Protocol](https://github.com/open-telemetry/opentelemetry-collector/blob/main/receiver/otlpreceiver/README.md).

- [Span Metrics](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor/spanmetricsprocessor). Converts traces into requests, duration, and error metrics using the spanmetrics processor.

### OpenTelemetry Cluster Collector

The OpenTelemetry Cluster Collector retrieves data from the cluster level, including Kubernetes events, cluster metrics, and additional Kubernetes-specific metrics. It enables you to gain insights into the health and performance of various objects within the cluster, such as deployments, nodes, and pods.

- [Cluster Metrics Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/k8sclusterreceiver). The Kubernetes Cluster receiver collects cluster-level metrics from the Kubernetes API server.

- [Kubernetes Events Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/k8seventsreceiver). This receiver collects Kubernetes events and sends them to the kube-events subsystem. It allows you to take advantage of other features, such as the [Kubernetes Dashboard](https://coralogixstg.wpengine.com/docs/kubernetes-dashboard/) and alerting, using Kubernetes events as the primary source of information.

- Kubernetes Extra Metrics. This preset enables the collection of extra Kubernetes-related metrics, such as node information, pod status, or container I/O metrics. These metrics are collected in particular for the [Kubernetes Dashboard](https://coralogixstg.wpengine.com/docs/kubernetes-dashboard/).

- [Integration Presets](https://github.com/coralogix/telemetry-shippers/tree/master/otel-integration/k8s-helm#integration-presets). This chart provides support to integrate with various applications (e.g. mysql) running on your cluster to monitor them out of the box.

### Kubernetes Dashboard

The OpenTelemetry Integration chart enables the collection of essential metrics needed for the [Kubernetes Dashboard](https://coralogixstg.wpengine.com/docs/kubernetes-dashboard/) setup. The [Kubernetes Cluster Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/k8sclusterreceiver#kubernetes-cluster-receiver) is an essential part that provides cluster-level metrics and entity events from the Kubernetes API server. It can report metrics of allocatable resource types such as `cpu` and `memory` and give an update on node conditions (e.g. `Ready`, `MemoryPressure`). As a whole, the metrics gathered are useful for the Kubernetes Dashboard to report on the health of your cluster.

## Next Steps

View our **basic configuration** instructions [here](https://coralogixstg.wpengine.com/docs/otel-collector-for-k8s/).

**Advanced configuration** instructions can be found [here](https://coralogixstg.wpengine.com/docs/set-up-kubernetes-observability-using-opentelemetry-advanced-configuration/).

## Support

**Need help?**

Our world-class customer success team is available 24/7 to answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)
