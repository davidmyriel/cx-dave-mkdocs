---
title: "Aggregation Function"
date: "2021-11-25"
---

In the past, you were only able to aggregate and group by a certain field. Today, in the log screen graph, we have added a new feature that allows you to aggregate on metrics data, AVG, MIN, SUM and MAX.

To do so, under the log screen on top you will see a graph, click on the blue Gear Icon.

![](images/Screen-Shot-2021-11-24-at-5.24.05-PM.png)

You will get the different options we support so far.

The option **Count** applies to the logs you are seeing in your screen. You can count them and/or group them by any field that is in the drop down menu.

![](images/Screen-Shot-2021-11-24-at-5.29.18-PM.png)

If you chose any of the options **AVG, MIN, MAX, and SUM**, you will need to have Log2Metrics configured, Prometheus logs, or both.

In case you chose any of these options and you do not have Log2Metrics or Prometheus logs you will get the screen below:

![](images/Screen-Shot-2021-11-24-at-5.31.48-PM.png)

If you have configured Log2Metrics or you have Prometheus logs, the **AVG, MIN, MAX,** and **SUM** would work like the example below. We aggregate on the **AVG** of a metric field called _https\_resp\_took\_ms_:

![](images/Screen-Shot-2021-11-24-at-5.40.50-PM-1-1024x271.png)

As before, you can also add a **Group By** to this graph and also **Compare To**.

![](images/Screen-Shot-2021-11-24-at-5.43.24-PM-1-1024x342.png)

Remember that **Group By** fields are supposed to come from L2M or Prometheus.
