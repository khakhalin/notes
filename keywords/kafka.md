# Kafka

Parents: [[streaming]]
See also: [[datalake]], [[rabbitmq]]

#db #systems


An open-source distributed event store and stream-processing platform. High-throughput low-latency solution to handle real-time data feeds. A typical message is a log of several messages bundled together and passed via 1 TCP packet (to make things faster); each message is a key-value pair. Kafka cluster sorts messages by topics and partitions, distributed over cluster nodes (if necessary), where messages are recorded. Conversely, one can request messages from a cluster, and read them. Kafka can also run Java code to read messages and write them back.

It was developed by Linkedin, written in Scala, and then donated to Apache.

Kafka is a direct competitor to [[rabbitmq]]: allegedly, RabbitMQ is easier to set up, but Kafka scales better with volume.

# Refs

https://en.wikipedia.org/wiki/Apache_Kafka

https://kafka.apache.org/documentation/#gettingStarted