---
title: "Enable Two-Way Server-Side Bucket Replication — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/bucket-replication/enable-server-side-two-way-bucket-replication.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/bucket-replication/enable-server-side-two-way-bucket-replication"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Enable Two-Way Server-Side Bucket Replication — SILO (MinIO Object Storage)"
---
# Enable Two-Way Server-Side Bucket Replication

The procedure on this page creates a new bucket replication rule for two-way “active-active” synchronization of objects between MinIO buckets.

[](../../_images/active-active-twoway-replication1.svg)

To configure replication between arbitrary S3-compatible services, use [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]].

To configure one-way “active-passive” replication between MinIO clusters, see [[administration/bucket-replication/enable-server-side-one-way-bucket-replication#minio-bucket-replication-serverside-oneway|Enable One-Way Server-Side Bucket Replication]].

To configure multi-site “active-active” replication between MinIO clusters, see [[administration/bucket-replication/enable-server-side-multi-site-bucket-replication#minio-bucket-replication-serverside-multi|Enable Multi-Site Server-Side Bucket Replication]].

This tutorial covers configuring Active-Active replication between two MinIO clusters. For a tutorial on multi-site replication between three or more MinIO clusters, see [[administration/bucket-replication/enable-server-side-multi-site-bucket-replication#minio-bucket-replication-serverside-multi|Enable Multi-Site Server-Side Bucket Replication]].

## Requirements

You must meet all of the basic requirements for bucket replication described in [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|Bucket Replication Requirements]].

In addition, to set up active-active bucket replication, you must meet the following additional requirements:

### Access to Both Clusters

You must have network access and login credentials with required permissions to both deployment to set up active-active bucket replication.

You can access the deployments by installing [[reference/minio-mc#command-mc|`mc`]] and using the command line.
Use the [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] command to create an alias for both MinIO deployments.

Alias creation requires specifying an access key for a user on the deployment.
This user must have permission to create and manage users and policies on the deployment.

Specifically, ensure the user has at minimum:

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-CreateUser|`admin:CreateUser`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-ListUsers|`admin:ListUsers`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-GetUser|`admin:GetUser`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-CreatePolicy|`admin:CreatePolicy`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-GetPolicy|`admin:GetPolicy`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-AttachUserOrGroupPolicy|`admin:AttachUserOrGroupPolicy`]]

## Considerations

Use Consistent Replication Settings

MinIO supports customizing the replication configuration to enable or disable the following replication behaviors:

Replication of [[administration/object-management/object-delete#minio-object-delete|delete operations]]

Replication of delete markers

Replication of existing objects

Replication of metadata-only changes

When configuring replication rules for a bucket, ensure that both MinIO deployments participating in active-active replication use the same replication behaviors to ensure consistent and predictable synchronization of objects.

Replication of Existing Objects

MinIO supports automatically replicating existing objects in a bucket.

MinIO requires explicitly enabling replication of existing objects using the [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-replicate|`mc replicate add --replicate`]] or [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-replicate|`mc replicate update --replicate`]] and including the `existing-objects` replication feature flag.
This procedure includes the required flags for enabling replication of existing objects.

Replication of Delete Operations

MinIO supports replicating delete operations onto the target bucket.
Specifically, MinIO can replicate versioning [Delete Markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/versioning-workflows.html) and the deletion of specific versioned objects:

For delete operations on an object, MinIO replication also creates the delete marker on the target bucket.

For delete operations on versions of an object, MinIO replication also deletes those versions on the target bucket.

MinIO requires explicitly enabling replication of delete operations using the [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-replicate|`mc replicate add --replicate`]] or [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-replicate|`mc replicate update --replicate`]].
This procedure includes the required flags for enabling replication of delete operations and delete markers.

MinIO does not replicate delete operations resulting from the application of [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|lifecycle management expiration rules]].
Configure matching expiration rules on both the source and destination bucket to ensure consistent application of object expiration.

See [[administration/bucket-replication#minio-replication-behavior-delete|Replication of Delete Operations]] and [[administration/object-management/object-delete#minio-object-delete|Object Deletion]] for more complete documentation.

Multi-Site Replication

MinIO supports configuring multiple remote targets per bucket or bucket prefix.
This enables configuring multi-site active-active replication between MinIO deployments.

This procedure covers active-active replication between two MinIO sites.
You can repeat this procedure for each “pair” of MinIO deployments in the replication mesh. For a dedicated tutorial, see [[administration/bucket-replication/enable-server-side-multi-site-bucket-replication#minio-bucket-replication-serverside-multi|Enable Multi-Site Server-Side Bucket Replication]].

## Procedure

[Configure Two-Way Bucket Replication Using the Command Line](#minio-bucket-replication-two-way-minio-cli-procedure)

[Create Replication Remote Targets](#minio-bucket-replication-two-way-minio-cli-create-remote-targets)

[Create a New Bucket Replication Rule on Each Deployment](#minio-bucket-replication-two-way-minio-cli-create-replication-rules)

[Validate the Replication Configuration](#minio-bucket-replication-two-way-minio-cli-verify-replication-config)

### Configure Two-Way Bucket Replication Using the Command Line mc

This procedure creates two-way, active-active replication between two MinIO deployments.

This procedure assumes you have already defined an alias for each deployment as a user with the [necessary replication permissions](#minio-bucket-replication-serverside-twoway-permissions).

Changed in version RELEASE.2022-12-24T15-21-38Z: [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]] automatically creates the necessary replication targets, removing the need for using the deprecated `mc admin remote bucket add` command.
This procedure only documents the procedure as of that release.

#### 1) Create a New Bucket Replication Rule on Each Deployment

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

The `USER:PASSWORD` must correspond to a user on the remote deployment with the [necessary replication permissions](#minio-bucket-replication-serverside-twoway-permissions).

The `HOSTNAME:PORT` must resolve to a reachable MinIO instance on the remote deployment.
The `BUCKET` must exist and otherwise meet all other [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|replication requirements]].

The `--replicate "delete,delete-marker,existing-objects"` flag enables the following replication features:

[[administration/bucket-replication#minio-replication-behavior-delete|Replication of Deletes]]

[[administration/bucket-replication#minio-replication-behavior-existing-objects|Replication of existing Objects]]

See [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-replicate|`mc replicate add --replicate`]] for more complete documentation.
Omit any field to disable replication of that component.

Specify any other supported optional arguments for [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]].

Repeat this step on the other MinIO deployment.
Change the `ALIAS` and `--remote-bucket` values to correspond to the first deployment.

You should have two replication rules configured at the conclusion of this step - one created on each deployment that points to the bucket on the other deployment.
Use the [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] command to verify the created replication rules.

#### 2) Validate the Replication Configuration

Use [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] to copy a new object to the replicated bucket on one of the deployments.

```
mc cp ~/foo.txt ALIAS/BUCKET
```

Use [[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]] to verify the object exists on the destination bucket:

```
mc ls ALIAS/BUCKET
```

Repeat this test by copying another object to the second deployment and verifying the object replicates to the first deployment.

Once both objects exist on both deployments, you have successfully set up two-way, active-active replication between MinIO buckets.

See also

Use the [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]] command to modify an existing
replication rule.

Use the [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]] command with the [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-state|`--state "disable"`]] flag to disable an existing replication rule.

Use the [[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`mc replicate rm`]] command to remove an existing replication rule.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).