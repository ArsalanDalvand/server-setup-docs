---
title: "Object Healing — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/concepts/healing.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/concepts/healing"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Object Healing — SILO (MinIO Object Storage)"
---
# Object Healing

## What is healing?

Healing is MinIO’s ability to restore an object that has been damaged, corrupted, or partially lost.
The loss can come from multiple types of corruptions or loss, such as but not limited to:

drive-level errors or failure

OS or filesystem errors or failure

[[operations/concepts/erasure-coding#minio-ec-bitrot|bit rot]]

## Healing and Erasure Coding

The ability of MinIO to restore a damaged object relates directly to the following:

total number of drives in the [[operations/concepts/erasure-coding#minio-erasure-coding|erasure set]] where the object exists

number of drives available with intact parts of the object

[[operations/concepts/erasure-coding#minio-ec-parity|parity setting]] for the erasure set

[[glossary#term-parity|Parity]] refers to the number of dedicated recovery shards MinIO creates when writing the object.
For example, an erasure set may have eight total drives and use three drives during a write for parity.
In this scenario, MinIO splits an object into 5 data shards and create 3 parity shards.
MinIO distributes these eight shards across the drives in the erasure set.
No one drive contains only parity shards or only data shards.
Instead, MinIO writes shards for each object in a randomized way to distribute reads evenly across drives.

When MinIO needs to provide the object, it looks for the data shards for the object.
If any of the data shards are missing or damaged, MinIO uses one or more of the parity shards to restore the object.
When looking for the parity shards, if any of the parity shards are missing or damaged, MinIO restores those as well, provided there are sufficient other shards to serve the object.
For this scenario, up to three of data shard parts can be lost or damaged and MinIO can still successfully restore and serve the object.

The number of drives available with intact data or parity shards of the object must meet or exceed the number of drives used for data shards in the erasure set.
In the scenario above, five drives with intact shards must be online and available for MinIO to successfully serve the object.

## When does MinIO heal an object?

MinIO has a robust system for healing objects.

### Healing during GET requests

MinIO automatically checks the consistency of an object’s data shards each time you request an object with a `GET` or `HEAD` operation.
For versioned buckets, MinIO also checks for consistency during `PUT` operation.

If all of the data shards are found intact, MinIO serves the object from the data shards without inspecting the corresponding parity shards.

If the object has missing or damaged data shards, MinIO uses the available parity shards to heal the object before serving it as part of the operation.
There must be an intact parity shard available for each lost or damaged data shard, otherwise the object cannot be recovered.
If any parity shards are lost or damaged, MinIO restores the parity shard, provided there are sufficient other parity shards to serve the object.

### Healing with the object scanner

MinIO uses an [[operations/concepts/scanner#minio-concepts-scanner|object scanner]] to perform a number of tasks related to objects.
One of these tasks checks the integrity of objects and, if found damaged or corrupted, heals them.

On each scanning pass, MinIO uses a hash of the object name to select one out of every 1,024 objects to check.

If any object is found to have lost shards, MinIO heals the object from available shards.
By default, MinIO does not check for [[glossary#term-bit-rot|bit rot]] corruption using the scanner.
This can be an expensive operation to perform and the risk of bit rot across multiple disks is low.

### Healing by manual request

Administrators can use [[reference/minio-mc-admin/mc-admin-heal#command-mc.admin.heal|`mc admin heal`]] to initiate a full system healing.
The procedure is very resource intensive and not typically needed.

Consult with MinIO Engineers before manually starting a healing process on a deployment.

## Healing metrics

MinIO provides several [healing metrics](https://silo.pigsty.io/operations/monitoring/metrics-v2.html#healing-metrics) to monitor the status of healing processes on a deployment.

Refer to the [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|Metrics and alerts]] for more information on available endpoints and configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).