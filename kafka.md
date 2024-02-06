# Apache Kafka Cheat Sheet

## Introduction to Apache Kafka

Apache Kafka is a distributed streaming platform that enables building real-time data pipelines and streaming applications. It is widely used for high-throughput, fault-tolerant messaging, as well as enabling stream processing. Kafka works on the concept of topics, producers, and consumers.

## Key Concepts

- **Topic**: A category or feed name to which records are published. Topics in Kafka are multi-subscriber.

- **Producer**: An entity that publishes data to Kafka topics.

- **Consumer**: An entity that subscribes to topics and processes the feed of published records.

- **Broker**: A Kafka server that stores data and serves clients.

- **Cluster**: A group of Kafka brokers.

- **Partition**: A division of a topic for scalability and parallel processing. Topics can have multiple partitions.

- **Offset**: A unique identifier of records within a partition.

- **Zookeeper**: A service for coordinating and managing the Kafka brokers.

## Basic Kafka Operations

- **Starting a Kafka Server**
  - Kafka requires Zookeeper for running. Start Zookeeper before starting the Kafka server.

- **Creating a Topic**
  - To create a topic, use the command:
    ```shell
    kafka-topics.sh --create --zookeeper [ZOOKEEPER_HOST:PORT] --replication-factor [NUMBER] --partitions [NUMBER] --topic [TOPIC_NAME]
    ```

- **Listing Topics**
  - To list all topics in the Kafka server:
    ```shell
    kafka-topics.sh --list --zookeeper [ZOOKEEPER_HOST:PORT]
    ```

- **Producing Messages**
  - To send messages to a Kafka topic:
    ```shell
    kafka-console-producer.sh --broker-list [BROKER_LIST] --topic [TOPIC_NAME]
    ```

- **Consuming Messages**
  - To read messages from a Kafka topic:
    ```shell
    kafka-console-consumer.sh --bootstrap-server [BROKER_LIST] --topic [TOPIC_NAME] --from-beginning
    ```

## Advanced Kafka Operations

- **Deleting a Topic**
  - To delete a topic:
    ```shell
    kafka-topics.sh --delete --zookeeper [ZOOKEEPER_HOST:PORT] --topic [TOPIC_NAME]
    ```

- **Modifying Topic Configuration**
  - To change the configuration of a topic:
    ```shell
    kafka-configs.sh --zookeeper [ZOOKEEPER_HOST:PORT] --entity-type topics --entity-name [TOPIC_NAME] --alter --add-config [CONFIG_KEY]=[VALUE]
    ```

- **Kafka Consumer Groups**
  - Consumers can be part of a consumer group for distributed processing:
    ```shell
    kafka-console-consumer.sh --bootstrap-server [BROKER_LIST] --topic [TOPIC_NAME] --group [GROUP_NAME]
    ```

- **Viewing Consumer Group Details**
  - To view details about a consumer group:
    ```shell
    kafka-consumer-groups.sh --bootstrap-server [BROKER_LIST] --describe --group [GROUP_NAME]
    ```

- **Kafka Streams**
  - Kafka Streams API allows building stream processing applications using Kafka.

## Kafka Connect

- **Kafka Connect**
  - A tool for scalably and reliably streaming data between Apache Kafka and other data systems.

- **Running a Kafka Connect Connector**
  - Connectors can be configured and managed via REST APIs or configuration files.
