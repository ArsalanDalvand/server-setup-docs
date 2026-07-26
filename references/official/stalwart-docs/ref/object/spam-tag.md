---
title: "SpamTag"
source: "https://stalw.art/docs/ref/object/spam-tag"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-tag"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamTag"
---
# SpamTag

      

Defines a score or action assigned to a spam classification tag.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › Scores

## Fields

[Section titled “Fields”](#fields)

SpamTag is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Score"

[Section titled “@type: "Score"”](#type-score)

Assign Score

##### tag

[Section titled “tag”](#tag)

Type: `String` · required

The spam tag name

##### score

[Section titled “score”](#score)

Type: `Float` · default: `0` · min: -999999 · max: 999999

The score for the tag

### @type: "Discard"

[Section titled “@type: "Discard"”](#type-discard)

Discard

##### tag

[Section titled “tag”](#tag-1)

Type: `String` · required

The spam tag name

### @type: "Reject"

[Section titled “@type: "Reject"”](#type-reject)

Reject

##### tag

[Section titled “tag”](#tag-2)

Type: `String` · required

The spam tag name

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamTag object is available via the `urn:stalwart:jmap` capability.

### x:SpamTag/get

[Section titled “x:SpamTag/get”](#xspamtagget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysSpamTagGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTag/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamTag/set

[Section titled “x:SpamTag/set”](#xspamtagset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysSpamTagCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTag/set",          {            "create": {              "new1": {                "@type": "Score",                "tag": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysSpamTagUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTag/set",          {            "update": {              "id1": {                "tag": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysSpamTagDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTag/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamTag/query

[Section titled “x:SpamTag/query”](#xspamtagquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysSpamTagQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTag/query",          {            "filter": {              "tag": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:SpamTag/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`tag`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamTag id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create SpamTag/Score \  --field tag=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query SpamTagstalwart-cli query SpamTag --where tag=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update SpamTag id1 --field tag='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete SpamTag --ids id1
```