# Tableau

Parent: [[01_Tools]]
See also: [[dataviz]]

#tools #datavis


Cool interactive visual tool for interactive dashboard creation. Several versions, including public (like googledocs, in a way), personal, professional. Server for local deployment, online for cloud subscription. Reader - for sharing data with people who don't have Tableau.

# Creating a visualization

You connect it to data (json, txt, excel, server-side ?? etc.) You kinda add tables (in long form), and you can join them by id. (It even tries to guess how to join them). Uses some funny terminology:
* **dimensions** are things one can grouping by, and filter by (qualitative)
* **measures**  are numerical values to plot.
It also tries to guess the type of each data column, including **dates**, **geography** (city names, country names - are all auto-recognized as geography), **strings**, **numbers**.  Measures can also be treated as continuous or discrete (which be changed at the dashboard construction stage below, as is bound to the visualization, not to the data itself). For dates, we can also switch between treating them as continuous (good for linear plots) or discrete values (good for showing similar plots by year, for example). Essentially it's as if we had `YEAR(mydate)` or `factor(YEAR(mydate))` derivative functions.

The interface is very similar to Excel pivot tables, but for visuals, where columns in the dataset can control: **columns** (essentially, X axis), **rows** (Y axis), aggregation level (what does a single point represent - this goes onto the **Marks** panel), filters to run (**Filters** panel), colors of the chart, marker sizes, column widths, etc. (different sub-parts of the "Marks" panel). For each of these "inputs" you drag-n-drop variables from a list on the left onto a respective panel / control-element for a visual. You can also apply aggregation functions, and so instead of setting `y=sales`, look at `SUM(sales)`, `MEAN(sales)`, `COUNTD` (for "count-distrinct") etc. For "columns" (X axis) it automatically does really nice hierarchical visuals (with dates, for example, where you can have it split first by year, and then by month etc.) The visualizations are available from the "Show me" button in the top right corner, but can also be changed later on (in the "Marks" panel).

Within the visualization, items can be sorted (sorting buttons in the toolbar), in different ways (by value, alphabetically etc.) We can drop more than one category into the "Columns" line on top, and get a hierarchical, grouped view (bars put in groups for a bar chart, or a "facet"-like view of several continuous charts). We can also sort of "bind" this grouping to the data, by dragging the subcategory on top of a category in the data field itself, and creating a **Hierarchy** (a shortcut for a bunch of variables, with their hierarchical relations baked-in). This "derivative variable" also allows doing drill-in in the visualization, with hierarchical filtering (that pretends to be the top category at first, but gives access to more granular categories if selected).

We can create **Calculated fields** by right-clicking in the white space below the list of existing values, and then typing a formula. If you want the formula to still make sense during filtering and aggregation (as opposed to be kinda calculated once at the line-by-line level), it may make sense to define it in an aggregated way: like `SUM(y1)/SUM(y2)` for example. Then it will stay valid even as you drill through the data (which for fields like profitability is what you actually want to happen). Has a bunch of aggregation 

# Dashboards

Finally, you get these individual visualizations called **Sheets**, and combine them into a **Dashboard**. Things like filtering can be done dashboard-wide, and then  user can click through it and change the all visuals within a dashboard. Essentially, one can use  one view (say value by date) to act as a filter for another view (say, values by group), so clicking the timeline would change the per-group split, and the other way around. This works great with geography, as it can update the map based on time-filtering, or update the time-plot based on the geo-filtering etc.

And then on top of these automated "actions" (filter, highlight) we can also create custom actions on a dashboard, to create a dynamic interface.

# Data storage and processing

It has its own format of storing data, and an interface to create data cleaning and processing pipelines (ETL = Extract Transform Load)

You can write your own code in Tableau version of [[javascript]].

# Deployment

?

When sharing on the web, the description of a dashboard is stored as an html, with divs and everything, and  `<object class=...>`  tags to host actual visualizations. `<param name=... value...>` tags specify parameters specify parameters. `host_url` parameter points at the server, and that's where the data comes from, while a script (written in [[javascript]]) will take care of working with it.

# Refs

Decent intro video: https://www.youtube.com/watch?v=jEgVto5QME8

A longer tutorial: https://www.youtube.com/watch?v=fO7g0pnWaRA