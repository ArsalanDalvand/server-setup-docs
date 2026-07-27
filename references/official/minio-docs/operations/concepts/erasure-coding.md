---
title: "Erasure Coding — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/concepts/erasure-coding.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/concepts/erasure-coding"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Erasure Coding — SILO (MinIO Object Storage)"
---
# Erasure Coding

[Overview of MinIO Erasure Coding](https://www.youtube.com/watch?v=QniHMNNmbfI)

MinIO implements Erasure Coding as a core component in providing data redundancy and availability.
This page provides an introduction to MinIO Erasure Coding.

See [[operations/concepts/availability-and-resiliency#minio-availability-resiliency|Availability and Resiliency]] and [[operations/concepts/architecture#minio-architecture|Deployment Architecture]] for more information on how MinIO uses erasure coding in production deployments.

## Erasure Coding Basics

Note

The diagrams and content in this section present a simplified view of MinIO erasure coding operations and are not intended to represent the complexities of MinIO’s full erasure coding implementation.

MinIO groups drives in each [[glossary#term-server-pool|server pool]] into one or more Erasure Sets of the same size.

The above example deployment consists of 4 nodes with 4 drives each.
MinIO initializes with a single erasure set consisting of all 16 drives across all four nodes.[](#id1)

MinIO determines the optimal number and size of erasure sets when initializing a [[glossary#term-server-pool|server pool]].
You cannot modify these settings after this initial setup.

For each write operation, MinIO partitions the object into data and parity shards.

Erasure set stripe size dictates the maximum possible [parity](#minio-ec-parity) of the deployment.
The formula for determining the number of data and parity shards to generate is:

```
N (ERASURE SET SIZE) = K (DATA) + M (PARITY)
```

The above example deployment has an erasure set of 16 drives.
This can support parity between `EC:0` and 1/2 the erasure set drives, or `EC:8`.[](#id2)

You can set the parity value between 0 and 1/2 the Erasure Set size.

MinIO uses a Reed-Solomon erasure coding implementation and partitions the object for distribution across an erasure set.
The example deployment above has an erasure set size of 16 and a parity of `EC:4`[](#id3)

Objects written with a given parity settings do not automatically update if you change the parity values later.

MinIO requires a minimum of `K` shards of any type to read an object.

The value `K` here constitutes the read quorum for the deployment.
The erasure set must therefore have at least `K` healthy drives in the erasure set to support read operations.

This deployment has one offline node, resulting in only 12 remaining healthy drives.
The object was written with `EC:4` with a read quorum of `K=12`.
This object therefore maintains read quorum and MinIO can reconstruct it for read operations.[](#id4)

MinIO cannot reconstruct an object that has lost read quorum.
Such objects may be recovered through other means such as [[administration/bucket-replication/server-side-replication-resynchronize-remote#minio-bucket-replication-resynchronize|replication resynchronization]].

MinIO requires a minimum of `K` erasure set drives to write an object.

The value `K` here constitutes the write quorum for the deployment.
The erasure set must therefore have at least `K` available drives online to support write operations.

This deployment has one offline node, resulting in only 12 remaining healthy drives.
A client writes an object with `EC:4` parity settings where the erasure set has a write quorum of `K=12`.
This erasure set maintains write quorum and MinIO can use it for write operations.[](#id5)

If Parity `EC:M` is exactly 1/2 the erasure set size, write quorum is `K+1`

This prevents a split-brain type scenario, such as one where a network issue isolates exactly half the erasure set drives from the other.

This deployment has two nodes offline due to a transient network failure.
A client writes an object with `EC:8` parity settings where the erasure set has a write quorum of `K=9`.
This erasure set has lost write quorum and MinIO cannot use it for write operations.[](#id6)

The `K+1` logic ensures that a client could not potentially write the same object twice - once to each “half” of the erasure set.

For an object maintaining read quorum, MinIO can use any data or parity shard to [[operations/concepts/healing#minio-concepts-healing|heal]] damaged shards.

An object with `EC:4` lost four data shards out of 12 due to drive failures.
Since the object has maintained read quorum, MinIO can heal those lost data shards using the available parity shards.[](#id7)

Use the MinIO [Erasure Coding Calculator](https://min.io/product/erasure-code-calculator) to explore the possible erasure set size and distributions for your planned topology.
Where possible, use an even number of nodes and drives per node to simplify topology planning and conceptualization of drive/erasure-set distribution.

Exclusive access to drives

MinIO requires exclusive access to the drives or volumes provided for object storage.
No other processes, software, scripts, or persons should perform any actions directly on the drives or volumes provided to MinIO or the objects or files MinIO places on them.

Unless directed by MinIO Engineering, do not use scripts or tools to directly modify, delete, or move any of the data shards, parity shards, or metadata files on the provided drives, including from one drive or node to another.
Such operations are very likely to result in widespread corruption and data loss beyond MinIO’s ability to heal.

## Erasure Parity and Storage Efficiency

Setting the parity for a deployment is a balance between availability and total usable storage.
Higher parity values increase resiliency to drive or node failure at the cost of usable storage, while lower parity provides maximum storage with reduced tolerance for drive/node failures.
Use the MinIO [Erasure Code Calculator](https://min.io/product/erasure-code-calculator?ref=docs) to explore the effect of parity on your planned cluster deployment.

The following table lists the outcome of varying erasure code parity levels on a MinIO deployment consisting of 1 node and 16 1TB drives:

Outcome of Parity Settings on a 16 Drive MinIO Cluster[](#id8)

Parity

Total Storage

Storage Ratio

Minimum Drives for Read Operations

Minimum Drives for Write Operations

`EC: 4` (Default)

12 Tebibytes

0.750

12

12

`EC: 6`

10 Tebibytes

0.625

10

10

`EC: 8`

8 Tebibytes

0.500

8

9

## Bit Rot Protection

[Bit rot](https://en.wikipedia.org/wiki/Data_degradation) is silent data corruption from random changes at the storage media level.
For data drives, it is typically the result of decay of the electrical charge or magnetic orientation that represents the data.
These sources can range from the small current spike during a power outage to a random cosmic ray resulting in flipped bits.
The resulting “bit rot” can cause subtle errors or corruption on the data medium without triggering monitoring tools or hardware.

MinIO’s optimized implementation of the [HighwayHash algorithm](https://github.com/minio/highwayhash/blob/master/README.md) ensures that it captures and heals corrupted objects on the fly.
Integrity is ensured from end to end by computing a hash on READ and verifying it on WRITE from the application, across the network, and to the memory or drive.
The implementation is designed for speed and can achieve hashing speeds over 10 GB/sec on a single core on Intel CPUs.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).