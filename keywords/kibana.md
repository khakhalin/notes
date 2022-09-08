# Kibana and Elasticsearch

Path: [[01_Tools]]
See also : [[tableau]], logging

#tools #logging


**Kibana** is an almost-open-source (source-available) dashboard / visualization software, that seems to be primarily used to analyze and root-cause logs and near-realtime events. Works with **Elasticsearch**: a realtime search engine targeted at processing [[json]] documents. Ideal for log-parsing.

The most common use it to pick a source on the left (typically, a [[microservice]] on [[kubernetes]]), then filter by fields (by finding fields in the list, and then creating filters on these fields). If the distribution of log messages changed a lot on a particular time period (say, your database writer stopped responding), you kinda know what broke.

To a get a distribution of log message sources, for example, find the field `name` on the left (assuming that your logger reports the name of the method that sent the message, which is pretty much the default behavior), and click to "visualize". At least for me, by defaut it just shows a static split by `name` values, but by adding `timestamp` as a "Horizontal axis" value on the right, it can be turned into a running stacked bar plot, or a line plot (there's a prominent chart type button on the top). Alternatively one can just drag-and-drop fields form the left into various semi-intuitive parts of the dashboard space, and then adjust details if necessary. Say, moving field to the "Vertical axis", changing "count unique" to "count", and then moving the same field again onto the bars in the middle (to turn on splitting by value), achieves the same results (stacked split graphs).

Individual vizualization can be saved and added to dashboards. All dashboards seem to be visible to all users (?), and editable by all users that have a certan type of access (?).

To add logs to the dashboard, go to "Discovery", adjust the search, save it as a "saved search", then go back to your dashboard, click "Add from library" button, and pick the saved search from the list. It will become a new panel.

# Random

Apparently Kibana was named that way but its creator Rashid Khan, who tried to use an african word, as he worked in peace corps Africa and wanted his product to sound African, so he google transalted "log hut" to Swahili, but made a typo when reading the output. Because Kibana actually means "baby" in Swahili, not a "log hut". Source: https://www.quora.com/What-is-the-origin-of-the-name-Kibana

# Refs

Wiki: https://en.wikipedia.org/wiki/Elasticsearch