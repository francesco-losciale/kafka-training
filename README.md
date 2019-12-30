Please read the [official Kafka introduction](https://kafka.apache.org/intro) page before practicing the following kata.

[Kafka cheatsheet here](https://github.com/francesco-losciale/cheat-sheets/blob/master/kafka_cluster_first_run.txt).


# Start zookeeper and kafka server

wget http://apache.mirror.anlx.net/kafka/2.4.0/kafka_2.12-2.4.0.tgz

tar -xzf kafka_2.12-2.4.0.tgz \
&& cd kafka_2.12-2.4.0

### start zookeeper (used by kafka)
bin/zookeeper-server-start.sh config/zookeeper.properties

### start the kafka server (single broker for now - ie. cluster of size one)
bin/kafka-server-start.sh config/server.properties

