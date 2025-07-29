# sb_apache_kafka
This project demonstrates how to integrate Apache Kafka with Spring Boot, covering both producer and consumer configurations. It includes examples of sending and receiving string messages, JSON payloads, handling Kafka topics, and customizing Kafka listeners. Ideal for developers looking to learn event-driven architecture and real-time data streaming using Kafka and Spring Boot.
------------------------------------------------
Apache Kafka Architecture - Detailed Explanation
------------------------------------------------

Overview:
---------
Apache Kafka is a distributed event streaming platform. Its architecture is designed for high throughput, fault tolerance, horizontal scalability, and real-time processing.

Kafka follows a publish-subscribe model where producers publish events (messages) to topics, and consumers subscribe to those topics.

Key Components:
---------------

1. Producer
-----------
- Producers are client applications that send data (messages) to Kafka topics.
- They push records to a specific topic and optionally to a specific partition.
- Messages can be sent with or without keys.
- A key ensures that messages with the same key go to the same partition.

2. Consumer
-----------
- Consumers are applications that subscribe to one or more topics and read messages.
- They pull messages from Kafka brokers.
- They maintain an offset (position) for each partition they consume from.
- Consumers can be grouped into "consumer groups" for load balancing.

3. Topic
--------
- A topic is a named stream/category to which records are sent by producers.
- Topics are split into multiple partitions for scalability and parallelism.
- Topics are durable and persistent by default.
- Examples: orders, payments, logs.

4. Partition
------------
- A partition is an ordered, immutable sequence of records.
- Each topic can have one or more partitions.
- Partitions enable Kafka to scale horizontally.
- Kafka maintains the order of records within a partition, not across partitions.

5. Offset
---------
- Offset is a unique ID assigned to each record in a partition.
- Consumers use the offset to track which messages have been read.
- Offsets are stored in Kafka or in an external system (e.g., ZooKeeper or Kafka's internal __consumer_offsets topic).

6. Kafka Broker
---------------
- A Kafka broker is a server that stores data and handles requests from producers and consumers.
- Each broker can handle thousands of partitions.
- Brokers manage topic partitions and coordinate with the controller.
- Brokers handle replication of partition data across the cluster.

7. Kafka Cluster
----------------
- A Kafka cluster is made up of multiple brokers.
- The cluster handles load balancing, replication, failover, and scaling.
- Each broker in the cluster has a unique broker ID.
- Clients can connect to any broker (called a bootstrap broker) to interact with the cluster.

8. Leader and Follower (Replication)
------------------------------------
- Kafka ensures high availability by replicating partitions across multiple brokers.
- One broker is elected as the leader for each partition.
- Other brokers with replicas act as followers.
- Only the leader handles reads and writes; followers replicate the data.
- If a leader fails, a follower is promoted to leader.

9. Controller
-------------
- The controller is a Kafka broker responsible for:
  - Electing partition leaders.
  - Handling broker joins and failures.
  - Managing topic and partition metadata.
- In ZooKeeper mode, the controller is elected via ZooKeeper.
- In KRaft mode, the controller is elected using Kafka's internal Raft consensus.

10. ZooKeeper (legacy mode)
---------------------------
- Used to manage metadata (before KRaft).
- Handles broker registrations, controller election, and topic metadata.
- Kafka communicates with ZooKeeper for cluster coordination.
- Deprecated in favor of KRaft mode in Kafka 3.x and above.

11. KRaft Mode (Kafka Raft Metadata Mode)
-----------------------------------------
- Kafka-native metadata management (no ZooKeeper).
- Uses Raft protocol to replicate metadata logs across controller nodes.
- Simplifies setup and improves performance.
- KRaft supports controller and broker separation or combination.

12. Consumer Group
------------------
- A consumer group is a group of consumers that coordinate to consume data from a topic.
- Each partition in a topic is consumed by only one consumer in the group.
- Enables horizontal scaling and parallel processing.
- Supports fault tolerance: if one consumer fails, another can take over.

13. Kafka Topic Log
--------------------
- Each partition in Kafka is a commit log stored on disk.
- Messages are appended to the log file.
- Kafka uses segment files to manage log size.
- Old data is deleted or compacted based on retention policy.

14. Retention and Compaction
----------------------------
- Kafka retains messages for a configurable time (retention.ms) or size.
- Log compaction allows Kafka to retain only the latest value per key.
- Useful for changelog or stateful applications.

15. Kafka Connect (Connector Framework)
---------------------------------------
- A framework to connect Kafka with external systems (DBs, files, cloud storage, etc.).
- Supports source connectors (import data) and sink connectors (export data).
- Example: JDBC connector to stream data from MySQL to Kafka.

16. Kafka Streams (Stream Processing)
-------------------------------------
- A lightweight Java library for real-time processing of Kafka data.
- Supports stateful and stateless operations.
- Scales elastically and is fault tolerant.
- Example: Filtering, grouping, windowed aggregations.

17. Schema Registry (optional)
------------------------------
- Manages schemas for Kafka messages (e.g., Avro, Protobuf, JSON).
- Ensures producers and consumers agree on the structure of data.
- Enables schema evolution and backward compatibility.

18. Admin Client
----------------
- Kafka provides an admin client to programmatically manage topics, brokers, ACLs, and configurations.
- Useful for automated Kafka operations in production systems.

Communication Flow:
-------------------
1. Producer connects to a broker and sends messages to a topic.
2. Broker stores messages in partition logs and replicates to followers.
3. Consumer connects to the broker and fetches messages from partitions.
4. Controller manages partition leadership and metadata.
5. Kafka Connect and Kafka Streams plug into the system for integrations and processing.

Summary:
--------
Kafka’s architecture is designed for:
- High throughput
- Horizontal scalability
- Durability and fault tolerance
- Real-time event streaming
- Decoupling between microservices

With modular components like brokers, partitions, controllers, and replication, Kafka provides a powerful backbone for data pipelines and event-driven systems.
------------------------------------------------
1. What is Apache Kafka, and how does it facilitate communication in a microservices architecture?

Apache Kafka is a distributed, high-throughput, fault-tolerant event streaming platform designed for real-time data pipelines.

In microservices, Kafka acts as a message broker. It enables asynchronous communication between services:
- Services publish events to Kafka topics.
- Other services consume those events.
- This decouples services, enhances scalability, and improves system resiliency.

Kafka provides:
- Loose coupling between producers and consumers.
- Durable message storage.
- Replayability of events.
- High fault tolerance and horizontal scalability.

---

2. Can you explain event-driven architecture using Apache Kafka?

Event-driven architecture (EDA) is a design paradigm where services react to events.

With Apache Kafka:
- Producers publish events to Kafka topics.
- Consumers subscribe to topics and react to those events.
- Events are stored durably in Kafka for replay and audit.

Example:
- Service A emits an "OrderPlaced" event.
- Service B listens and initiates "InventoryCheck".
- Service C listens and sends confirmation email.

Kafka ensures reliable delivery, multiple subscribers, and decoupled communication.

---

3. What is the difference between a message and an event in Apache Kafka, and what is the purpose of each?

- Message: A data record transmitted through Kafka (includes key, value, headers).
- Event: The meaning behind the message, e.g., "UserRegistered", "OrderCompleted".

Purpose:
- A message carries the data.
- An event represents a change or action in the system.

Every event is transported as a message in Kafka.

---

4. How do Kafka topics, partitions, and offsets differ, and what are their roles?

- Topic: A logical channel to which messages are sent and read.
- Partition: A sub-log of a topic. Kafka splits topics into partitions for parallelism.
- Offset: A unique identifier for each message within a partition.

Roles:
- Partitions enable parallelism and scalability.
- Offsets track message position and allow replay.

---

5. How is event ordering managed in Apache Kafka?

Kafka guarantees message ordering only within a single partition.

To maintain order:
- Use the same key while producing (e.g., userId).
- All messages with the same key go to the same partition.
- A single consumer per partition maintains order.

---

6. What is a Kafka broker, and how does it become a leader or follower for a partition?

A Kafka broker is a server that stores messages and serves producer/consumer requests.

Each partition has:
- One leader broker (handles all reads/writes).
- Zero or more follower brokers (replicate data).

Leader election:
- ZooKeeper mode: ZooKeeper handles elections.
- KRaft mode: Kafka's controller node handles elections via Raft protocol.

---

7. What is the difference between ZooKeeper and KRaft in Kafka, and why is KRaft preferred in modern deployments?

ZooKeeper:
- External coordination service.
- Manages broker metadata, leader election.

KRaft (Kafka Raft):
- Kafka-native metadata quorum.
- No ZooKeeper dependency.

KRaft Benefits:
- Simpler deployment.
- Higher performance.
- Better integration with Kafka internals.
- Preferred for modern clusters (Kafka 3+).

---

8. How do you start a single Kafka broker instance?

1. Configure `server.properties`:

broker.id=1  
log.dirs=/tmp/kafka-logs  
listeners=PLAINTEXT://localhost:9092  

2. Start the broker:

bin/kafka-server-start.sh config/server.properties

For KRaft mode, include `process.roles=broker,controller` and `node.id`.

---

9. How do you start multiple Kafka broker instances for a multi-node setup?

1. Create config files:
- broker-1.properties
- broker-2.properties

Change:
- broker.id
- log.dirs
- port/listeners

2. Start brokers:

bin/kafka-server-start.sh config/broker-1.properties  
bin/kafka-server-start.sh config/broker-2.properties  

For KRaft mode: ensure unique `node.id` and shared `controller.quorum.voters`.

---

10. How can Kafka be configured to achieve high throughput, low latency, scalability, data integrity, and reliability?

High Throughput:
- batch.size
- compression.type (snappy/lz4)
- linger.ms

Low Latency:
- fast disks (e.g., SSD)
- optimize acks and batch sizes

Scalability:
- increase partitions
- add more brokers

Data Integrity:
- acks=all
- min.insync.replicas
- replication.factor >= 2

Reliability:
- enable log compaction
- use monitoring (Prometheus, Grafana)

---

11. How do you create, list, describe, and delete a Kafka topic using the CLI?

Create:
bin/kafka-topics.sh --create --topic my-topic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 2

List:
bin/kafka-topics.sh --list --bootstrap-server localhost:9092

Describe:
bin/kafka-topics.sh --describe --topic my-topic --bootstrap-server localhost:9092

Delete:
bin/kafka-topics.sh --delete --topic my-topic --bootstrap-server localhost:9092

Additional arguments:
--config retention.ms=60000  
--config cleanup.policy=compact  

---

12. How do you produce messages to a Kafka topic using the CLI, both with and without a key?

Without key:
bin/kafka-console-producer.sh --topic my-topic --bootstrap-server localhost:9092

With key:
bin/kafka-console-producer.sh --topic my-topic --bootstrap-server localhost:9092 --property "parse.key=true" --property "key.separator=:"

Input format:
user1:Hello  
user2:World  

---

13. How do you consume messages from the beginning of a Kafka topic using the CLI?

bin/kafka-console-consumer.sh --topic my-topic --from-beginning --bootstrap-server localhost:9092

---

14. How do you consume only the new (latest) messages from a Kafka topic using the CLI?

bin/kafka-console-consumer.sh --topic my-topic --bootstrap-server localhost:9092

(Default behavior is to consume from latest offset.)

---

15. How do you consume key-value messages from a Kafka topic using the CLI?

bin/kafka-console-consumer.sh --topic my-topic --bootstrap-server localhost:9092 --property print.key=true --property key.separator=:

Output:
user1:Hello  
user2:World  

---

16. How do you ensure ordered consumption of messages from a Kafka topic using the CLI?

Kafka guarantees order within a partition.

To consume in order:
- Ensure the producer uses the same key for a logical entity (same partition).
- Consume from a specific partition:

bin/kafka-console-consumer.sh --topic my-topic --partition 0 --bootstrap-server localhost:9092

This ensures ordered consumption for that partition.

---



