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




# Test 1 
## Create a single partition topic, create one consumer and see the result, then add a new one to the same group.
## Expectation: creating the second consumer on the single partition topic will not make any change (consumers count needs to be <= TOPIC_PARTITION_COUNT)
```
> bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic topic-with-one-partition
```
```
> bin/kafka-topics.sh --list --bootstrap-server localhost:9092
__consumer_offsets
topic-with-one-partition
```
- Create a consumer for the topic with a group id `group-1`
```
> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topic-with-one-partition --from-beginning --group group-1
```
### describe the group just created
```
> bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group group-1

GROUP           TOPIC                    PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                             HOST            CLIENT-ID
group-1         topic-with-one-partition 0          0               0               0               consumer-group-1-1-22d36a91-7adb-474f-94bc-d37cdb40721b /127.0.0.1      consumer-group-1-1
```
- Create a new consumer on the same topic and let's make it join the group with id `group-1`
```
> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topic-with-one-partition --from-beginning --group group-1
```
### adding new consumer with only one partition hasn't changed anything (number of consumer can't be greater than the numnber of partitions!)
```
> bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group group-1

GROUP           TOPIC                    PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                             HOST            CLIENT-ID
group-1         topic-with-one-partition 0          0               0               0               consumer-group-1-1-22d36a91-7adb-474f-94bc-d37cdb40721b /127.0.0.1      consumer-group-1-1
```

