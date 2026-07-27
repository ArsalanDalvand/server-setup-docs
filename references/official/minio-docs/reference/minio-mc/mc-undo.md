---
title: "mc undo — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-undo.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-undo"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc undo — SILO (MinIO Object Storage)"
---
# mc undo

## Syntax

The [`mc undo`](#command-mc.undo) command reverses changes due to either a `PUT` or `DELETE` operation at a specified path.

EXAMPLE

The following command reverts the last three uploads and/or deletions of the `file.zip` object on the `myminio` deployment in the `data` bucket:

```
mc undo myminio/data/file.zip --last 3
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] undo                \
                 TARGET              \
                 [--action "type"]   \
                 [--force]           \
                 [--last "integer"]  \
                 [--recursive, r]    \
                 [--dry-run]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.undo.TARGET)
Required

The full path to the object or prefix where the command should run.
The path must include the [[reference/minio-mc/mc-alias-set#minio-mc-alias|ALIAS]], bucket, and prefix or object name.

--action[](#mc.undo.-action)
Optional

Undo the most recent change of the specified type.
Accepted values are `DELETE` or `PUT`.

By default, [`mc undo`](#command-mc.undo) reverses both `DELETE` and `PUT` operations.
Use [`--action`](#mc.undo.-action) to choose one or the other, but only for the most recent operation of the specified type.

The following command reverts the most recent `PUT` for the object `today.zip` in bucket `data`, reverting to the previous object version:

```
mc undo myminio/data/today.zip --action "PUT"
```

This example reverts the most recent `DELETE` for the prefix `archive`, recursively restoring it and any child objects:

```
mc undo myminio/data/archive --recursive --action "DELETE"
```

Mutually exclusive with [`--last`](#mc.undo.-last).

--dry-run[](#mc.undo.-dry-run)
Optional

Output the results of the command without actually performing the operations.
Use this flag to test the outcome of running the command in a particular way.

--force[](#mc.undo.-force)
Optional

Force a recursive operation.

--last[](#mc.undo.-last)
Optional

Accepts an integer value specifying the number of `PUT` and/or `DELETE` changes to undo.

If not specified, the command reverses one (`1`) operation.
Mutually exclusive with [`--action`](#mc.undo.-action).

--recursive, r[](#mc.undo.-recursive)
Optional

Performs the command in a recursive fashion.
Use this flag to undo changes on a prefix, for example.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Undo the Last Three Uploads or Deletions on an Object

The following command reverts the last three uploads and/or deletions of the `file.zip` object on the `myminio` deployment in the `data` bucket:

```
mc undo myminio/data/file.zip --last 3
```

### Undo the Last Upload or Deletion of any Object at a Prefix

Use [`mc undo`](#command-mc.undo) to reverse the most recent `PUT` or `DELETE` operation performed on the `myminio` alias in the `data` bucket under the `presentations/recordings/` [[glossary#term-prefix|prefix]]:

```
mc undo myminio/data/presentations/recordings/ --recursive --force
```

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