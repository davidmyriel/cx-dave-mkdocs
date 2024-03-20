---
title: "Tableau Plugin"
date: "2021-03-14"
---

The [Coralogix JDBC driver](https://github.com/coralogix/coralogix-jdbc) allows you to investigate your log data with your favorite database tool using SQL queries.

JDBC, which stands for Java Database Connectivity, is a common standard for database drivers, and many popular querying tools support it. With the Coralogix JDBC driver, you can quickly get started on performing SQL queries against the data already stored in your Coralogix account.

This tutorial provides instructions on using the Coralogix JDBC driver with Tableau.

## Getting Started

**STEP 1**. Place the .jar files in the folder for your operating system. Create a folder if it doesn't exist already.

| OS | Path |
| --- | --- |
| Windows | C:\\Program Files\\Tableau\\Drivers |
| Mac | ~/Library/Tableau/Drivers |
| Linux | /opt/tableau/tableau\_driver/jdbc |

**STEP 2**. Copy the [coralogix.tdc](https://github.com/coralogix/coralogix-jdbc/blob/master/coralogix.tdc) file to `~/Documents/My Tableau Repository/Datasources`. For more details, view the Tableau documentation  [here.](https://kb.tableau.com/articles/howto/using-a-tdc-file-with-tableau-server)

**STEP 3**. Create a file `coralogix.properties` and add an entry with your `apiKey`. In your navigation pane, select **Data Flow** \> API Keys > Logs Query Key.

```
apiKey=<Logs Query Key>
```

1. 1. On the main screen in `To a Server` section, select `Other Databases (JDBC)`.
    2. Set `URL` to \[table id=106 /\].
    3. Set `Dialect` to `MySQL` and leave the `Username` and `Password` blank. Click on `Browse` next `Properties file` and choose `coralogix.properties` you created.
    4. Click on `Sign In`.

The Coralogix SQL support is based on the [OpenDistro SQL](https://opendistro.github.io/for-elasticsearch-docs/docs/sql/sql-full-text/) interface. Refer to its documentation for further references on supported SQL features.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email to [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
