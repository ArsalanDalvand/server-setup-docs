---
title: "mc find — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-find.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-find"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc find — SILO (MinIO Object Storage)"
---
# mc find

## Syntax

The [`mc find`](#command-mc.find) command supports searching for objects on a MinIO deployment.
You can also use the command to search for files on a filesystem.

EXAMPLE

The following command searches for all objects matching the specified
pattern in the `mydata` bucket on the `myminio` MinIO deployment:

```
mc find myminio/mydata --name "*.jpg"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] find                    \
                 [--exec "string"]       \
                 [--ignore "string"]     \
                 [--larger "string"]     \
                 [--maxdepth "string"]   \
                 [--metadata "string"]   \
                 [--name "string"]       \
                 [--newer-than "string"] \
                 [--older-than "string"] \
                 [--path "string"]       \
                 [--print "string"]      \
                 [--regex "string"]      \
                 [--smaller "string"]    \
                 [--tags "string"]`      \
                 [--versions]            \
                 [--watch]               \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.find.ALIAS)
Required

For objects on  MinIO or an S3-compatible host, specify the [[reference/minio-mc/mc-alias-set#alias|alias]] and the full path to search (e.g. bucket and prefixes).
For example:

```
mc find play/mydata/
```

For objects on a filesystem, specify the full path to search.
For example:

```
mc find ~/mydata/
```

Issuing [`mc find ALIAS`](#mc.find.ALIAS) with no other arguments returns a list of
all objects or files at the specified path, similar to [[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]].

--exec[](#mc.find.-exec)
Optional

Spawns an external process for each object returned by [`mc find`](#command-mc.find).
Supports [substitution formatting](#mc-find-substitution-format) of the output.

--ignore[](#mc.find.-ignore)
Optional

Exclude objects whose names match the specified [[reference/minio-mc#minio-wildcard-matching|wildcard pattern]].

--larger[](#mc.find.-larger)
Optional

Match all objects larger than the specified size in [units](#mc-find-units).

--maxdepth[](#mc.find.-maxdepth)
Optional

Limits directory navigation to the specified depth.

--metadata[](#mc.find.-metadata)
Optional

New in version mc: RELEASE.2023-04-12T02-21-51Z

For use with MinIO deployments only.

Return objects with metadata that matches a specified `key=value`.
Use the format `--metadata="KEY=value"`.

You can pass a key with an empty value.
In that case, `mc find` matches objects that do not have the metadata key or where the metadata key’s value is empty.

You can use the flag multiple times to match objects for additional metadata keys.
To return, an object must have matching values for all metadata keys.

--name[](#mc.find.-name)
Optional

Return objects whose names match the specified [[reference/minio-mc#minio-wildcard-matching|wildcard pattern]].

--newer-than[](#mc.find.-newer-than)
Optional

Mirror object(s) newer than the specified number of days.
Specify a string in `#d#hh#mm#ss` format.
For example: `--older-than 1d2hh3mm4ss`

Changed in version RELEASE.2025-02-04T04-57-50Z: The datetime may also be specified in absolute time of `YYYY-MM-DD HH:MM:SS TMZ` format.
For example, `mc find --newer-than="2025-01-22 09:57:00 CET" minioalias/mybucket`.

--older-than[](#mc.find.-older-than)
Optional

Mirror object(s) older than the specified time limit. Specify a
string in `#d#hh#mm#ss` format.
For example: `--older-than 1d2hh3mm4ss`

Changed in version RELEASE.2025-02-04T04-57-50Z: The datetime may also be specified in absolute time of `YYYY-MM-DD HH:MM:SS TMZ` format.
For example, `mc find --newer-than="2025-01-22 09:57:00 CET" minioalias/mybucket`.

Defaults to `0` (all objects).

--path[](#mc.find.-path)
Optional

Return the contents of directories whose names match the specified [[reference/minio-mc#minio-wildcard-matching|wildcard pattern]].

--print[](#mc.find.-print)
Optional

Prints results to `STDOUT`.
Supports [substitution formatting](#mc-find-substitution-format) of the output.

--regex[](#mc.find.-regex)
Optional

Returns objects or the contents of directories whose names match
the specified PCRE regex pattern.

--tags[](#mc.find.-tags)
Optional

New in version mc: RELEASE.2023-04-12T02-21-51Z

For use with MinIO deployments only.

Return objects with a tag that matches a specified [RE2 RegEx pattern](https://github.com/google/re2/wiki/Syntax).
Use the format `--tag="KEY=regexValue"`.

You can pass a key with an empty value.
In that case, `mc find` matches objects that do not have the metadata key or where the metadata key’s value is empty.

You can use the flag multiple times to match objects for additional tags.
To return, an object must have matching values for all tags.

--smaller[](#mc.find.-smaller)
Optional

Match all objects smaller than the specified size in
[units](#mc-find-units).

--versions[](#mc.find.-versions)
Optional

Include all object versions in the results.

--watch[](#mc.find.-watch)
Optional

Continuously monitor the [`ALIAS`](#mc.find.ALIAS) and return any
new objects which match the specified criteria.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Find a Specific Object in a Bucket

```
mc find ALIAS/PATH --name NAME
```

Replace [`ALIAS`](#mc.find.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.find.ALIAS) with the path to a bucket on the
S3-compatible host. Omit the path to search from the root of the S3 host.

Replace [`NAME`](#mc.find.-name) with the object.

### Find Objects with File Extension in Bucket

```
mc find ALIAS/PATH --name *.EXTENSION
```

Replace [`ALIAS`](#mc.find.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.find.ALIAS) with the path to a bucket on the
S3-compatible host.

Replace [`EXTENSION`](#mc.find.-name) with the file extension of the object.

### Find All Matching Files and Copy To S3 Service

Use [`mc find`](#command-mc.find) with the [`--exec`](#mc.find.-exec) option to find
files on a local filesystem and pass them to an mc command for
further processing. The following example uses [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] to copy the
output of [`mc find`](#command-mc.find) to an S3-compatible host.

```
mc find FILEPATH --name "*.EXTENSION" --exec "mc cp {} ALIAS/PATH"
```

Replace [`FILEPATH`](#mc.find.ALIAS) with the full file path to the
directory to search.

Replace [`EXTENSION`](#mc.find.-name) with the file extension of the object.

Replace [`ALIAS`](#mc.find.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.find.ALIAS) with the path to a bucket on the
S3-compatible host.

To continuously watch the specified directory and copy new objects,
include the [`--watch`](#mc.find.-watch) argument:

```
mc find --watch FILEPATH --name "*.EXTENSION" --exec "mc cp {} ALIAS/PATH"
```

### Find Objects with a Matching Tag

Note

Tag matching is only available for use on MinIO deployments.

```
mc find --tags="key=v*" ALIAS/BUCKET/
```

Replace `key` with the name of a tag key to match.

Replace `v*` with the RE2 Regular Expression to evaluate against.

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace `BUCKET` with the bucket or prefix to search.

You can add additional `--tags="key=RegExpression"` flags to match.
Matching objects must match all included tags.

### Find Objects with Matching Metadata

Note

Metadata matching is only available for use on MinIO deployments.

```
mc find --json --metadata="content-type=text/csv" ALIAS/BUCKET/
```

Replace `content-type=text/csv` with the a key-value pair of the metadata field and value to match.

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace `BUCKET` with the bucket or prefix to search.

You can add additional `--tags="metadata=value"` flags to match.
Matching objects must match all included metadata fields.

## Behavior

### Units of Measurement

The [`mc find --smaller`](#mc.find.-smaller) and [`mc find --larger`](#mc.find.-larger) flags
accept the following case-insensitive suffixes to represent the unit of the
specified size value:

Suffix

Unit Size

`k`

KB (Kilobyte, 1000 Bytes)

`m`

MB (Megabyte, 1000 Kilobytes)

`g`

GB (Gigabyte, 1000 Megabytes)

`t`

TB (Terabyte, 1000 Gigabytes)

`ki`

KiB (Kibibyte, 1024 Bites)

`mi`

MiB (Mebibyte, 1024 Kibibytes)

`gi`

GiB (Gibibyte, 1024 Mebibytes)

`ti`

TiB (Tebibyte, 1024 Gibibytes)

Omitting the suffix defaults to `bytes`.

### Substitution Format

The [`mc find --exec`](#mc.find.-exec) and [`mc find --print`](#mc.find.-print) commands
support string substitutions with special interpretations for following
keywords.

The following keywords are supported for both filesystem and S3 service targets:

`{}` - Substitutes to full path.

`{base}` - Substitutes to basename of path.

`{dir}` - Substitutes to dirname of the path.

`{size}` - Substitutes to object size of the path.

`{time}` - Substitutes to object modified time of the path.

The following keyword is supported only for S3 service targets:

`{url}` - Substitutes to a shareable URL of the path.

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