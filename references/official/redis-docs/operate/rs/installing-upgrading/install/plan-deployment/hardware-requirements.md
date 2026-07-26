---
title: "Hardware requirements"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/plan-deployment/hardware-requirements/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install/plan-deployment/hardware-requirements"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Hardware requirements"
---
# Hardware requirements

Redis Software hardware requirements for development and production environments.

            Redis Software
          

            Redis Enterprise for Kubernetes
          

The hardware requirements for Redis Software are different for development and production environments.

In a development environment, you can test your application with a live database.

If you want to test your application under production conditions, use the production environment requirements.

In a production environment, you must have enough resources to handle the load on the database and recover from failures.

## Architecture

Redis Software supports AMD64 (x86_64) and ARM64 architectures as shown in the following table:

Operating system
AMD64 (x86_64) support
ARM64 support

RHEL 9
✅ Redis Software 7.4.2 and later
✅ Redis Software 8.0.10 and later

RHEL 8
✅ Redis Software 6.2.8 and later
❌ Not supported

Ubuntu 22
✅ Redis Software 7.8.4-66 and later
✅ Redis Software 8.0.10 and later

Ubuntu 20
✅ Redis Software 6.4.2-43 and later
❌ Not supported

Amazon Linux 2
✅ Redis Software 6.4.2-69 and later
❌ Not supported

## DevelopmentÂ environment

You can build your development environment with non-production hardware, such as a laptop, desktop, or small VM or instance,
and with these hardware requirements:

Item
Description
Minimum requirements
Recommended

Nodes per cluster
You can install on one node but many features require at least two nodes.
1 node
>= 2 nodes

RAM per node
The amount of RAM for each node.
4GB
>= 10GB

Storage per node
The amount of storage space for each node.
10GB
>= 20GB

## ProductionÂ environment

We recommend these hardware requirements for production systems or for development systems that are designed to demonstrate production use cases:

Item
Description
Minimum requirements
Recommended

Nodes[1](#table-note-1) per cluster
At least three nodes are required to support a reliable, highly available deployment that handles process failure, node failure, and network split events in a consistent manner.
3 nodes
>= 3 nodes (Must be an odd number of nodes)

Cores[2](#table-note-2) per node
Redis SoftwareÂ is based on a multi-tenant architecture and can run multiple Redis processes (or shards) on the same core without significant performance degradation.
2 cores
>=8 cores

RAM[3](#table-note-3) per node
Defining your RAM size must be part of the capacity planning for your Redis usage.
8GB
>=32GB

Ephemeral storage
Used for storing [[operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage|replication files (RDB format) and cluster log files]].
RAM x 2
>= RAM x 4

Persistent storage[4](#table-note-4)
Used for storing [[operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage|snapshot (RDB format) and AOF files]] over a persistent storage media, such as AWS Elastic Block Storage (EBS) or Azure Data Disk.
RAM x 3
In-memory >= RAM x 4 (except for [[operate/rs/clusters/optimize/disk-sizing-heavy-write-scenarios|extreme 'write' scenarios]])

 [Redis Flex and Auto Tiering](/docs/latest/operate/rs/databases/flash/)Â >= (RAM + Flash) x 4.

Network[5](#table-note-5)
We recommend using multiple NICs per node where each NIC is >1Gbps, but Redis SoftwareÂ can also run over a single 1Gbps interface network used for processing application requests, inter-cluster communication, and storage access.
1G
>=10G

Local disk for [Redis Flex and Auto Tiering](/docs/latest/operate/rs/databases/flash/)
used to to extend databases DRAM capacity with solid state drives (SSDs). Flash memory must be locally attached. [Read more](/docs/latest/operate/rs/databases/flash/)
(RAM+Flash) x 1.6
(RAM+Flash) x 2.5

Additional considerations:

Nodes per cluster:

Clusters with more than 35 nodes are not supported. Please contact the Redis support team for assistance if your sizing calls for deploying a larger number of nodes.

Quorum nodes also must comply with the above minimal hardware requirements.

To ensure synchronization and consistency, Active-Active deployments with three-node clusters should not use quorum nodes. Because quorum nodes do not store data shards, they cannot support replication. In case of a node failure, replica shards aren't available for Active-Active synchronization.

Cores:

When the CPU load reaches a certain level, Redis Software sends an alert to the operator.

If your application is designed to put a lot of load on your Redis database, make sure that you have at least one available core for each shard of your database.

If some of the cluster nodes are utilizing more than 80% of the CPU, consider migrating busy resources to less busy nodes.

If all the cluster nodes are utilizing over 80% of the CPU, highly consider scaling out the cluster by [adding a node](/docs/latest/operate/rs/clusters/add-node/).

RAM:

Redis uses a relatively large number of buffers, which enable replica communication, client communication, pub/sub commands, and more.  As a result, you should ensure that 30% of the RAM is available on each node at any given time.

If one or more cluster nodes utilizes more than 65% of the RAM, consider migrating resources to less active nodes.

If all cluster nodes are utilizing more than 70% of available RAM, highly consider [adding a node](/docs/latest/operate/rs/clusters/add-node/).

Do not run any other memory-intensive processes on the Redis Software node.

Persistent storage:

If no databases on the cluster have [[operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage|persistence]] enabled, minimum persistent storage is RAM x 1.1 and the recommended persistent storage is RAM x 2. Persistent storage is essential because Redis Software also uses it to maintain the cluster and database health, configurations, recovery procedures, and more.

Network:

Only static IP addresses are supported to ensure nodes remain part of the cluster after a reboot.

## Sizing considerations

### General database sizing

Factors to consider when sizing your database.

Dataset size â Your limit should be greater than your dataset size to leave room for overhead.
Database throughput â High throughput needs more shards, leading to a higher memory limit.
[Modules](/docs/latest/operate/oss_and_stack/stack-with-enterprise/) â Using modules with your database consumes more memory.
[Database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/) â Allows you to spread your data into shards across multiple nodes.
[Database replication](/docs/latest/operate/rs/databases/durability-ha/replication/) â Enabling replication doubles memory consumption.

### Active-Active database sizing

Additional factors for sizing Active-Active databases:

[Active-Active replication](/docs/latest/operate/rs/databases/active-active/) â Requires double the memory of regular replication, which can be up to two times (2x) the original data size per instance.
[Database replication backlog](/docs/latest/operate/rs/databases/durability-ha/replication/#database-replication-backlog) â For synchronization between shards. By default, this is set to 1% of the database size.
[Active-Active replication backlog](/docs/latest/operate/rs/databases/active-active/manage/#replication-backlog) â For synchronization between clusters. By default, this is set to 1% of the database size.

Note:
  Active-Active databases have a lower threshold for activating the eviction policy, because it requires propagation to all participating clusters. The eviction policy starts to evict keys when one of the Active-Active instances reaches 80% of its memory limit.

### Sizing databases with Auto Tiering enabled

Additional factors for sizing  databases with Auto Tiering enabled:

[Database persistence](/docs/latest/operate/rs/databases/configure/database-persistence/#redis-on-flash-data-persistence) â Auto Tiering uses dual database persistence where both the primary and replica shards persist to disk. This may add some processor and network overhead, especially in cloud configurations with network-attached storage.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page