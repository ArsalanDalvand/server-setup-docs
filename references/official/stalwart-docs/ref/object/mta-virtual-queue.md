---
title: "MtaVirtualQueue"
source: "https://stalw.art/docs/ref/object/mta-virtual-queue"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-virtual-queue"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaVirtualQueue"
---
# MtaVirtualQueue

      

Defines a virtual queue for organizing outbound message delivery.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Outbound › Virtual Queues

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `String` · read-only · max length: 8

Unique identifier for the queue, max 8 characters

##### description

[Section titled “description”](#description)

Type: `String?`

A short description of the queue, which can be used to identify it in the list of queues

##### threadsPerNode

[Section titled “threadsPerNode”](#threadspernode)

Type: `UnsignedInt` · default: `25` · min: 1

Maximum number of threads to use for delivery on each node in the cluster

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaVirtualQueue object is available via the `urn:stalwart:jmap` capability.

### x:MtaVirtualQueue/get

[Section titled “x:MtaVirtualQueue/get”](#xmtavirtualqueueget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaVirtualQueueGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaVirtualQueue/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaVirtualQueue/set

[Section titled “x:MtaVirtualQueue/set”](#xmtavirtualqueueset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaVirtualQueueCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaVirtualQueue/set",          {            "create": {              "new1": {}            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaVirtualQueueUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaVirtualQueue/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaVirtualQueueDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaVirtualQueue/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaVirtualQueue/query

[Section titled “x:MtaVirtualQueue/query”](#xmtavirtualqueuequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaVirtualQueueQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaVirtualQueue/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MtaVirtualQueue/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaVirtualQueue id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaVirtualQueue
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaVirtualQueuestalwart-cli query MtaVirtualQueue --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaVirtualQueue id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaVirtualQueue --ids id1
```