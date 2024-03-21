---
title: "Querying Coralogix with SQL"
date: "2021-01-25"
---

The Coralogix JDBC driver allows you to investigate your log data using SQL queries with your favorite database tool. With the Coralogix JDBC driver, you can quickly get started on performing SQL queries against the data already stored in your Coralogix account.

JDBC, which stands for "Java Database Connectivity", is a common standard for database drivers, and many popular querying tools support it. In this tutorial, you'll find instructions on using the Coralogix JDBC driver with two popular tools - [DataGrip](https://www.jetbrains.com/datagrip/) and [DBeaver](https://dbeaver.io).

## Getting Started

Follow these steps to set up the connection to Coralogix:

1. Download the [latest driver](https://artifacts.opensearch.org/opensearch-clients/jdbc/opensearch-sql-jdbc-1.1.0.1.jar).

3. Follow the steps below for client-specific instructions. Other clients should have a similar procedure for installing the driver.

5. Test the connection using a simple query: `SELECT * FROM logs LIMIT 50`

### [](https://github.com/coralogix/coralogix-jdbc/blob/master/USAGE.md#datagrip)DataGrip

1. Click on `+` icon in the Database menu and choose `Driver`

3. Into `Name` field write `Coralogix`

5. Click on `+` under `Driver Files` and pick the driver file you downloaded earlier (Getting Started step #1 above)

7. Open `Class` picker and pick `org.opensearch.jdbc.Driver`

9. Click on `Apply` then `OK`

11. Click on `+` icon in the database menu -> choose `Data Source` -> choose `Coralogix`

13. Copy your `apiKey` from `` `Data Flow` -> `API Keys` -> `Logs Query Key` `` in the Coralogix UI and replace <Logs Query Key> with this value in the JDBC URL below as per your Team's cluster location.

15. In the `General` tab change the url to:  
      
    `jdbc:opensearch://https://ng-api-http.coralogixstg.wpengine.com`/sql/<Logs Query Key>  
    If your account is in **Europe** (ends in .com)  
      
    ``jdbc:opensearch://https://ng-api`-http`.app.coralogix.in``/sql/<Logs Query Key>  
    If your account is in **India** (ends in .in)  
      
    ``jdbc:opensearch://https://ng-api`-http`.coralogix.us``/sql/<Logs Query Key>  
    If your account is in the **US** (ends in .us)  
      
    ``jdbc:opensearch://https://ng-api`-http`.eu2.coralogixstg.wpengine.com``/sql/<Logs Query Key>  
    If your account is in **Europe** (ends in `.eu2.coralogixstg.wpengine.com`)  
      
    ``jdbc:opensearch://https://ng-api`-http`.coralogixsg.com``/sql/<Logs Query Key>  
    If your account is in **Singapore** (ends in `.coralogixsg.com`)  
    

17. Click on `Apply` then `OK`

19. Congratulations! You are ready to query your logs using DataGrip

### [](https://github.com/coralogix/coralogix-jdbc/blob/master/USAGE.md#dbeaver)DBeaver

1. In the top menu select `Database` -> `Driver manager` and click the `New` button

3. In `Driver Name` field write `Coralogix`

5. Click the Libraries tab

7. Click `Add File` button and pick the driver file you downloaded earlier (Getting Started step #1 above)

9. Click the `Find Class` button. It should show you `org.opensearch.jdbc.Driver` in the `Driver Class` field, click on it

11. Click `OK`

13. Click Close

15. Click `Database/New Database Connection` (make sure that **All** is selected)

17. Type `coralogix` into the search box and choose `Coralogix` driver, and click `Next`

19. Copy your `apiKey` from `` `Data Flow` -> `API Keys` -> `Logs Query Key` `` in the Coralogix UI and replace <Logs Query Key> with this value in the JDBC URL below as per your Team's cluster location.

21. Set `JDBC URL` to:   
      
    ``jdbc:opensearch://https://ng-api`-http`.coralogixstg.wpengine.com``/sql/<Logs Query Key>  
    If your account is in **Europe** (ends in .com)  
      
    ``jdbc:opensearch://https://ng-api`-http`.app.coralogix.in``/sql/<Logs Query Key>  
    If your account is in **India** (ends in .in)  
      
    ``jdbc:opensearch://https://ng-api`-http`.coralogix.us``/sql/<Logs Query Key>  
    If your account is in the **US** (ends in .us)  
      
    ``jdbc:opensearch://https://ng-api`-http`.eu2.coralogixstg.wpengine.com``/sql/<Logs Query Key>  
    If your account is in **Europe** (ends in `.eu2.coralogixstg.wpengine.com`)  
      
    ``jdbc:opensearch://https://ng-api`-http`.coralogixsg.com``/sql/<Logs Query Key>  
    If your account is in **Singapore** (ends in `.coralogixsg.com`)  
    

23. Click Test Connection to ensure it all works

25. Click OK

27. Congratulations! The `Coralogix` connection in the `Database Navigator` has just been created

### Tableau

1. Download the [**cx\_sql\_jdbc.taco**](https://github.com/coralogix/tableau_jdbc_connector/blob/main/cx_sql_jdbc.taco) connector file, and copy it to:

- Windows: `C:\Users\%USERNAME%\Documents\My Tableau Repository\Connectors`

- MacOS: `~/Documents/My Tableau Repository/Connectors`

2. Place the OpenSearch JDBC driver (jar file) downloaded earlier (Getting Started step #1 above) into:

- Windows: `C:\Program Files\Tableau\Drivers`

- MacOS: `~/Library/Tableau/Drivers`

3. Run the Tableau Desktop with the command line flag `-DDisableVerifyConnectorPluginSignature=true`:

- Windows: `C:\Program Files\Tableau\Tableau 2022.1\bin\tableau.exe" -DDisableVerifyConnectorPluginSignature=true`

- MacOS: `open -n /Applications/Tableau\ Desktop\ 2022.1.app --args -DDisableVerifyConnectorPluginSignature=true`   
      
    (Adjust the command line according to the Tableau version you have. You can create a shortcut or a script to simplify the above step).

3. Copy your `apiKey` from `` `Data Flow` -> `API Keys` -> `Logs Query Key` `` in the Coralogix UI and replace <Logs Query Key> with this value in the JDBC URL below as per your Team's cluster location.

5. Open Tableau, and select to a Server -> Coralogix by Coralogix, and set the `JDBC URL` to:   
      
    `` `jdbc:opensearch://https://`ng-api`-http`.coralogixstg.wpengine.com ``/sql/<Logs Query Key>  
    If your account is in **Europe** (ends in .com)  
      
    `` `jdbc:opensearch://https://`ng-api`-http`.app.coralogix.in ``/sql/<Logs Query Key>  
    If your account is in **India** (ends in .in)  
      
    ``jdbc:opensearch://https://ng-api`-http`.coralogix.us``/sql/<Logs Query Key>  
    If your account is in the **US** (ends in .us)  
      
    ``jdbc:opensearch://https://ng-api`-http`.eu2.coralogixstg.wpengine.com``/sql/<Logs Query Key>  
    If your account is in **Europe** (ends in `.eu2.coralogixstg.wpengine.com`)  
      
    ``jdbc:opensearch://https://ng-api`-http`.coralogixsg.com``/sql/<Logs Query Key>  
    If your account is in **Singapore** (ends in `.coralogixsg.com`)

7. Congratulations! You are ready to query your logs using Tableau  
    

## [](https://github.com/coralogix/coralogix-jdbc/blob/master/USAGE.md#queries)The SQL Data Model

The SQL data model exposed by the Coralogix SQL interface currently includes just one table: `logs`. This table contains all the log records currently stored in Coralogix. More tables might be exposed in the future for other, distinct data types stored in Coralogix.

For convenience, you may query logs for specific application names and subsystems through the table name: querying the table `logs.production.billing` will query for logs from the `production` application and `billing` subsystem.

The field names of your log records are mapped to column names. Nested field names are mapped to column names using their full path with the path elements concatenated by dots (`.`). The types of the table fields correspond to the types specified in the fields' mapping. Here's a short example of how a log record document is mapped to the tabular format.

This document:

```
{
  "kubernetes": {
    "container_name": "some_service"
  },
  "log": "Starting up"
}
```

Will be emitted as this result-set:

```
kubernetes.container_name | log
---------------------------------------------
some_service              | Starting up
```

In addition, every row available in the `logs` table contains a `coralogix` object with standard metadata; for example:

- `coralogix.timestamp`

- `coralogix.metadata.applicationName`

- `coralogix.metadata.subsystemName`

### Available queries

The vast majority of standard SQL functionality works with the Coralogix SQL interface: selecting fields, using `WHERE` clauses for filtering, aggregations with `GROUP BY` and `HAVING` clauses and so forth. There are two  exceptions to this: joins and set queries (`UNION / MINUS / INTERSECT`) are currently unsupported. It is possible that support for these constructs will be added in the future.

Beyond standard SQL functionality, a number of full-text search constructs are supported:

#### Match Query

The `MATCH_QUERY` function can be used to apply a full-text search to a specific field. For example:

```
SELECT text, coralogix.metadata.severity
 FROM logs
 WHERE text = MATCH_QUERY('healthcheck')
```

This query will fetch the `text` and `severity` fields of all log records whose body contains the word `healthcheck`.

#### Query String

More complex predicates can be expressed using Query Strings with the `QUERY` function, for example:

```
SELECT COUNT(*), coralogix.metadata.processName
FROM logs
WHERE QUERY('coralogix.metadata.applicationName:production AND coralogix.metadata.subsystemName:billing')
GROUP BY coralogix.metadata.processName
```

This query will count the number of logs from the `production` application and `billing` subsystem and group them by their `processName` value.

## Additional Resources

<table><tbody><tr><td>API</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/direct-query-http-api/">Direct Archive Query HTTP API</a></strong></td></tr><tr><td>External</td><td><a href="https://opendistro.github.io/for-elasticsearch-docs/docs/sql/sql-full-text/"><strong>OpenDistro SQL</strong></a><br>Refer to this documentation for further references on supported SQL features.</td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us via our in-app chat or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
