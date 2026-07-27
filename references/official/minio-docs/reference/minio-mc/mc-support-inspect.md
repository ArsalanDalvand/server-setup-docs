---
title: "mc support inspect — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-inspect.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-inspect"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support inspect — SILO (MinIO Object Storage)"
---
# mc support inspect

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Description

The [`mc support inspect`](#command-mc.support.inspect) command collects the data and metadata associated to objects at the specified path.

MinIO assembles this data from each backend drive storing an [[operations/concepts/erasure-coding#minio-erasure-coding|erasure shard]] for each specified object.
The command produces an encrypted zip file that includes all matching files with their respective host+drive+path.

If this information is required to diagnose a [MinIO SUBNET](https://min.io/pricing?jmp=docs) issue, MinIO Engineering will provide the appropriate command.
The resulting report is intended for use by MinIO Engineering via SUBNET and may contain internal or private data points associated to the object.
Exercise caution before sending a report to a third party or posting the report in a public forum.

Changed in version RELEASE.2023-01-11T03-14-16Z: The file uploads to MinIO for use by the engineering team in support efforts.
The file saves the the current working directory in the event the file does not successfully upload (such as in an airgapped environment).

Changed in version RELEASE.2022-12-12T19-27-27Z: When writing the zip archive, MinIO also encrypts the zip index of file names included in the archive.

Changed in version RELEASE.2024-10-29T15-34-59Z: Inspect now generates unique file names to help distinguish one inspect file from another.
The file name reflects the inspected path.

Important

[`mc support inspect`](#command-mc.support.inspect) requires a MinIO deployment server from October 2021 or later.

## Wildcards

The command supports wildcard `*` pattern matching for prefixes or objects when using the Bash shell.
For non-Bash shells, a message displays indicating that wildcard patterns are only supported in Bash.

```
mc support inspect ALIAS/bucket/path/**/xl.meta
```

This command collects all `xl.meta` associated to objects at `ALIAS/bucket/path/`.

## Examples

### Download Metadata for an Object

You can download the metadata for an object.
Metadata stores in an `xl.meta` binary file.

The following command downloads the `xl.meta` from `mybucket/myobject` on the `minio1` deployment.

The file downloads from all drives as a zip archive file.

```
mc support inspect minio1/mybucket/myobject/xl.meta
```

The contents of the `xl.meta` file are not human readable.
You can convert the contents of an `xl.meta` file to JSON format.

### Download All Objects at a Prefix Recursively

The following command downloads all objects recursively found at a prefix.

Caution

This can be an expensive operation.
Proceed with caution.

```
mc support inspect minio1/mybucket/myobject/**
```

## Syntax

The command has the following syntax:

```
mc [GLOBALFLAGS] support inspect       \
                         [--legacy]   \
                         TARGET
```

### Parameters

--legacy[](#mc.support.inspect.-legacy)
Optional

Use the older method of exporting inspection data, which does not encrypt data by default.

TARGET[](#mc.support.inspect.TARGET)
Required

The path to the location or object to inspect.
The path should include the alias <alias> of the MinIO deployment and, if needed, the prefix and/or object name.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).