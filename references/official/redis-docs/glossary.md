---
title: "Glossary"
source: "https://redis.io/docs/latest/glossary/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "glossary"
category: "redis"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Glossary"
---
# Glossary

## A, B

    access control list (ACL)

Allows you to manage permissions based on key patterns.

More info: [[operate/oss_and_stack/management/security/acl|redis.io/operate/oss_and_stack/management/security/acl]]; [ACL wikipedia](https://en.wikipedia.org/wiki/Access-control_list); [[operate/rs/security/access-control|Database access control]]; [[operate/rs/security/access-control/ldap/update-database-acls|Update database ACLs]]; [[operate/rc/security/access-control/data-access-control/role-based-access-control|Role-based access control]]

    Active-Active database (CRDB)

Geo-distributed databases that span multiple [Redis Enterprise Software](#redis-enterprise-software) [clusters](#cluster). Active-Active databases, also known as conflict-free replicated databases (CRDB), depend on [multi-primary replication](#multi-primary-replication) and [conflict-free replicated data types (CRDTs)](#conflict-free-replicated-data-types-crdt) to power a simple development experience for geo-distributed applications.

More info: [Active-Active geo-distributed Redis](/docs/latest/operate/rs/databases/active-active/), [Geo-distributed Active-Active Redis applications](/docs/latest/operate/rs/databases/active-active/), [Developing applications for Active-Active databases](/docs/latest/operate/rs/databases/active-active/develop/)

    Active-Active database instance

A "member database" of a global [Active-Active database](#active-active) which is made up of its own master and replica [shards](#shard) spanning a single [cluster](#cluster).

More info: [Active-Active database instances](/docs/latest/operate/rs/databases/active-active/create/)

    active-passive database replication

Provides applications read-only access to replicas of the data set from different geographical locations. The Redis Enterprise implementation of active-passive [replication](#replication) is called [Replica Of](#replica-of).

More info: [Database replication](/docs/latest/operate/rs/databases/durability-ha/replication/), [Replica Of](/docs/latest/operate/rs/databases/import-export/replica-of/)

    admin console

Each node runs a web server that is used to provide the user with access to the Redis Enterprise Software admin console (previously known as management UI). The admin console allows viewing and managing the entire [cluster](#cluster), so it does not matter which [node](#redis-enterprise-nodes) is used to access it.

More info: [[operate/rs/clusters|Manage clusters]]

    admission controller

A piece of code that intercepts requests to the Kubernetes API server prior to persistence of the object.

More info: [Using Admission Controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)

    append-only file (AoF)

Log files that keep a record of data changes by writing to the end of a file. This happens with every write, or every second to allow data recovering the entire
dataset by replaying the append-only log from the beginning to the end.

More info: [Data Persistence](/docs/latest/operate/rc/databases/configuration/data-persistence/), [Data Persistence with Redis Enterprise Software](/docs/latest/operate/rs/databases/configure/database-persistence/)

    Bloom filter

A probabilistic data structure that tests whether an element is a member of a set. False positive matches are possible, but false negatives are not. Bloom filters are space-efficient and provide fast membership testing.

More info: [Bloom filters](/docs/latest/develop/data-types/probabilistic/bloom-filter/), [RedisBloom](/docs/latest/operate/oss_and_stack/stack-with-enterprise/bloom/)

## C

    causal consistency

A distributed database is causally consistent if it maintains the same order of operations on a piece of data across all database copies.

More info: [Causal consistency wikipedia](https://en.wikipedia.org/wiki/Causal_consistency), [Causal consistency in an Active-Active database](/docs/latest/operate/rs/databases/active-active/causal-consistency/)

    CIDR allowlist

Classless Inter-Domain Routing (CIDR) is a method to allocate and route IP addresses. A CIDR allowlist defines a range of IP addresses and permits connections to them.

More info: [CIDR wikipedia](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing), [[operate/rc/security/cidr-whitelist|Configure CIDR allowlist]]

    concurrent writes

Concurrency or updates and writes refer to more than events that happen at the same wall clock time across member [Active-Active databases](#active-active). Concurrent updates refer to the fact that updates happen in between sync events that catch up member Active-Active databases with updates that happened on other member Active-Active databases.

More info: [Developing applications for Active-Active databases](/docs/latest/operate/rs/databases/active-active/develop/), [Conflict-free replicated data types (CRDT)](#conflict-free-replicated-data-types-crdt)

    consistency

Consistency models describe the way a distributed system keeps replicated data consistent between copies.

More info: [Consistency models](https://en.wikipedia.org/wiki/Consistency_model)

    cluster

A Redis Enterprise cluster is composed of identical nodes that are deployed within a data center or stretched across local availability zones.

More info: [Database clustering](/docs/latest/operate/rc/databases/configuration/clustering/)

    Cluster Configuration Store (CCS)

An internally managed [Redis database](#redis-enterprise-database) that acts as a single repository for all [cluster](#cluster) metadata.

More info: [[operate/rs/clusters|Redis Enterprise cluster architecture]]

    Cluster Node Manager (CNM)

A collection of Redis Enterprise services responsible for [provisioning](#provisioning), [migration](#migration), monitoring, [re-sharding](#re-sharding), [rebalancing](#rebalancing), de-provisioning, auto-scaling.

More info: [[operate/rs/clusters|Redis Enterprise cluster architecture]], [Database operations](/docs/latest/operate/rs/databases/)

    conflict-free replicated databases (CRDB)

Conflict-free replicated databases (CRDB) are an alternate name for [Active-Active databases](#active-active).

    conflict-free replicated data types (CRDT)

Techniques used by Redis data types in Active-Active databases that handle conflicting concurrent writes across member Active-Active databases. The Redis Enterprise implementation of CRDT is called an Active-Active database (formerly known as CRDB).

More info: [CRDT info](/docs/latest/operate/rs/databases/active-active/develop/#info), [Active-Active geo-distributed Redis](/docs/latest/operate/rs/databases/active-active/), [CRDT wikipedia](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type)

    controllers

Control loops that watch the state of your Kubernetes cluster and make or request changes where needed to move the current cluster state closer to the desired state.

More info: [Controllers](https://kubernetes.io/docs/concepts/architecture/controller/)

    custom resources (CRs)

Extensions of the Kubernetes API that allow you to store and retrieve structured data. Custom resources let you extend Kubernetes capabilities without modifying the core Kubernetes code.

More info: [Custom Resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)

    CustomResourceDefinition (CRD)

A cluster-wide resource that specifies which settings can be configured via custom resource files. CRDs define the structure and validation rules for custom resources.

More info: [CustomResourceDefinition](https://kubernetes.io/docs/reference/glossary/?fundamental=true#term-CustomResourceDefinition), [Custom Resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)

## D - F

    data eviction policy

Defines how excess data is handled when the [database](#redis-enterprise-database) exceeds the memory limit.

More info: [Data Eviction Policy](/docs/latest/operate/rc/databases/configuration/data-eviction-policies/), [Redis Enterprise data eviction](/docs/latest/operate/rs/databases/memory-performance/eviction-policy/)

    declarative configuration

A configuration approach where you specify the desired state of your system, and the system automatically makes the necessary changes to achieve that state.

More info: [Managing Kubernetes Objects Using Declarative Configuration](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/)

    deprecated

Features are marked as deprecated when they're scheduled to be removed from our products, generally because they've been replaced by new features.

For details, see [obsolete](#obsolete).

    Domain Name Service (DNS)

Naming system for computers, services, or other resources connected to the Internet or a private network. It associates various information with domain names assigned to each of the participating entities.

More info: [DNS wikipedia](https://en.wikipedia.org/wiki/Domain_Name_System)

    eventual consistency

After updating data on one instance of a distributed database, the other database copies may have stale data for a short time while they sync. Eventual consistency means that the updated data will eventually be the same across all database copies.

More info: [Eventual consistency wikipedia](https://en.wikipedia.org/wiki/Eventual_consistency)

    failover

The automatic process of switching to a backup system when the primary system fails. In Redis Enterprise, failover promotes replica shards to primary when the original primary becomes unavailable.

More info: [High availability](/docs/latest/operate/rs/databases/durability-ha/), [Replica HA](/docs/latest/operate/rs/databases/configure/replica-ha/)

    Fully qualified domain name (FQDN)

A domain name that includes a list of domain labels to specify the exact location in the DNS.

More info: [FQDN wikipedia](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)

    `fsync`

Linux command to synchronize a file's in-core state with a storage device. Used in [data persistence](/docs/latest/operate/rs/databases/configure/database-persistence/) operations.

More info: [`fsync` man page](https://man7.org/linux/man-pages/man2/fsync.2.html), [Data persistence](/docs/latest/operate/rs/databases/configure/database-persistence/)

    full-text search

A search technique that examines all words in every stored document to find matches to search criteria. Redis provides full-text search capabilities through its Search and Query features.

More info: [Redis Search](/docs/latest/develop/ai/search-and-query/), [Full-text search](/docs/latest/develop/ai/search-and-query/query/full-text/)

## G - J

    indexing

The process of creating data structures that improve the speed of data retrieval operations. Redis supports various types of indexing including secondary indexing, vector indexing, and full-text indexing.

More info: [Redis Search](/docs/latest/develop/ai/search-and-query/), [Indexing concepts](/docs/latest/develop/ai/search-and-query/advanced-concepts/)

    JSON

JavaScript Object Notation (JSON) is a lightweight data-interchange format. Redis provides native JSON support through RedisJSON, allowing you to store, update, and retrieve JSON values.

More info: [JSON data type](/docs/latest/develop/data-types/json/), [RedisJSON](/docs/latest/operate/oss_and_stack/stack-with-enterprise/json/)

    hash slot

The result of a hash calculation used in [database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/) to determine which [shard](#shard) stores a particular key.

More info: [Database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/), [Sharding](#sharding)

    hash tag

A part of the key that is used in the hash calculation to determine the [hash slot](#hash-slot) for [database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/).

More info: [Database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/), [Hash slots](#hash-slot)

    high availability

High availability (HA) is a characteristic of distributed systems that keeps systems available for users for longer than normal periods of time. This is done by reducing single points of failure, increasing redundancy, and making recovering from failures easier.

More info: [Redis Enterprise durability and high availability](/docs/latest/operate/rs/databases/durability-ha/), [High availability wikipedia](https://en.wikipedia.org/wiki/High_availability)

    ingress

An API object that manages external access to the services in a Kubernetes cluster, typically HTTP.

More info: [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/), [Ingress routing for Redis Enterprise for Kubernetes](/docs/latest/operate/kubernetes/networking/ingress/)

## K, L

    kubectl

A command-line tool for communicating with a Kubernetes API server.

More info: [Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)

    Lightweight Directory Access Protocol (LDAP)

A protocol for accessing and maintaining distributed directory services over an IP network, often used to authenticate users.

More info: [LDAP wikipedia](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol), [[operate/rs/security/access-control/ldap|LDAP authentication]]

    latency

The time delay between when a command is issued and when a response is received. Redis is designed for low latency, typically operating in the sub-millisecond range.

More info: [[operate/oss_and_stack/management/optimization/latency-monitor|Latency monitoring]], [[operate/oss_and_stack/management/optimization/latency|Latency diagnosis]]

## M - O

    master node

[Node](#redis-enterprise-nodes) that operates as the leader of a [cluster](#cluster). Also known as the primary node.

More info: [[operate/rs/clusters|Redis Enterprise cluster architecture]], [High availability](#high-availability)

    migration

Deciding when and where [shards](#shard) will be moved if more network throughput, memory, or CPU resources are needed.

More info: [Database operations](/docs/latest/operate/rs/databases/), [[operate/rs/clusters|Cluster management]]

    multicast DNS (mDNS)

Protocol that resolves hostnames to the IP addresses that do not include a local name server.

More info: [multicast DNS wikipedia](https://en.wikipedia.org/wiki/Multicast_DNS)

    module

A Redis extension that adds new commands, data types, and capabilities to Redis. Redis Enterprise includes several modules like RedisJSON, RedisSearch, RedisTimeSeries, and RedisBloom.

More info: [Redis modules](/docs/latest/operate/oss_and_stack/stack-with-enterprise/), [Install modules](/docs/latest/operate/oss_and_stack/stack-with-enterprise/install/)

    monitoring

The process of observing and tracking the performance, health, and behavior of Redis databases, clusters, and nodes using metrics and alerts.

More info: [[operate/rs/monitoring|Monitoring with metrics and alerts]], [Metrics reference](/docs/latest/operate/rs/references/metrics/)

    multi-factor authentication (MFA)

Method of authenticating users with pieces of evidence of the user's identity. When MFA is enabled on Redis Cloud, users must enter their username, password, and an authentication code when logging in.

More info: [[operate/rc/security/access-control/multi-factor-authentication|Multi-factor authentication]]

    multi-primary replication

Also known as multi-master replication, Active-Active databases have multiple primary nodes (one on each participating cluster) to enable concurrent writes operations.

More info: [Multi-primary replication](/docs/latest/operate/rs/databases/active-active/#multi-primary-replication)

    namespace

An abstraction used by Kubernetes to support multiple virtual clusters on the same physical cluster.

More info: [Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

    obsolete

When features are removed from our products, they're generally replaced by new features that provide a better experience, more functionality, improved security, and other benefits.

To provide a transition period, we mark older features as deprecated when introducing replacement features.  This gives you time to adjust your deployments, apps, and processes to support the new features.  During this transition, the older features continue to work as a courtesy.

Eventually, older features are removed from the product.  When this happens, they're considered obsolete, partly because they can no longer be used.

For best results, we advise against relying on deprecated features for any length of time.

    operator

Operators are software extensions to Kubernetes that make use of custom resources to manage applications and their components.

More info: [operator pattern](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)

    Out-of-Memory (OOM)

If a member [Active-Active database](#active-active) is in an out of memory situation, that member is marked "inconsistent" by Redis Enterprise Software, the member stops responding to user traffic, and the [syncer](#syncer) initiates full reconciliation with other peers in the Active-Active database.

More info: [Active-Active database troubleshooting](/docs/latest/operate/rs/databases/active-active/syncer/), [Memory management](/docs/latest/operate/rs/databases/memory-performance/)

## P - Q

    persistence

The ability to store data permanently to disk so that it survives server restarts. Redis supports RDB snapshots and AOF (Append-Only File) persistence methods.

More info: [[operate/oss_and_stack/management/persistence|Data persistence]], [Redis Enterprise persistence](/docs/latest/operate/rs/databases/configure/database-persistence/)

    participating clusters

[Clusters](#cluster) participating in the [multi-primary replication](#multi-primary-replication) of an [Active-Active database](#active-active).

More info: [Active-Active geo-distributed Redis](/docs/latest/operate/rs/databases/active-active/), [Create Active-Active database](/docs/latest/operate/rs/databases/active-active/create/)

    PersistentVolume (PV)

A piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using storage classes.

More info: [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

    PersistentVolumeClaims (PVC)

A request for storage by a user that serves as an abstract representation of PersistentVolume (PV) resources. PVCs consume PV resources and can request specific size and access modes.

More info: [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

    pods

The smallest deployable units of computing that you can create and manage in Kubernetes. A pod represents a single instance of a running process in your cluster.

More info: [Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

    provisioning

Deciding where [shards](#shard) will be created and placed within a [cluster](#cluster).

More info: [Database operations](/docs/latest/operate/rs/databases/), [Create databases](/docs/latest/operate/rs/databases/create/)

    proxy policy

Determines the number and location of active proxies tied to a single endpoint that receive incoming traffic for a database.

For more info, see [Proxy policy](/docs/latest/operate/rs/databases/configure/proxy-policy/).

    quorum node

[Node](#redis-enterprise-nodes) provisioned only for [cluster](#cluster) operations that can be elected as a [master node](#master-node). The quorum node participates in the cluster quorum and must be explicitly assigned this role via the `rladmin` command.

More info: [[operate/rs/clusters|Redis Enterprise cluster architecture]], [rladmin command reference](/docs/latest/operate/rs/references/cli-utilities/rladmin/)

## R

    Redis Stack

A collection of Redis modules that extends Redis with modern data models and processing engines. Includes capabilities for document databases, graph databases, time series, and more.

More info: [Redis Stack](/docs/latest/operate/oss_and_stack/stack-with-enterprise/), [Redis Stack features](/docs/latest/operate/oss_and_stack/stack-with-enterprise/enterprise-capabilities/)

    rack-zone awareness

Redis Enterprise feature that helps to ensure high availability in the event of a rack or zone failure. In the event of a rack or zone failure, the replicas and endpoints in the remaining racks/zones will be promoted.

More info: [[operate/rs/clusters/configure/rack-zone-awareness|Rack-zone awareness in Redis Enterprise Software]]

    replication backlog

[Databases](#redis-enterprise-database) using [replication](#replication) or [Active-Active](#active-active) maintain a backlog to synchronize the primary and replica [shards](#shard).

More info: [Database replication](/docs/latest/operate/rs/databases/durability-ha/replication/), [Active-Active databases](/docs/latest/operate/rs/databases/active-active/)

    re-sharding

Distributing keys and their values among new [shards](#shard) to optimize performance and resource utilization.

More info: [Database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/), [Database operations](/docs/latest/operate/rs/databases/)

    rebalancing

Moving [shards](#shard) to [nodes](#redis-enterprise-nodes) where more resources are available.

More info: [Database operations](/docs/latest/operate/rs/databases/), [[operate/rs/clusters|Cluster management]]

    Redis Cloud

The cloud version of Redis Enterprise.

More info: [[operate/rc|Redis Cloud]], [[operate/rc/rc-quickstart|Redis Cloud quick start]]

    Redis Enterprise cluster

Collection of [Redis Enterprise nodes](#redis-enterprise-nodes). A [cluster](#cluster) pools system resources across nodes in the cluster and supports multi-tenant [database](#redis-enterprise-database) instances.

More info: [[operate/rs/clusters|Redis Enterprise cluster architecture]], [[operate/rs/clusters|Cluster management]]

    Redis Enterprise database

Logical entity that manages your entire dataset across multiple [Redis instances](#redis-instance). It segments the data into [shards](#shard) and distributes them among [nodes](#redis-enterprise-nodes).

More info: [Database operations](/docs/latest/operate/rs/databases/), [Create databases](/docs/latest/operate/rs/databases/create/)

    Redis Enterprise nodes

Physical or virtual machines or containers that runs a collection of Redis Enterprise services within a [cluster](#cluster).

More info: [[operate/rs/clusters|Redis Enterprise cluster architecture]], [[operate/rs/installing-upgrading|Install and setup]]

    Redis Enterprise Software

The on-premises version of Redis Enterprise.

More info: [[operate/rs|Redis Enterprise Software]], [[operate/rs/installing-upgrading|Install Redis Enterprise Software]]

    Redis instance

Single-threaded Redis Open Source database. Redis OSS was renamed Redis Open Source with the Redis 8 in Redis Open Source release.

More info: [[operate/oss_and_stack|Redis Open Source]], [Database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/)

    Auto Tiering 

Previously known as Redis on Flash. Enables your Redis databases to span both RAM and dedicated flash memory (SSD). Auto Tiering manages the location of key values (RAM vs Flash) in the database via a LRU-based (least-recently-used) mechanism.

More info: [Auto Tiering](/docs/latest/operate/rs/databases/flash/), [Auto Tiering  quick start](/docs/latest/operate/rs/databases/flash/quickstart/)

    replica high availability (replicaHA)

High availability feature of Redis Enterprise Software. After a node failure, the cluster automatically migrates remaining replica shards to available nodes. Previously known as "Slave HA" or `slave_ha`.

More info: [High availability for replica shards](/docs/latest/operate/rs/databases/configure/replica-ha/)

    Replica Of

The Redis Enterprise implementation of active-passive database replication.

More info: [Replica Of](/docs/latest/operate/rs/databases/import-export/replica-of/)

    ReplicaSet

A ReplicaSet is a type of Kubernetes resource that (aims to) maintain a set of replica pods running at any given time.

More info: [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

    replication

Database replication provides a mechanism to ensure high availability. When replication is enabled, your dataset is replicated to a replica shard,
which is constantly synchronized with the primary shard. If the primary
shard fails, an automatic failover happens and the replica shard is promoted.

More info: [Database replication](/docs/latest/operate/rs/databases/durability-ha/replication/)

    role bindings

Kubernetes objects that grant the permissions defined in a role to a user or set of users, including service accounts.

More info: [RoleBinding and ClusterRoleBinding](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#rolebinding-and-clusterrolebinding)

    role-based access control (RBAC)

A security approach that restricts system access to authorized users.

More info: [RBAC wikipedia](https://en.wikipedia.org/wiki/Role-based_access_control); [[operate/rs/security/access-control|Database access control]]; [[operate/rc/security/access-control/data-access-control/role-based-access-control|Role-based access control]]

    roles

Kubernetes objects that contain rules representing a set of permissions. Roles define what actions can be performed on which resources.

More info: [Role and ClusterRole](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole)

## S

    search

The ability to query and retrieve data using various search methods including full-text search, vector search, and secondary indexing. Redis provides search capabilities through its Search and Query features.

More info: [Redis Search](/docs/latest/develop/ai/search-and-query/), [RedisSearch](/docs/latest/operate/oss_and_stack/stack-with-enterprise/search/)

    secondary indexing

Creating additional data structures to enable fast lookups on fields other than the primary key. Redis supports secondary indexing for JSON documents, hashes, and other data types.

More info: [Redis Search](/docs/latest/develop/ai/search-and-query/), [Indexing concepts](/docs/latest/develop/ai/search-and-query/advanced-concepts/)

    secret

Kubernetes term for object that stores sensitive information, such as passwords, OAuth tokens, and ssh keys.

More info: [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

    service account

Provides an identity for processes that run in a pod, allowing them to access the Kubernetes API and other resources.

More info: [Service Accounts](https://kubernetes.io/docs/concepts/security/service-accounts/)

    services

An abstract way to expose an application running on a set of pods as a network service in Kubernetes.

More info: [Services](https://kubernetes.io/docs/concepts/services-networking/service/)

    shard

Redis process that is part of the Redis clustered database.

More info: [Database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/), [terminology](/docs/latest/operate/rs/references/terminology/)

    sharding

Technique that has been used to scale larger data storage and processing loads. Sharding take your data, partitions it into smaller pieces and then send the data to different locations depending on which partition the data has been assigned to. Related to [hash slots](#hash-slot) and [shards](#shard).

More info: [Database clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/), [Shards](#shard)

    Simple Authentication and Security Layer (SASL)

Framework for adding authentication support and data security to connection-based protocols via replaceable mechanisms.

More info: [SASL wikipedia](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer)

    snapshot (RDB)

Data persistence file that performs a data dump every one, six, or twelve hours. Used alongside [append-only file (AoF)](#append-only-file-aof) for data persistence.

More info: [Data persistence](/docs/latest/operate/rs/databases/configure/database-persistence/), [Append-only file (AoF)](#append-only-file-aof)

    storage class

A way for administrators to describe the classes of storage they offer in Kubernetes, including different quality-of-service levels, backup policies, or arbitrary policies.

More info: [Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/)

    syncer

Process on each node hosting an Active-Active database instance that synchronizes a backlog of operations between participating clusters.

More info: [Syncer process](/docs/latest/operate/rs/databases/active-active/syncer/)

## T - Z

    throughput

The number of operations or requests that can be processed per unit of time. Redis is designed for high throughput, capable of handling hundreds of thousands to millions of operations per second.

More info: [[operate/oss_and_stack/management/optimization/benchmarks|Redis benchmarks]], [[operate/oss_and_stack/management/optimization|Performance optimization]]

    time series

A sequence of data points indexed in time order. Redis provides native time series support through RedisTimeSeries for storing and querying time-stamped data.

More info: [Time series data type](/docs/latest/develop/data-types/timeseries/), [RedisTimeSeries](/docs/latest/operate/oss_and_stack/stack-with-enterprise/timeseries/)

    tombstone

A key that is logically deleted but stays in memory until it is collected by the garbage collector. Common in [Active-Active databases](#active-active) during conflict resolution.

More info: [Active-Active database development](/docs/latest/operate/rs/databases/active-active/develop/), [Conflict-free replicated data types (CRDT)](#conflict-free-replicated-data-types-crdt)

    Transport Layer Security (TLS)

Protocols that provide communications security over a computer network.

More info: [TLS wikipedia](https://en.wikipedia.org/wiki/Transport_Layer_Security), [[operate/rc/security/database-security/tls-ssl|Cloud database TLS]], [[operate/rs/security/encryption/tls|Redis Enterprise TLS]]

    vector search

A search method that finds similar items by comparing high-dimensional vectors representing data features. Redis supports vector search for AI applications, machine learning, and similarity matching.

More info: [Vector search](/docs/latest/develop/ai/search-and-query/vectors/), [Vector indexing](/docs/latest/develop/ai/search-and-query/query/vector-search/)

    VPC peering

Networking connection between two VPCs that enables you to route traffic between them using private IP addresses. Instances in either VPC can communicate with each other as if they are within the same network.

More info: [VPC wikipedia](https://en.wikipedia.org/wiki/Virtual_private_cloud), [[operate/rc/security/vpc-peering|Enable VPC peering]]

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page