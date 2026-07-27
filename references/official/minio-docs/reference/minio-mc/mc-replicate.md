---
title: "mc replicate — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate — SILO (MinIO Object Storage)"
---
# mc replicate

## Description

The [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate`]] command configures and manages the [[administration/bucket-replication#minio-bucket-replication-serverside|Server-Side Bucket Replication]] for a MinIO deployment, including [[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|active-active replication configurations]] and [[administration/bucket-replication#minio-replication-behavior-resync|resynchronization]].

Note

For multi-site replication, see [[reference/minio-mc-admin/mc-admin-replicate#command-mc.admin.replicate|`mc admin replicate`]].

## Subcommands

[`mc replicate`](#command-mc.replicate) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`add`]]

The [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]] command creates a new [[administration/bucket-replication#minio-bucket-replication-serverside|server-side replication]] rule for a bucket on a MinIO deployment.

[[reference/minio-mc/mc-replicate-backlog#command-mc.replicate.backlog|`backlog`]]

The [[reference/minio-mc/mc-replicate-backlog#command-mc.replicate.backlog|`mc replicate backlog`]] shows a list of unreplicated new or deleted objects.

[[reference/minio-mc/mc-replicate-export#command-mc.replicate.export|`export`]]

The [[reference/minio-mc/mc-replicate-export#command-mc.replicate.export|`mc replicate export`]] command exports the JSON-formatted
[[administration/bucket-replication#minio-bucket-replication-serverside|replication rules]] for a
MinIO bucket to `STDOUT`.

[[reference/minio-mc/mc-replicate-import#command-mc.replicate.import|`import`]]

The [[reference/minio-mc/mc-replicate-import#command-mc.replicate.import|`mc replicate import`]] command imports JSON-formatted
[[administration/bucket-replication#minio-bucket-replication-serverside|replication rules]] for a
MinIO bucket from `STDIN`.

[[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`ls`]]

The [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] command lists all
[[administration/bucket-replication#minio-bucket-replication-serverside|replication rules]] on a
MinIO bucket.

[[reference/minio-mc/mc-replicate-resync#command-mc.replicate.resync|`resync`]]

The [[reference/minio-mc/mc-replicate-resync#command-mc.replicate.resync|`mc replicate resync`]] command resynchronizes all objects in the
specified MinIO bucket to a remote [[administration/bucket-replication#minio-bucket-replication-serverside|replication]] target.

[[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`rm`]]

The [[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`mc replicate rm`]] command removes a
[[administration/bucket-replication#minio-bucket-replication-serverside|replication rule]] from a
MinIO bucket.

[[reference/minio-mc/mc-replicate-status#command-mc.replicate.status|`status`]]

The [[reference/minio-mc/mc-replicate-status#command-mc.replicate.status|`mc replicate status`]] command displays the [[administration/bucket-replication#minio-bucket-replication-serverside|replication status]] of a MinIO bucket.
The status also lists the remote target path or location.

[[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`update`]]

The [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]] command modifies an existing
[[administration/bucket-replication#minio-bucket-replication-serverside|bucket replication rule]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).