---
title: "Enable Multi-Site Server-Side Bucket Replication — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/bucket-replication/enable-server-side-multi-site-bucket-replication.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/bucket-replication/enable-server-side-multi-site-bucket-replication"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Enable Multi-Site Server-Side Bucket Replication — SILO (MinIO Object Storage)"
---
# Enable Multi-Site Server-Side Bucket Replication

The procedure on this page configures automatic server-side bucket replication between multiple MinIO deployments. Multi-Site Active-Active replication builds on the [[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|Enable Two-Way Server-Side Bucket Replication]] procedure with additional considerations required to ensure predictable replication behavior across all sites.

[](../../_images/active-active-multi-replication1.svg)

To configure replication between arbitrary S3-compatible services, use [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]].

To configure one-way “active-active” replication between two MinIO deployments, see [[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|Enable Two-Way Server-Side Bucket Replication]].

To configure one-way “active-passive” replication between MinIO deployments, see [[administration/bucket-replication/enable-server-side-one-way-bucket-replication#minio-bucket-replication-serverside-oneway|Enable One-Way Server-Side Bucket Replication]].

Multi-Site Active-Active replication configurations can span multiple racks, datacenters, or geographic locations. Complexity of configuring and maintaining multi-site configurations generally increase with the number of sites and size of each site. Enterprises looking to implement multi-site replication should consider leveraging [MinIO SUBNET](https://min.io/pricing?ref=docs) support to access the expertise, planning, and engineering resources required for addressing that use case.

See also

Use the [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]] command to modify an existing replication rule.

Use the [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]] command with the [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-state|`--state "disable"`]] flag to disable an existing replication rule.

Use the [[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`mc replicate rm`]] command to remove an existing replication rule.

## Requirements

You must meet all of the basic requirements for bucket replication described in [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|Bucket Replication Requirements]].

In addition, to create multi-site bucket replication set up, you must meet the following additional requirements:

### Access to All Clusters

You must have network access and log in credentials with correct permissions to all deployments to set up multi-site active-active bucket replication.

You can access the deployments by installing [[reference/minio-mc#command-mc|`mc`]] and using the command line.
Use the [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] command to create an alias for each MinIO deployment.

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

Click to expand any of the following:

Use Consistent Replication Settings

MinIO supports customizing the replication configuration to enable or disable the following replication behaviors:

Replication of [[administration/object-management/object-delete#minio-object-delete|delete operations]]

Replication of delete markers

Replication of existing objects

Replication of metadata-only changes

When configuring replication rules for a bucket, ensure that all MinIO deployments participating in multi-site replication use the same replication behaviors to ensure consistent and predictable synchronization of objects.

Replication of Existing Objects

MinIO supports automatically replicating existing objects in a bucket.

MinIO requires explicitly enabling replication of existing objects using the [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-replicate|`mc replicate add --replicate`]] or [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-replicate|`mc replicate update --replicate`]] and including the `existing-objects` replication feature flag.
This procedure includes the required flags for enabling replication of existing objects.

Replication of Delete Operations

MinIO supports replicating [[administration/object-management/object-delete#minio-object-delete|delete operations]] onto the target bucket.
Specifically, MinIO can replicate versioning [Delete Markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/versioning-workflows.html) and the deletion of specific versioned objects:

For delete operations on an object, MinIO replication also creates the delete marker on the target bucket.

For delete operations on versions of an object, MinIO replication also deletes those versions on the target bucket.

MinIO requires explicitly enabling replication of delete operations using the [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-replicate|`mc replicate add --replicate`]] or [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-replicate|`mc replicate update --replicate`]].
This procedure includes the required flags for enabling replication of delete operations and delete markers.

MinIO does not replicate delete operations resulting from the application of [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|lifecycle management expiration rules]].
Configure matching expiration rules for the bucket on all replication sites to ensure consistent application of object expiration.

## Procedure

This procedure requires repeating steps for each MinIO deployment participating in the multi-site replication configuration. Depending on the number of deployments, this procedure may require significant time and care in implementation. MinIO recommends reading through the procedure before attempting to implement the documented steps.

Configure Multi-Site Bucket Replication Using the Command Line

[Create New Bucket Replication Rules](#minio-bucket-replication-multi-site-minio-cli-create-replication-rules)

[Validate the Replication Configuration](#minio-bucket-replication-multi-site-minio-cli-verify-replication-config)

### Configure Multi-Site Bucket Replication Using the Command Line mc

This procedure uses the placeholder `ALIAS` to reference the [[reference/minio-mc/mc-alias-set#alias|alias]] each MinIO deployment being configured for replication.
Replace these values with the appropriate alias for each MinIO deployment.

This procedure assumes each alias corresponds to a user with the [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|necessary replication permissions]].

Changed in version RELEASE.2022-12-24T15-21-38Z: [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]] automatically creates the necessary replication targets, removing the need for using the deprecated `mc admin remote bucket add` command.
This procedure only documents the procedure as of that release.

#### 1) Create New Bucket Replication Rules

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

Repeat these commands for each remote MinIO deployment participating in the multi-site replication configuration.
For example, a multi-site replication configuration consisting of MinIO deployments `minio1`, `minio2`, and `minio3` would require repeating this step on each deployment for each remote.

Specifically, in this scenario, perform this step twice on each deployment:

On the `minio1` deployment, once for a rule for `minio2` and again for a separate rule for `minio3`.

On the `minio2` deployment, once for a rule for `minio1` and again for a separate rule for `minio3`.

On the `minio3` deployment, once for a rule for `minio1` and again for a separate rule for `minio2`.

#### 2) Validate the Replication Configuration

Use [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] to copy a new object to the replicated bucket on one of the deployments.

```
mc cp ~/foo.txt ALIAS/BUCKET
```

Use [[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]] to verify the object exists on the destination bucket:

```
mc ls ALIAS/BUCKET
```

Repeat this test on each deployment by copying a new unique file and checking that the file replicates to each of the other deployments.

You can also use [[reference/minio-mc/mc-stat#command-mc.stat|`mc stat`]] to check the file to check the current [[administration/bucket-replication#minio-replication-process|replication stage]] of the object.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).