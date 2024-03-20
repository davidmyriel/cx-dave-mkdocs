---
title: "FAQs"
date: "2023-10-30"
---

Check out these **frequently asked questions** regarding [Kubernetes Observability using OpenTelemetry](https://coralogixstg.wpengine.com/docs/introduction-to-kubernetes-observability-using-opentelemetry/).

## How do I upgrade the Otel integration chart to its latest version?

**STEP 1**. Ensure that your existing Values YAML file is aligned with the latest [values.yaml](https://github.com/coralogix/telemetry-shippers/blob/master/otel-integration/k8s-helm/values.yaml) file. This is to avoid [configuration issues](https://coralogixstg.wpengine.com/docs/troubleshooting/#collector-configuration-issues) that may arise.

**STEP 2**. Update the chart repository listing, assuming the OpenTelemetry Integration chart is named `otel-integration`.

```
helm repo update coralogix

```

**STEP 3**. Compare the latest chart version with the Helm installed chart version.

```
helm search repo coralogix | grep otel-integration
helm list -n $NAMESPACE

```

**STEP 4**. Upgrade to the latest with the following command:

```
helm upgrade otel-integration coralogix/otel-integration -f values.yaml -n $NAMESPACE

```

## How do I prevent telemetry from being exported to Coralogix?

In cases where telemetry data (such as logs, metrics, or traces) and their corresponding metadata are no longer needed for observability in Coralogix, the [Coralogix Exporter](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter/coralogixexporter) can be replaced with the [Debug Exporter](https://github.com/open-telemetry/opentelemetry-collector/blob/main/exporter/debugexporter). This ensures that the telemetry data is not sent to Coralogix. It is important to note that at least one exporter is required, otherwise the Debug Exporter will fail.

Below is an example that demonstrates the definition and configuration of the Debug Exporter for traces.

```yaml
opentelemetry-agent:
  config:    
    exporters:
      debug: # Define the debug exporter here
        verbosity: detailed
      coralogix:
        timeout: "30s"
        private_key: "${CORALOGIX_PRIVATE_KEY}"
        domain: "{{ .Values.global.domain }}"
        application_name: "{{ .Values.global.defaultApplicationName }}"
        subsystem_name: "{{ .Values.global.defaultSubsystemName }}"
    service:
      pipelines:
        metrics:
          exporters:
            - coralogix
          processors:
            - batch
          receivers:
            - otlp
        traces:
          exporters: 
            - debug # Enable only the Debug Exporter for Traces
          processors: 
            - batch
          receivers:
            - otlp
        logs:
          exporters:
            - coralogix
          processors:
            - batch
          receivers:
            - otlp
```

## How do I fix multi-line logs using recombine FileLog Operator?

In the case of multi-line logs, a single logical log entry may exist in multiple lines, resulting in multiple log records by default. The [OpenTelemetry Integration Helm Chart](https://github.com/coralogix/telemetry-shippers/tree/master/otel-integration/k8s-helm) includes a commented section in the [values.yaml](https://github.com/coralogix/telemetry-shippers/blob/master/otel-integration/k8s-helm/values.yaml) file to address this issue. Navigate to `presets` > `logsCollection` > `extraFilelogOperators`.

```
presets:
    metadata:
      enabled: true
      clusterName: "{{.Values.global.clusterName}}"
      integrationName: "coralogix-integration-helm"
    logsCollection:
      enabled: true
      storeCheckpoints: true
      maxRecombineLogSize: 1048576
      extraFilelogOperators: []
#     - type: recombine
#       combine_field: body
#       source_identifier: attributes["log.file.path"]
#       is_first_entry: body matches "^(YOUR-LOGS-REGEX)"

```

The `extraFilelogOperators` is an array where we can define any additional Filelog Operators that we want to leverage when shipping logs.

Uncomment the example with the [recombine](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/pkg/stanza/docs/operators/recombine.md) operator. This operator is used to recombine log entries from a single log file, as defined by the `source_identifier` looking at the `log.file.path` attribute, in cases where multi-line has occurred. To achieve this, a regex is defined for the first line of the log using the `is_first_entry` field.

A working example of a values.yaml file can be seen below:

```
global:
  domain: "eu2.coralogixstg.wpengine.com"
  clusterName: "coralogix-cluster"

opentelemetry-agent:
  presets:
    logsCollection:
      extraFilelogOperators:
        - type: recombine
          combine_field: body
          combine_with: "\n"
          source_identifier: attributes["log.file.path"]
          is_first_entry: body matches "(\\d{2}:\\d{2}:\\d{2}\\.\\d{3}\\s\\[\\w+\\]\\s\\w+\\s|\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2}\\.\\d{6}Z\\s\\w+\\s)"

```

**Notes:**

- The example seeks 2 different regex patterns to recognize the start of the log entry.

- Double-escape all special characters to match the GoLang standard patterns.

```
\\d{2}:\\d{2}:\\d{2}\\.\\d{3}\\s\\[\\w+\\]\\s\\w+\\s
AND
\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2}\\.\\d{6}Z\\s\\w+\\s

```

## How do I optimize batch sizing?

Coralogix recommends the default otel-integration chart settings for [batch processors](https://github.com/open-telemetry/opentelemetry-collector/tree/main/processor/batchprocessor) in all collectors:

```
  batch:
    send_batch_size: 1024
    send_batch_max_size: 2048
    timeout: "1s"
```

This sizing ensures that the telemetry sent to the Coralogix backend is batched into larger requests, reducing networking overhead and improving performance.

These settings impose a hard limit of 2048 units (spans, metrics, logs) on the batch size, balancing the recommended batch size and networking overhead.

While you can adjust these settings to suit your requirements, considering the size limits enforced by Coralogix endpoints, [currently set to a max of 10 MB after decompression](https://coralogix.com/docs/coralogix-endpoints/#limits--quotas), is essential.

Find out more [here](https://github.com/open-telemetry/opentelemetry-collector/tree/main/processor/batchprocessor#batch-processor) about configuring your batch processor.

## Support

**Need help?**

Contact us **via our in-app chat** or by emailing [support@coralogix.com](mailto:support@coralogixstg.wpengine.com).
