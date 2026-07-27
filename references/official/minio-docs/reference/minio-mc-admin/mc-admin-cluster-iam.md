---
title: "mc admin cluster iam — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-cluster-iam.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-cluster-iam"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin cluster iam — SILO (MinIO Object Storage)"
---
# mc admin cluster iam

## Description

New in version RELEASE.2022-06-26T18-51-48Z.

The [`mc admin cluster iam`](#command-mc.admin.cluster.iam) command and its subcommands provide tools for manually importing and exporting MinIO [[administration/identity-access-management#minio-authentication-and-identity-management|identity and access management (IAM)]] metadata.

For automatic synchronization of all IAM configurations in a deployment to a remote site, use [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]].

The [`mc admin cluster iam`](#command-mc.admin.cluster.iam) command has the following subcommands:

Subcommand

Description

[[reference/minio-mc-admin/mc-admin-cluster-iam-import#command-mc.admin.cluster.iam.import|`import`]]

The [[reference/minio-mc-admin/mc-admin-cluster-iam-import#command-mc.admin.cluster.iam.import|`mc admin cluster iam import`]] command imports [[administration/identity-access-management#minio-authentication-and-identity-management|IAM]] metadata as created by the [[reference/minio-mc-admin/mc-admin-cluster-iam-export#command-mc.admin.cluster.iam.export|`mc admin cluster iam export`]] command.

[[reference/minio-mc-admin/mc-admin-cluster-iam-export#command-mc.admin.cluster.iam.export|`export`]]

The [[reference/minio-mc-admin/mc-admin-cluster-iam-export#command-mc.admin.cluster.iam.export|`mc admin cluster iam export`]] command exports [[administration/identity-access-management#minio-authentication-and-identity-management|IAM]] metadata for use with the [[reference/minio-mc-admin/mc-admin-cluster-iam-import#command-mc.admin.cluster.iam.import|`mc admin cluster iam import`]] command.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).