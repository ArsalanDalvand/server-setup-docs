---
title: "NATS"
source: "https://stalw.art/docs/cluster/coordination/nats"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/coordination/nats"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "NATS"
---
# NATS

      

NATS is a lightweight messaging system built for cloud-native and edge environments. It provides a publish/subscribe model for real-time communication with low latency and low overhead. NATS suits distributed systems that need fast, reliable event propagation without the operational complexity of heavier platforms such as Apache Kafka.

As a coordination backend in a Stalwart cluster, NATS handles the exchange of internal events between nodes, keeping the cluster synchronised. When a mailbox is updated on one node, a push notification is triggered, or an IP address is blocked for abuse, NATS disseminates the information to the other nodes.

NATS is well-matched to medium-sized deployments where low-latency communication is essential but Kafka-level durability and persistence are not required. Its compact footprint and straightforward configuration also suit teams that want to minimise operational overhead while retaining reliable coordination.

NATS does not persist messages in the same way as Kafka (unless extended via JetStream), but it excels in real-time scenarios where speed and efficiency matter more than long-term storage. For many organisations, it balances performance, simplicity, and scalability for distributed mail coordination.

## Configuration

[Section titled “Configuration”](#configuration)

NATS coordination is configured by selecting the `Nats` variant on the [[ref/object/coordinator|Coordinator]] singleton (found in the WebUI under  Settings ›  Cluster › Coordinator). The variant carries the following fields:

[[ref/object/coordinator#addresses|`addresses`]]: list of NATS server addresses. Default `["127.0.0.1:4444"]`; at least one address is required.
[[ref/object/coordinator#authusername|`authUsername`]] and [[ref/object/coordinator#authsecret|`authSecret`]]: optional basic authentication credentials. `authSecret` is a [[ref/object/coordinator#secretkeyoptional|SecretKeyOptional]], which accepts a direct value, an environment-variable reference, or a file reference.
[[ref/object/coordinator#credentials|`credentials`]]: a [[ref/object/coordinator#secrettextoptional|SecretTextOptional]] holding a JWT used for authentication; an alternative to the username/password pair.
[[ref/object/coordinator#noecho|`noEcho`]]: when enabled, prevents clients from receiving messages they publish themselves. Recommended for coordination to avoid redundant updates. Default `true`.
[[ref/object/coordinator#usetls|`useTls`]]: enables TLS for encrypted communication with NATS servers. Default `false`.
[[ref/object/coordinator#timeoutconnection|`timeoutConnection`]]: maximum time to wait when establishing a connection. Default `"5s"`.
[[ref/object/coordinator#timeoutrequest|`timeoutRequest`]]: timeout for request/response operations. Default `"10s"`.
[[ref/object/coordinator#pinginterval|`pingInterval`]]: interval between pings sent to maintain connection health. Default `"60s"`.
[[ref/object/coordinator#maxreconnects|`maxReconnects`]]: number of reconnection attempts after a connection is lost. When left unset the client reconnects indefinitely.
[[ref/object/coordinator#capacityclient|`capacityClient`]]: internal channel size for outgoing client operations. Default `2048`.
[[ref/object/coordinator#capacitysubscription|`capacitySubscription`]]: capacity of the internal channel for message subscriptions. Default `65536`.
[[ref/object/coordinator#capacityreadbuffer|`capacityReadBuffer`]]: size of the read buffer for incoming messages. Default `65535`.

For example:

```
{  "@type": "Nats",  "addresses": ["nats1:4222", "nats2:4222"],  "authUsername": "nats-user",  "authSecret": {    "@type": "Value",    "secret": "nats-password"  },  "pingInterval": "60s",  "noEcho": true,  "useTls": false,  "timeoutConnection": "5s",  "timeoutRequest": "10s",  "capacityClient": 2048,  "capacitySubscription": 65536,  "capacityReadBuffer": 65535}
```