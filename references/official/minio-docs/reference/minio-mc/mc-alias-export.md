---
title: "mc alias export — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-alias-export.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-alias-export"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc alias export — SILO (MinIO Object Storage)"
---
# mc alias export

New in version mc.RELEASE.2023-11-15T22-45-58Z.

## Syntax

The [`mc alias export`](#command-mc.alias.export) command exports an alias configuration from the existing [[reference/minio-mc#mc-configuration|configuration]].

The command outputs the result to `STDOUT` where you can either capture the output as a file or perform further modifications to the output as necessary.

Use the [[reference/minio-mc/mc-alias-import#command-mc.alias.import|`mc alias import`]] command to import the resulting JSON configuration.

EXAMPLE

The following command exports an alias configuration from the existing host and outputs it to a file:

```
mc alias export play > play.json
```

The command outputs the file to Standard Out (`STDOUT`).
You can alternatively pipe the output to a utility of your choice for further operations.

SYNTAX

The [`mc alias export`](#command-mc.alias.export) command has the following syntax:

```
mc [GLOBALFLAGS] alias export ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.alias.export.ALIAS)
Required

The name of the alias to export.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Behavior

### JSON Format

The command outputs a JSON object with the following schema:

```
{
   "url" : "https://hostname:port",
   "accessKey": "<STRING>",
   "secretKey": "<STRING>",
   "api": "s3v4",
   "path": "auto"
}
```

You can use the [[reference/minio-mc/mc-alias-import#command-mc.alias.import|`mc alias import`]] to import the JSON document.

## Examples

### Export and Transform an Alias

The following example exports the alias for the [play.min.io](https://play.min.io) sandbox.
It then transforms the configuration using the [jq](https://jqlang.github.io/jq/) utility and creates a new alias from the modified configuration:

```
mc alias export play | jq '.accessKey = "minioadmin" | .secretKey = "minioadmin"' | mc alias import play-custom
```

### Back Up An Alias Configuration

The following command exports an alias configuration to a JSON file.
You can then back up that file using your preferred process.

```
mc alias export play > play-backup.json
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