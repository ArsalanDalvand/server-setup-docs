---
title: "Coordination mechanism"
source: "https://stalw.art/docs/cluster/configuration/coordination"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/configuration/coordination"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Coordination mechanism"
---
# Coordination mechanism

      

Stalwart requires a coordination mechanism to enable communication between nodes in a cluster. Coordination allows nodes to exchange internal updates, detect failures, and remain synchronised during operation. For a full overview of how coordination works and the available options (peer-to-peer, Kafka, NATS, and Redis), refer to the [[cluster/coordination|Coordination]] section of this documentation.

Coordination is configured on the [[ref/object/coordinator|Coordinator]] singleton (found in the WebUI under  Settings ›  Cluster › Coordinator). The singleton is a multi-variant object: selecting a variant chooses the backend, and each variant carries its own fields. The supported variants are:

`Disabled`: coordination is turned off. Suitable for single-node installations.
`Default`: uses the configured [[storage/in-memory|in-memory store]] for coordination (Redis only).
`Zenoh`: peer-to-peer coordination over Eclipse Zenoh.
`Kafka`: Apache Kafka or a Kafka-compatible platform such as Redpanda.
`Nats`: NATS messaging.
`Redis`: a standalone Redis server.
`RedisCluster`: a Redis Cluster deployment.

All nodes in the cluster must use the same coordination backend to function correctly. The backend-specific configuration fields are covered in the dedicated pages for [[cluster/coordination/peer-to-peer|peer-to-peer]], [[cluster/coordination/kafka|Kafka]], [[cluster/coordination/nats|NATS]], and [[cluster/coordination/redis|Redis]].