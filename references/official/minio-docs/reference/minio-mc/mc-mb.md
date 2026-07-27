---
title: "mc mb — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-mb.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-mb"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc mb — SILO (MinIO Object Storage)"
---
# mc mb

## Syntax

The [`mc mb`](#command-mc.mb) command creates a new bucket or directory at the
specified path.

You can also use [`mc mb`](#command-mc.mb) against the local filesystem to produce
similar results to the `mkdir -p` commandline tool.

EXAMPLE

The following command creates a new bucket `mydata` on the
`myminio` MinIO deployment. The command creates the bucket
with [[administration/object-management/object-retention#minio-object-locking|object locking enabled]].

```
mc mb --with-locks myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] mb                   \
                 [--ignore-existing]  \
                 [--region "string"]  \
                 [--with-lock]        \
                 [--with-versioning]  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.mb.ALIAS)
Required

The MinIO or other S3-compatible service on which to create the new bucket.

For creating a bucket on MinIO, specify the
[[reference/minio-mc/mc-alias-set#alias|alias]] and the name of the bucket. For example:

```
mc mb play/mybucket
```

For creating a directory on a local filesystem, specify the full
path to that directory. For example:

```
mc mb ~/mydata/mydir
```

--ignore-existing, p[](#mc.mb.-ignore-existing)
Optional

Directs [`mc mb`](#command-mc.mb) to do nothing if the bucket or directory already exists.

--region[](#mc.mb.-region)
Optional

The region in which to create the specified bucket.
Has no effect if the specified [`ALIAS`](#mc.mb.ALIAS) is a filesystem directory.

If not specified, default value is `us-east-1`.

--with-lock, l[](#mc.mb.-with-lock)
Optional

Enables [[administration/object-management/object-retention#minio-object-locking|object locking]] on the specified bucket.
Object locking requires, and therefore implies, enabling object versioning.

Important

You can only enable object locking when creating the bucket.
Buckets created without object locking cannot use
[[administration/object-management/object-lifecycle-management#minio-lifecycle-management|Bucket Lifecycle Management]] or
[[administration/object-management/object-retention#minio-object-locking|Bucket Object Locking]] functionality.

--with-versioning[](#mc.mb.-with-versioning)
Optional

Enables [[administration/object-management/object-versioning#minio-bucket-versioning|object versioning]] on the new bucket.
With versioning enabled, by default MinIO allows up to the maximum value of an Int64 versions per object, or over 9.2 quintillion.
Define [[administration/object-management/create-lifecycle-management-expiration-rule#minio-lifecycle-management-create-expiry-rule|object expiration]] rules to remove versions of objects no longer needed, such as by the number of versions or the date of versions.

Versioning is required for [[administration/bucket-replication#minio-bucket-replication|bucket replication]] or [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]].
Versioning does not imply or require object locking.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Create Bucket with Object Locking

Use [`mc mb`](#command-mc.mb) to create a bucket on an S3-compatible host.
The [`--with-lock`](#mc.mb.-with-lock) option creates the bucket with locking enabled:

```
mc mb --with-lock ALIAS/BUCKET
```

Replace [`ALIAS`](#mc.mb.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`BUCKET`](#mc.mb.ALIAS) with the bucket to create.

### Create a New Bucket in a Specific Region

Use [`mc mb`](#command-mc.mb) to create a bucket on an S3-compatible host.
The [`--region`](#mc.mb.-region) option creates the bucket in a desired region.

```
mc mb --region --region=us-west-2 myminio/mynewbucket
```

The above command creates a new bucket, `mynewbucket` on the `myminio` bucket within the `us-west-2` region.

### Create a New Bucket with Versioning Enabled

```
mc mb --with-versioning myminio/myversionedbucket
```

The above command creates a new bucket, `myversionedbucket`, on the `myminio` alias.
The new bucket enables [[administration/object-management/object-versioning#minio-bucket-versioning|object versioning]] for all objects in the bucket.

## Behavior

### Bucket Limits Per Deployment

MinIO does not limit the number of buckets you can create on a deployment.
However, MinIO recommends no more than 500,000 buckets per deployment as a general guideline.

### Bucket Limits for Non-MinIO S3 Services

Certain S3 services may restrict the number of buckets a given user or account
can create. For example, Amazon S3 limits each account to
[100 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/BucketRestrictions.html). [`mc mb`](#command-mc.mb) may return an
error if the user has reached bucket limits on the target S3 service.

MinIO Object Storage deployments do not place any limits on the number of
buckets each user can create.

### Enable Object Locking at Bucket Creation

MinIO follows
[AWS S3 behavior](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html#object-lock-bucket-config)
where you must enable [[administration/object-management/object-retention#minio-object-locking|object locking]] at
bucket creation. Buckets created without object locking can never enable object
retention or locking.

Enabling bucket locking does not set any object locking or retention settings.
Consider enabling bucket locking as standard practice.

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).