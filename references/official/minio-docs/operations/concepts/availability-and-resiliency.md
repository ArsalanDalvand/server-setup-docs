---
title: "Availability and Resiliency — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/concepts/availability-and-resiliency.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/concepts/availability-and-resiliency"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Availability and Resiliency — SILO (MinIO Object Storage)"
---
# Availability and Resiliency

This page provides an overview of MinIO’s availability and resiliency design and features from a production perspective.

Note

The contents of this page are intended as a best-effort guide to understanding MinIO’s intended design and philosophy behind availability and resiliency.
It cannot replace the functionality of [MinIO SUBNET](https://min.io/pricing?jmp=docs), which allows for coordinating with MinIO Engineering when planning your MinIO deployments.

Community users can seek support on the [MinIO Community Slack](https://slack.min.io).
Community Support is best-effort only and has no SLAs around responsiveness.

## Distributed MinIO Deployments

MinIO implements [[operations/concepts/erasure-coding#minio-erasure-coding|erasure coding]] as the core component in providing availability and resiliency during drive or node-level failure events.

MinIO partitions each object into data and [[operations/concepts/erasure-coding#minio-ec-parity|parity]] shards and distributes those shards across a single [[operations/concepts/erasure-coding#minio-ec-erasure-set|erasure set]].

This small one-node deployment has 16 drives in one erasure set.
Assuming default [[operations/concepts/erasure-coding#minio-ec-parity|parity]] of `EC:4`, MinIO partitions the object into 4 (four) parity shards and 12 (twelve) data shards.
MinIO distributes these shards evenly across each drive in the erasure set.[](#id1)

MinIO uses a deterministic algorithm to select the erasure set for a given object.

For each unique object namespace `BUCKET/PREFIX/[PREFIX/...]/OBJECT.EXTENSION`, MinIO always selects the same erasure set for read/write operations.
This includes all [[administration/object-management/object-versioning#minio-bucket-versioning|versions]] of that same object.

MinIO calculates the destination erasure set using the full object namespace.[](#id2)

MinIO requires [[operations/concepts/erasure-coding#minio-read-quorum|read and write quorum]] to perform read and write operations against an erasure set.

The quorum depends on the configured parity for the deployment.
Read quorum always equals the configured parity, such that MinIO can perform read operations against any erasure set that has not lost more drives than parity.

This node has two failed drives.
MinIO uses parity shards to replace the lost data shards automatically and serves the reconstructed object to the requesting client.[](#id3)

With the default parity of `EC:4`, the deployment can tolerate the loss of 4 (four) drives per erasure set and still serve read operations.

Write quorum depends on the configured parity and the size of the erasure set.

If parity is less than 1/2 (half) the number of erasure set drives, write quorum equals parity and functions similarly to read quorum.

MinIO automatically increases the parity of objects written to a degraded erasure set to ensure that object can meet the same SLA as objects in healthy erasure sets.
The parity upgrade behavior provides an additional layer of risk mitigation, but cannot replace the long-term solution of repairing or replacing damaged drives to bring the erasure set back to full healthy status.

This node has two failed drives.
MinIO writes the object with an upgraded parity of `EC:6` to ensure this object meets the same SLA as other objects.[](#id4)

With the default parity of `EC:4`, the deployment can tolerate the loss of 4 drives per erasure set and still serve write operations.

If parity equals 1/2 (half) the number of erasure set drives, write quorum equals parity + 1 (one) to avoid data inconsistency due to “split brain” scenarios.

For example, if exactly half the drives in the erasure set become isolated due to a network fault, MinIO would consider quorum lost as it cannot establish a N+1 group of drives for the write operation.

This node has 50% drive failure.
If parity is `EC:8`, this erasure set cannot meet write quorum and MinIO rejects write operations to that set.
Since the erasure set still maintains read quorum, read operations to existing objects can still succeed.[](#id5)

An erasure set which permanently loses more drives than the configured parity has suffered data loss.

For maximum parity configurations, the erasure set goes into “read only” mode if drive loss equals parity.
For the maximum erasure set size of 16 and maximum parity of 8, this would require the loss of 9 drives for data loss to occur.

This erasure set has lost more drives than the configured parity of `EC:4` and has therefore lost both read and write quorum.
MinIO cannot recover any data stored on this erasure set.[](#id6)

Transient or temporary drive failures, such as due to a failed storage controller or connecting hardware, may recover back to normal operational status within the erasure set.

MinIO further mitigates the risk of erasure set failure by “striping” erasure set drives symmetrically across each node in the pool.

MinIO automatically calculates the optimal erasure set size based on the number of nodes and drives, where the maximum set size is 16 (sixteen).
It then selects one drive per node going across the pool for each erasure set, circling around if the erasure set stripe size is greater than the number of nodes.
This topology provides resiliency to the loss of a single node, or even a storage controller on that node.

In this 16 x 8 deployment, MinIO would calculate 8 erasure sets of 16 drives each.
It allocates one drive per node across the available nodes to fill each erasure set.
If there were 8 nodes, MinIO would need to select 2 drives per node for each erasure set.[](#id7)

In the above topology, the pool has 8 erasure sets of 16 drives each striped across 16 nodes.
Each node would have one drive allocated per erasure set.
While losing one node would technically result in the loss of 8 drives, each erasure set would only lose one drive each.
This maintains quorum despite the node downtime.

Each erasure set is independent of all others in the same pool.

If one erasure set becomes completely degraded, MinIO can still perform read/write operations on other erasure sets.

One pool has a degraded erasure set.
While MinIO can no longer serve read/write operations to that erasure set, it can continue to serve operations on healthy erasure sets in that pool.[](#id8)

However, the lost data may still impact workloads which rely on the assumption of 100% data availability.
Furthermore, each erasure set is fully independent of the other such that you cannot restore data to a completely degraded erasure set using other erasure sets.
You must use [[operations/replication/multi-site-replication#minio-site-replication-overview|Site]] or [[administration/bucket-replication#minio-bucket-replication|Bucket]] replication to create a BC/DR-ready remote deployment for restoring lost data.

For multi-pool MinIO deployments, each pool requires at least one erasure set maintaining read/write quorum to continue performing operations.

If one pool loses all erasure sets, MinIO can no longer determine whether a given read/write operation would have routed to that pool.
MinIO therefore stops all I/O to the deployment, even if other pools remain operational.

One pool in this deployment has completely failed.
MinIO can no longer determine which pool or erasure set to route I/O to.
Continued operations could produce an inconsistent state where an object and/or it’s versions reside in different erasure sets.
MinIO therefore halts all I/O in the deployment until the pool recovers.[](#id9)

To restore access to the deployment, administrators must restore the pool to normal operations.
This may require formatting disks, replacing hardware, or replacing nodes depending on the severity of the failure.
See [[operations/data-recovery#minio-restore-hardware-failure|Recover after Hardware Failure]] for more complete documentation.

Use replicated remotes to restore the lost data to the deployment.
All data stored on the healthy pools remain safe on disk.

Exclusive access to drives

MinIO requires exclusive access to the drives or volumes provided for object storage.
No other processes, software, scripts, or persons should perform any actions directly on the drives or volumes provided to MinIO or the objects or files MinIO places on them.

Unless directed by MinIO Engineering, do not use scripts or tools to directly modify, delete, or move any of the data shards, parity shards, or metadata files on the provided drives, including from one drive or node to another.
Such operations are very likely to result in widespread corruption and data loss beyond MinIO’s ability to heal.

## Replicated MinIO Deployments

MinIO implements [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]] as the primary measure for ensuring Business Continuity and Disaster Recovery (BC/DR) in the case of both small and large scale data loss in a MinIO deployment.

Each peer site is deployed to an independent datacenter to provide protection from large-scale failure or disaster.
If one datacenter goes completely offline, clients can fail over to the other site.[](#id10)

MinIO replication can automatically [[operations/concepts/healing#minio-concepts-healing|heal]] a site that has partial or total data loss due to transient or sustained downtime.

Datacenter 2 was down and Site B requires resynchronization.
The Load Balancer handles routing operations to Site A in Datacenter 1.
Site A continuously replicates data to Site B.[](#id11)

Once all data synchronizes, you can restore normal connectivity to that site.
Depending on the amount of replication lag, latency between sites and overall workload I/O, you may need to temporarily stop write operations to allow the sites to completely catch up.

If a peer site completely fails, you can remove that site from the configuration entirely.
The load balancer configuration should also remove that site to avoid routing client requests to the offline site.

You can then restore the peer site, either after repairing the original hardware or replacing it entirely, by [[operations/replication/multi-site-replication#minio-expand-site-replication|adding it back to the site replication configuration]].
MinIO automatically begins resynchronizing existing data while continuously replicating new data.

Sites can continue processing operations during resynchronization by proxying `GET/HEAD` requests to healthy peer sites

Site B does not have the requested object, possibly due to replication lag.
It proxies the `GET` request to Site A.
Site A returns the object, which Site B then returns to the requesting client.[](#id12)

The client receives the results from first peer site to return any version of the requested object.

`PUT` and `DELETE` operations synchronize using the regular replication process.
`LIST` operations do not proxy and require clients to issue them exclusively against healthy peers.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).