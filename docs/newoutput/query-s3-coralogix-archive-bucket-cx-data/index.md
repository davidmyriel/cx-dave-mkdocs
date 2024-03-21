---
title: "Access CX-Data Directly"
date: "2023-01-04"
---

This guide explains how to query your S3 Coralogix archive bucket (`cx-data`) using a third-party framework with the standard [Apache Parquet](https://parquet.apache.org/) reader provided by the relevant framework and required schema.

## Folder Structure

`cx-data` is stored in standard hive-like partitions, with the following partition fields:

- `team_id=<team-id>`: [Coralogix Team ID](https://coralogixstg.wpengine.com/docs/coralogix-team-id/)

- `dt=YYYY-MM-DD`: Date of the data in UTC

- `hr=HH`: Hour of the data in UTC

These fields can be defined as virtual columns inside the framework, serving as filters in a query.

**Note**:

- Both `dt` and `hr` are based on the event timestamp.

- The `team_id=<team-id>` partition allows reusing the same bucket and prefix to write data from multiple Coralogix teams and query them all in one query.

## Fields

Each Apache Parquet file has three fields with data as JSON-formatted strings:

- `src_obj__event_metadata`: JSON object containing metadata related to the event

- `src_obj__event_labels`: JSON object containing the labels of the event (such as the Coralogix [applicationName and subsystemName](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/))

- `src_obj__user_data`: JSON object containing actual event data

### Examples

Below is an example of each of the 3 payload fields.

`src_obj__event_metadata`:

```
{
  "timestamp": "2022-03-28T08:50:57.946",
  "severity": "Debug",
  "priorityclass": "low",
  "logid": "some-uuid"
}

```

`src_obj__event_labels`:

```
{
  "applicationname": "some-app",
  "subsystemname": "some-subsystem",
  "category": "some-category",
  "classname": "some-class",
  "methodname": "some-method",
  "computername": "some-computer",
  "threadid": "some-thread-id",
  "ipaddress": "some-ip-address"
}

```

`src_obj__user_data`:

```
{
  "_container_id": "0f099482cf3b507462020e9052516554b65865fb761af8e076735312772352bf",
  "host": "ip-10-1-11-144",
  "short_message": "10.1.11.144 - - [28/Mar/2022:08:50:57 +0000] \\"GET /check HTTP/1.1\\" 200 16559 \\"-\\" \\"Consul Health Check\\" \\"-\\""
}

```

## Reading `cx-data` Files Using a Standard Framework

### Pandas

Loading `cx-data` files in Pandas can be done using the `read_parquet` method:

```
import pandas as pd

# Notice that only the three payload columns are passed eventually to read_parquet()

cx_columns = [
  'src_obj__event_metadata',
  'src_obj__event_labels',
  'src_obj__user_data'
]

df = pd.read_parquet('s3://.../myfile.parquet',columns = cx_columns)

# The dataframe `df` contains all the data needed for further processing.

```

Here is the output of `df.info()` showing the expected schema of the DataFrame:

```
print(df.info())

### Output

RangeIndex: 161050 entries, 0 to 161049
Data columns (total 3 columns):
 #   Column                   Non-Null Count   Dtype
--- ------ -------------- -----
 0   src_obj__event_metadata  161050 non-null  object
 1   src_obj__event_labels    161050 non-null  object
 2   src_obj__user_data       161050 non-null  object
dtypes: object(3)


```

### Athena

To use `cx-data` directly in Athena, you’ll need to create an `EXTERNAL` table as follows:

```
CREATE EXTERNAL TABLE IF NOT EXISTS "my_table" (
	`src_obj__event_labels` STRING,
	`src_obj__event_metadata` STRING,
	`src_obj__user_data` STRING,
)
PARTITIONED BY (`team_id` string, `dt` string, `hr` string)
ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
STORED AS INPUTFORMAT 'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat'
OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
LOCATION 's3://<bucket>/parquet/v1/'

```

After creating the external table, table partitions should be added to limit the scope of queries, reducing costs and improving performance.

Table partitions can be added manually:

```
ALTER TABLE "my_table" ADD PARTITION (team_id='23333', dt='2022-05-30', hr='01');

# Additional ALTER TABLE ... ADD PARTITION statements according to which dates and hours are needed

```

Table partitions can also be added automatically:

```
MSCK REPAIR TABLE "my_table"

```

**Note**! This command scans all of the files in the table location, which may be a time-consuming task when the amount of partitions detected is large. It may be easier to add the relevant new partitions manually. One option to expedite automatic partitioning is to limit the scope of the data by specifying a specific `team_id` and `dt` in the table definition `LOCATION` (e.g. modify the `CREATE EXTERNAL TABLE` command to include the `team_id=<team-id>/dt=<date>/` subpath). This restricts the partition auto-detection to the relevant dates.

After adding the relevant partitions, a query can be executed on the table. Accessing a specific field inside one of the payload fields should be done using the Athena function `json_extract_scalar`, as in the following example:

```
SELECT
  json_extract_scalar(src_obj__event_metadata,'$.severity') severity,
  json_extract_scalar(src_obj__event_labels,'$.applicationName') app_name,
  json_extract_scalar(src_obj__user_data,'$.my_obj.my_message_field') message_field
FROM "my_table"

```

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up. Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
