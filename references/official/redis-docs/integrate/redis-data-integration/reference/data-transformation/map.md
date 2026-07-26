---
title: "map"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/data-transformation/map/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/data-transformation/map"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "map"
---
# map

Map a record into a new output based on expressions

Map a record into a new output based on expressions

Properties

Name
Type
Description
Required

[expression](#expression)
`object`, `string`
Expression

yes

language
`string`
Language
Enum: `"jmespath"`, `"sql"`

yes

cache
`object`
Cache the result of the map expression. See [`cache`](/docs/latest/integrate/redis-data-integration/reference/data-transformation/cache/) for the property list. Flink processor only.

no

Additional Properties: not allowed

Example

```yaml
source:
  schema: dbo
  table: emp
transform:
  - uses: map
    with:
      expression:
        first_name: first_name
        last_name: last_name
        greeting: >-
          'Hello ' || CASE WHEN gender = 'F' THEN 'Ms.' WHEN gender = 'M' THEN 'Mr.'
          ELSE 'N/A' END || ' ' || full_name
        country: country
        full_name: full_name
      language: sql
```

Example

```yaml
source:
  table: customer
transform:
  - uses: map
    with:
      expression: |
        {
          "CustomerId": customer_id,
          "FirstName": first_name,
          "LastName": last_name,
          "Company": company,
          "Location":
          {
            "Street": address,
            "City": city,
            "State": state,
            "Country": country,
            "PostalCode": postal_code
          },
          "Phone": phone,
          "Fax": fax,
          "Email": email
        }
      language: jmespath
```

## expression: object

Expression

No properties.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page