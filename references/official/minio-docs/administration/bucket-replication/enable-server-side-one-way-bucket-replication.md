---
title: "Enable One-Way Server-Side Bucket Replication — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/bucket-replication/enable-server-side-one-way-bucket-replication.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/bucket-replication/enable-server-side-one-way-bucket-replication"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Enable One-Way Server-Side Bucket Replication — SILO (MinIO Object Storage)"
---
# Enable One-Way Server-Side Bucket Replication

The procedure on this page creates a new bucket replication rule for one-way synchronization of objects from one MinIO bucket to another MinIO bucket.
The buckets can be on the same MinIO deployment or on separate MinIO deployments.

[](../../_images/active-passive-oneway-replication1.svg)

To configure two-way “active-active” replication between MinIO buckets, see [[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|Enable Two-Way Server-Side Bucket Replication]].

To configure multi-site “active-active” replication between MinIO deployments, see [[administration/bucket-replication/enable-server-side-multi-site-bucket-replication#minio-bucket-replication-serverside-multi|Enable Multi-Site Server-Side Bucket Replication]]

Note

To configure replication between arbitrary S3-compatible services (not necessarily MinIO), use [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]].

## Requirements

Replication requires all participating clusters meet the [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|following requirements]].
This procedure assumes you have reviewed and validated those requirements.

For more details, see the [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|Bucket Replication Requirements]] page.

## Considerations

Click to expand any of the following:

Replication of Existing Objects

MinIO supports automatically replicating existing objects in a bucket.

MinIO requires explicitly enabling replication of existing objects using the [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-replicate|`mc replicate add --replicate`]] or [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-replicate|`mc replicate update --replicate`]] and including the `existing-objects` replication feature flag.
This procedure includes the required flags for enabling replication of existing objects.

Replication of Delete Operations

MinIO supports replicating S3 `DELETE` operations onto the target bucket.
Specifically, MinIO can replicate versioning [Delete Markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/versioning-workflows.html) and the deletion of specific versioned objects:

For delete operations on an object, MinIO replication also creates the delete marker on the target bucket.

For delete operations on versions of an object, MinIO replication also deletes those versions on the target bucket.

MinIO requires explicitly enabling replication of delete operations using the [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-replicate|`mc replicate add --replicate`]] or [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-replicate|`mc replicate update --replicate`]].
This procedure includes the required flags for enabling replication of delete operations and delete markers.

MinIO does not replicate delete operations resulting from the application of [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|lifecycle management expiration rules]].

See [[administration/bucket-replication#minio-replication-behavior-delete|Replication of Delete Operations]] and [[administration/object-management/object-delete#minio-object-delete|Object Deletion]] for more complete documentation.

Multi-Site Replication

MinIO supports configuring multiple remote targets per bucket or bucket prefix.
For example, you can configure a bucket to replicate data to two or more remote MinIO deployments, where one deployment is a 1:1 copy (replication of all operations including deletions) and another is a full historical record (replication of only non-destructive write operations).

This procedure documents one-way replication to a single remote MinIO deployment.
You can repeat this tutorial to replicate a single bucket to multiple remote targets.

## Procedure

[Configure One-Way Bucket Replication Using the Command Line](#minio-bucket-replication-one-way-minio-cli-procedure)

[Create a Replication Remote Target](#minio-bucket-replication-one-way-minio-cli-create-remote-targets)

[Create a New Bucket Replication Rule](#minio-bucket-replication-one-way-minio-cli-create-replication-rules)

[Validate the Replication Configuration](#minio-bucket-replication-one-way-minio-cli-verify-replication-config)

### Configure One-Way Bucket Replication Using the Command Line mc

This procedure uses the [[reference/minio-mc/mc-alias-set#alias|aliases]] `SOURCE` and `REMOTE` to reference each MinIO deployment being configured for replication.
Replace these values with the appropriate alias for your target MinIO deployments.

This procedure assumes each alias corresponds to a user with the [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-serverside-oneway-permissions|necessary replication permissions]].

Changed in version RELEASE.2022-12-24T15-21-38Z: [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]] automatically creates the necessary replication targets, removing the need for using the deprecated `mc admin remote bucket add` command.
This procedure only documents the procedure as of that release.

#### 1) Create a New Bucket Replication Rule

Use the [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]] command to add a new replication rule to each MinIO deployment.

```
mc replicate add ALIAS/BUCKET \
   --remote-bucket 'https://USER:PASSWORD@HOSTNAME:PORT/BUCKET' \
   --replicate "delete,delete-marker,existing-objects"
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the origin MinIO deployment.
The name must match the bucket specified when creating the remote target in the previous step.

Replace `BUCKET` with the name of the bucket to replicate from on the origin deployment.

Replace the `--remote-bucket` to specify the remote MinIO deployment and bucket to which the `ALIAS/BUCKET` replicates.

The `USER:PASSWORD` must correspond to a user on the remote deployment with the [[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway-permissions|necessary replication permissions]].

The `HOSTNAME:PORT` must resolve to a reachable MinIO instance on the remote deployment.
The `BUCKET` must exist and otherwise meet all other [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|replication requirements]].

The `--replicate "delete,delete-marker,existing-objects"` flag enables the following replication features:

[[administration/bucket-replication#minio-replication-behavior-delete|Replication of Deletes]]

[[administration/bucket-replication#minio-replication-behavior-existing-objects|Replication of existing Objects]]

See [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-replicate|`mc replicate add --replicate`]] for more complete documentation.
Omit any field to disable replication of that component.

Specify any other supported optional arguments for [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]].

#### 2) Validate the Replication Configuration

Use [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] to copy a new object to the replicated bucket on one of the deployments.

```
mc cp ~/foo.txt ALIAS/BUCKET
```

Use [[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]] to verify the object exists on the destination bucket:

```
mc ls ALIAS/BUCKET
```

See also

Use the [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]] command to modify an existing replication rule.

Use the [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]] command with the [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-state|`--state "disable"`]] flag to disable an existing replication rule.

Use the [[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`mc replicate rm`]] command to remove an existing replication rule.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).