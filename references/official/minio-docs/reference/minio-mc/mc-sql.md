---
title: "mc sql — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-sql.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-sql"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc sql — SILO (MinIO Object Storage)"
---
# mc sql

## Syntax

The [`mc sql`](#command-mc.sql) command provides an S3 Select interface for performing sql queries on objects in the specified MinIO deployment.

See [Selecting content from objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/selecting-content-from-objects) for more information on S3 Select behavior and limitations.

EXAMPLE

The following command queries all objects in the `mydata` bucket on the `myminio` MinIO deployment:

```
mc sql --recursive --query "select * from S3Object" myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] mc sql                          \
                 --query "string"                \
                 [--csv-input "string"]          \
                 [--compression "string"]        \
                 [--csv-output "string"]         \
                 [--csv-output-header "string"]  \
                 [--enc-c "string"]              \
                 [--json-input "string"]         \
                 [--json-output "string"]        \
                 [--recursive]                   \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.sql.ALIAS)
Required

The full path to the bucket or object to run the SQL query against.
Specify the [[reference/minio-mc/mc-alias-set#alias|alias]] of a configured S3 service as the prefix to the `ALIAS` path.
For example:

```
mc sql [FLAGS] play/mybucket
```

--query, e[](#mc.sql.-query)
Required

The SQL statement to execute on the specified [`ALIAS`](#mc.sql.ALIAS) directory or object.
Wrap the entire SQL query in double quotes `"`.

Defaults to `"select * from S3Object"`.

--csv-input[](#mc.sql.-csv-input)
Optional

The data format for `.csv` input objects.
Specify a string of comma-seperated `key=value,...` pairs.
See [CSV Formatting Fields](#mc-sql-csv-format) for more information on valid keys.

--compression[](#mc.sql.-compression)
Optional

The compression type of the input object.
Specify one of the following supported values:

`GZIP`

`BZIP2`

`NONE` (default)

Compression schemes supported by MinIO backend only:

`ZSTD` [Zstandard](https://facebook.github.io/zstd/)

`LZ4` [LZ4](https://lz4.github.io/lz4/) stream

`S2` [S2](https://github.com/klauspost/compress/tree/master/s2#s2-compression) framed stream

`SNAPPY` [Snappy](http://google.github.io/snappy/) framed stream

--csv-output[](#mc.sql.-csv-output)
Optional

The data format for `.csv` output.
Specify a string of comma-seperated `key=value,...` pairs.
See [CSV Formatting Fields](#mc-sql-csv-format) for more information on valid keys.

See the S3 API [CSVOutput](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CSVOutput.html) for more information.

--csv-output-header[](#mc.sql.-csv-output-header)
Optional

The header row of the `.csv` output file.
Specify a string of comma-separated fields as `field1,field2,...`.

Omit to output a `.csv` with no header row.

--enc-c[](#mc.sql.-enc-c)
Optional

Encrypt or decrypt objects using server-side [[administration/server-side-encryption#minio-sse|SSE-C encryption]] with client-managed keys.

The parameter accepts a key-value pair formatted as `KEY=VALUE`

`KEY`

The full path to the object as `alias/bucket/path/object.ext`.

You can specify only the top-level path to use a single encryption key for all operations in that path.

`VALUE`

Specify either a 32-byte RawBase64-encoded key or a 64-byte hex-encoded key for use with SSE-C encryption.

Raw Base64 encoding rejects `=`-padded keys.
Omit the padding or use a Base64 encoder that supports RAW formatting.

`KEY` - the full path to the object as `alias/bucket/path/object`.

`VALUE` - the 32-byte RAW Base64-encoded data key to use for encrypting object(s).

For example:

```
# RawBase64-Encoded string "mybucket32byteencryptionkeyssec"
--enc-c "myminio/mybucket/prefix/object.obj=bXlidWNrZXQzMmJ5dGVlbmNyeXB0aW9ua2V5c3NlYwo"
```

You can specify multiple encryption keys by repeating the parameter.

Specify the path to a prefix to apply encryption to all matching objects at that path:

```
--enc-c "myminio/mybucket/prefix/=bXlidWNrZXQzMmJ5dGVlbmNyeXB0aW9ua2V5c3NlYwo"
```

Note

MinIO strongly recommends against using SSE-C encryption in production workloads.
Use SSE-KMS via the `--enc-kms` or SSE-S3 via `--enc-s3` parameters instead.

--json-input[](#mc.sql.-json-input)
Optional

The data format for `.json` or `.ndjson` input objects.
Specify the type of the JSON contents as `type=<VALUE>`.
The value can be either:

`DOCUMENT` - JSON [document](https://www.json.org/json-en.html).

`LINES` - JSON [lines](http://jsonlines.org/).

See the S3 API [JSONInput](https://docs.aws.amazon.com/AmazonS3/latest/API/API_JSONInput.html) for more information.

--json-output[](#mc.sql.-json-output)
Optional

The data format for the `.json` output.
Supports the `rd=value` key, where `rd` is the `RecordDelimiter` for the JSON document.

Omit to use the default newline character `\n`.

See the S3 API [JSONOutput](https://docs.aws.amazon.com/AmazonS3/latest/API/API_JSONOutput.html) for more information.

--recursive, r[](#mc.sql.-recursive)
Optional

Recursively searches the specified [`ALIAS`](#mc.sql.ALIAS) directory using the [`--query`](#mc.sql.-query) SQL statement.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Select all Columns in all Objects in a Bucket

Use [`mc sql`](#command-mc.sql) with the [`--recursive`](#mc.sql.-recursive) and [`--query`](#mc.sql.-query) options to apply the query to all objects  in a bucket:

```
mc sql --recursive --query "select * from S3Object" ALIAS/PATH
```

Replace [`ALIAS`](#mc.sql.ALIAS) with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

Replace [`PATH`](#mc.sql.ALIAS) with the path to the bucket on the MinIO deployment.

### Run an Aggregation Query on an Object

Use [`mc sql`](#command-mc.sql) with the [`--query`](#mc.sql.-query) option to query an object on an MinIO deployment:

```
mc sql --query "select count(s.power) from S3Object" ALIAS/PATH
```

Replace [`ALIAS`](#mc.sql.ALIAS) with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

Replace [`PATH`](#mc.sql.ALIAS) with the path to the object on the MinIO deployment.

## Behavior

### Input Formats

[`mc sql`](#command-mc.sql) supports the following input formats:

Input Format Types[](#id1)

Type

`content-type` Value

`.csv`

`text/csv`

`.json`

`application/json`

`.parquet`

none

For `.csv` file types, use [`mc sql --csv-input`](#mc.sql.-csv-input) to specify the CSV data format.
See [CSV Formatting Fields](#mc-sql-csv-format) for more information on CSV formatting fields.

For `.json` file types, use [`mc sql --json-input`](#mc.sql.-json-input) to specify the JSON data format.

For `.parquet` file types, [`mc sql`](#command-mc.sql) automatically interprets the data format.

[`mc sql`](#command-mc.sql) determines the type by the file extension of the target object.
For example, an object named `data.json` is interpreted as a JSON file.

You can query data of a supported type but a different extension if the object has the appropriate `content-type`.
For more information, see [[reference/minio-mc/mc-cp#mc.cp.-attr|`mc cp --attr`]].

### CSV Formatting Fields

The following table lists valid key-value pairs for use with [`mc sql --csv-input`](#mc.sql.-csv-input) and [`mc sql --csv-output`](#mc.sql.-csv-output).
Certain key pairs are only valid for [`--csv-input`](#mc.sql.-csv-input).
See the documentation for S3 API [CSVInput](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CSVInput.html) for more information on S3 CSV formatting.

Key

`--csv-input` Only

Description

`rd`

The character that seperates each record (row) in the input `.csv` file.

Corresponds to `RecordDelimiter` in the S3 API `CSVInput`.

`fd`

The character that seperates each field in a record. Defaults to `,`.

Corresponds to `FieldDelimeter` in the S3 API `CSVInput`.

`qc`

The character used for escaping when the `fd` character is part of a value. Defaults to `"`.

Corresponds to `QuoteCharacter` in the S3 API `CSVInput`.

`qec`

The character used for escaping a quotation mark `"` character inside an already escaped value.

Corresponds to `QuoteEscapeCharacter` in the S3 API `CSVInput`.

`fh`

Yes

The content of the first line in the `.csv` file.

Specify one of the following supported values:

`NONE` - The first line is not a header.

`IGNORE` - Ignore the first line.

`USE` - The first line is a header.

For `NONE` or `IGNORE`, you must specify column positions `_#` to identify a column in the [`--query`](#mc.sql.-query) statement.

For `USE`, you can specify header values to identify a column in the [`--query`](#mc.sql.-query) statement.

Corresponds to `FieldHeaderInfo` in the S3 API `CSVInput`.

`cc`

Yes

The character used to indicate a record should be ignored.
The character must appear at the beginning of the record.

Corresponds to `Comment` in the S3 API `CSVInput`.

`qrd`

Yes

Specify `TRUE` to indicate that fields may contain record delimiter values (`rd`).

Defaults to `FALSE`.

Corresponds to `AllowQuotedRecordDelimiter` in the S3 API `CSVInput`.

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