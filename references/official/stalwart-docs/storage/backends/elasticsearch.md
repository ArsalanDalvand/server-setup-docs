---
title: "ElasticSearch"
source: "https://stalw.art/docs/storage/backends/elasticsearch"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/elasticsearch"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ElasticSearch"
---
# ElasticSearch

      

Elasticsearch is an open-source, distributed search and analytics engine built on Apache Lucene. It handles textual, numerical, geospatial, structured, and unstructured data, providing high scalability and near real-time search. When full-text search volumes grow beyond what the internal engine handles comfortably, Elasticsearch is a common choice as an external search backend.

## Configuration

[Section titled “Configuration”](#configuration)

The Elasticsearch backend is selected by choosing the `ElasticSearch` variant on the [[ref/object/search-store|SearchStore]] object (found in the WebUI under  Settings ›  Storage › Search Store). The variant exposes the following fields:

[[ref/object/search-store#url|`url`]]: URL of the Elasticsearch server or cluster endpoint (required).
[[ref/object/search-store#timeout|`timeout`]]: maximum time to wait for an HTTP response. Default: `"30s"`.
[[ref/object/search-store#allowinvalidcerts|`allowInvalidCerts`]]: when `true`, accepts connections with invalid TLS certificates. Default: `false`.
[[ref/object/search-store#httpauth|`httpAuth`]]: authentication method used for HTTP requests (required). The `Unauthenticated`, `Basic`, and `Bearer` variants are supported.
[[ref/object/search-store#httpheaders|`httpHeaders`]]: additional headers attached to every HTTP request.

### Authentication

[Section titled “Authentication”](#authentication)

For HTTP Basic authentication, select the `Basic` variant of [[ref/object/search-store#httpauth|`httpAuth`]] and supply `username` and `secret`. For bearer-token authentication, select the `Bearer` variant and supply `bearerToken`. Secrets may be provided inline, read from an environment variable, or loaded from a file.

### Index settings

[Section titled “Index settings”](#index-settings)

Stalwart maps its search data to an Elasticsearch index whose shape is controlled by the following fields:

[[ref/object/search-store#numshards|`numShards`]]: number of primary shards for the index. Default: `3`.
[[ref/object/search-store#numreplicas|`numReplicas`]]: number of replica shards per primary. Default: `0`.
[[ref/object/search-store#includesource|`includeSource`]]: when `true`, indexes the full source document alongside the inverted index. Default: `false`.