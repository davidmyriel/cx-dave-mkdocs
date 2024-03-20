---
title: "Python OpenTelemetry Instrumentation"
date: "2023-01-18"
---

This tutorial shows simple examples of instrumenting Python applications for metrics and traces using OpenTelemetry and sending them to Coralogix. While the examples utilize a Flask web framework, it is also possible to instrument using other frameworks such as [Tornado](https://www.tornadoweb.org/en/stable/), [Django](https://www.djangoproject.com/), and [more](https://opentelemetry.io/ecosystem/registry/?language=python&component=instrumentation).

## Overview

OpenTelemetry supports [auto](https://opentelemetry.io/docs/instrumentation/python/automatic/), [programmatic](https://opentelemetry.io/docs/languages/python/automatic/example/#programmatically-instrumented-server), and [manual](https://opentelemetry.io/docs/instrumentation/python/manual/) instrumentation for Python applications. While auto instrumentation is the easiest way to instrument an application for traces, it significantly reduces your level of control. For instance, it denies you the ability to explicitly define relationships between functions or effectively propagate context to other applications.

This guide provides examples of **auto**, **programmatic**, and **manual** instrumentation.

## Prerequisites

- Your Coralogix [Send-Your-Data API Key](http://www.coralogixstg.wpengine.com/docs/send-your-data-api-key) and [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/)

- Python 3.9+

### Initial Setup and Dependencies Installation for Flask Application

Prerequisite steps for this integration include setting up a Python virtual environment, creating a Python file, and installing the required packages for a Flask application.

**STEP 1**. Set up a Python virtual environment.

```
# create python virtual environment
python -m venv env

# activate python environment
source env/bin/active

# create app.py
touch app.py

```

**STEP 2**. Add a requirements.txt file and install dependencies.

```
flask==2.3.8
opentelemetry-distro
opentelemetry-exporter-otlp
opentelemetry-instrumentation
opentelemetry-instrumentation-flask
coralogix-opentelemetry==0.1.3
Werkzeug==2.3.8

```

```
pip install -r requirements.txt

```

**Notes**:

- Werkzeug is pinned to version 2.3.8, given **[this issue](https://github.com/open-telemetry/opentelemetry-python-contrib/issues/2058)**.

## **Auto Instrumentation**

Auto instrumentation works by running your application within another wrapper application provided by OpenTelemetry. This wrapper handles collecting traces from all compatible parts of your application.

### **Traces**

Example with a simple Flask app ‘[app.py](http://app.py/)’.

```
from random import randint
from flask import Flask

app = Flask(__name__)

@app.route("/roll")
def roll_dice():
    return str(do_roll())

def do_roll():
    res = randint(1, 6)
    return f"you've rolled {res}"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5001)

```

**STEP 1.** You can now run the application using the `opentelemetry-instrument` wrapper. Before proceeding, configure the auto instrumentation tool. The wrapper is configured using either CLI arguments or environment variables.

The following example uses environment variables.

```
# set exporter headers. These are the headers used when calling the Coralogix Endpoint
export OTEL_EXPORTER_OTLP_HEADERS='Authorization=Bearer%20[your-private-key], CX-Application-Name=dev-traces, CX-Subsystem-Name=dev-traces'

# set the coralogix endpoint for traces
export OTEL_EXPORTER_OTLP_ENDPOINT='ingress.<cx_domain>:443'

# set the exporter protocol, in this case we're using grpc
export OTEL_TRACES_EXPORTER=otlp_proto_grpc

```

**Notes**:

- The values of the environment variable OTEL\_EXPORTER\_OTLP\_HEADERS must be URL encoded. The example uses the value %20 to represent a space between the bearer and the token.

- Select the Coralogix [OpenTelemetry endpoint](https://coralogix.com/docs/coralogix-endpoints/#opentelemetry) associated with your Coralogix [domain](https://coralogix.com/docs/coralogix-domain/).

- View [this page](https://opentelemetry.io/docs/concepts/sdk-configuration/otlp-exporter-configuration/) for a list of all the environment variable options for configuring the `opentelemetry-instrument` tool.

**STEP 2**. Once you’ve set your environment variables, run the application.

```
opentelemetry-instrument \\
    --traces_exporter console,otlp \\ # export traces to console and otel protocol
    --metrics_exporter none \\ # disable metrics
    --service_name py-auto-instr-test \\ # service name is required
    python app.py

```

The application will be served on port `5001`. A trace can be triggered by calling the endpoint `localhost:5001/roll`.

### Metrics

To add metrics, add `otlp` as an argument to the `--metrics_exporter` flag.

```
opentelemetry-instrument \\
    --traces_exporter console,otlp \\
    --metrics_exporter console,otlp \\ # send metrics to console and otel exporters
    --service_name py-auto-instr-test \\
    python app.py

```

**Notes**:

- The metrics collected will depend on supporting the underlying framework being instrumented. View [this page](https://opentelemetry.io/ecosystem/registry/?language=python&component=instrumentation) for a list of all supported frameworks.

- Forwarding metrics to Coralogix is required for leveraging Coralogix platform functionalities that depend on service applicative metrics from OpenTelemetry (OTEL). However, it is **not** required for utilizing Coralogix [APM features](https://coralogix.com/docs/apm/).

## **Programatic Instrumentation**

### Traces

**STEP 1.**  Create a file `app.py` with the following content:

```
from os import environ
import random

from flask import Flask

# opentelemetry imports
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.sdk.resources import Resource, SERVICE_NAME
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import SimpleSpanProcessor
from opentelemetry.sdk.trace.sampling import StaticSampler, Decision
from opentelemetry import trace

# coralogix sampler
from coralogix_opentelemetry.trace.samplers import CoralogixTransactionSampler

```

**STEP 2.** Define the headers necessary for calling the Coralogix endpoint and use them to define an exporter and tracer. An exporter is the mechanism responsible for exporting/sending traces during runtime.

**Notes**:

- CX\_ENDPOINT: Select the Coralogix [OpenTelemetry endpoint](https://coralogix.com/docs/coralogix-endpoints/#opentelemetry) associated with your Coralogix [domain](https://coralogix.com/docs/coralogix-domain/).

- The `headers` variable is a `string` and not a `dictionary`.

```
headers = ', '.join([
    f'Authorization=Bearer%20{environ.get("CX_TOKEN")}',
    'CX-Application-Name=my-instrumented-app',
    'CX-Subsystem-Name=my-instrumented-app'
])

tracer_provider = TracerProvider(
    resource=Resource.create({
        SERVICE_NAME: 'my-instrumented-app'
    }),
    sampler=CoralogixTransactionSampler(StaticSampler(Decision.RECORD_AND_SAMPLE))
)

# set up an OTLP exporter to send spans to coralogix directly.
exporter = OTLPSpanExporter(
    endpoint=environ.get('CX_ENDPOINT'),
    headers=headers
)

# set up a span processor to send spans to the exporter
span_processor = SimpleSpanProcessor(exporter)
# span_processor = ConsoleSpanExporter(exporter)

# add the span processor to the tracer provider
tracer_provider.add_span_processor(span_processor)
trace.set_tracer_provider(tracer_provider)

instrumentor = FlaskInstrumentor()
tracer = trace.get_tracer_provider().get_tracer(__name__)

```

**STEP 3.** Define the Flask application and instrument the functions.

```
# initialise flask app
app = Flask(__name__)
instrumentor.instrument_app(app) # Instrument the flask app

@app.route("/roll")
def roll():
    res = dice()
    span = trace.get_current_span()
    span.set_attribute("roll.value", res) # Set custom tags
    return f"you rolled {res}\\n"

# example of adding a span to a function using a decorator with the tracer we defined in step 2
@tracer.start_as_current_span("diceFunc")
def dice():
    return random.randint(1, 6)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5001)

```

The code above has 2 functions: `roll` and `dice`. Select one depending on the specifics of your use case and codebase.

| Function | Description | Instrumentation |
| --- | --- | --- |
| roll | The HTTP handle for the /roll path | Instrumented by defining a trace within the function body. |
| dice | A simple function that returns a number between 1 and 6 | Instrumented using a decorator |

**STEP 4.** To run the app, set the required environment variables:

- CX\_TOKEN

- CX\_ENDPOINT

**Notes**:

- When using a Python virtual environment, these environment variables must be set inside that environment.

Run the dev server:

```
python app.py

```

To produce a trace, go to: [http://localhost:5001/roll](http://localhost:5001/roll).

**STEP 5**. Confirm that your traces are being sent to Coralogix. From your Coralogix toolbar, navigate to **Explore** > **Tracing.** You should now see the traces generated by your application.

### **Metrics**

Forwarding metrics to Coralogix is required for leveraging Coralogix platform functionalities that depend on service applicative metrics from OpenTelemetry (OTEL). However, it is **not** required for utilizing Coralogix [APM features](https://coralogix.com/docs/apm/).

**STEP 1.** Define the metrics exporter and metrics provider.

```
from opentelemetry import metrics
from opentelemetry.sdk.metrics import MeterProvider
from opentelemetry.sdk.metrics.export import (
    PeriodicExportingMetricReader,
)

from opentelemetry.exporter.otlp.proto.grpc.metric_exporter import OTLPMetricExporter
from os import environ

# define headers. Single string with comma separated key-value pairs
# Authorization, CX-Application-Name and CX-Subsystem-Name are mandatory
# when calling the coralogix endpoint directly.
headers = ', '.join([
    f'Authorization=Bearer%20{environ.get("CX_TOKEN")}',
    "CX-Application-Name=manual-instro-traces",
    "CX-Subsystem-Name=manual-instro-traces",
])

# set up an OTLP exporter to send spans to coralogix directly.
exporter = OTLPMetricExporter(
    endpoint=environ.get('CX_ENDPOINT'),
    headers=headers,
)

metric_reader = PeriodicExportingMetricReader(exporter)
provider = MeterProvider(metric_readers=[metric_reader])

# Sets the global default meter provider
metrics.set_meter_provider(provider)

# Creates a meter from the global meter provider
meter = metrics.get_meter("my.meter.name")

```

Above, the `meter` created a counter metric called `http.request.counter`. The counter counts the number of requests made to the `/roll` endpoint.

View [this page](https://opentelemetry.io/docs/specs/otel/metrics/api/#meter) for more information on the `meter` and a list of other metric types that can be defined.

**STEP 3.** Add the application logic and instrument the `/roll` endpoint.

```
from random import randint
from flask import Flask

app = Flask(__name__)

@app.route("/roll")
def roll_dice():
    # increment the counter
    http_req_counter.add(1, {"request.path": '/roll'})
    return str(do_roll())

def do_roll():
    res = randint(1, 6)
    return f"you've rolled {res}"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5001)

```

Note that counter is incremented each time the `/roll` endpoint is called.

**STEP 4.** Execute.

To run the app as before, set the required environment variables:

- CX\_TOKEN

- CX\_ENDPOINT

```
python app.py
```

**STEP 5**. Confirm that your metrics are being sent to Coralogix. From your Coralogix toolbar, navigate to **Grafana** > **Explore** > **Metric Browser**. Search for the name of your metric.

## Manual Instrumentation

Implement the same example above using manual instrumentation.

### **Traces**

**STEP 1.** Create a file `app.py` and import the required packages.

```
from os import environ
from random import randint

from flask import Flask

# opentelemetry imports
from opentelemetry import trace
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.resources import Resource, SERVICE_NAME
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import SimpleSpanProcessor
from opentelemetry.sdk.trace.sampling import StaticSampler, Decision

# coralogix sampler 
from coralogix_opentelemetry.trace.samplers import CoralogixTransactionSampler

```

**STEP 2.** Define the headers necessary for calling the Coralogix endpoint and use them to define an exporter and tracer. An exporter is the mechanism responsible for exporting / sending traces during runtime.

**Notes**:

- CX\_ENDPOINT: Select the Coralogix [OpenTelemetry endpoint](https://coralogix.com/docs/coralogix-endpoints/#opentelemetry) associated with your Coralogix [domain](https://coralogix.com/docs/coralogix-domain/).

- The `headers` variable is a `string` and not a `dictionary`.

```
# define headers. Single string with comma separated key-value pairs
# Authorization, CX-Application-Name and CX-Subsystem-Name are mandatory when calling the coralogix endpoint directly.
headers = ', '.join([
    f'Authorization=Bearer%20{environ.get("CX_TOKEN")}',
    "CX-Application-Name=<APPLICATION NAME>",
    "CX-Subsystem-Name=<SUBSYSTEM NAME>",
])


# create a tracer provider
tracer_provider = TracerProvider(
    resource=Resource.create({
        SERVICE_NAME: '<SERVICE NAME>'
    }),
    sampler=CoralogixTransactionSampler(StaticSampler(Decision.RECORD_AND_SAMPLE)))

# set up an OTLP exporter to send spans to coralogix directly.
exporter = OTLPSpanExporter(
    endpoint=environ.get('CX_ENDPOINT'),
    headers=headers,
)

# set up a span processor to send spans to the exporter
span_processor = SimpleSpanProcessor(exporter)
# span_processor = ConsoleSpanExporter(exporter)

# add the span processor to the tracer provider
tracer_provider.add_span_processor(span_processor)

# set trace provider
trace.set_tracer_provider(tracer_provider)

# create a tracer
tracer = trace.get_tracer_provider().get_tracer(__name__)
```

```
# initialise flask app
app = Flask(__name__)

# example of adding a span to a function using a context manager
# this is useful when you want to dynamic attribuf"http://{os.getenv('OTEL_COLLECTOR_SERVICE_HOST')}:4318"tes to the span
@app.route("/roll")
def roll():
    # create a span
    with tracer.start_as_current_span("roll_v2") as span:
        span.set_attribute("http.method", "GET")
        span.set_attribute("http.host", "localhost")
        span.set_attribute("http.url", "/v2/rolldice")
        res = dice()
        span.set_attribute("roll.value", res)
        return f"you rolled {res}\\n"

# example of adding a span to a function using a decorator
@tracer.start_as_current_span("diceFunc")
def dice():
    return randint(1, 6)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5001)

```

The code above has 2 functions: `roll` and `dice`. Select one depending on the specifics of your use-case and codebase.

| Function | Description | Instrumentation |
| --- | --- | --- |
| roll | The HTTP handle for the /roll path | Instrumented by defining a trace within the function body. |
| dice | A simple function that returns a number between 1 and 6 | Instrumented using a decorator |

**STEP 4.** To run the app, set the required environment variables:

- CX\_TOKEN

- CX\_ENDPOINT

**Notes**:

- When using a Python virtual environment, these environment variables must be set inside that environment.

```
python app.py
```

**STEP 5**. Confirm that your traces are being sent to Coralogix. From your Coralogix toolbar, navigate to **Explore** > **Tracing.** You should now see the traces generated by your application.

### **Metrics**

Like traces, metrics can also be instrumented manually. Choose this option if there are specific metrics that you wish to extract from your application runtime context.

**Notes**:

- The dependencies are the same as those utilized in the `requirements.txt` above for traces.

- Forwarding metrics to Coralogix is required for leveraging Coralogix platform functionalities that depend on service applicative metrics from OpenTelemetry (OTEL). However, it is **not** required for utilizing Coralogix [APM features](https://coralogix.com/docs/apm/).

**STEP 1.** Define the metrics exporter and metrics provider.

```
from opentelemetry import metrics
from opentelemetry.sdk.metrics import MeterProvider
from opentelemetry.sdk.metrics.export import (
    PeriodicExportingMetricReader,
)

from opentelemetry.exporter.otlp.proto.grpc.metric_exporter import OTLPMetricExporter
from os import environ

# define headers. Single string with comma separated key-value pairs
# Authorization, CX-Application-Name and CX-Subsystem-Name are mandatory
# when calling the coralogix endpoint directly.
headers = ', '.join([
    f'Authorization=Bearer%20{environ.get("CX_TOKEN")}',
    "CX-Application-Name=manual-instro-traces",
    "CX-Subsystem-Name=manual-instro-traces",
])

# set up an OTLP exporter to send spans to coralogix directly.
exporter = OTLPMetricExporter(
    endpoint=environ.get('CX_ENDPOINT'),
    headers=headers,
)

metric_reader = PeriodicExportingMetricReader(exporter)
provider = MeterProvider(metric_readers=[metric_reader])

# Sets the global default meter provider
metrics.set_meter_provider(provider)

# Creates a meter from the global meter provider
meter = metrics.get_meter("my.meter.name")

```

Above, the `meter` created a counter metric called `http.request.counter`. The counter counts the number of requests made to the `/roll` endpoint.

View [this page](https://opentelemetry.io/docs/specs/otel/metrics/api/#meter) for more information on the `meter` and a list of other metric types that can be defined.

**STEP 3.** Add the application logic and instrument the `/roll` endpoint.

```
from random import randint
from flask import Flask

app = Flask(__name__)

@app.route("/roll")
def roll_dice():
    # increment the counter
    http_req_counter.add(1, {"request.path": '/roll'})
    return str(do_roll())

def do_roll():
    res = randint(1, 6)
    return f"you've rolled {res}"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5001)

```

Note that counter is incremented each time the `/roll` endpoint is called.

**STEP 4.** Execute.

To run the app as before, set the required environment variables:

- CX\_TOKEN

- CX\_ENDPOINT

```
python app.py
```

**STEP 5**. Confirm that your metrics are being sent to Coralogix. From your Coralogix toolbar, navigate to **Grafana** > **Explore** > **Metric Browser**. Search for the name of your metric.

## Service Flows

For customers with functional Python OpenTelemetry instrumentation that was [configured manually](#ManualInstrumentation), this section guides reconfiguring the existing setup to define, report, and monitor Coralogix [Service Flows](https://coralogixstg.wpengine.com/docs/service-flows/).

For new customers or those who haven't configured the Python OpenTelemetry instrumentation manually, you are **required** to follow the [Manual Instrumentation](#ManualInstrumentation) instructions above. The steps in this section are included in those instructions.

Add packages to the requirements.txt file:

```
coralogix-opentelemetry==0.1.3

```

And then run:

```
pip install -r requirements.txt

```

Add to the code:

```
from coralogix_opentelemetry.trace.samplers import CoralogixTransactionSampler
# create a tracer provider
tracer_provider = TracerProvider(sampler=CoralogixTransactionSampler(StaticSampler(Decision.RECORD_AND_SAMPLE)))

```

## Additional Resources

For more information on Python OpenTelemetry instrumentation, view the official [OpenTelemetry documentation](https://opentelemetry.io/docs/instrumentation/python/getting-started/).

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Contact us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
