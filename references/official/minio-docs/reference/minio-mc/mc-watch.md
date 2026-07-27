---
title: "mc watch — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-watch.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-watch"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc watch — SILO (MinIO Object Storage)"
---
# mc watch

## Syntax

The [`mc watch`](#command-mc.watch) command watches for events on the specified MinIO bucket or
local filesystem path. For S3 services, use [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] to configure
bucket event notifications on S3-compatible services.

You can also use [`mc watch`](#command-mc.watch) against a local filesystem directory to
produce similar results to running
the `inotify -e modify,create,delete,move` command.

EXAMPLE

The following command watches for
[[reference/minio-mc/mc-event-add#mc-event-supported-events|events]] on any object or prefix in the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc watch --recursive myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] watch                \
                 [--event "string"]   \
                 [--prefix "string"]  \
                 [--recursive]        \
                 [--suffix "string"]  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.watch.ALIAS)

Required The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment and the
full path to the bucket to watch for configured events. For example:

```
mc watch myminio/mybucket
```

--event[](#mc.watch.-event)

The event(s) to watch for. Specify multiple events using a comma `,`
delimiter. See [[reference/minio-mc/mc-event-add#mc-event-supported-events|Supported Bucket Events]] for supported events.

Defaults to `put,delete, get`.

--prefix[](#mc.watch.-prefix)

The bucket prefix in which to watch for the specified
[`--event`](#mc.watch.-event).

For example, given a [`ALIAS`](#mc.watch.ALIAS) of `play/mybucket` and a
[`--prefix`](#mc.watch.-prefix) of `photos`, only events in
`play/mybucket/photos` trigger bucket notifications.

--recursive, r[](#mc.watch.-recursive)

Recursively watch for events in the specified
[`ALIAS`](#mc.watch.ALIAS) bucket path or local directory.

--suffix[](#mc.watch.-suffix)

The bucket suffix in which to watch for the specified
[`--event`](#mc.watch.-event).

For example, given a [`ALIAS`](#mc.watch.ALIAS) of `play/mybucket` and a
[`--suffix`](#mc.watch.-suffix) of `.jpg`, only events in
`play/mybucket/*.jpg` trigger bucket notifications.

### Global Flags

--json[](#mc.watch.-json)
Optional

Enables [JSON lines](http://jsonlines.org/) formatted output to the
console.

For example:

```
mc --json COMMAND
```

## Examples

### Watch for Events in a Bucket

```
mc watch --recursive ALIAS/PATH
```

Replace [`ALIAS`](#mc.watch.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]]
of the S3-compatible host.

Replace [`PATH`](#mc.watch.ALIAS) with the path to the bucket.

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