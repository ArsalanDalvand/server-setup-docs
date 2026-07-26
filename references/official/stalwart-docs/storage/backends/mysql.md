---
title: "MySQL / MariaDB"
source: "https://stalw.art/docs/storage/backends/mysql"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/mysql"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MySQL / MariaDB"
---
# MySQL / MariaDB

      

MySQL is a widely used open-source relational database management system, known for its reliability, simplicity, and broad platform compatibility. It handles large volumes of data efficiently and is a popular choice for web applications and online transaction processing. MariaDB is a drop-in compatible fork of MySQL and is supported through the same variant.

## Configuration

[Section titled “Configuration”](#configuration)

The MySQL backend is selected by choosing the `MySql` variant on the [[ref/object/data-store|DataStore]] object (found in the WebUI under  Settings ›  Storage › Data Store). The variant exposes the following fields:

[[ref/object/data-store#host|`host`]]: hostname or IP address of the MySQL server (required).
[[ref/object/data-store#port|`port`]]: TCP port. Default: `3306`.
[[ref/object/data-store#database|`database`]]: name of the database. Default: `"stalwart"`.
[[ref/object/data-store#authusername|`authUsername`]]: username used to authenticate. Default: `"stalwart"`.
[[ref/object/data-store#authsecret|`authSecret`]]: password or other secret reference used to authenticate (required).
[[ref/object/data-store#timeout|`timeout`]]: maximum time to wait when establishing a connection. Default: `"15s"`.
[[ref/object/data-store#maxallowedpacket|`maxAllowedPacket`]]: maximum size of a single network packet sent to or received from the server, in bytes.

### Connection pool

[Section titled “Connection pool”](#connection-pool)

Pool sizing is controlled through [[ref/object/data-store#poolmaxconnections|`poolMaxConnections`]] (default `10`) and [[ref/object/data-store#poolminconnections|`poolMinConnections`]] (default `5`). The minimum ensures a baseline of ready connections at all times.

### TLS

[Section titled “TLS”](#tls)

TLS is enabled through [[ref/object/data-store#usetls|`useTls`]]. When enabled, [[ref/object/data-store#allowinvalidcerts|`allowInvalidCerts`]] determines whether connections with invalid certificates are accepted, which is typically only useful in development or testing.

### Read replicas

[Section titled “Read replicas”](#read-replicas)

In Enterprise deployments, write operations target the primary connection and read operations are distributed across one or more replicas. Replicas are configured through the [[ref/object/data-store#readreplicas|`readReplicas`]] field on the same DataStore object, which accepts a list of replica connection settings. Each entry defines a full set of connection parameters ([[ref/object/data-store#host|`host`]], [[ref/object/data-store#port|`port`]], [[ref/object/data-store#database|`database`]], [[ref/object/data-store#authusername|`authUsername`]], and [[ref/object/data-store#authsecret|`authSecret`]]). The same pattern is available on the [[ref/object/blob-store|BlobStore]] and [[ref/object/search-store|SearchStore]] objects when their `MySql` variant is selected.

## Authentication directory

[Section titled “Authentication directory”](#authentication-directory)

When MySQL is used as an authentication directory, the queries that resolve account names, group members, recipients, and email addresses are configured on the [[ref/object/directory|Directory]] object with the SQL variant, which references this store. See the [[auth/backend/sql|SQL directory]] page for details.

## FTS Limitations

[Section titled “FTS Limitations”](#fts-limitations)

MySQL’s built-in full-text search is a lightweight feature designed primarily for simple keyword matching. While sufficient for basic search use cases, it has several inherent limitations that are important to understand when using it as a full-text search backend for email.

One significant limitation is that MySQL full-text search does not support multiple languages within the same indexed column. A full-text index is tied to a single parser and language configuration, which means messages containing mixed languages cannot be tokenised or ranked correctly. In mail environments where users regularly exchange messages in different languages, this can lead to inconsistent or incomplete search results.

MySQL full-text search also does not support stemming. Words are indexed and matched in their exact forms, so different grammatical variants of the same word (singular versus plural, different verb tenses) are treated as unrelated terms. As a result, searches are less tolerant of natural-language variation and may miss relevant messages unless the exact word forms present in the message are used.

Another important constraint is that words shorter than three characters are not indexed by default. Common short words, abbreviations, and identifiers (many of which appear in email content) are therefore not searchable at all. This is particularly noticeable when searching for short names, acronyms, or codes.

Taken together, these limitations can significantly affect search quality and recall. If accurate, language-aware search behaviour is a priority for the deployment, a different [[storage/fts|full-text search backend]] that provides better linguistic support and indexing is strongly recommended.