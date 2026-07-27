---
title: "Managing Objects — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/console/managing-objects.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/console/managing-objects"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Managing Objects — SILO (MinIO Object Storage)"
---
# Managing Objects

You can use the MinIO Console to perform several of the bucket and object management and interaction functions available in MinIO.
Depending on the permissions and IAM policies for the authenticated user, you can:

[Browse, upload, revert, manage, and interact with objects](#minio-console-object-browser).

[Browse, create, and manage buckets](#minio-console-buckets).

[Create or monitor remote tiers](#minio-console-tiers) for object transition rules.

## Object Browser

The Object Browser lists the buckets and objects the authenticated user has access to on the deployment.

After logging in or navigating to the tab, the object browser displays a list of the user’s buckets, which the user can filter.
Select a bucket to show a list of objects in the bucket.

Select a specific object to display summary information about the object such as name, size, tags, holds, and retention policies that apply.
The console also shows the object’s metadata.

The user can perform actions on the bucket’s objects, depending on the policies and permissions that apply.
Example actions the user may be able to perform include:

Rewind to a previous version

Create prefixes

View deleted objects

Upload objects

Download objects

Share

Preview

Manage legal holds

Manage retention

Manage tags

Inspect

Display versions

[[administration/object-management/object-delete#minio-object-delete|Delete]]

New in version Console: v0.24.0

View the status of uploading or downloading objects with the object manager button available on the top right corner of the Console.
If you have not uploaded or downloaded any objects during the current session, the button does not appear.

Changed in version Console: v0.35.0

If you select multiple objects to download, MinIO creates a ZIP archive of those objects for downloading.
You must unzip or uncompress this archive after downloading to access the files.

## Buckets

The Console’s Bucket section displays all buckets to which the authenticated user has [[administration/identity-access-management/policy-based-access-control#minio-policy|access]].
Use this section to create or manage these buckets, depending on your user’s access.

### Creating Buckets

Select Create Bucket to create a new bucket on the deployment.
MinIO validates bucket names.
To see the rules for bucket names, select View Bucket Naming Rules.

MinIO does not limit the total number of buckets allowed on a deployment.
However, MinIO recommends no more than 500,000 buckets per deployment as a general guideline.

While creating a bucket, you can enable [[administration/object-management/object-versioning#minio-bucket-versioning|versioning]], [[administration/object-management/object-retention#minio-object-locking|object locking]], bucket size (quota) limits, and [[administration/object-management/object-retention#minio-object-locking-retention-modes|retention rules]] (which require versioning).

Changed in version Console: v0.35.0

If you enable versioning, you can specify prefixes to exclude from versioning.

You must configure replication, locking, and versioning options at the time of bucket creation.
You cannot change these settings for the bucket later.

### Managing Buckets

Use the Search bar to filter for specific buckets.
Select the row for the bucket to display summary information about the bucket.

Form the summary screen, select any of the available tabs to further manage the bucket.

Note

Some management features may not be available if the authenticated user does not have the [[administration/identity-access-management/policy-based-access-control#minio-policy-mc-admin-actions|required administrative permissions]].

When managing a bucket, your access settings may allow you to view or change any of the following:

The Summary section displays a summary of the bucket’s configuration.

Use this section to view and modify the bucket’s access policy, encryption, quota, and tags.

Configure alerts in the Events section to trigger [[administration/monitoring/bucket-notifications#minio-bucket-notifications|notification events]] when a user uploads, accesses, or deletes matching objects.

Copy objects to remote locations in the Replication section with [[administration/bucket-replication#minio-bucket-replication-serverside|Server Side Bucket Replication Rules]].

Expire or transition objects in the bucket from the Lifecycle section by setting up [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|Object Lifecycle Management Rules]].

Review security in the Access section by listing the [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] and [[administration/identity-access-management/minio-user-management#minio-users|users]] with access to that bucket.

Properly secure unauthenticated access with the Anonymous section by managing rules for prefixes that unauthenticated users can use to read or write objects.

## Tiers

The Tiering section provides an interface for adding and managing [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|remote tiers]] to support lifecycle management transition rules.
MinIO tiering supports moving objects from the deployment to the remote storage, but does not support automatically restoring them to the deployment.

The tiering tab allows users with the appropriate permissions to:

Review the status and summary information for all configured remote tiers.

Create a tier for a new remote target to storage on another MinIO deployment, Google Cloud Storage, Amazon’s AWS S3, or Azure.

Cycle the access credentials for any of the configured tiers with the tier’s  icon.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).