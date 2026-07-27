---
title: "mc rb — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-rb.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-rb"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc rb — SILO (MinIO Object Storage)"
---
# mc rb

## Syntax

The [`mc rb`](#command-mc.rb) command removes one or more buckets on MinIO or
another S3-compatible service.

To remove only the contents of a bucket, use [[reference/minio-mc/mc-rm#command-mc.rm|`mc rm`]] instead.

Important

[`mc rb`](#command-mc.rb) permanently deletes bucket(s) on the target deployment,
including any and all [[administration/object-management/object-versioning#minio-bucket-versioning|object versions]]
and bucket configurations such as
[[administration/object-management/object-lifecycle-management#minio-lifecycle-management|lifecycle management]] or
[[administration/bucket-replication#minio-bucket-replication-serverside|replication]].

You can also use [`mc rb`](#command-mc.rb) against the local filesystem to produce
similar results to the `rm --rf` commandline tool.

EXAMPLE

The following command removes the `mydata` bucket on the
`myminio` MinIO deployment:

```
mc rb --force myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] rb             \
                 --force        \
                 [--dangerous]  \
                 ALIAS [ALIAS...]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.rb.ALIAS)

Required The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO or other S3-compatible
service and the full path to the bucket to remove. For example:

```
mc rb --force myminio/mydata
```

Omit the bucket path to perform a site-wide removal of buckets on the MinIO
deployment. This operation requires specifying
[`--dangerous`](#mc.rb.-dangerous) to explicitly acknowledge the permanent
removal of all data on the deployment. For example:

```
mc rb --force --dangerous myminio
```

For removing a directory and its contents on a local filesystem, specify
the full path to that directory. The
[`--force`](#mc.rb.-force) flag is ignored if specified. For example:

```
mc rb ~/data/myolddata
```

You can specify multiple `ALIAS` targets consisting of either
MinIO or local filesystem directories. The command attempts to remove
all specified targets. For example:

```
mc rb --force myminio/mydata ~/data/myolddata
```

--force[](#mc.rb.-force)

Required Safety flag to confirm removal of the bucket contents.

--dangerous[](#mc.rb.-dangerous)

Optional Directs [`mc rb`](#command-mc.rb) to perform a site-wide removal of all
buckets on each specified [`ALIAS`](#mc.rb.ALIAS) (e.g. `myminio/`).

If any `ALIAS` specifies a filesystem directory, this option
results in the removal of all subdirectories and files at that directory
path similar to `rm --rf`.

Warning

Running [`mc rb --dangerous`](#mc.rb.-dangerous) is irreversible. Exercise all
possible due diligence in ensuring the command applies to only the
desired `ALIAS` targets prior to execution.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Remove a Bucket

```
mc rb --force ALIAS/PATH
```

Replace [`ALIAS`](#mc.rb.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of
a configured S3-compatible host.

Replace [`PATH`](#mc.rb.ALIAS) with the path to the bucket to remove.

## Behavior

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