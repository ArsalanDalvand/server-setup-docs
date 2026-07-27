---
title: "mc update — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-update.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-update"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc update — SILO (MinIO Object Storage)"
---
# mc update

## Syntax

The [`mc update`](#command-mc.update) command automatically updates the mc binary to
the latest stable version.

Running this command is equivalent to manually downloading the latest
stable binary and using it to replace the existing `mc` installation on the
host machine.

EXAMPLE

The following command updates the mc binary on the local host:

```
mc update
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] update
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

Use [`mc update`](#command-mc.update) after updating the minio server binary to
ensure consistent behavior and compatibility.

### Global Flags

--json[](#mc.update.-json)
Optional

Enables [JSON lines](http://jsonlines.org/) formatted output to the
console.

For example:

```
mc --json COMMAND
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).