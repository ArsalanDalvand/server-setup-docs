---
title: "Local"
source: "https://stalw.art/docs/storage/lookup/local"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/lookup/local"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Local"
---
# Local

      

Local lookup lists hold key-only membership sets and key-value pairs entirely in memory. They are well suited to small, slow-changing sets of static data, consulted from expressions or Sieve filters.

## Key-only lists

[Section titled “Key-only lists”](#key-only-lists)

Key-only lists, useful for membership tests such as allow-lists and deny-lists, are defined through the [[ref/object/memory-lookup-key|MemoryLookupKey]] object (found in the WebUI under  Settings ›  Lookups › In-Memory Keys,  Settings ›  Spam Filter › Lists › Blocked Domains,  Settings ›  Spam Filter › Lists › Spam Traps,  Settings ›  Spam Filter › Lists › Trusted Domains,  Settings ›  Spam Filter › Lists › URL Redirectors). Each record has:

[[ref/object/memory-lookup-key#namespace|`namespace`]]: the lookup namespace the key belongs to (required). All records sharing a namespace form a single lookup list.
[[ref/object/memory-lookup-key#key|`key`]]: the key name (required, up to 255 characters).
[[ref/object/memory-lookup-key#isglobpattern|`isGlobPattern`]]: when `true`, the key is interpreted as a glob pattern, matching multiple concrete keys (for example `*.example.com` or `mx?.example.org`). Default: `false`.

To build a list of allowed domains, for instance, create one MemoryLookupKey record per entry, all sharing the namespace `allow-list-domain`, with the domain as the key.

## Key-value lists

[Section titled “Key-value lists”](#key-value-lists)

Lists that associate a value with each key are defined through the [[ref/object/memory-lookup-key-value|MemoryLookupKeyValue]] object (found in the WebUI under  Settings ›  Lookups › In-Memory Key-Values). Each record has the same [[ref/object/memory-lookup-key-value#namespace|`namespace`]], [[ref/object/memory-lookup-key-value#key|`key`]], and [[ref/object/memory-lookup-key-value#isglobpattern|`isGlobPattern`]] fields as MemoryLookupKey, plus:

[[ref/object/memory-lookup-key-value#value|`value`]]: the value associated with the key (required).