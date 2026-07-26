---
title: "Redis Data Integration (RDI)"
source: "https://redis.io/docs/latest/operate/rdi/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rdi"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Data Integration (RDI)"
---
# Redis Data Integration (RDI)

Keep Redis in sync with a primary database in near real time.

Redis Data Integration (RDI) is a [change data capture](https://en.wikipedia.org/wiki/Change_data_capture) (CDC) system that tracks changes to the data in a non-Redis source database and makes corresponding changes to a Redis target database. You can use the target as a cache to improve performance because it will typically handle read queries much faster than the source.

See the main [[integrate/redis-data-integration|RDI docs section]]
under [[integrate|Libraries and tools]] to learn how to install and use RDI on your own servers.  See the
[[operate/rc/rdi|Redis Cloud RDI guide]] to learn how to set up RDI for a cloud database.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)