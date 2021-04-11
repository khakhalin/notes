# Tableau

Parent: [[01_Tools]]
See also: [[dataviz]]

#tools #datavis


Cool interactive visual tool for interactive dashboard creation.

Several versions, including public (like googledocs, in a way), personal, professional. Server - for local deployment, online - for cloud subscription. Reader - for sharing data with people who don't have Tableau.

# Interface creation

You connect it to data (json, txt, excel, server-side ?? etc.) You kinda add tables (in long form), and you can join them by id. (It even tries to guess how to join them). Uses funny terminology: dimensions (for grouping) and measures (numerical values). The interface very similar to Excel pivot tables, but for visuals: rows, columns, filters, colors, etc., and for each of these "inputs" you can drag-n-drop variables there. You can also apply aggregation functions. For "columns" (future X axis) it automatically does really nice hierarchical visuals (with dates, for example, where you can have it split first by year, and then by month etc.)

Finally, you get these individual visualizations called Sheets, and combine them into a Dashboard. Things like filtering can be also brought to dashboard, and then the user can click through it and change the visuals, within the dashboard. It's also possible to use one view (say value by date) to act as a filter for another visual (say, values by group), so clicking on the timeline will change the per-group split, and the other way around. (There's a funnel button for it). This also works great with geography, as it can auto-plot it on the map, and then the map participates in the same dynamic filtering as other visuals.

And then on top of these automated "actions" (filter, highlight) we can also create custom actions on a dashboard, to create a dynamic interface.

# Data storage and processing

It has its own format of storing data, and an interface to create data cleaning and processing pipelines (ETL = Extract Transform Load)

You can write your own code in Tableau version of [[javascript]].

# Deployment

?

When sharing on the web, the description of a dashboard is stored as an html, with divs and everything, and  `<object class=...>`  tags to host actual visualizations. `<param name=... value...>` tags specify parameters specify parameters. `host_url` parameter will link at the server, and that's where the data comes from, while a script (written in [[javascript]]) will take care of working with it.

# Refs

Decent intro video: https://www.youtube.com/watch?v=jEgVto5QME8

A longer tutorial: https://www.youtube.com/watch?v=fO7g0pnWaRA