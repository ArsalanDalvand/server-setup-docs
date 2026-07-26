---
title: "MtaQueueQuota"
source: "https://stalw.art/docs/ref/object/mta-queue-quota"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-queue-quota"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaQueueQuota"
---
# MtaQueueQuota

      

Defines a quota rule for message queues.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Rates & Quotas › Queue Quotas

## Fields

[Section titled “Fields”](#fields)

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Whether to enable this quota

##### description

[Section titled “description”](#description)

Type: `String?` · read-only

Short description for the quota

##### key

[Section titled “key”](#key)

Type: [`MtaQueueQuotaKey`](#mtaqueuequotakey)`[]` · min items: 1

Optional list of context variables that determine where this quota should be applied

##### match

[Section titled “match”](#match)

Type: [`Expression`](#expression)

Enable the imposition of concurrency and rate limits only when a specific condition is met

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### messages

[Section titled “messages”](#messages)

Type: `UnsignedInt?` · min: 1

Maximum number of messages in the queue that this quota will allow

##### size

[Section titled “size”](#size)

Type: `Size?`

Maximum total size of messages in the queue that this quota will allow

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaQueueQuota object is available via the `urn:stalwart:jmap` capability.

### x:MtaQueueQuota/get

[Section titled “x:MtaQueueQuota/get”](#xmtaqueuequotaget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaQueueQuotaGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaQueueQuota/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaQueueQuota/set

[Section titled “x:MtaQueueQuota/set”](#xmtaqueuequotaset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaQueueQuotaCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaQueueQuota/set",          {            "create": {              "new1": {                "key": {},                "match": {                  "else": "Example",                  "match": {}                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaQueueQuotaUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaQueueQuota/set",          {            "update": {              "id1": {                "enable": true              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaQueueQuotaDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaQueueQuota/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaQueueQuota/query

[Section titled “x:MtaQueueQuota/query”](#xmtaqueuequotaquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaQueueQuotaQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaQueueQuota/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaQueueQuota id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaQueueQuota \  --field 'key={}' \  --field 'match={"else":"Example","match":{}}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaQueueQuota
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaQueueQuota id1 --field enable=true
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaQueueQuota --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Expression

[Section titled “Expression”](#expression)

A conditional expression with match rules and a default value.

##### match

[Section titled “match”](#match-1)

Type: [`ExpressionMatch`](#expressionmatch)`[]`

List of conditions and their corresponding results

##### else

[Section titled “else”](#else)

Type: `String` · required

Else condition

#### ExpressionMatch

[Section titled “ExpressionMatch”](#expressionmatch)

A single condition-result pair in an expression.

##### if

[Section titled “if”](#if)

Type: `String` · required

If condition

##### then

[Section titled “then”](#then)

Type: `String` · required

Then clause

## Enums

[Section titled “Enums”](#enums)

### MtaQueueQuotaKey

[Section titled “MtaQueueQuotaKey”](#mtaqueuequotakey)
ValueLabel`sender`Sender`senderDomain`Sender Domain`rcpt`Recipient`rcptDomain`Recipient Domain

## Expression references

[Section titled “Expression references”](#expression-references)

The following expression contexts are used by fields on this page:

[[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]] (Variables)