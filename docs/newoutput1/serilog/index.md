---
title: "SeriLog"
date: "2023-12-18"
---

Coralogix customers with Microsoft .NET applications using the [Serilog](https://serilog.net/) logging library, can send logs to Coralogix using OpenTelemetry (OTEL).

The following steps guide you through configuring a Serilog logger in a C# .NET application to send logs to Coralogix, either through an OTEL Collector \[**recommended**\] or directly to a Coralogix endpoint.

## Prerequisites

- Coralogix account

- .NET application that uses Serilog logging

- A running OTEL Collector configured to your Coralogix account (recommended)

💡 **OTEL Collector vs Direct to Coralogix**. It is highly recommended to send telemetry via your own OTEL Collector instead of directly to the Coralogix endpoint. An architecture using OTEL Collectors provides the advantages of batching, reliability, and advanced functionality. For instructions on configuring an OTEL Collector to Coralogix, see the [Coralogix OpenTelemetry page](https://coralogixstg.wpengine.com/docs/opentelemetry/).

## Set up a C# .NET application with Serilog

The following console commands create a simple “Hello World” console app and add NuGet packages for Serilog and various Sinks. If you have an existing application using Serilog, you will only need to add the [Serilog.Sinks.OpenTelemetry](https://www.nuget.org/packages/Serilog.Sinks.OpenTelemetry) package.

```powershell
dotnet new console
dotnet run
dotnet add package Serilog
dotnet add package Serilog.Sinks.Console
dotnet add package Serilog.Sinks.OpenTelemetry
```

### Configure Logging to the Serilog OTEL Sink

The [Serilog Sink for OpenTelemetry](https://github.com/serilog/serilog-sinks-opentelemetry) is the key to enabling the .NET application to send logs to an OTEL Collector.

### Programmatic Configuration Approach

In the following example, Serilog is configured to send logs to an OpenTelemetry sink at a specified network endpoint and logs a simple text message. If required, update the OTEL collector endpoint accordingly for your environment.

```csharp
using System;
using Serilog;
using Serilog.Sinks.OpenTelemetry;
using Serilog.Debugging;
internal class Program
{
    static void Main(string[] args)
    {
        Log.Logger = new LoggerConfiguration()
            .WriteTo.OpenTelemetry(endpoint: "http://127.0.0.1:4317")
            .WriteTo.Console()
            .MinimumLevel.Debug()
            .CreateLogger();
        Serilog.Debugging.SelfLog.Enable(Console.Error);
        Log.Information("Hello, world!");
    }
}
```

Alternatively, while an OTEL Collector is the recommended architecture, you may choose to send send telemetry directly to your Coralogix account instead. To do so, adjust the Logger creation statement as follows, specifying additional options for your [Coralogix endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/), and additional headers for your [Coralogix API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/), Application Name, and Subsystem Name. Substitute your values accordingly.

```csharp
        Log.Logger = new LoggerConfiguration()
                     .WriteTo.OpenTelemetry(options => { 
                       options.Endpoint = "https://ingress.coralogixstg.wpengine.com/";
                       options.Headers = new Dictionary<string, string> {
                         {"Authorization", "Bearer <REPLACE_WITH_YOUR_API_KEY>"},
                         {"CX-Application-Name", "serilog-demo"},
                         {"CX-Subsystem-Name", "serilog-demo"},
                       };
                     })
                     .WriteTo.Console()
                     .MinimumLevel.Debug()
                     .CreateLogger();
```

### Declarative Configuration Approach

Serilog can be alternately configured using a configuration file. The following is an **appsettings.json** configuration file configuring an OpenTelemetry sink to send logs to a configured OpenTelemetry Collector agent endpoint. Edit the OpenTelemetry endpoint if required.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "Serilog": {
    "MinimumLevel": {
      "Default": "Debug",
      "Override": {
        "Microsoft": "Error",
        "Microsoft.AspNetCore": "Warning",
        "System": "Error"
      },
      "Using": [ "Serilog.Sinks.Console", "Serilog.Sinks.OpenTelemetry"]
    },
    "WriteTo": [
      {
        "Name": "Console"
      },
      {
        "Name": "OpenTelemetry",
        "Args": { "endpoint": "http://127.0.0.1:4317/" }
      }
    ]
  }
}
```

The OpenTelemetry collector agent should be network-accessible, and configured to export telemetry to your Coralogix account.

💡 Declarative configuration currently requires an OTEL collector. Currently, Serilog does not support sending OTEL headers required by the Coralogix endpoint. If you wish to use the latter option, please use the programmatic configuration approach.

## Verify logging

Run the application.

```csharp
dotnet run
```

Verify logs appear correctly at the Coralogix web console.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
