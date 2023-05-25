# Tableau

Parent: [[01_Tools]]
See also: [[dataviz]]

#tools #datavis


Cool interactive visual tool for interactive dashboard creation. Proprietary. Several versions, including public (like googledocs, in a way), personal, professional. Server for local deployment, online for cloud subscription (going cloud seems to be a trend). There's also a "reader" that can be used for sharing data with people who don't have Tableau installed.

# Creating a visualization

First, you connect it to data; typically, by creating a DB connection (but can also use static json, excel, etc.) Add tables in long form, join them by id (except that Tableau manuals pretend that it's not a join, but something a bit more flexible, but for now I don't feel the difference). It even tries to guess how to join them. 

For the fields, T uses some funny terminology:
* **dimensions** are things one can be grouping by, and filter by (qualitative, that I think of as "factors")
* **measures**  are numerical values to plot.

T also tries to guess the type of each data column, including **dates**, **geography** (city names, country names - are all auto-recognized as geography), **strings**, **numbers**.  Measures can also be treated as continuous or discrete (which be changed at the dashboard construction stage, as is bound to the visualization, not to the data itself). For dates, we can also switch between treating them as continuous (good for linear plots) or discrete values (good for showing similar plots by year, for example). Essentially it's as if we had `YEAR(mydate)` or `factor(YEAR(mydate))` derivative functions. With dates, it is also possible to create hierarchies (see below), but I don't like them.

The interface is similar to Excel pivot tables, but for visuals, where columns in the dataset can control: **columns** (essentially, X axis), **rows** (Y axis), aggregation level (what does a single point represent - this goes onto the **Marks** panel), filters to run (**Filters** panel), colors of the chart, marker sizes, column widths, etc. (different sub-parts of the "Marks" panel). For each of these "inputs" you drag-n-drop variables from a list on the left onto a respective panel / control-element for a visual. You can also apply aggregation functions, and so instead of setting `y=sales`, look at `SUM(sales)`, `MEAN(sales)`, `COUNTD` (for "count-distrinct") etc. For "columns" (X axis) it automatically does really nice hierarchical visuals (with dates, for example, where you can have it split first by year, and then by month etc.) The visualizations are available from the "Show me" button in the top right corner, but can also be changed later on (in the "Marks" panel).

Within the visualization, items can be sorted (sorting buttons in the toolbar), in different ways (by value, alphabetically etc.) We can drop more than one category into the "Columns" line on top, and get a hierarchical, grouped view (bars put in groups for a bar chart, or a "facet"-like view of several continuous charts). We can also sort of "bind" this grouping to the data, by dragging the subcategory on top of a category in the data field itself, and creating a **Hierarchy** (a shortcut for a bunch of variables, with their hierarchical relations baked-in). This "derivative variable" also allows doing drill-in in the visualization, with hierarchical filtering (that pretends to be the top category at first, but gives access to more granular categories if selected).

We can create **Calculated fields** by right-clicking in the white space below the list of existing values, and then typing a formula. If you want the formula to still make sense during filtering and aggregation (as opposed to be kinda calculated once at the line-by-line level), it may make sense to define it in an aggregated way: like `SUM(y1)/SUM(y2)` for example. Then it will stay valid even as you drill through the data (which for fields like profitability is what you actually want to happen). Has a bunch of aggregation 

# Dashboards

Finally, you get these individual visualizations called **Sheets**, and combine them into a **Dashboard**. Things like filtering can be done dashboard-wide, and then  user can click through it and change the all visuals within a dashboard. Essentially, one can use  one view (say value by date) to act as a filter for another view (say, values by group), so clicking the timeline would change the per-group split, and the other way around. This works great with geography, as it can update the map based on time-filtering, or update the time-plot based on the geo-filtering etc.

And then on top of these automated "actions" (filter, highlight) we can also create custom actions on a dashboard, to create a dynamic interface.

# Deployment

When sharing on the web, the description of a dashboard is stored as an html, with divs and everything, and  `<object class=...>`  tags to host actual visualizations. `<param name=... value...>` tags specify parameters specify parameters. `host_url` parameter points at the server, and that's where the data comes from, while a script (written in [[javascript]]) will take care of working with it.

# Level of Data Aggregation (LOD)

Some paradigmatic usages:

### Calculate the average of daily maxima

Instead of a simple basic `MAX([value])`, write: `AVG({INCLUDE DATE([time]): MAX([value])})`

In practice, this processing is probably used together with some other grouping: perhaps by `id` (the way reporting is built), and some higher-order groups of id. Which means that `INCLUDE` command adds the date to `id`, and then runs `MAX` on this granules, then `AVG` is run on these values.

### Finding latest used value for an id

In a normal world (like, in [[pandas]], for example), you would probably summarize the table, finding the last value of the target field after grouping by id and sorting by data, and then left-joined this summary table back on the full table by id. Tableau doesn't have normal joins though, but we can emulate this logic with LODs. 

```
{FIXED [id]: 
    MAX(
        IF [date] = {FIXED [id]: MAX([date])} 
        THEN [name]
        END
    )
}
```
Code analysis: The inner part returns `None` for non-recent values, and target value (`name` in this case) for the most recent value. Just to be on a safe side, we also calculate the most recent value separately for each `id`, although if the data is orthogonal on `time/id` dimensions, this is technically not necessary. Then it calculates `max()` on them, and real values are `>` than `none`, so they win in this battle. And finally, we match these values to `id` (the part that feels most similar to to a join, conceptually).

# Maps

See also: [[maps]]

Refs:
* https://www.tableau.com/blog/10-map-tips-tableau-62949 - an intresting intro to making custom maps from layers tha I haven't quite tried yet. Apparently when working with OpenStreetMap one can remove the background layer, but manually add vector components of the map (?), as described here: https://help.tableau.com/current/pro/desktop/en-us/maps_options.htm

# Misc Hacks

**Renaming Columns and Rows** is apparently quite easy using an "inline custom calculation", as if a calculation starts with a comment, then this comment becomes the title of this calculation. So if you have a field named `Ugly`, and you want to hide its name in the dashboard, then instead of using it directly (as `[Ugly]`), do this:
```
//Pretty
[Ugly]
```
And it will be shown everyehwere as "Pretty" :) Even if the editing window seems to be a one-liner (say, the windows inside the Column / Row fields on top), they are actually secretly multi-line, and one can go to the next line with Shift-Enter.

Similarly, this hack can be really useful for creating ad-hoc calculated fields: if you aren't going to reuse this field, and only need to do some cosmetic improvement once (like flip the sign, or something like that), istead of using a field directly, do something to it (like `-[X]` for example), and use this "Naming with a multiline comment" trick to give it a reasonable name.

🛑 Unfortunately, it seems that renaming a field breaks all action-based filters, if you have one part of your dashboard affect some other part. One would have thought that generating a `//Pretty; [Ugly]` field would still retain the connection between `Pretty` and `Ugly`, and thus between `Ugly` and all the filters that it was involved in, but it seems that on-the-fly this connection is not established. If you create a calculated field named `Pretty`, with the same trivial formula of `[Ugly]`, then filters based on `Pretty` work. But if you do it "on the fly" (aka "in the shelf"), the on-the-fly field is shown in the action dialog, but doesn't seem to work.

Footnotes:
* https://vizpainter.com/10-things-you-didnt-know-about-ad-hoc-calculations-in-tableau-9/

# Other

Apparently, one can write their own code in Tableau version of [[javascript]], and it also has something like internal restricted [[python]], but I haven't tried either yet.

# Competitors

Apache Superset
https://en.wikipedia.org/wiki/Apache_Superset

PowerBI from Microsoft

# Refs

Basic intros and tutorials:
* Decent intro video: https://www.youtube.com/watch?v=jEgVto5QME8
* A longer tutorial: https://www.youtube.com/watch?v=fO7g0pnWaRA

LODs
* https://www.flerlagetwins.com/2020/02/lod-uses.html