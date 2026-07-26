---
title: "Apache Kafka / Redpanda"
source: "https://stalw.art/docs/cluster/coordination/kafka"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/coordination/kafka"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Apache Kafka / Redpanda"
---
# Apache Kafka / Redpanda

      

Apache Kafka is a distributed event streaming platform for publishing, subscribing to, storing, and processing large volumes of real-time data. It is widely used in distributed systems to provide reliable communication between services, particularly where durability, scalability, and fault tolerance are critical.

Redpanda is a Kafka-compatible streaming data platform that acts as a drop-in replacement for Kafka, offering similar semantics and APIs with lower latency and simpler operations. It is written in C++ and removes Kafka’s JVM-based dependencies such as ZooKeeper.

When used as a coordination backend in a Stalwart cluster, Kafka or Redpanda acts as the message bus for distributing internal updates between nodes. These updates include mailbox changes for IMAP IDLE, push notification events, IP blocking alerts, TLS certificate distribution, and more. By publishing and subscribing to coordination topics, each Stalwart node stays synchronised with the rest of the cluster.

Using Kafka or Redpanda provides a durable, centralised coordination layer. Messages are reliably delivered even when a node fails, and recovering nodes can catch up on missed updates by replaying messages from the log. Kafka and Redpanda are a good fit for very large Stalwart deployments that require strict delivery guarantees.

Kafka adds infrastructure requirements, but its ability to handle millions of events per second, support long-term message retention, and integrate with a wide ecosystem of tools makes it well-suited to enterprises with complex or high-traffic mail infrastructures.

## Configuration

[Section titled “Configuration”](#configuration)

Kafka coordination is configured by selecting the `Kafka` variant on the [[ref/object/coordinator|Coordinator]] singleton (found in the WebUI under  Settings ›  Cluster › Coordinator). The variant carries the following fields:

[[ref/object/coordinator#brokers|`brokers`]]: list of Kafka broker addresses used to bootstrap the client connection. At least one broker is required; without it the Kafka coordinator fails to initialise.
[[ref/object/coordinator#groupid|`groupId`]]: Kafka consumer group identifier that each Stalwart node joins. It coordinates consumption of coordination events among cluster members.
[[ref/object/coordinator#timeoutsession|`timeoutSession`]]: session timeout for the Kafka consumer. If the broker receives no heartbeat from a consumer within this window, it treats the consumer as failed and rebalances the group. Default `"5s"`.
[[ref/object/coordinator#timeoutmessage|`timeoutMessage`]]: maximum time to wait for a message to be delivered to Kafka. Exceeding this returns an error. Default `"5s"`.

For example:

```
{  "@type": "Kafka",  "brokers": ["kafka1:9092", "kafka2:9092"],  "groupId": "stalwart-cluster",  "timeoutSession": "5s",  "timeoutMessage": "5s"}
```