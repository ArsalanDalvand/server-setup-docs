---
title: "mc alias import — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-alias-import.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-alias-import"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc alias import — SILO (MinIO Object Storage)"
---
# mc alias import

## Syntax

The [`mc alias import`](#command-mc.alias.import) command imports an alias configuration from a JSON document.

You can use [[reference/minio-mc/mc-alias-export#command-mc.alias.export|`mc alias export`]] to create the necessary JSON for import.

EXAMPLE

The following command imports an alias configuration from a JSON document:

```
mc alias import newalias ./credentials.json
```

Use [[reference/minio-mc/mc-alias-list#command-mc.alias.list|`mc alias list newalias`]] to confirm the import succeeded.

SYNTAX

The [`mc alias import`](#command-mc.alias.import) command has the following syntax:

```
mc [GLOBALFLAGS] alias import ALIAS PATH|STDIN
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.alias.import.ALIAS)
Required

The name of the alias to assign to the imported configuration.

PATH[](#mc.alias.import.PATH)
Required

The full path to the JSON object representing the alias configuration to import.

Mutually exclusive with the [`STDIN`](#mc.alias.import.STDIN) parameter.

STDIN[](#mc.alias.import.STDIN)
Required

Directs the command to use the Standard Input (STDIN) as the source of the JSON object for import.

Mutually exclusive with the [`PATH`](#mc.alias.import.PATH) parameter.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Behavior

### JSON Format

The JSON object must have the following format:

```
{
   "url" : "https://hostname:port",
   "accessKey": "<STRING>",
   "secretKey": "<STRING>",
   "api": "s3v4",
   "path": "auto"
}
```

You can use the [[reference/minio-mc/mc-alias-export#command-mc.alias.export|`mc alias export`]] command to export an existing alias from the local host configuration.
Alternatively, you can manually extract the necessary JSOn fields from the [[reference/minio-mc#command-mc|`mc`]] [[reference/minio-mc#mc-configuration|configuration file]].

## Examples

### Import an Alias Using Standard Input

The following example imports a custom alias for the [play.min.io](https://play.min.io) sandbox.
You can modify this example to use user credentials you have already created or validated as existing on the sandbox:

```
echo '
{
 "url": "https://play.min.io",
 "accessKey": "minioadmin",
 "secretKey": "minioadmin",
 "api": "s3v4",
 "path": "auto"
}' | mc alias import play-minioadmin
```

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