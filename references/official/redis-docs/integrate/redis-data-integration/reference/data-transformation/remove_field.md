---
title: "remove_field"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/data-transformation/remove_field/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/data-transformation/remove_field"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "remove_field"
---
# remove_field

Remove fields

Remove fields

Option 1 (alternative):
Remove multiple fields

Properties

Name
Type
Description
Required

[fields](#option1fields)
`object[]`
Fields

yes

Additional Properties: not allowed

Example

```yaml
source:
  schema: dbo
  table: emp
transform:
  - uses: remove_field
    with:
      fields:
        - field: credit_card
        - field: name.mname
```

Option 2 (alternative):
Remove one field

Properties

Name
Type
Description
Required

field
`string`
Field

yes

Additional Properties: not allowed
Example

```yaml
source:
  schema: dbo
  table: emp
transform:
  - uses: remove_field
    with:
      field: credit_card
```

## Option 1: fields[]: array

Fields

Items

Item Properties

Name
Type
Description
Required

field
`string`
Field

yes

Item Additional Properties: not allowed

Example

```yaml
- {}
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page