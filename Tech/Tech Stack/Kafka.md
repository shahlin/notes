# Kafka
## Introduction
1. Event streaming platform
2. An event is basically `Notification` + `State`
3. An event in Kafka is just a key-value pair
4. The value can be a domain object (serialized) or any kind of raw message input
5. Keys can be some complex serialized values but usually they're just primitive types like strings or integers

## Topics
1. The fundamental way of event organization in Kafka is by using topics
2. Topics != Queue. Topics are a log of events. They're append only. No inserting in between
3. Topics are like a relational database table. They have a name and they're a container for similar things
4. Can duplicate data between topics. For example, you can have one topic with all the data for daily events and then another topic filtered to have just afternoon events
5. Kafka topics are not indexed
6. Events in a topic are immutable
7. Topics are durable. Expiry is configurable but unlimited duration is also allowed

## Partitioning
1. Breaks a single topic into multiple topics
2. Basically, spans a single topic over multiple partitions
3. If there is no key provided (key is null), the events are stored in a round robin order. Each event will be stored by circling through each of the partitions evenly.
4. If there is a key provided, the key is sent through a hash function, the output of that is passed to a mod function (modulus). So: `partionToStoreIn = hash(key) % numOfPartitions`
5. In the above case, there is no order maintained. If you need order, then you need to pick a key that preserves that order.

## Brokers
1. Brokers are computers, instances or containers running Kafka process
2. They manage partitions
3. Handle read and write requests
4. Manage replication of partitions

## Replication
1. Copies of data for fault tolerance
2. The copied partitions are called Followers and the partition from which they were copied are called Leaders
3. Every partition that is replicated has 1 leader and `n-1` followers
4. Whenever you write data, you're always writing to the leader

## Producers
1. Producers write data to topics
2. Producers can choose to receive acknowledgment during data writes.
    2.1. Acks = 0: Producer won't wait for acknowledgment (possible data loss)
    2.2. Acks = 1: Producer will wait for leader acknowledgment (limited data loss)
    2.3. Acks = all: Leader + replicas acknowledgment (no data loss)

## Consumers
1. Consumers can read data from topics
2. Reading a message from the topic does not destroy the message unlike messaging queues
3. Consumers usually grow more than producers over time

## Kafka Connect
1. Tool to connect Kafka to other services like AWS S3 or JDBC, ElasticSearch, etc and read or write logs to/from Kafka topics
2. Lots of connectors available to choose from
3. Can write your own connector using the Connect API
