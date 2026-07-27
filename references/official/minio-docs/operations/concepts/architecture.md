---
title: "Deployment Architecture — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/concepts/architecture.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/concepts/architecture"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Deployment Architecture — SILO (MinIO Object Storage)"
---
# Deployment Architecture

This page provides an overview of MinIO deployment architectures from a production perspective.
For information on specific hardware or software configurations, see:

[[operations/checklists/hardware#minio-hardware-checklist|Hardware Checklist]]

[[operations/checklists/security#minio-security-checklist|Security Checklist]]

[[operations/checklists/software#minio-software-checklists|Software Checklist]]

[[operations/concepts/thresholds#minio-server-limits|Thresholds and Limits]]

## Distributed MinIO Deployments

A production MinIO deployment consists of at least 4 MinIO hosts with homogeneous storage and compute resources.

MinIO aggregates these resources together as a [[operations/concepts#minio-intro-server-pool|pool]] and presents itself as a single object storage service.

Each MinIO host in this pool has matching compute, storage, and network configurations[](#id1)

MinIO provides best performance when using locally-attached storage, such as NVMe or SSD drives attached to a PCI-E controller board on the host machine.

Storage controllers should present XFS-formatted drives in “Just a Bunch of Drives” (JBOD) configurations with no RAID, pooling, or other hardware/software resiliency layers.
MinIO recommends against caching, either at the drive or the controller layer.
Either type of caching can cause I/O spikes as the cache fills and clears, resulting in unpredictable performance.

Each SSD connects by SAS to a PCI-E-attached storage controller operating in HBA mode[](#id2)

MinIO automatically groups drives in the pool into [[operations/concepts/erasure-coding#minio-ec-erasure-set|erasure sets]].

Erasure sets are the foundational component of MinIO [[operations/concepts/availability-and-resiliency#minio-availability-resiliency|availability and resiliency]].
MinIO stripes erasure sets symmetrically across the nodes in the pool to maintain even distribution of erasure set drives.
MinIO then partitions objects into data and parity shards based on the deployment [[operations/concepts/erasure-coding#minio-ec-parity|parity]] and distributes them across an erasure set.

For a more complete discussion of MinIO redundancy and healing, see [[operations/concepts/erasure-coding#minio-erasure-coding|Erasure Coding]] and [[operations/concepts/healing#minio-concepts-healing|Object Healing]].

With the maximum parity of `EC:8`, MinIO shards the object into 8 data and 8 parity blocks, distributing them across the drives in the erasure set.
All erasure sets in this pool have the same stripe size and shard distribution.[](#id3)

MinIO uses a deterministic hashing algorithm based on object name and path to select the erasure set for a given object.

For each unique object namespace `BUCKET/PREFIX/[PREFIX/...]/OBJECT.EXTENSION`, MinIO always selects the same erasure set for read/write operations.
MinIO handles all routing within pools and erasure sets, making the select/read/write process entirely transparent to applications.

MinIO reconstructs objects from data or parity shards transparently before returning the object to the requesting client.[](#id4)

Each MinIO server has a complete picture of the distributed topology, such that an application can connect and direct operations against any node in the deployment.

The MinIO responding node automatically handles routing internal requests to other nodes in the deployment and returning the final response to the client.

Applications typically should not manage those connections, as any changes to the deployment topology would require application updates.
Production environments should instead deploy a load balancer or similar network control plane component to manage connections to the MinIO deployment.
For example, you can deploy an NGINX load balancer to perform “least connections” or “round robin” load balancing against the available nodes in the deployment.

The load balancer routes the request to any node in the deployment.
The receiving node handles any internode requests thereafter.[](#id5)

You can expand a MinIO deployment’s available storage through [[operations/deployments/baremetal-expand-minio-deployment#expand-minio-distributed|pool expansion]].

Each pool consists of an independent group of nodes with their own erasure sets.
MinIO must query each pool to determine the correct erasure set to which it directs read and write operations, such that each additional pool adds increased internode traffic per call.
The pool which contains the correct erasure set then responds to the operation, remaining entirely transparent to the application.

If you modify the MinIO topology through pool expansion, you can update your applications by modifying the load balancer to include the new pool’s nodes.
Applications can continue using the load balancer address for the MinIO deployment without any updates or modifications.
This ensures even distribution of requests across all pools, while applications continue using the single load balancer URL for MinIO operations.

The PUT request requires checking each pool for the correct erasure set.
Once identified, MinIO partitions the object and distributes the data and parity shards across the appropriate set.[](#id6)

Client applications can use any S3-compatible SDK or library to interact with the MinIO deployment.

MinIO publishes its own [[developers/minio-drivers#minio-drivers|SDK]] specifically intended for use with S3-compatible deployments.

Clients using a variety of S3-compatible SDKs can perform operations against the same MinIO deployment.[](#id7)

MinIO uses a strict implementation of the S3 API, including requiring clients to sign all operations using AWS [Signature V4](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html) or the legacy Signature V2.
AWS signature calculation uses the client-provided headers, such that any modification to those headers by load balancers, proxies, security programs, or other components will result in signature mismatch errors and request failure.
Ensure any such intermediate components support pass-through of unaltered headers from client to server.

While the S3 API uses HTTP methods like `GET` and `POST` for all operations, applications typically use an SDK for S3 operations.
In particular, the complexity of signature calculation typically makes interfacing via `curl` or similar REST clients impractical.
MinIO recommends using S3-compatible SDKs or libraries which perform the signature calculation automatically as part of operations.

## Replicated MinIO Deployments

MinIO [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]] provides support for synchronizing distinct independent deployments.

You can deploy peer sites in different racks, datacenters, or geographic regions to support functions like BC/DR or geo-local read/write performance in a globally distributed MinIO object store.

A MinIO multi-site deployment with three peers.
Write operations on one peer replicate to all other peers in the configuration automatically.[](#id8)

Replication performance primarily depends on the network latency between each peer site.

With geographically distributed peer sites, high latency between sites can result in significant replication lag.
This can compound with workloads that are near or at the deployment’s overall performance capacity, as the replication process itself requires sufficient free I/O to synchronize objects.

In this peer configuration, the latency between Site A and its peer sites is 100ms.
The soonest the object fully synchronizes to all sites is at least 110ms.[](#id9)

Deploying a global load balancer or similar network appliance with support for site-to-site failover protocols is critical to the functionality of multi-site deployments.

The load balancer should support a health probe/check setting to detect the failure of one site and automatically redirect applications to any remaining healthy peer.

The Load Balancer automatically routes client requests using configured logic (geo-local, latency, etc.).
Data written to one site automatically replicates to the other peer site.[](#id10)

The load balancer should meet the same requirements as single-site deployments regarding connection balancing and header preservation.
MinIO replication handles transient failures by queuing objects for replication.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).