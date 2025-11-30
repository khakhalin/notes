# Streaming

Parents: [[system_design]]
See also: [[database]], [[parallelism]]

#systems


.

 **Message queuing / message broker:**
 * [[kafka]] - open-source, more performant, faster and higher volumes, but harder to configure
 * [[rabbitmq]] - open source, easier to work with, especially for smaller applications

**Stream processor:** a piece of software that receives the stream of individual messages and aggregates them slightly, say, at one-second granularity for large-data real-time processes, to make it database-ingestable.
* flink