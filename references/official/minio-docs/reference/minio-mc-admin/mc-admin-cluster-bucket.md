---
title: "mc admin cluster bucket — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-cluster-bucket.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-cluster-bucket"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin cluster bucket — SILO (MinIO Object Storage)"
---
# mc admin cluster bucket

## Description

New in version RELEASE.2022-06-17T02-52-50Z.

The [`mc admin cluster bucket`](#command-mc.admin.cluster.bucket) command and its subcommands provide tools for manually importing and exporting MinIO bucket metadata.

This metadata includes configurations related to features like [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|lifecycle management rules]].
You can use this metadata as a snapshot of the bucket configuration for restoration later, such as part of BC/DR or backup/restore operations.

You can use this command on individual buckets or on all buckets in a MinIO deployment.
For automatic synchronization of all buckets in a deployment to a remote site, use [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]].

The [`mc admin cluster bucket`](#command-mc.admin.cluster.bucket) command has the following subcommands:

Subcommand

Description

[[reference/minio-mc-admin/mc-admin-cluster-bucket-import#command-mc.admin.cluster.bucket.import|`import`]]

The [[reference/minio-mc-admin/mc-admin-cluster-bucket-import#command-mc.admin.cluster.bucket.import|`mc admin cluster bucket import`]] command imports bucket metadata as created by the [[reference/minio-mc-admin/mc-admin-cluster-bucket-export#command-mc.admin.cluster.bucket.export|`mc admin cluster bucket export`]] command.

[[reference/minio-mc-admin/mc-admin-cluster-bucket-export#command-mc.admin.cluster.bucket.export|`export`]]

The [[reference/minio-mc-admin/mc-admin-cluster-bucket-export#command-mc.admin.cluster.bucket.export|`mc admin cluster bucket export`]] command exports bucket metadata for use with the [[reference/minio-mc-admin/mc-admin-cluster-bucket-import#command-mc.admin.cluster.bucket.import|`mc admin cluster bucket import`]] command.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).