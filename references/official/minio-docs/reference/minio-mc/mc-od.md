---
title: "mc od — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-od.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-od"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc od — SILO (MinIO Object Storage)"
---
# mc od

## Syntax

The [`mc od`](#command-mc.od) command copies a local file to a remote location in a specified number of parts and part sizes.
The command outputs the time it took to upload the file.

Use the [`mc od`](#command-mc.od) to mimic the functionality of the Linux `dd` command.

EXAMPLE

The following command Upload 200MiB of a file to a bucket in 5 parts of size 40MiB.
The output shows the results of the upload, including the length of time it took for the upload to complete.

```
mc od if=file.zip of=myminio/mybucket/file.zip size=40MiB parts=5
```

If passing the `--json` [[reference/minio-mc#minio-mc-global-options|global flag]], the output of the command resembles the following:

```
{
  "source": "home/user/file.zip"
  "target": "myminio/mybucket/file.zip"
  "partSize": 41943040
  "totalSize": 209715200
  "parts": 5
  "elapsed": "314ms"
}
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] od                                            \
                 if=<path of source file to upload>            \
                 of=<target MinIO path to upload to>           \
                 [size=<size of file>]                         \
                 [parts=<number of parts to split file into>]  \
                 [skip=<number of parts to skip>]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

if[](#mc.od.if)
Required

The path of the source object to use for the upload.
Use the full path relative to your current location.

```
mc od if=file.zip of=myminio/mybucket/file.zip
```

of[](#mc.od.of)
Required

The full target path to upload the object to.

size[](#mc.od.size)
Optional

The size for each part of the file to upload.
If not specified, MinIO determines the size for parts from the source stream.

parts[](#mc.od.parts)
Optional

The number of parts to divide the object into for uploading.
If not specified, MinIO determines the number of parts based on the size of the source stream.

skip[](#mc.od.skip)
Optional

The number of parts of the file to skip during the upload.
For example, use this option to test the upload speed for a large file of many parts on only a portion of the object’s parts.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Upload a Full File with 40MiB Parts

Use [`mc od`](#command-mc.od) to upload a file to MinIO in a set of parts of specified size.
The [`size`](#mc.od.size) option allows you to specify the desired part size.

```
mc od if=file.zip of=myminio/mybucket/file.zip size=40MiB
```

Replace `myminio/mybucket/file.zip` with the path of the object or file stream to upload.

Replace [`size`](#mc.od.size) with the desired size of the object parts.

MinIO examines the source file and divides it into the necessary number of parts so that no part is larger than the specified 40MiB part size.

### Upload a First Five 40 MiB Parts of a File

Use [`mc od`](#command-mc.od) to upload parts of a file to MinIO of specified part size.
The [`size`](#mc.od.size) option allows you to specify the desired part size.
The [`parts`](#mc.od.parts) option allows you to specify the total number of parts to use for the object.

```
mc od if=file.zip of=myminio/mybucket/file.zip size=40MiB parts=5
```

Replace `myminio/mybucket/file.zip` with the path of the object or file stream to upload.

Replace [`size`](#mc.od.size) with the desired size of the object parts.

Replace [`parts`](#mc.od.parts) with the number of desired parts to use for the object.

In this command example, if the source object stream is larger than 200MiB (40MiB × 5 parts), only the first 200MiB of the file upload.

Important

Using the command this way may not upload the entirety of an object.

### Upload a Full File in 5 Parts

Take a source file, divide the file into a specified number of parts, then upload all parts of the file to a MinIO target.

```
mc od if=file.zip of=myminio/mybucket/file.zip parts=5
```

The above command divides the source file into five equal parts, then uploads those parts.

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