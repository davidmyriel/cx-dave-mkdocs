---
title: "Fluent Bit"
date: "2021-08-07"
---

Coralogix provides seamless integration with Fluent Bit, allowing you to send your logs from anywhere and parse them according to your needs.

Coralogix supports Fluent Bit **v2.0.7** and onwards.

## Prerequisites

- Fluent Bit [installed](https://docs.fluentbit.io/manual/installation/getting-started-with-fluent-bit)

## Environment Variables

Unless you want to hardcode your values into their appropriate positions, you must export the following four environment variables when deploying the Fluent Bit integration.

| Variable | Description |
| --- | --- |
| Api\_Key | Your [**Send-Your-Data API key**](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) is a unique ID that represents your Coralogix team. Input the key without quotation marks or apostrophes. |
| Application\_Name | The name of your **[application](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/)**, as will appear in your Coralogix dashboard. For example, a company named SuperData might insert the SuperData string parameter. If SuperData wants to debug its test environment, it might use SuperData–Test. |
| SubSystem\_Name | The name of your [**subsystem**](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/), as will appear in your Coralogix dashboard. Applications often have multiple subsystems (ie. Backend Servers, Middleware, Frontend Servers, etc.). In order to help you examine the data you need, inserting the subsystem parameter is vital. |
| Endpoint | Find the [**Coralogix REST API Singles endpoint**](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/#coralogix-rest-api-singles) corresponding to your Coralogix [**domain**](https://coralogixstg.wpengine.com/docs/coralogix-domain/).  
Enter only the FQDN portion.  
Example: **ingress.coralogixstg.wpengine.com** |

## **Configuration**

Open your existing `Fluent-Bit` configuration file and add the following:

```
[FILTER]
        Name            nest
        Match           *
        Operation       nest
        Wildcard        *
        Nest_under      text
[FILTER]
        Name            modify
        Match           *
        Add             applicationName ${Application_Name}
        Add             subsystemName ${SubSystem_Name}
        Add             computerName ${HOSTNAME}
[OUTPUT]
        Name            http
        Match           *
        Host            ${Endpoint}
        Port            443
        URI             /logs/v1/singles
        Format          json_lines
        TLS             On
        Header          Authorization Bearer ${Api_Key}
        compress        gzip
        Retry_Limit     10

```

### **Application and Subsystem Name**

If you wish to set your [application and subsystem names](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as fixed values, use Application\_Name and SubSystem\_Name as described above in your configuration file.

If your input stream is a `JSON` object, you can extract an Application\_Name and/or SubSystem\_Name from the `JSON` by adding our LUA filter just before the OUTPUT:

```
[FILTER]
  Name    lua
  Match   *
  call set_cx_keys
  code function set_cx_keys(tag, timestamp, record) new_record = record	new_record["applicationName"] = record["<your_app_key>"] new_record["subsystemName"] = record["parent_key"]["<your_subsystem_key>"] return 2, timestamp, new_record end

```

For instance, in the following example, `JSON` `new_record["applicationName"] = record["application"]` will extract "testApp" into the Coralogix applicationName.

```
{
    "application": "testApp",
    "subsystem": "testSub",
    "code": "200",
    "stream": "stdout",
    "timestamp": "2016-07-20T17:05:17.743Z",
    "message": "hello_world",
}

```

**Notes**:

- Nested JSONs are supported.

- Extract nested values as your `applicationName` and/or `subsystemName`.

### Example

The following is an example configuration.

```
[SERVICE]
    Flush           1
    Daemon          Off
    Log_Level       info
    Parsers_File    parsers.conf

[INPUT]
    Name            dummy
    Tag             dummy_input
    Rate            1

[OUTPUT]
    Name            stdout
    Match           *

[FILTER]
    Name            nest
    Match           *
    Operation       nest
    Wildcard        *
    Nest_under      text

[FILTER]
    Name            modify
    Match           *
    Add             computerName ${HOSTNAME}

[FILTER]
    Name            lua
    Match           *
    call            applicationNameFromEnv
    code            function applicationNameFromEnv(tag, timestamp, record) record["applicationName"] = record["metadata"]["computerName"] or os.getenv("Application_Name") return 2, timestamp, record end

[FILTER]
    Name            lua
    Match           *
    call            subsystemNameFromEnv
    code            function subsystemNameFromEnv(tag, timestamp, record) record["subsystemName"] = tag or os.getenv("SubSystem_Name") return 2, timestamp, record end

[OUTPUT]
    Name            http
    Match           *
    Host            ${ENDPOINT}
    Port            443
    URI             /logs/v1/singles
    Format          json_lines
    TLS             On
    Header          Authorization Bearer ${Api_Key}
    compress        gzip
    Retry_Limit     10

```

## Additional Resources

| Documentation | [**GitHub**](https://github.com/coralogix/telemetry-shippers/tree/fluent-bit-standalone/logs/fluent-bit/standalone) |
| --- | --- |

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
