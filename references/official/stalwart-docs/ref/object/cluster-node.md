---
title: "ClusterNode"
source: "https://stalw.art/docs/ref/object/cluster-node"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/cluster-node"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ClusterNode"
---
# ClusterNode

      

Represents a node in the cluster

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Cluster

## Fields

[Section titled “Fields”](#fields)

##### nodeId

[Section titled “nodeId”](#nodeid)

Type: `UnsignedInt` · default: `1`

Unique identifier for the node in the cluster

##### hostname

[Section titled “hostname”](#hostname)

Type: `String` · required

Hostname of the node

##### lastRenewal

[Section titled “lastRenewal”](#lastrenewal)

Type: `UTCDateTime` · required

Timestamp of the last lease renewal from this node, used to determine if the node is still active in the cluster

##### status

[Section titled “status”](#status)

Type: [`ClusterNodeStatus`](#clusternodestatus) · required

Current status of the node in the cluster

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The ClusterNode object is available via the `urn:stalwart:jmap` capability.

### x:ClusterNode/get

[Section titled “x:ClusterNode/get”](#xclusternodeget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysClusterNodeGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterNode/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ClusterNode/set

[Section titled “x:ClusterNode/set”](#xclusternodeset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysClusterNodeCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterNode/set",          {            "create": {              "new1": {                "hostname": "Example",                "lastRenewal": "2026-01-01T00:00:00Z",                "status": "active"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysClusterNodeUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterNode/set",          {            "update": {              "id1": {                "hostname": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysClusterNodeDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterNode/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ClusterNode/query

[Section titled “x:ClusterNode/query”](#xclusternodequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysClusterNodeQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterNode/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get ClusterNode id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create ClusterNode \  --field hostname=Example \  --field lastRenewal=2026-01-01T00:00:00Z \  --field status=active
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query ClusterNode
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update ClusterNode id1 --field hostname='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete ClusterNode --ids id1
```

## Enums

[Section titled “Enums”](#enums)

### ClusterNodeStatus

[Section titled “ClusterNodeStatus”](#clusternodestatus)
ValueLabel`active`Active`stale`Stale`inactive`Inactive