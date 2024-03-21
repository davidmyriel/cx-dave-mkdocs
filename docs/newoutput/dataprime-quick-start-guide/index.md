---
title: "DataPrime Quick-Start Guide"
date: "2022-05-25"
---

## **Namespaces**

**The user-data (JSON):**

```
$d / $data
```

**Engine-related event metadata**. Ex - "timestamp", "severity", "logid", "priorityclass":

```
$m / $metadata
```

**User-managed event labels.** Flat, key/values (strings only) Known labels: "applicationname", "subsystemname", "category", "classname", "computername", "methodname", "threadid", "ipaddress":

```
$l / $labels
```

### **Example keypath**s

```
$d.kubernetes.pod_name
```

```
$l.applicationName
```

### **Example expressions**

Refer to the key `key` inside the key `stats` and apply lowercase function to it:

```
$d.stats.key.toLowerCase()
```

The result of multiplying the value of 8 and the `radius` key casted to number (does not work now will be fixed soon):

```
$d.radius:num * 8
```

The logical timestamp of the event (any keypath is valid expression):

```
$m.timestamp
```

## **Operators syntax**

**Filter data matching expression-predicate:**

```
filter <expression>
```

- Ex:

```
filter $d.k8s.pod_name == 'pod1'
```

**Find entries containing search-string:**

```
wildfind '<search-string>’
```

- Ex:

```
wildfind 'foo'
```

**Find entries matching lucene-query:**

```
lucene '<lucene-query>’
```

- Ex:

```
lucene 'hello -world'
```

**Find entries containing search-string in given keypath:**

```
find '<search-string>' in <key-path>
```

- Ex:

```
find 'west' in $d.kubernetes.labels.CX_REGION
```

**\*\*\*\*Order entries by given expression:**

```
order by <expression> 
```

- Ex:

```
order by $d.priority * -1
```

**Take first N entries:**

```
limit <N>
```

- Ex:

```
limit 10
```

**Leave only the keypaths provided, discarding all other keys from an entry:**

```
choose <keypath>, <keypath> …, <keypath>
```

**Cast any expression to one of the following types \[bool, num, string\]:**

```
: (cast)
```

- Ex:

```
filter $d.x:num > 3
```

**Extract parts of one keypath into new keypath using extractor-function:**

```
extract <keypath> into <keypath> using <extractor-function>
```

- Ex:
    - \*\*_Creates field_ `"y"`_of shape:_ `{"name" : "foo" , "id" : "42"}` given `x:"Name:foo Id:42”`

```
extract $d.x into $d.y using regexp(e=/Name:(?<name>[\\w\\s]+) Id:(?<id>\\d+)/) 
```

- Ex:
    - \*\*_Creates field_ `"y"`_of shape:_ `{"a" : "42", "b" : "11"}` given `x: "a=42 b=11"`

```
extract $d.x into $d.y using kv(pair_delimiter=' ', key_delimiter='=') 
```

- Ex:
    - \*\*_Creates field_ `"y"` _of shape:_ `{"a": 1, "b": true}` given `x:"{\\"a\\": 1, \\"b\\": true}"` (stringified json object)

```
extract $d.x into $d.y using jsonobject()
```

## **Example queries**

**Select the 10 ‘successful’ logs ordered by department\_id:**

```
source logs
| find 'success' in $d.result
| order by $d.department_id
| limit 10

```

**Find cx-cluster logs (without knowing the log structure):**

```
source logs | wildfind 'cx-cluster'

```

**Select 100 log messages along with 'processed’ statuses from ‘enrichment-ingest’ service where processed ≠ 0:**

```
source logs
| lucene 'NOT log:"stderr F"'
| lucene 'log:"stdout F"'
| filter $d.kubernetes.labels.CX_SERVICE_NAME != 'enrichment-ingest'
| extract $d.log into $d.stats using regexp(e=/.*T?(?<processed>\\d+:\\d+:\\d+[.,]\\d+).*/)
| filter $d.stats.processed != '0'
| limit 100

```

**\[NEW\] DataPrime** now supports Data Aggregation, for more information and examples please refer to the [DataPrime Cheat Sheet](https://coralogixstg.wpengine.com/docs/dataprime-cheat-sheet/).
