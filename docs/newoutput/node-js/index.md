---
title: "Node.js OpenTelemetry Instrumentation"
date: "2024-01-22"
---

This tutorial demonstrates how to instrument Node.js applications to capture metrics and traces using OpenTelemetry and send them to Coralogix.

## Overview

Coralogix offers three ways to collect instrumentation data from your applications:

- Automatic instrumentation without application code changes

- Manual instrumentation

- Instrumentation injection with K8s deployed applications

To configure your instrumentation to define, report, and monitor Coralogix [Service Flows](https://coralogixstg.wpengine.com/docs/service-flows/), you must use [individual auto-instrumentation](#IndividualMethod) or [manual instrumentation](#ManualInstrumentation). The bundled auto-instrumentation method is not supported.

## Auto Instrumentation

Many popular Node.js packages have auto instrumentation libraries that allow your application to automatically instrument key elements of your code without modifying it.

The Node.js packages that offer these automatic instrumentation libraries can be found [here](https://github.com/open-telemetry/opentelemetry-js-contrib/tree/main/plugins/node).[](https://github.com/open-telemetry/opentelemetry-js-contrib/tree/main/plugins/node)

If you need to collect instrumentation of other libraries or application code, you must manually instrument those portions.

### Auto Instrumentation using Instrumentation Libraries

Utilize instrumentation libraries individually or using a bundled package called [auto-instrumentations-node](https://opentelemetry.io/docs/instrumentation/js/automatic/)**.** The bundled approach will load all currently available auto-instrumentation libraries into your application to ensure it captures all possible instrumentation. This is a great option for testing purposes, to see what is available to your application, but it does come with the overhead of increased application size and memory footprint. Once you’ve identified which libraries best match your telemetry objectives, you can use the individual method to load the required packages.

### Bundled Method

The bundled method is a fully automatic solution that doesn’t require any JavaScript code to be written or modified. You’ll first need to install the prerequisite packages below to use the bundled method.

```
npm install --save @opentelemetry/api
npm install --save @opentelemetry/auto-instrumentations-node

```

You then need to set environment variables to allow the auto-instrumentation registration to know what to do. Using this method, you can set almost every configuration option available in OTEL. For example, you set up the exporter to forward messages directly to Coralogix as follows:

```
export OTEL_TRACES_EXPORTER="otlp"
export OTEL_EXPORTER_OTLP_PROTOCOL="grpc"
export OTEL_EXPORTER_OTLP_COMPRESSION="gzip"
export OTEL_EXPORTER_OTLP_TRACES_ENDPOINT="<https://ingress.coralogixstg.wpengine.com:443>"
export OTEL_EXPORTER_OTLP_HEADERS="Authorization=Bearer <CXPrivateKey>"
export OTEL_RESOURCE_ATTRIBUTES="cx.application.name=AppName, cx.subsystem.name=SubName, service.namespace=YourAppNamespace"
export OTEL_NODE_RESOURCE_DETECTORS="all"
export OTEL_SERVICE_NAME="YourAppServiceName"

```

These environment variables can be set in any manner available for your deployment type. However, they cannot be set within the application, as they need to be available to the auto-instrumentation library at startup. Full documentation of the environment variables can be found in the [official documentation](https://opentelemetry.io/docs/concepts/sdk-configuration/).

Example:  
In the given instance, the variable OTEL\_NODE\_RESOURCE\_DETECTORS is configured with the value 'all'. Certain resource types might be absent if you plan to execute the test application on your local machine, leading to errors. To address this, set OTEL\_NODE\_RESOURCE\_DETECTORS to "env,host,os,process" to avoid encountering errors. You can locate the Resource detectors [here](link_to_resource_detectors).

Lastly, you must execute your application and —require the registration library. You can do so two ways, using an additional environment variable, or adding the requirement directly to your execution command:

```
export NODE_OPTIONS="--require @opentelemetry/auto-instrumentations-node/register"
node YourApp.js

```

or

```
node --require @opentelemetry/auto-instrumentations-node/register YourApp.js

```

### Individual Method

You’ll first need to install the prerequisite packages below.

```
npm install --save @opentelemetry/api
npm install --save @opentelemetry/auto-instrumentations-node
npm install --save @coralogix/opentelemetry

```

If you don’t want to use the bundled method, you can also create your instrumentation module and “—require” it. This method is a bit more complicated, but it does give you more control as to which libraries you want to import and how you want to process the different data sources.

Below is a short example of instrumentation for an “express” application. We’ll create a new instrumentation.js file to avoid having to modify the base code of the application.

```
//instrumentation.js

//load the auto-instrumentation libraries we want to use
const { HttpInstrumentation } = require('@opentelemetry/instrumentation-http');
const { ExpressInstrumentation } = require('@opentelemetry/instrumentation-express');

//load some base OTEL modules
const opentelemetry = require("@opentelemetry/sdk-node");
const { getNodeAutoInstrumentations } = require("@opentelemetry/auto-instrumentations-node");

//load an Exporter (OTLP to Coralogix ingress API)
const { OTLPTraceExporter } = require("@opentelemetry/exporter-trace-otlp-proto");

//transaction sampler
**import {CoralogixTransactionSampler} from "@coralogix/opentelemetry";**

// // Optional libraries for setting resource attributes via code
// const { Resource } = require('@opentelemetry/resources');
// const { SemanticResourceAttributes } = require('@opentelemetry/semantic-conventions');

// // Optional libraries for metrics configuration
// const { OTLPMetricExporter } = require("@opentelemetry/exporter-metrics-otlp-proto");
// const { PeriodicExportingMetricReader } = require('@opentelemetry/sdk-metrics');

//Now we'll initialize the OTEL sdk:
const sdk = new opentelemetry.NodeSDK({
  // // Optional resource configuration section
  // resource: new Resource({
  //   [SemanticResourceAttributes.SERVICE_NAME]: 'yourServiceName',
  //   [SemanticResourceAttributes.SERVICE_VERSION]: '1.0',
  // }),
  // // Optional metrics configuration section
  // metricReader: new PeriodicExportingMetricReader({
  //   exporter: new OTLPMetricExporter({
  //     concurrencyLimit: 1,
  //   }),
  // }),
	**sampler: new CoralogixTransactionSampler(new AlwaysOnSampler()),**
  instrumentations: [
    new HttpInstrumentation(),
    new ExpressInstrumentation(),
  ],
});

sdk.start();

```

With this custom instrumentation.js, we’ll initialize it as before using the same environment variables and load it using a required stanza:

```
node --require ./instrumentation.js YourApp.js

```

## Manual Instrumentation

Manual instrumentation of your code may be beneficial when you need control of span creation, managing context propagation, or when you need to instrument code without existing auto-instrumentation libraries. You’ll also need to do some manual instrumentation if you want to add support for logs, as log support is not native to the Node.js OTEL instrumentation framework. Examples of Log and Metrics support will be provided at a later date.

Initially, you’ll have to decide what trace provider you intend to use. Commonly, this would be the `BasicTracerProvider` from the `@opentelemetry/sdk-trace-base` module, but there are several other options available.

In addition, you’ll need to configure resource attribute tags manually, commonly done with the `SemanticResourceAttributes` from `@opentelemetry/semantic-conventions` though custom attributes can also be used.

You’ll have to select a span processor, based on your application load. In the provided example, We’ll be using the `SimpleSpanProcessor` but `BatchSpanProcessor` is typically preferred.

You can add in a trace sampling configuration, but by default, it will submit 100% of your traces. We do not use a trace sampling configuration in our example.

Lastly, you’ll need an exporter. We’ll want to use the otlp+protobuf trace exporter as we used in our earlier examples. This will allow us to submit traces directly to the Coralogix backend. If you want to submit through an Opentelemetry Collector to leverage resource detection and other enrichments, that can be done using various trace exporters.

Below is an example application from the [OpenTelemetry-js repository](https://github.com/open-telemetry/opentelemetry-js/blob/main/examples/basic-tracer-node/index.js), adjusted to support submission to Coralogix:

```
const opentelemetry = require('@opentelemetry/api');
const { Resource } = require('@opentelemetry/resources');
const { SemanticResourceAttributes } = require('@opentelemetry/semantic-conventions');
const { BasicTracerProvider, ConsoleSpanExporter, SimpleSpanProcessor } = require('@opentelemetry/sdk-trace-base');
const { OTLPTraceExporter } = require("@opentelemetry/exporter-trace-otlp-grpc");
const { diag, DiagConsoleLogger, DiagLogLevel } = require('@opentelemetry/api');

**import {CoralogixTransactionSampler} from "@coralogix/opentelemetry";**

// Diagnostic Logger, useful for troubleshooting
diag.setLogger(new DiagConsoleLogger(), DiagLogLevel.DEBUG);

// First we need to initialize a Trace Provider, for this example we're using BasicTraceProvider from the
// <https://www.npmjs.com/package/@opentelemetry/sdk-trace-base> package.

const provider = new BasicTracerProvider({
  // If submitting directly to Coralogix, set cx.application.name and cx.subsystem.name as Resource Attributes.
  resource: new Resource({
    [SemanticResourceAttributes.SERVICE_NAME]: 'nodejs-manual-instrumentation',
    'cx.application.name': 'nodejs',
    'cx.subsystem.name': 'manual',
  }),
  **sampler:  new CoralogixTransactionSampler(new AlwaysOnSampler()) // or your original sampler**
  ,
});

// Create an OTLP/protobuf Exporter to send spans directly to Coralogix.
// For production, obviously don't hardcode these values, but rather use an environment variables
const collectorOptions = {
  timeoutMillis: 15000,
  url: '<https://ingress.coralogix.com:443/v1/traces>',
  headers: {
    Authorization: 'Bearer cxtp_<redacted>',
  },
};

const exporter = new OTLPTraceExporter(collectorOptions);

// Configure span processor to send spans to the exporter
provider.addSpanProcessor(new SimpleSpanProcessor(exporter));

// Initialize the OpenTelemetry APIs to use the BasicTracerProvider bindings.

// This registers the tracer provider with the OpenTelemetry API as the global
// tracer provider. This means when you call API methods like
// `opentelemetry.trace.getTracer`, they will use this tracer provider. If you
// do not register a global tracer provider, instrumentation which calls these
// methods will receive no-op implementations.
provider.register();
const tracer = opentelemetry.trace.getTracer('nodejs-manual-instrumentation');

// Create a span. A span must be closed.
const parentSpan = tracer.startSpan('main');
for (let i = 0; i < 2; i += 1) {
  doWork(parentSpan);
}
// Be sure to end the span.
parentSpan.end();

// flush and close the connection.
exporter.shutdown();

function doWork(parent) {
  // Start another span. In this example, the main method already started a
  // span, so that'll be the parent span, and this will be a child span.
  const ctx = opentelemetry.trace.setSpan(opentelemetry.context.active(), parent);
  const span = tracer.startSpan('doWork', undefined, ctx);

  // simulate some random work.
  for (let i = 0; i <= Math.floor(Math.random() * 40000000); i += 1) {
    // empty
  }

  // Set attributes to the span.
  span.setAttribute('SomeKey', 'SomeValue');

  // Annotate our span to capture metadata about our operation
  span.addEvent('invoking doWork');

  span.end();
}
```

Below is a list of the required libraries needed for this example:

```
"dependencies": {
    "@opentelemetry/api": "^1.7.0",
    "@opentelemetry/exporter-trace-otlp-proto": "^0.47.0",
    "@opentelemetry/resources": "^1.20.0",
    "@opentelemetry/sdk-trace-base": "^1.20.0",
    "@coralogix/opentelemetry": "^0.1.2"


```

## Kubernetes Injection Auto Instrumentation

You may also implement [auto-instrumentation using the OpenTelemetry Operator](https://opentelemetry.io/docs/k8s-operator/automatic/).

The OpenTelemetry Operator supports injecting and configuring auto-instrumentation libraries for .NET, Java, Node.js, and Python services.

## Service Flows

For customers with existing functional Node.js OpenTelemetry instrumentation enabling data transmission to Coralogix using [individual auto-instrumentation](#IndividualMethod) or [manual instrumentation](#ManualInstrumentation), this section guides reconfiguring the existing setup to define, report, and monitor Coralogix [Service Flows](https://coralogixstg.wpengine.com/docs/service-flows/).

For new customers or those who haven't initiated Node.js OpenTelemetry instrumentation, follow our [individual auto-instrumentation](#IndividualMethod) or [manual instrumentation](#ManualInstrumentation) instructions above, which include the steps below. The bundled auto-instrumentation method is **not** supported.

### Individual Auto-Instrumentation

Terminal command:

```
npm install --save @coralogix/opentelemetry
```

Add to the code:

```
import {CoralogixTransactionSampler} from "@coralogix/opentelemetry";
```

```
sampler:  new CoralogixTransactionSampler(new AlwaysOnSampler()) // or your original sampler
```

### Manual Instrumentation

Add to the package:

```
"dependencies": {
    "@opentelemetry/api": "^1.7.0",
    "@opentelemetry/exporter-trace-otlp-proto": "^0.47.0",
    "@opentelemetry/resources": "^1.20.0",
    "@opentelemetry/sdk-trace-base": "^1.20.0",
    "@Coralogix/opentelemetry": "^0.1.2"
```

Add to the code:

```
import {CoralogixTransactionSampler} from "@coralogix/opentelemetry";
```

```
sampler:  new CoralogixTransactionSampler(new AlwaysOnSampler()) // or your original sampler
```

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/apm/">Introduction to Application Performance Monitoring</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Contact us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
