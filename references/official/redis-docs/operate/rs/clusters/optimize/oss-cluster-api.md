---
title: "OSS Cluster API"
source: "https://redis.io/docs/latest/operate/rs/clusters/optimize/oss-cluster-api/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/optimize/oss-cluster-api"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "OSS Cluster API"
---
# OSS Cluster API

Use the Redis OSS Cluster API to improve performance and keep applications current with cluster topology changes.

            Redis Software
          

            Redis Cloud
          

            Redis Enterprise for Kubernetes
          

OSS Cluster API reduces access times and latency with near-linear scalability.
The OSS Cluster API provides a simple mechanism for Redis clients to know the cluster topology.

Clients must first connect to the master node to get the cluster topology,
and then they connect directly to the Redis proxy on each node that hosts a master shard.

Note:
  You must use a client that supports the cluster API to connect to a database
that has the cluster API enabled.

You can use the Redis OSS Cluster API along with other Redis Enterprise Software high availability
to get high performance with low latency
and let applications stay current with cluster topology changes, including add node, remove node, and node failover.

For more about working with the OSS Cluster API in Redis Software, see [Enable OSS Cluster API](/docs/latest/operate/rs/databases/configure/oss-cluster-api/).

To learn how to enable OSS Cluster API in Redis Cloud, see [Clustering Redis databases](/docs/latest/operate/rc/databases/configuration/clustering/#cluster-api).

To enable OSS Cluster API in Kubernetes, see [Enable cluster-aware clients (OSS Cluster API)](/docs/latest/operate/kubernetes/networking/cluster-aware-clients/).

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)