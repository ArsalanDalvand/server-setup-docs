---
title: "Overview"
source: "https://stalw.art/docs/storage/lookup"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/lookup"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

Lookup lists serve a similar role to in-memory stores such as Redis, but with one key distinction: they are read-only at runtime. They hold key-value pairs that remain unchanged during server operation and are well suited to cases where a fixed set of values is consulted from configuration expressions or scripts.

The primary way to interact with lookup lists is through [[configuration/expressions|expressions]], using specialised functions such as `key_get`, `counter_get`, and others. These functions allow administrators to query values from the lists and evaluate configuration dynamically based on predefined key-value pairs.

Stalwart supports both local and remote lookup lists:

[[storage/lookup/local|Local lookup lists]] are held as records managed through the [[ref/object/memory-lookup-key|MemoryLookupKey]] and [[ref/object/memory-lookup-key-value|MemoryLookupKeyValue]] objects. They are suitable for cases where all key-value pairs are known in advance and managed directly by the administrator.
[[storage/lookup/remote|Remote lookup lists]] are fetched over HTTP and defined through the [[ref/object/http-lookup|HttpLookup]] object. They are useful in dynamic environments where configuration is managed centrally or updated independently of the server.

Additionally, the [[ref/object/store-lookup|StoreLookup]] object exposes an external data store as a lookup namespace, which is convenient when an existing database already holds the data the server should consult.