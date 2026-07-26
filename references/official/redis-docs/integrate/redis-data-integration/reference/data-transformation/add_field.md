---
title: "add_field"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/data-transformation/add_field/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/data-transformation/add_field"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "add_field"
---
# add_field

Add fields to a record

Add fields to a record

Option 1 (alternative):
Add multiple fields

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
  - uses: add_field
    with:
      fields:
        - field: name.full_name
          language: jmespath
          expression: concat([name.fname, ' ', name.lname])
        - field: name.fname_upper
          language: jmespath
          expression: upper(name.fname)
```

Option 2 (alternative):
Add one field

Properties

Name
Type
Description
Required

field
`string`
Field

yes

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
Cache the result of the field expression. See [`cache`](/docs/latest/integrate/redis-data-integration/reference/data-transformation/cache/) for the property list. Flink processor only.

no

Additional Properties: not allowed

Example

```yaml
source:
  schema: dbo
  table: emp
transform:
  - uses: add_field
    with:
      field: country
      language: sql
      expression: country_code || ' - ' || UPPER(country_name)
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
Cache the result of the field expression. See [`cache`](/docs/latest/integrate/redis-data-integration/reference/data-transformation/cache/) for the property list. Flink processor only.

no

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