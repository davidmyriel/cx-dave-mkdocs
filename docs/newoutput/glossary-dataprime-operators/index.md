---
title: "DataPrime Glossary: Operators & Expressions"
date: "2023-05-11"
---

This guide provides a **full glossary** of all available DataPrime operators and expressions.

To hit the ground running using DataPrime and to view only our most frequently-used operators with examples, view our [DataPrime Cheat Sheet](https://coralogixstg.wpengine.com/docs/dataprime-cheat-sheet/).

## Operators

### block

The negation of `filter`. Filters-out all events where the condition is `true`. The same effect can be achieved by using `filter` with `!(condition)`.

```
block $d.status_code >= 200 && $d.status_code <= 299         # Leave all events which don't have a status code of 2xx
```

The data is exposed using the following top-level fields:

- `$m` -Event metadata
    - `timestamp`
    
    - `severity` - Possible values are `V`ERBOSE, `D`EBUG, `I`NFO, `W`ARNING, `E`RROR, `C`RITICAL
    
    - `priorityclass` - Possible values are `high`, `medium`, `low`
    
    - `logid`

- `$l` -Event labels
    - `applicationname`
    
    - `subsystemname`
    
    - `category`
    
    - `classname`
    
    - `computername`
    
    - `methodname`
    
    - `threadid`
    
    - `ipaddress`

- `$d` -The user's data

### bottom

**No grouping variation**

Limits the rows returned to a specified number and order the result by a set of expressions.

```
order_direction := "descending"/"ascending" according to top/bottom

bottom <limit> <result_expression1> [as <alias>] [, <result_expression2> [as <alias2>], ...] by <orderby_expression> [as alias>]
```

For example, the following query:

```
bottom 5 $m.severity as $d.log_severity by $d.duration
```

Will result in logs of the following form:

```
[
   { "log_severity": "Debug", "duration":  1000 }
   { "log_severity": "Warning", "duration": 2000 },
   ...
]
```

**Grouping variation**

Limits the rows returned to a specified number and group them by a set of aggregation expressions and order them by a set of expressions.

```
order_direction := "descending"/"ascending" according to top/bottom

bottom <limit> <(groupby_expression1|aggregate_function1)> [as <alias>] [, <(groupby_expression2|aggregate_function2)> [as <alias2>], ...] by <(groupby_expression1|aggregate_function1)> [as <alias>]
```

For example, the following query:

```
bottom 10 $m.severity, count() as $d.number_of_severities by avg($d.duration) as $d.avg_duration
```

Will result in logs of the following form:

```
[
   { "severity": "Warning", "number_of_severities": 50, avg_duration: 1000 },
   { "severity": "Debug", "number_of_severities":  10, avg_duration: 2000 }
   ...
]
```

Supported aggregation functions are listed in "Aggregation Functions" section.

### choose

Leave only the keypaths provided, discarding all other keys. Fully supports nested keypaths in the output.

```
(choose|select) <keypath1> [as <new_keypath>],<keypath2> [as <new_keypath>],...
```

Examples:

```
choose $d.mysuperkey.myfield
choose $d.my_superkey.mykey as $d.important_value, 10 as $d.the_value_ten
```

### convert

Convert the data types of keys.

The `datatypes` keyword is optional and can be used for readability.

```
(conv|convert) [datatypes] <keypath1>:<datatype1>,<keypath2>:<datatype2>,...
```

Examples:

```
convert $d.level:number
conv datatypes $d.long:number,$d.lat:number
convert $d.data.color:number,$d.item:string
```

### count

Returns a single row containing the number of rows produced by the preceding operators.

```
count [into <keypath>]
```

An alias can be provided to override the keypath the result will be written to.

For example, the following part of a query

```
count into $d.num_rows
```

will result in a single row of the following form:

```
{ "num_rows": 7532 }
```

### countby

Returns a row counting all the rows grouped by the expression.

```
countby <expression> [as <alias>] [into <keypath>]
```

An alias can be provided to override the keypath the result will be written into.

For example, the following part of a query

```
countby $d.verb into $d.verb_count
```

will result in a row for each group.

It is functionally identical to

```
groupby $data.verb calculate count() as $d.verb_count
```

### create

Create a new key and set its value to the result of the expression. Key creation is granular, meaning that parent keys in the path are not overwritten.

```
  (a|add|c|create) <keypath> from <expression> [on keypath exists (fail|skip|overwrite)] [on keypath missing (fail|create|skip)] [on datatype change (skip|fail|overwrite)
```

The creation can be controlled by adding the following clauses:

- Adding `on keypath exists` allows to choose what to do when the keypath already exists.

- `overwrite` - Overwrites the old value. This is the default value

- `fail` - Fails the query

- `skip` - Skips the creation of the key

- Adding `on keypath missing` allows to choose what to do when the new keypath does not exist.

- `create` - Creates the key. This is the default value

- `fail` - Fails the query

- `skip` - Skips the creation of the new key

- Adding `on datatype changed` allows to choose what to do if the key already exists and the new data changes the datatype of the value

- `overwrite` - Overwrites the value anyway. This is the default value

- `fail` - Fails the query

- `skip` - Leaves the key with the original value (and type)

Examples:

```
create $d.radius from 100+23
c $d.log_data.truncated_message from $d.message.substring(1,50)
c $data.trimmed_name from $data.username.trim()

create $d.temperature from 100*23 on datatype changed skip
```

### distinct

Returns one row for each distinct combination of the provided expressions.

```
distinct <expression> [as <alias>] [, <expression_2> [as <alias_2>], ...]
```

This operator is functionally identical to `groupby` without any aggregate functions.

### enrich

Enrich your logs using additional context from a lookup table.

Upload your lookup table using the **Data Flow** > **Data Enrichment** > **Custom Enrichment** section.  
For more details, see [Custom Enrichment](https://coralogixstg.wpengine.com/docs/custom-log-enrichment/) documentation.

```
enrich <value_to_lookup> into <enriched_key> using <lookup_table>
```

- `value_to_lookup` - A string expression that will be looked up in the lookup table.

- `enriched_key` - Destination key to store the enrichment result in.

- `lookup_table` - The name of the Custom Enrichment table to be used.

The table's columns will be added as sub-keys to the destination key. If `value_to_lookup` is not found, the destination key will be `null`.  
You can then filter the results using the DataPrime capabilities, such as filtering logs by specific value in the enriched field.

**Example:**

The original log:

```
{
    "userid": "111",
    ...
}
```

The Custom Enrichment lookup table called `my_users`:

| ID | Name | Department |
| --- | --- | --- |
| 111 | John | Finance |
| 222 | Emily | IT |

Running the following query:

```
enrich $d.userid into $d.user_enriched using my_users
```

Gives the following enriched log:

```
{
    "userid": "111",
    "user_enriched": {
        "ID": "111",
        "Name": "John",
        "Department": "Finance"
    },
    ...
}
```

**Notes**:

- Run the DataPrime query `source <lookup_table>` to view the enrichment table.

- If the original log already contains the enriched key:
    - If `<value_to_lookup>` exists in the `<lookup_table>`, the sub-keys will be updated with the new value. If the `<value_to_lookup>` does not exist, their current value will remain.
    
    - Any other sub-keys which are not columns in the `<lookup_table>` will remain with their existing values.

- All values in the `<lookup_table>` are considered to be strings. This means that:
    - The `<value_to_lookup>` must be in a string format.
    
    - All values are enriched in a string format. You may then convert them to your preferred format (e.g. JSON, timestamp) using the appropriate functions.

For more information, see the [enrich section](https://coralogixstg.wpengine.com/docs/glossary-dataprime-operators/#enrich) in the DataPrime Glossary.

### extract

Extract data from some string value into a new object. Multiple extraction methods are supported.

```
(e|extract) <expression> into <keypath> using <extraction-type>(<extraction-params>) [datatypes keypath:datatype,keypath:datatype,...]
```

Here are the currently supported extraction methods, and their parameters:

- `regexp` - Create a new object based on regexp capture-groups

- `e` - A regular expression with names capture-groups.

Example:

```
extract $d.my_text into $d.my_data using regexp(e=/user (?<user>.*) has logged in/)
```

- `kv` - Extract a new object from a string that contains `key=value key=value...` pairs

- `pair_delimiter` - The delimiter to expect between pairs. Default is (a space)

- `key_delimiter` - The delimiter to expect separating between a key and a value. Default is `=`.

Examples:

```
extract $d.text into $d.my_kvs using kv()
e $d.text into $d.my_kvs using kv(pair_delimiter=' ',key_delimiter='=')
```

- `jsonobject` - Extract a new object from a string contains an encoded json object, potentially attempting to unescape the string before decoding it into a json

- `max_unescape_count` - Max number of escaping levels to unescape before parsing the json. Default is 1. When set to 1 or more, the engine will detect whether the value contains an escaped JSON string and unescape it until its parsable or max unescape count ie exceeded.

Example:

```
e $d.json_message_as_str into $d.json_message using jsonobject(max_unescape_count=1)
```

Additional extraction methods will be supported in the future.

It is possible to provide datatype information as part of the extraction, by using the `datatypes` clause. For example, adding `datatypes my_field:number` to an extraction would cause the extract `my_field` keypath to be a number instead of a string. For example:

```
extract $d.my_msg into $d.data using kv() datatypes my_field:number
```

Extracted data always goes into a new keypath as an object, allowing further processing of the new keys inside that new object. For example:

```
# Assuming a dataset which look like that:
{ "msg": "query_type=fetch query_id=100 query_results_duration_ms=232" }
{ "msg": "query_type=fetch query_id=200 query_results_duration_ms=1001" }

# And the following DataPrime query:
source logs
  | extract $d.msg into $d.query_data using kv() datatypes query_results_duration_ms:number
  | filter $d.query_data.query_results_duration_ms > 500

# The results will contain only the second message, in which the duration is larger than 500 ms
```

### filter

Filter events, leaving only events for which the condition evaluates to true.

```
(f|filter|where) <condition-expression>
```

Examples:

```
f $d.radius > 10
filter $m.severity.toUpperCase() == 'INFO'
filter $l.applicationname == 'recommender'
filter $l.applicationname == 'myapp' && $d.msg.contains('failure')
```

Note: Comparison with null currently works only for scalar values and will always return null on json subtrees.

### groupby

Groups the results of the preceding operators by the specified grouping expressions and calculates aggregate functions for every group created.

```
groupby <grouping_expression> [as <alias>] [, <grouping_expression_2> [as <alias_2>], ...] [calculate]
  <aggregate_function> [as <result_keypath>]
  [, <aggregate_function_2> [as <result_keypath_2], ...]
```

For example, the following query:

```
groupby $m.severity calculate sum($d.duration)
```

Will result in logs of the following form:

```
{ "severity": "Warning", "_sum": 17045 }
```

The keypaths for the grouping expressions will always be under `$d`. Using the `as` keyword, we can rename the keypath for the grouping expressions and aggregation functions. The following query:

```
groupby $l.applicationname as $d.app calculate sum($d.duration) as $d.sum_duration
```

Will result in logs of the following form:

```
{ "app": "web-api", "sum_duration": 17045 }
```

**Notes**:

- Supported aggregation functions are listed in "Aggregation Functions" section below.

- When querying with the groupby operator, you can now apply an aggregation function (such as`avg`, `max`, `sum`) to the bucket of results. This feature gives you the power to **manipulate an aggregation expression inside the expression itself**, allowing you to calculate and manipulate your data simultaneously. Examples of DataPrime expressions in aggregations can be found [here](https://coralogixstg.wpengine.com/docs/glossary-dataprime-operators/#dp-expressions-in-aggregations-examples).

### limit

Limits the output to the first `<event-count>` events.

```
limit <event-count>
```

Examples

```
limit 100
```

### move

Move a key (including its child keys, if any) to a new location.

```
(m|move) <source-keypath> to <target-keypath>
```

Examples:

```
move $d.my_data.hostname to $d.my_new_data.host
m $d.kubernetes.labels to $d.my_labels
```

### orderby / sortby / order by / sort by

Sort the data by ascending/descending order of the expression value. Ordering by multiple expressions is supported.

```
(orderby|sortby|order by|sort by) <expression> [(asc|desc)] , ...
```

Examples:

```
orderby $d.myfield.myfield
orderby $d.myfield.myfield:number desc
sortby $d.myfield desc
```

Note: Sorting numeric values can be done by casting expression to the type: e.g.`<expression>: number`. In some cases, this will be inferred automatically by the engine.

### redact

Replace all substrings matching a regexp pattern from some keypath value, effectively hiding the original content.

The matching keyword is optional and can be used to increase readability.

```
redact <keypath> [matching] /<regular-expression>/ to '<redacted_str>'
redact <keypath> [matching] <string> to '<redacted_str>'
```

Examples:

```
redact $d.mykey /[0-9]+/ to 'SOME_INTEGER'
redact $d.mysuperkey.user_id 'root' to 'UNKNOWN_USER'
redact $d.mysuperkey.user_id matchingn 'root' to 'UNKNOWN_USER'
```

### remove

The negation of `choose`. Remove a keypath from the object.

```
r|remove <keypath1> [ "," <keypath2> ]...
```

Examples:

```
r $d.mydata.unneeded_key
remove $d.mysuperkey.service_name, $d.mysuperkey.unneeded_key
```

### replace

Replace the value of some key with a new value.

If the replacement value changes the datatype of the keypath, the following will happen:

- `skip` - The replacement will be ignored

- `fail` - The query will fail

- `overwrite` - The new value will overwrite the previous one, changing the datatype of the keypath

```
replace <keypath> with <expression> [on datatype changed skip/fail/overwrite]
```

Examples:

```
replace $d.message with null
replace $d.some_superkey.log_length_plus_10 with $d.original_log.length()+10 on datatype changed overwrite
```

### roundtime

Rounds the time of the event into some time interval, possibly creating a new key for the result.

If `source-timestamp` is not provided, then `$m.timestamp` is used as the source timestamp.  
If `source-timestamp` is provided, it should be of type (or cast to) `timestamp`.

By default, the rounded result is written back to the source keypath `[source-timestamp]`.  
If `into <target-keypath>` is provided, then `[source-timestamp]` is not modified, and the result is written to a new `target-keypath`.

Supported time intervals are:

- `Xns` - X nanoseconds (beware of the `source-timestamp`'s resolution)

- `Xms` - X milliseconds

- `Xs` - X seconds

- `Xm` - X minutes

- `Xh` - X hours

- `Xd` - X days

And any combination of the above from bigger to smaller time unit, e.g. `1h30m15s`.

```
roundtime [source-timestamp] to <time-interval> [into <target-keypath>]
```

Examples:

```
roundtime to 1h into $d.tm
roundtime $d.timestamp to 1h
roundtime $d.my_timestamp: timestamp to 60m
roundtime to 60s into $d.rounded_ts_to_the_minute
```

### source

Set the data source that your DataPrime query is based on.

```
(source|from) <data_store>
```

Where `<data_store>` can be either:

- `logs`

- `spans` (supported only in the API)

- The name of the custom enrichment. In this case, the command will display the custom enrichment table.

**Examples:**

```
source logs
```

### top

**No grouping variation**

Limits the rows returned to a specified number and order the result by a set of expressions.

```
order_direction := "descending"/"ascending" according to top/bottom

top <limit> <result_expression1> [as <alias>] [, <result_expression2> [as <alias2>], ...] by <orderby_expression> [as alias>]
```

For example, the following query:

```
top 5 $m.severity as $d.log_severity by $d.duration
```

Will result in logs of the following form:

```
[
   { "log_severity": "Warning", "duration": 2000 },
   { "log_severity": "Debug", "duration":  1000 }
   ...
]
```

**Grouping variation**

Limits the rows returned to a specified number and group them by a set of aggregation expressions and order them by a set of expressions.

```
order_direction := "descending"/"ascending" according to top/bottom

top <limit> <(groupby_expression1|aggregate_function1)> [as <alias>] [, <(groupby_expression2|aggregate_function2)> [as <alias2>], ...] by <(groupby_expression1|aggregate_function1)> [as <alias>]
```

For example, the following query:

```
top 10 $m.severity, count() as $d.number_of_severities by avg($d.duration) as $d.avg_duration
```

Will result in logs of the following form:

```
[
   { "severity": "Debug", "number_of_severities":  10, avg_duration: 2000 }
   { "severity": "Warning", "number_of_severities": 50, avg_duration: 1000 },
   ...
]
```

Supported aggregation functions are listed in "Aggregation Functions" section.

## Text Search Operators

### find / text

Search for the string in a certain keypath.

```
(find|text) <free-text-string> in <keypath>

```

Examples:

```
find 'host1000' in $d.kubernetes.hostname
text 'us-east-1' in $d.msg

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#lucene)lucene

A generic lucene-compatible operator, allowing both free and wild text searches, and more complex search queries.

Field names inside the lucene query are relative to `$d` (the root level of user-data).

```
lucene <lucene-query-as-a-string>

```

Examples:

```
lucene 'pod:recommender AND (is_error:true or status_code:404)'

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#wildfindwildtext)wildfind / wildtext

Search for the string in the entire user data. This can be used when the keypath in which the text resides is unknown.

Note: The performance of this operator is worse than when using the `find`/`text` operator. Prefer using those operators when you know the keypath to search for.

```
(wildfind/wildtext) <string>

```

Examples:

```
wildfind 'my-region'
wildfind ':9092'

```

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#expressions)**Expressions**

DataPrime supports a limited set of javascript constructs that can be used in expressions.

The data is exposed using the following top-level fields:

- `$m` - Event metadata
    - `timestamp`
    
    - `severity` - Possible values are `V`ERBOSE, `D`EBUG, `I`NFO, `W`ARNING, `E`RROR, `C`RITICAL
    
    - `priorityclass` - Possible values are `high`, `medium`, `low`
    
    - `logid`

- `$l` - Event labels
    - `applicationname`
    
    - `subsystemname`
    
    - `category`
    
    - `classname`
    
    - `computername`
    
    - `methodname`
    
    - `threadid`
    
    - `ipaddress`

- `$d` - The user's data

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#field-access)Field Access

Accessing nested data is done by using a keypath, similar to any programming language or json tool. Keys with special characters can be accessed using a map-like syntax, with the key string as the map index, e.g. `$d.my_superkey['my_field_with_a_special/character']`.

Examples:

```
$m.timestamp
$d.my_superkey.myfield
$d.my_superkey['my_field_with_a_special/character']
$l.applicationname

```

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#language-constructs)Language Constructs

All standard language constructs are supported:

- Constants

- Nested field access, as mentioned above

- Basic math operations between numbers, including modulo (%)

- Boolean operations `&&`, `||`, `!`

- Comparisons

- String concatenations through `concat` (string interpolation will be supported soon)

- casting - A simple notation for casting data types: e.g. `$d.temperature:number`. Type inference is automatically applied when possible. We'll support full type-inference soon, reducing the need for casting.

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#text-search)Text Search

Boolean expressions for text search:

- `$d.field ~ 'text phrase'` - case-insensitive search for a text phrase in a specific field.

- `$d ~~ 'text phrase'` - case-insensitive search for a text phrase in `$d`.

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#scalar-functions)Scalar Functions

Various functions can be used to transform values. All functions can be called as methods as well, e.g. `$d.msg.contains('x')` is equivalent to `contains($d.msg,'x')`.

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#string-functions)String Functions

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#chr)chr

`chr(number: number): string`

Returns the Unicode code point `number` as a single character string.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#codepoint)codepoint

`codepoint(string: string): number`

Returns the Unicode code point of the only character of `string`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#concat)concat

`concat(value: string, ...values: string): string`

Concatenates multiple strings into one.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#contains)contains

`contains(string: string, substring: string): bool`

Returns `true` if `substring` is contained in `string`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#endswith)endsWith

`endsWith(string: string, suffix: string): bool`

Returns `true` if `string` ends with `suffix`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#indexof)indexOf

`indexOf(string: string, substring: string): number`

Returns the position of `substring` in `string`, or `-1` if not found.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#length)length

`length(value: string): number`

Returns the length of `value`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#ltrim)ltrim

`ltrim(value: string): string`

Removes whitespace to the left of the string `value`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#matches)matches

`matches(string: string, regexp: regexp): bool`

Evaluates the regular expression pattern and determines if it is contained within string.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#pad)pad

Alias for `padLeft`

`pad(value: string, charCount: number, fillWith: string): string`

Left pads string to charCount. If `size < fillWith.length()` of string, result is truncated. See [padLeft](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#string_functions.padleft) for more details.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#padleft)padLeft

`padLeft(value: string, charCount: number, fillWith: string): string`

Left pads string to charCount. If `size < fillWith.length()` of string, result is truncated.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#padright)padRight

`padRight(value: string, charCount: number, fillWith: string): string`

Right pads string to charCount. If `size < fillWith.length()` of string, result is truncated.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#regexpsplitparts)regexpSplitParts

`regexpSplitParts(string: string, delimiter: regexp, index: number): string`

Splits string on regexp-delimiter, returns the field at index. Indexes start with 1.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#rtrim)rtrim

`rtrim(value: string): string`

Removes whitespace to the right of the string `value`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#splitparts)splitParts

`splitParts(string: string, delimiter: string, index: number): string`

Splits string on delimiter, returns the field at index. Indexes start with 1.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#startswith)startsWith

`startsWith(string: string, prefix: string): bool`

Returns `true` if `string` starts with `prefix`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#substr)substr

`substr(value: string, from: number, length: number?): string`

Returns the substring in `value`, from position `from` and up to length `length`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#tolowercase)toLowerCase

`toLowerCase(value: string): string`

Converts `value` to lowercase

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#touppercase)toUpperCase

`toUpperCase(value: string): string`

Converts `value` to uppercase

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#trim)trim

`trim(value: string): string`

Removes whitespace from the edges of a string `value`

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#ip-functions)IP Functions

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#ipinsubnet)ipInSubnet

`ipInSubnet(ip: string, ipPrefix: string): bool`

Returns true if ip is in the subnet of ipPrefix.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#ipprefix)ipPrefix

`ipPrefix(ip: string, subnetSize: number): string`

Returns the IP prefix of a given ip\_address with subnetSize bits (e.g.: `192.128.0.0/9`).

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#string-interpolation)String iInterpolation

- `` `this is an interpolated {$d.some_keypath} string` `` - `{$d.some_keypath}` will be replaced with the evaluated expression that is wrapped by the brackets

- ``` `this is how you escape \{ and \} and \`` ``` - Backward slash (`\`) is used to escape characters like `{`, `}` that are used for keypaths.

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#uuid-functions)UUID Functions

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#isuuid)isUuid

`isUuid(uuid: string): bool`

Returns true if uuid is valid.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#randomuuid)randomUuid

`randomUuid(): string`

Returns a random UUIDv4.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#uuid)uuid

_Deprecated:_ use `randomUuid` instead

`uuid(): string`

Returns a random UUIDv4. See [randomUuid](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#uuid_functions.randomuuid) for more details.

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#general-functions)General Functions

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#firstnonnull)firstNonNull

`firstNonNull(value: any, ...values: any): any`

Returns the first non-null value from the parameters. Works only on scalars for now.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#if)if

`if(condition: bool, then: any, else: any?): any`

return either the `then` or `else` according to the result of `condition`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#in)in

`in(comparand: any, value: any, ...values: any): bool`

Tests if the `comparand` is equal to any of the values in a set `v1 ... vN`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#recordlocation)recordLocation

`recordLocation(): string`

Returns the location of the record (e.g.: s3 URL)

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#number-functions)Number Functions

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#abs)abs

`abs(number: number): number`

Returns the absolute value of `number`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#ceil)ceil

`ceil(number: number): number`

Rounds the value up to the nearest integer

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#e)e

`e(): number`

Returns the constant Euler’s number.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#floor)floor

`floor(number: number): number`

Rounds the value down to the nearest integer

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#frombase)fromBase

`fromBase(string: string, radix: number): number`

Returns the value of `string` interpreted as a base-radix number.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#ln)ln

`ln(number: number): number`

Returns the natural log of `number`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#log)log

`log(base: number, number: number): number`

Returns the log of `number` in base `base`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#log2)log2

`log2(number: number): number`

Returns the log of `number` in base 2. Equivalent to `log(2, number)`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#max)max

`max(value: number, ...values: number): number`

Returns the largest number of all the numbers passed to the function

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#min)min

`min(value: number, ...values: number): number`

Returns the smallest number of all the numbers passed to the function

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#mod)mod

`mod(number: number, divisor: number): number`

Returns the modulus (remainder) of `number` divided by `divisor`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#pi)pi

`pi(): number`

Returns the constant Pi.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#power)power

`power(number: number, exponent: number): number`

Returns `number^exponent`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#random)random

`random(): number`

Returns a pseudo-random value in the range `0.0 <= x < 1.0`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#randomint)randomInt

`randomInt(upperBound: number): number`

Returns a pseudo-random integer number between 0 and n (exclusive)

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#round)round

`round(number: number, digits: number?): number`

Round `number` to `digits` decimal places

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#sqrt)sqrt

`sqrt(number: number): number`

Returns square root of a number.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#tobase)toBase

`toBase(number: number, radix: number): string`

Returns the base-radix representation of `number`.

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#url-functions)URL Functions

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#urldecode)urlDecode

`urlDecode(string: string): string`

Unescapes the URL encoded in string.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#urlencode)urlEncode

`urlEncode(string: string): string`

Escapes string by encoding it so that it can be safely included in URL.

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#datetime-functions)Date / Time Functions

Functions for processing timestamps, intervals and other time-related constructs.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#time-units)Time Units

Many date/time functions accept a time unit argument to tweak their behaviour. Dataprime supports time units from nanoseconds to days. They are represented as literal strings of the time unit name in either long or short notation:

- long notation: `'day'`, `'hour'`, `'minute'`, `'second'`, `'milli'`, `'micro'`, `'nano'`

- short notation: `'d'`, `'h'`, `'m'`, `'s'`, `'ms'`, `'us'`, `'ns'`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#time-zones)Time Zones

Dataprime timestamps are always stored in the UTC time zone, but some date/time functions accept a time zone argument to tweak their behaviour. Time zone arguments are strings that specify a time zone offset, shorthand or identifier:

- time zone offset in hours (e.g. `'+01'` or `'-02'`)

- time zone offset in hours and minutes (e.g. `'+0130'` or `'-0230'`)

- time zone offset in hours and minutes with separator (e.g. `'+01:30'` or `'-02:30'`)

- time zone shorthand (e.g. `'UTC'`, `'GMT'`, `'EST'`, etc.)

- time zone identifier (e.g. `'Asia/Yerevan'`, `'Europe/Zurich'`, `'America/Winnipeg'`, etc.)

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#addinterval)addInterval

`addInterval(left: interval, right: interval): interval`

Adds two intervals together. Works also with negative intervals. Equivalent to `left + right`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#addtime)addTime

`addTime(t: timestamp, i: interval): timestamp`

Adds an interval to a timestamp. Works also with negative intervals. Equivalent to `t + i`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#difftime)diffTime

`diffTime(to: timestamp, from: timestamp): interval`

Calculates the duration between two timestamps. Positive if `to > from`, negative if `to < from`. Equivalent to `to - from`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#extracttime)extractTime

`extractTime(timestamp: timestamp, unit: dateunit | timeunit, tz: string?): number`

Extracts either a date or time unit from a `timestamp`. Returns a floating point number for time units smaller than a `'minute'`, otherwise an integer. Date units such as `'month'` or `'week'` start from 1 (not from 0).

Function parameters:

- `timestamp` (required) - the timestamp to extract from.

- `unit` (required) - the date or time unit to extract. Must be a string literal and one of:
    - any [time unit](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.time_units) in either long or short notation
    
    - a date unit in long notation: `'year'`, `'month'`, `'week'`, `'day_of_year'`, `'day_of_week'`
    
    - a date unit in short notation: `'Y'`, `'M'`, `'W'`, `'doy'`, `'dow'`

- `tz` (optional) - a [time zone](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.time_zones) to convert the timestamp before extracting.

Example 1: extract the hour in Tokyo

```
limit 1 | choose $m.timestamp.extractTime('h', 'Asia/Tokyo') as h # Result 1: 11pm { "h": 23 }
```

Example 2: extract the number of seconds

```
limit 1 | choose $m.timestamp.extractTime('second') as s # Result 2: 38.35 seconds { "s": 38.3510265 }
```

Example 3: extract the timestamp's month

```
limit 1 | choose $m.timestamp.extractTime('month') as m # Result 3: August { "m": 8 } 
```

Example 4: extract the day of the week

```
limit 1 | choose $m.timestamp.extractTime('dow') as d # Result 4: Tuesday { "d": 2 }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#formatinterval)formatInterval

`formatInterval(interval: interval, scale: timeunit?): string`

Formats `interval` to a string with an optional time unit `scale`.

Function parameters:

- `interval` (required) - the interval to format.

- `scale` (optional) - the largest [time unit](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.time_units) of the interval to show. Defaults to `nano`.

Example:

```
limit 3 | choose formatInterval(now() - $m.timestamp, 's') as i # Results: { "i": "122s261ms466us27ns" } { "i": "122s359ms197us227ns" } { "i": "122s359ms197us227ns" }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#formattimestamp)formatTimestamp

`formatTimestamp(timestamp: timestamp, format: string?, tz: string?): string`

Formats a `timestamp` to a string with an optional format specification and destination time zone.

Function parameters:

- `timestamp` (required) - the timestamp to format.

- `format` (optional) - a date/time format specification for parsing timestamps. Defaults to `'iso8601'`. The format can be any string with embedded date/time formatters, or one of several shorthands. Here are a few samples:
    - `'%Y-%m-%d'` - print the date only, e.g. `'2023-04-05'`
    
    - `'%H:%M:%S'` - print the time only, e.g. `'16:07:33'`
    
    - `'%F %H:%M:%S'` - print both date and time, e.g. `'2023-04-05 16:07:33'`
    
    - `'iso8601'` - print a timestamp in ISO 8601 format, e.g. `'2023-04-05T16:07:33.123Z'`
    
    - `'timestamp_milli'` - print a timestamp in milliseconds (13 digits), e.g. `'1680710853123'`

- `tz` (optional) - the destination [time z](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.time_zones)[one](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md?plain=1#L1093) to convert the timestamp before formatting.

Example 1: print a timestamp with default format and +5h offset

```
limit 1 | choose $m.timestamp.formatTimestamp(tz='+05') as ts # Result 1: { "ts": "2023-08-29T19:08:37.405937400+0500" }
```

Example 2: print only the year and month

```
limit 1 | choose $m.timestamp.formatTimestamp('%Y-%m') as ym # Result 2: { "ym": "2023-08" } 
```

Example 3: print only the hours and minutes

```
limit 1 | choose $m.timestamp.formatTimestamp('%H:%M') as hm # Result 3: { "hm": "14:11" }
```

Example 4: print a timestamp in milliseconds (13 digits)

```
limit 1 | choose $m.timestamp.formatTimestamp('timestamp_milli') as ms # Result 4: { "ms": "1693318678696" }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#fromunixtime)fromUnixTime

`fromUnixTime(unixTime: number, timeUnit: timeunit?): timestamp`

Converts a number of a specific time units since the UNIX epoch to a timestamp (in UTC). The UNIX epoch starts on January 1, 1970 - earlier timestamps are represented by negative numbers.

Function parameters:

- `unixTime` (required) - the amount of time units to convert. Can be either positive or negative and will be rounded down to an integer.

- `timeUnit` (optional) - the [time units](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.time_units) to convert. Defaults to `'milli'`.

Example:

```
limit 1 | choose fromUnixTime(1658958157515, 'ms') as ts # Result: { "ts": 1658958157515000000 }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#multiplyinterval)multiplyInterval

`multiplyInterval(i: interval, factor: number): interval`

Multiplies an interval by a numeric `factor`. Works both with integer and fractional numbers. Equivalent to `i * factor`

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#now)now

`now(): timestamp`

Returns the current time at query execution time. Stable across all rows and within the entire query, even when used multiple times. Nanosecond resolution if the runtime supports it, otherwise millisecond resolution.

Example:

```
limit 3 | choose now() as now, now() - $m.timestamp as since # Results: { "now": 1693312549105874700, "since": "14m954ms329us764ns" } { "now": 1693312549105874700, "since": "14m954ms329us764ns" } { "now": 1693312549105874700, "since": "14m960ms519us564ns" }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#parseinterval)parseInterval

`parseInterval(string: string): interval`

Parses an interval from a `string` with format `NdNhNmNsNmsNusNns` where `N` is the amount of each time unit. Returns `null` when the input does not match the expected format:

- It consists of time unit components - a non-negative integer followed by the short time unit name. Supported time units are: `'d'`, `'h'`, `'m'`, `'s'`, `'ms'`, `'us'`, `'ns'`.

- There must be at least one time unit component.

- The same time unit cannot appear more than once.

- Components must be decreasing in time unit order - from days to nanoseconds.

- It can start with `-` to represent negative intervals.

Example 1: parse a zero interval

```
limit 1 | choose '0s'.parseInterval() as i # Result 1: { "i": "0ns" }
```

Example 2: parse a positive interval

```
limit 1 | choose '1d48h0m'.parseInterval() as i # Result 2: { "i": "3d" } 
```

Example 3: parse a negative interval

```
limit 1 | choose '-5m45s'.parseInterval() as i # Result 3: { "i": "-5m45s" }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#parsetimestamp)parseTimestamp

`parseTimestamp(string: string, format: string?, tz: string?): timestamp`

Parses a timestamp from `string` with an optional format specification and time zone override. Returns `null` when the input does not match the expected format.

Function parameters:

- `string` (required) - the input from which the timestamp will be extracted.

- `format` (optional) - a date/time format specification for parsing timestamps. Defaults to `'auto'`. The format can be any string with embedded date/time extractors, one of several shorthands, or a cascade of formats to be attempted in sequence. Here are a few samples:
    - `'%Y-%m-%d'` - parse date only, e.g. `'2023-04-05'`
    
    - `'%F %H:%M:%S'` - parse date and time, e.g. `'2023-04-05 16:07:33'`
    
    - `'iso8601'` - parse a timestamp in ISO 8601 format, e.g. `'2023-04-05T16:07:33.123Z'`
    
    - `'timestamp_milli'` - parse a timestamp in milliseconds (13 digits), e.g. `'1680710853123'`
    
    - `'%m/%d/%Y|timestamp_second'` - parse either a date or a timestamp in seconds, in that order

- `tz` (optional) - a [time zone](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md?plain=1#L1191) override to convert the timestamp while parsing. This parameter will override any time zone present in the input. A time zone can be extracted from the string by using an appropriate format and omitting this parameter.

Example 1: parse a date with the default format

```
limit 1 | choose '2023-04-05'.parseTimestamp() as ts # Result 1: { "ts": 1680652800000000000 }
```

Example 2: parse a date in US format

```
limit 1 | choose '04/05/23'.parseTimestamp('%D') as ts # Result 2: { "ts": 1680652800000000000 } 
```

Example 3: parse date and time with units

```
limit 1 | choose '2023-04-05 16h07m'.parseTimestamp('%F %Hh%Mm') as ts # Result 3: { "ts": 1680710820000000000 } 
```

Example 4: parse a timestamp in seconds (10 digits)

```
limit 1 | choose '1680710853'.parseTimestamp('timestamp_second') as ts # Result 4: { "ts": 1680710853000000000 }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#parsetotimestamp)parseToTimestamp

_Deprecated:_ use `parseTimestamp` instead

`parseToTimestamp(string: string, format: string?, tz: string?): timestamp`

Parses a timestamp from `string` with an optional format specification and time zone override. See [parseTimestamp](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.parsetimestamp) for more details.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#roundinterval)roundInterval

`roundInterval(interval: interval, scale: timeunit): interval`

Rounds an interval to a time unit `scale`. Smaller time units will be zeroed out.

Function parameters:

- `interval` (required) - the interval to round.

- `scale` (required) - the largest [time unit](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.time_units) of the interval to keep.

Example:

```
limit 1 | choose 2h5m45s.roundInterval('m') as i # Result: { "i": "2h5m" }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#roundtime-1)roundTime

`roundTime(date: timestamp, interval: interval): timestamp`

Rounds a timestamp to the given interval. Useful for bucketing, e.g. rounding to `1h` for hourly buckets. Equivalent to `date / interval`.

Example:

```
groupby $m.timestamp.roundTime(1h) as bucket count() as n # Results: { "bucket": "29/08/2023 15:00:00.000 pm", "n": 40653715 } { "bucket": "29/08/2023 14:00:00.000 pm", "n": 1779386 }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#subtractinterval)subtractInterval

`subtractInterval(left: interval, right: interval): interval`

Subtracts one interval from another. Equivalent to `addInterval(left, -right)` and `left - right`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#subtracttime)subtractTime

`subtractTime(t: timestamp, i: interval): timestamp`

Subtracts an interval from a timestamp. Equivalent to `addTime(t, -i)` and `t - i`.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#timeround)timeRound

_Deprecated:_ use `roundTime` instead

`timeRound(date: timestamp, interval: interval): timestamp`

Rounds a timestamp to the given interval. See [roundTime](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.roundtime) for more details.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#tointerval)toInterval

`toInterval(number: number, timeUnit: timeunit?): interval`

Converts a `number` of specific time units to an interval. Works with both integer / floating point and positive / negative numbers.

Function parameters:

- `number` (required) - the amount of time units to convert.

- `timeUnit` (optional) - [Time units](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.time_units) to convert. Defaults to `nano`.

Example 1: convert a floating point number

```
limit 1 | choose 2.5.toInterval('h') as i # Result 1: { "i": "2h30m" } # Example 2: convert an integer number limit 1 | choose -9000.toInterval() as i # Result 2: { "i": "-9us" }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#toiso8601datetime)toIso8601DateTime

_Deprecated_

`toIso8601DateTime(timestamp: timestamp): string`

Alias to `formatTimestamp(timestamp, 'iso8601')`.

Formats `timestamp` to an ISO 8601 string with nanosecond output precision.

Example:

```
limit 1 | choose $m.timestamp.toIso8601DateTime() as ts # Result: { "ts": "2023-08-11T07:29:17.634Z" }
```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#tounixtime)toUnixTime

`toUnixTime(timestamp: timestamp, timeUnit: timeunit?): number`

Converts `timestamp` to a number of specific time units since the UNIX epoch (in UTC). The UNIX epoch starts on January 1, 1970 - earlier timestamps are represented by negative numbers.

Function parameters:

- `timestamp` (required) - the timestamp to convert.

- `timeUnit` (optional) - the [time units](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#date_time_functions.time_units) to convert to. Defaults to `'milli'`.

Example:

```
limit 1 | choose $m.timestamp.toUnixTime('hour') as hr # Result: { "hr": 470363 }
```

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#encodingdecoding-functions)Encoding / Decoding Functions

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#decodebase64)decodeBase64

`decodeBase64(value: string): string`

Decode a base-64 encoded string

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#encodebase64)encodeBase64

`encodeBase64(value: string): string`

Encode a string into base-64

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#case-expressions)Case Expressions

Case expressions are special constructs in the language that allow choosing between multiple options in an easy manner and in a readable way. They can be wherever an expression is expected.

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#case)case

Choose between multiple values based on several generic conditions. Resort to a `default-value` if no condition is met.

```
case {
  condition1 -> value1,
  condition2 -> value2,
  ...
  conditionN -> valueN,
  _          -> <default-value>
}

```

Example:

```
case {
  $d.status_code == 200 -> 'success',
  $d.status_code == 201 -> 'created',
  $d.status_code == 404 -> 'not-found',
  _ -> 'other'
}

# Here's the same example inside the context of a query. A new field is created with the `case` result,
# and then a filter will be applied, leaving only non-successful responses.

source logs | ... | create $d.http_response_outcome from case {
  $d.status_code == 200 -> 'success',
  $d.status_code == 201 -> 'created',
  $d.status_code == 404 -> 'not-found',
  _                     -> 'other'
} | filter $d.http_response_outcome != 'success'


```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#case_contains)case\_contains

A shorthand for `case` which allowing checking if a string `s` contains one of several substrings without repeating the expression leading to `s`. The chosen value is the first which matches `s.contains(substring)`.

```
case_contains {
  s: string,
  substring1 -> result1,
  substring2 -> result2,
  ...
  substring3 -> resultN
}

```

Example:

```
case_contains {
  $l.subsystemname,
  '-prod-' -> 'production',
  '-dev-'  -> 'development',
  '-stg-'  -> 'staging',
  _        -> 'test'
}

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#case_equals)case\_equals

A shorthand for `case` which allowing comparing some expression `e` to several results without repeating the expression. The chosen value is the first which matches `s == value`

```
case_equals {
  e: any,
  value1 -> result1,
  value2 -> result2,
  ...
  valueN -> resultN
}

```

Example:

```
case_equals {
  $m.severity,
  'info'   -> true,
  'warning -> true,
  _        -> false
}

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#case_greaterthan)case\_greaterthan

A shorthand for `case` which allows comparing `n` to multiple values without repeating the expression leading to `n`. The chosen value is the first which matches `expression > value`.

```
case_greaterthan {
  n: number,
  value1: number -> result1,
  value2: number -> result2,
  ...
  valueN: number -> resultN,
  _              -> <default-value>
}

```

Example:

```
case_greaterthan {
  $d.status_code,
  500 -> 'server-error',
  400 -> 'client-error',
  300 -> 'redirection',
  200 -> 'success',
  100 -> 'information',
  _   -> 'other'
}

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#case_lessthan)case\_lessthan

A shorthand for `case` which allows comparing a number `n` to multiple values without repeating the expression leading to `n`. The chosen value is the first which matches `expression < value`.

```
case_lessthan {
  n: number,
  value1: number -> result1,
  value2: number -> result2,
  ...
  valueN: number -> resultN,
  _              -> <default-value>
}

```

Example:

```
case_lessthan {
  $d.temperature_celsius,
  10 -> 'freezing',
  20 -> 'cold',
  30 -> 'fun',
  45 -> 'hot',
  _  -> 'burning'
}

```

## [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#aggregation-functions)

## **Aggregation Functions**

### any\_value

Returns any non-null expression value in the group. If expression is not defined, it defaults to the `$data` object.

```
any_value(expression: any?)

```

Returns null if all expression values in the group are null.

Example:

```
groupby $m.severity calculate any_value($d.url)

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#avg)avg

Calculates the average value of a numerical expression in the group.

```
avg(expression: number)

```

Example:

```
groupby $m.severity calculate avg($d.duration) as average_duration

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#count-1)count

Counts non-null expression values. If expression is not defined, all rows will be counted.

```
count(expression: any?) [into <keypath>]

```

An alias can be provided to override the keypath the result will be written to.

For example, the following part of a query

```
count() into $d.num_rows

```

will result in a single row of the following form:

```
{ "num_rows": 7532 }

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#count_if)count\_if

Counts non-null expression values on rows which satisfy condition. If expression is not defined, all rows that satisfy condition will be counted.

```
count_if(condition: bool, expression: any?)

```

Example:

```
groupby $m.severity calculate count_if($d.duration > 500) as $d.high_duration_logs
groupby $m.severity calculate count_if($d.duration > 500, $d.company_id) as $d.high_duration_logs

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#distinct_count)distinct\_count

Counts non-null distinct expression values.

```
distinct_count(expression: any)

```

Example:

```
groupby $l.applicationname calculate distinct_count($d.username) as active_users

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#distinct_count_if)distinct\_count\_if

Counts non-null distinct expression values on rows which satisfy condition.

```
distinct_count_if(condition: bool, expression: any)

```

Example:

```
groupby $l.applicationname calculate distinct_count_if($m.severity == 'Error', $d.username) as users_with_errors

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#max-1)max

Calculates the maximum value of a numerical expression in the group.

```
max(expression: number | timestamp)

```

Example:

```
groupby $m.severity calculate max($d.duration)

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#min-1)min

Calculates the minimum value of a numerical expression in the group.

```
min(expression: number | timestamp)

```

Example:

```
groupby $m.severity calculate min($d.duration)

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#percentile)percentile

Calculates the approximate n-th percentile value of a numerical expression in the group.

```
percentile(percentile: number, expression: number, error_threshold: number?)

```

Since the percentile calculation is approximate, the accuracy may be controlled with the `error_threshold` parameter which ranges from `0` to `1` (defaults to `0.01`). A lower value will result in better accuracy at the cost of longer query times.

Example:

```
groupby $m.severity calculate percentile(0.99, $d.duration) as p99_latency

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#sample_stddev)sample\_stddev

Computes the sample standard deviation of a numerical expression in the group.

```
sample_stddev(expression: number)

```

Example:

```
groupby $m.severity calculate sample_stddev($d.duration)

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#sample_variance)sample\_variance

Computes the variance of a numerical expression in the group.

```
sample_variance(expression: number)

```

Example:

```
groupby $m.severity calculate sample_variance($d.duration)

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#stddev)stddev

Computes the standard deviation of a numerical expression in the group.

```
stddev(expression: number)

```

Example:

```
groupby $m.severity calculate stddev($d.duration)

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#sum)sum

Calculates the sum of a numerical expression in the group.

```
sum(expression: number)

```

Example:

```
groupby $m.severity calculate sum($d.duration) as total_duration

```

### [](https://github.com/coralogix/dataprime-parser/blob/master/docs/cheat-sheet.md#variance)variance

Computes the variance of a numerical expression in the group.

```
variance(expression: number)

```

Example:

```
groupby $m.severity calculate variance($d.duration)
```

## DP Expressions in Aggregations

When querying with the **groupby** operator, you can now apply an aggregation function (such asavg, max, sum) to the bucket of results. This feature gives you the power to manipulate an aggregation expression inside the expression itself, allowing you to calculate and manipulate your data simultaneously.

**Example 1**

This examples takes logs which have some `connect_duration` and `batch_duration` fields, and calculates the ratio between the averages of those durations, per `region`.

```
# Query
source logs 
  | groupby region aggregate avg(connect_duration) / avg(batch_duration)

```

**Example 2**

This query calculates the percentage of logs which don’t have a `kubernetes_pod_name` out of the total number of logs. The calculation is done per subsystem.

```
# Query
source logs 
| groupby $l.subsystemname aggregate
  sum(if(kubernetes.pod_name != null,1,0)) / count() as pct_without_pod_name

```

**Example 3**

This query calculates the ratio between the maximum and minimum salary per department, and provides a `Based on N Employees` string as an additional column per row.

```
# Query
source logs
| groupby department_id aggregate
    max(salary) / min(salary) as salary_ratio
    `Based on {count()} Employees`)

```

**Example 4**

This query calculates the ratio between error logs and info logs.

```
source logs
| groupby $m.timestamp / 1h as hour aggregate 
    count_if($m.severity == '5') / count_if($m.severity == '3') as error_to_info_ratio
```

## Support

**Need help?**

Our world-class customer success team is available 24/7 to answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
