---
title: "PostgreSQL"
source: "https://stalw.art/docs/storage/backends/postgresql"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/postgresql"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "PostgreSQL"
---
# PostgreSQL

      

PostgreSQL (often called Postgres) is an open-source object-relational database system known for its architecture, reliability, data integrity, and feature set. It handles workloads ranging from single machines to heavily loaded web services with many concurrent users.

## Configuration

[Section titled “Configuration”](#configuration)

The PostgreSQL backend is selected by choosing the `PostgreSql` variant on the [[ref/object/data-store|DataStore]] object (found in the WebUI under  Settings ›  Storage › Data Store). The variant exposes the following fields:

[[ref/object/data-store#host|`host`]]: hostname or IP address of the PostgreSQL server (required).
[[ref/object/data-store#port|`port`]]: TCP port. Default: `5432`.
[[ref/object/data-store#database|`database`]]: name of the database. Default: `"stalwart"`.
[[ref/object/data-store#authusername|`authUsername`]]: username used to authenticate. Default: `"stalwart"`.
[[ref/object/data-store#authsecret|`authSecret`]]: password or other secret reference used to authenticate (required). The secret may be supplied inline, read from an environment variable, or loaded from a file.
[[ref/object/data-store#timeout|`timeout`]]: maximum time to wait when establishing a connection. Default: `"15s"`.
[[ref/object/data-store#options|`options`]]: additional connection options passed to the driver.

### Connection pool

[Section titled “Connection pool”](#connection-pool)

Pool sizing is controlled through [[ref/object/data-store#poolmaxconnections|`poolMaxConnections`]] (default `10`) and [[ref/object/data-store#poolrecyclingmethod|`poolRecyclingMethod`]] (default `"fast"`).

### TLS

[Section titled “TLS”](#tls)

TLS is enabled through [[ref/object/data-store#usetls|`useTls`]]. When enabled, [[ref/object/data-store#allowinvalidcerts|`allowInvalidCerts`]] determines whether connections with invalid certificates are accepted. The default is `false` (strict validation).

### Read replicas

[Section titled “Read replicas”](#read-replicas)

In Enterprise deployments, write operations target the primary connection and read operations are distributed across one or more replicas. Replicas are configured through the [[ref/object/data-store#readreplicas|`readReplicas`]] field on the same DataStore object, which accepts a list of replica connection settings. Each entry defines a full set of connection parameters ([[ref/object/data-store#host|`host`]], [[ref/object/data-store#port|`port`]], [[ref/object/data-store#database|`database`]], [[ref/object/data-store#authusername|`authUsername`]], and [[ref/object/data-store#authsecret|`authSecret`]]). The same pattern is available on the [[ref/object/blob-store|BlobStore]] and [[ref/object/search-store|SearchStore]] objects when their `PostgreSql` variant is selected.

## Authentication directory

[Section titled “Authentication directory”](#authentication-directory)

When PostgreSQL is used as an authentication directory, the queries that resolve account names, group members, recipients, and email addresses are configured on the [[ref/object/directory|Directory]] object with the SQL variant, which references this store. See the [[auth/backend/sql|SQL directory]] page for details.

## FTS Limitations

[Section titled “FTS Limitations”](#fts-limitations)

PostgreSQL provides a built-in full-text search engine based on `tsvector` and `tsquery`. While reliable and well integrated, it has important [limitations](https://www.postgresql.org/docs/current/textsearch-limitations.html) that are particularly relevant when indexing email content.

The most significant constraint is that the total size of a `tsvector`, including both lexemes and positional information, must not exceed 1 megabyte. Because positional data is stored alongside indexed terms, the amount of actual text that can be indexed is substantially less than 1 MB, especially for natural-language content with many tokens and repeated words.

For email workloads, this limitation directly affects message bodies and attachments, which can easily exceed the effective size limit when indexed as full text. To keep indexing reliable and avoid exceeding PostgreSQL’s hard limits, Stalwart truncates content before converting it into a `tsvector`. Message bodies are truncated to 650 KB, and attachments are truncated to 650 KB total, combined across all attachments in a message. In practice, only the first 650 KB of the message body is indexed, and only the first 650 KB of all attachments combined are indexed. Any content beyond these limits is not searchable.

This truncation is a deliberate and conservative choice that leaves sufficient room for lexeme positions and avoids indexing failures caused by exceeding PostgreSQL’s maximum `tsvector` size. For most typical email messages, this behaviour is not noticeable and does not materially affect search quality.

However, in environments where messages frequently contain very large bodies or where full search coverage of large attachments is required, this limitation may be problematic. In such cases, administrators should consider a different [[storage/fts|full-text search backend]] that does not impose strict per-document size limits.

These constraints are inherent to PostgreSQL’s full-text search implementation and should be taken into account when deciding whether PostgreSQL is an appropriate full-text search backend for email workloads.