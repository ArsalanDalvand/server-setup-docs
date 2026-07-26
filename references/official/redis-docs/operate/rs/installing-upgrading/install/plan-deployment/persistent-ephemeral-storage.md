---
title: "Persistent and ephemeral node storage"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Persistent and ephemeral node storage"
---
# Persistent and ephemeral node storage

Configure paths for persistent storage and ephemeral storage.

            Redis Software
          

            Redis Enterprise for Kubernetes
          

For each node in the cluster, you can configure paths for both persistent
storage and ephemeral storage. To do so, the volume must have full permissions for user and group `redislabs` or users:group `redislabs:redislabs`. See the [[operate/rs/installing-upgrading/install/customize-user-and-group|Customize system user and group]] page for instructions.

Note:
  The persistent storageÂ and ephemeral storage discussed in this document are not related
to Redis persistence or AWS ephemeral drives.

## Persistent storage

Persistent storage is mandatory. The cluster uses persistent storage to store
information that needs to persist if a shard or a node fails,
such as server logs, configurations, and files.

To set the frequency of syncs, you can configure [persistence](/docs/latest/operate/rs/databases/configure/database-persistence/)
options for a database.

The persistent volume must be a storage area network (SAN)
using an EXT4 or XFS file system and be connected as an external storage volume.
NFS, NFS-like, and multi-read-write/shared storage options are not supported. These types of storage are often slow and can cause locking behaviors that are incompatible with the requirements of database storage.

When using append-only file (AOF) persistence, use flash-based storage
for the persistent volume.

## Ephemeral storage

Ephemeral storage is optional. If configured, temporary information that does not need to be persisted is stored by the cluster in the ephemeral storage.
This improves performance and helps reduce the load on the persistent storage.

Ephemeral storage must be a locally attached volume on each node.

## Disk size requirements

For disk size requirements, see:

[[operate/rs/installing-upgrading/install/plan-deployment/hardware-requirements|Hardware
requirements]]
for general guidelines regarding the ideal disk size for each type of
storage.
[[operate/rs/clusters/optimize/disk-sizing-heavy-write-scenarios|Disk size requirements for extreme write
scenarios]]
for special considerations when dealing with a high rate of write
commands.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page