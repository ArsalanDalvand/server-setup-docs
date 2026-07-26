---
title: "Data transformation"
source: "https://redis.io/docs/latest/integrate/write-behind/data-transformation/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/data-transformation"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Data transformation"
---
# Data transformation

The key functionality that Write-behind performs is mapping the data coming from [Debezium Server](https://debezium.io/documentation/reference/stable/operations/debezium-server.html) (representing a Source Database row data or row state change) into a Redis key with a value of [Hash](/docs/latest/develop/data-types/hashes/) or [JSON](/docs/latest/develop/data-types/json/).
There are two types of data transformations in Write-behind:

By default, each source row is converted into one hash or one JSON key in Redis.
This conversion uses the Debezium schema-based conversion. The incoming data includes the schema and Write-behind uses a set of handlers to automatically convert each source column to a Redis Hash field or JSON type based on the Debezium type in the schema. See
[[integrate/write-behind/reference/data-types-conversion|data type conversion]]
for a full reference on these conversions.

If the user wants to add or modify this default mapping, Write-behind provides declarative data transformations. These transformations are represented in YAML files. Each file contains a job, which is a set of transformations per source table. See
[[integrate/write-behind/data-transformation/data-transformation-pipeline|declarative transformations]] for more information.

[](/docs/latest/images/rdi/data-transformation-flow.png)

## More info

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page