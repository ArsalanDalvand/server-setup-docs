---
title: "rename_field"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/data-transformation/rename_field/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/data-transformation/rename_field"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "rename_field"
---
# rename_field

Rename fields. All other fields remain unchanged.

Rename fields. All other fields remain unchanged.

Option 1 (alternative):
Rename multiple fields

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
  - uses: rename_field
    with:
      fields:
        - from_field: name.lname
          to_field: name.last_name
        - from_field: name.fname
          to_field: name.first_name
```

Option 2 (alternative):
Rename one field

Properties

Name
Type
Description
Required

from_field
`string`
From field

yes

to_field
`string`
To field

yes

Additional Properties: not allowed

Example

```yaml
source:
  schema: dbo
  table: emp
transform:
  - uses: rename_field
    with:
      from_field: name.lname
      to_field: name.last_name
```

## Option 1: fields[]: array

Fields

Items

Item Properties

Name
Type
Description
Required

from_field
`string`
From field

yes

to_field
`string`
To field

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