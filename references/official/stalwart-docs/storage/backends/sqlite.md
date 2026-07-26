---
title: "SQLite"
source: "https://stalw.art/docs/storage/backends/sqlite"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/sqlite"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SQLite"
---
# SQLite

      

SQLite is a self-contained, serverless, zero-configuration database engine. Its simplicity and reliability make it well suited to small single-node installations. Data can be replicated for redundancy using external tools such as Litestream, which provides continuous replication of SQLite database files.

## Configuration

[Section titled “Configuration”](#configuration)

The SQLite backend is selected by choosing the `Sqlite` variant on the [[ref/object/data-store|DataStore]] object (found in the WebUI under  Settings ›  Storage › Data Store). The variant exposes the following fields:

[[ref/object/data-store#path|`path`]]: path to the SQLite database file (required). If the file does not exist, it is created automatically.
[[ref/object/data-store#poolmaxconnections|`poolMaxConnections`]]: maximum number of concurrent database connections. Default: `10`.
[[ref/object/data-store#poolworkers|`poolWorkers`]]: number of worker threads dedicated to blocking database operations. Defaults to the number of CPU cores available on the host.