---
layout: post
title:  "Kafka Consumer"
date:   2024-01-02 12:41:58 +0530
categories: learning
---

<nav>
  <h4>Table of Contents</h4>
  * this unordered seed list will be replaced by toc as unordered list
  {:toc}
</nav>

# Kafka

![Example](/weblogs/assets/example.drawio.png)

Kafka is a distributed real-time data handling system. It stores real-time data and serves it to consumers. 
Clients who post data to kafka are called as `producers` and who listens to get the data are called the `consumers`.

# Kafka Topic

Data is organized into `topics` in Kafka. These are immutable list of data (append only). 

For example, You may have a system to store real-time super-hero data for `Marvel` & `DC` but you don't want to store everything in a single area. We need a way to store these data and process separately. This is the use case for `topics`. We can have `Marvel` and `DC` topics to store the respective data.

## Kafka Partition

Remember, Kafka is a distributed system. That means, data is stored in multiple systems.

If a topic's data is stored in a single system and if that system goes down, no one can read from that topic until the system gets up.

But if we split the topic and store it in multiple places (`partitioning`) we can avoid single point of failure. We can configure the number of partitions to be created for each topic.

Simply, `partition` is nothing but splitting the `topic` data in multiple places. Topic data is distributed across all the partitions. There are algorithms used to distribute like round-robin that you can explore separately.

# Kafka Consumer

Kafka `Consumer` is a client who wants to subscribe to a `topic` data. If I want to get all the `Marvel` super-hero updates then I can subscribe to the `Marvel` topic. Now I can get the real-time updates from `Marvel`. 

## Kafka Consumer Group

We have the `Marvel` data splitted across `partitions` so currently we have a single consumer fetching all the data and processing it. 

But if the data is huge and keep coming then we can make use of the parallel processing across multiple consumers. This is where `consumer groups` shines. We can create multiple consumers with the same `group id` and the coordinator of the topic (usually the kafka broker (server)) will assign the `partitions` to each of them.

Now each consumer can consumer from their respective `partition` and process it. This makes our processing very fast.

## Kafka Consumer Configurations

### `bootstrap.servers`

The Broker (Server) that we are trying to connect to

### `group.id`

The group which the consumer belongs to

### `enable.auto.commit`

Config to enable automatic save of the latest offset read by the `consumer` of the `partition`. Offset is like an index of an array. But in this case the position of the message of a partition

### `auto.offset.reset`

From where the new consumer to start consume from? If the group is created for the first time or when the commit offset is invalid or not available. Example, `earliest` - Read from the beginning of the partition, `latest` - Read the messages that are coming from now on.

### `session.timeout.ms`

If the heartbeat is not sent to the coordinator (broker) within the timeout period then it removes the consumer from the group.

# References

* [https://docs.confluent.io/platform/current/clients/consumer.html](https://docs.confluent.io/platform/current/clients/consumer.html)
* [https://www.baeldung.com/kafka-topics-partitions](https://www.baeldung.com/kafka-topics-partitions)
* [https://stackoverflow.com/questions/38024514/understanding-kafka-topics-and-partitions](https://stackoverflow.com/questions/38024514/understanding-kafka-topics-and-partitions)
* [https://www.google.com/search?q=what+is+kafka](https://www.google.com/search?q=what+is+kafka)