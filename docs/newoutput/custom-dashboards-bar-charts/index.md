---
title: "Vertical Bar Charts"
date: "2023-06-15"
---

The vertical bar chart widget in Coralogix offers an additional way to view your data. Vertical bar charts sort your data alphabetically by column name by default. In [horizontal bar charts](https://coralogixstg.wpengine.com/docs/horizontal-bar-charts/), data is usually sorted by value.

## Create a Vertical Bar Chart

**Create a customized vertical bar chart visualization.**

**STEP 1**. Drag and drop the **Vertical Bar Chart** widget from the left-hand side bar to get started.

![](images/Custom-Dashboards-Vertical-Bar-Chart-Add-Widget.png)

**STEP 2**. Set the definitions for your Bar Chart in the right-hand sidebar.

- **Name & Description**. Create a name and description.

- **Load data from.** Select whether to load data from [Frequent Search](https://coralogixstg.wpengine.com/docs/optimize-log-management-costs/#frequent-search-data-high-priority) or [Monitoring](https://coralogixstg.wpengine.com/docs/optimize-log-management-costs/#monitoring-data-medium-priority).

![](images/Frequent-Search-Monitoring.png)

- **Source**. Select a data type.
    
    - If the data type chosen is metrics, specify the metric or desired PromQL in the **Query** field. Use free text to search for a metric of your choice. As you do so, all relevant metrics will appear. Hover over any metric to view its system-generated metadata labels. Hover over a label to see its values.
        - When creating a bar chart with metrics as the data type, the categories specified in the PromQL query appear in the Group By field automatically. Within the **Group By** field it is possible to reorder the categories by dragging and dropping.Drag and drop categories from the **Category** field into the **Stacking** field, to stack by a particular category.
    
    - **ADD FILTER.** \[Optional\] Add a filter to your bar chart.
        - As opposed to the dashboard filter in the left-hand sidebar which affects the entire dashboard, this filter only affects the widget.
        
        - The widget and dashboard filters operate in parallel to one another and intersect. If they negate one another, dashboard filters override widget filters.

- **Group By.** Select the fields to group by from the dropdown menu.

![](images/Vertical-Bar-Chart-with-Load-Data-From.png)

- **X AXIS**. Select whether the X axis of the chart should be grouped by value or by time. If you group by value, the chart is grouped by a field or label (or several fields or labels), and if you group by time, the main grouping is based on periods of time (time buckets).
    - If you select **Value**, enter the value to show (for example, Country).
    
    - If you select **Time**, select Auto or Manual time buckets, and if Manual, the length of the time bucket.

![](images/Vertical-Bar-Chart-By-Time-With-Load-Data-From-1024x504.png)

- **Aggregation**: Aggregate by **Count**, **Count Distinct**, **Sum**, **Min**, **Max**, and/or **Average**.
    - In Bar charts, changing the aggregation type will change the type of data you see.
    
    - For example, aggregating by Count, might show you the number of people in a country. On the other hand when aggregating by Average, for example, you need to provide additional parameters, such as height, which will give you a bar chart displaying the average height by country.

- **Stack By.** \[Optional\] Select a field by which to stack the chart. This shows you a second layer of data on the chart.

![](images/Vertical-Bar-Chart-Stack-By-with-Load-Data-From-1024x504.png)

- **Advanced.** Select from the following advanced options.
    - **Legend Colors By.** Select whether you want your legend colors to be by group or by stack.
    
    - **Scale.** Select whether you want the scale of the bar chart to be **Logarithmic** or **Linear**. The default setting is linear, however if you have large differences between the different values, it can be helpful to show the logarithmic scale instead. For example, if the majority of your values are under 1k and one value is 10k, using the logarithmic scale will show you an easier to read bar chart than the linear scale.
    
    - **Sort By.** Select whether to sort the chart by column name or by value. By default the vertical bar chart is sorted alphabetically by name.
    
    - **Color Scheme.** Select the color scheme for your chart. Note that when Legend Colors By Group is selected, color scheme is disabled.
    
    - ![](images/Custom-Dashboards-Color-Scheme-Vertical-Bar-Chart.png)
    
    - **Max Bars Per Graph.** Select the maximum number of bars you want to show per graph.
    
    - **Group Name.** Customize the displayed group name.
    
    - **Unit.** Select the unit type to use in your bar chart.

![](images/Custom-Dashboards-Vertical-Bar-Chart-Advanced-1024x611.png)

**STEP 3.** \[Optional\] If you want to save your dashboard for future use, click **SAVE** in the upper right hand corner.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/custom-dashboards/">Custom Dashboards</a></strong><br><strong><a href="http://www.coralogixstg.wpengine.com/docs/custom-dashboards-line-charts">Line Charts</a><br><a href="http://www.coralogixstg.wpengine.com/docs/custom-dashboards-data-tables">Data Tables</a><br><a href="http://www.coralogixstg.wpengine.com/docs/custom-dashboards-gauges">Gauges</a><br><a href="http://www.coralogixstg.wpengine.com/docs/custom-dashboards-pie-charts">Pie Charts</a></strong><br><strong><a href="https://coralogixstg.wpengine.com/docs/horizontal-bar-charts/">Horizontal Bar Charts</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
