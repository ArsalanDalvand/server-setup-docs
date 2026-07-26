---
title: "filter"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/data-transformation/filter/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/data-transformation/filter"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "filter"
---
# filter

Filter records

Filter records

Properties

Name
Type
Description
Required

expression
`string`
Expression

yes

language
`string`
Language
Enum: `"jmespath"`, `"sql"`

yes

cache
`object`
Cache the result of the filter expression. See [`cache`](/docs/latest/integrate/redis-data-integration/reference/data-transformation/cache/) for the property list. Flink processor only.

no

Additional Properties: not allowed

Example

```yaml
source:
  schema: dbo
  table: emp
transform:
  - uses: filter
    with:
      language: sql
      expression: age>20
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)