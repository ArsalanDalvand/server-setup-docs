---
title: "ClusterRole"
source: "https://stalw.art/docs/ref/object/cluster-role"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/cluster-role"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ClusterRole"
---
# ClusterRole

      

Defines a cluster node role with enabled tasks and listeners.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Cluster › Roles

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `String` · read-only

Unique identifier for the role

##### description

[Section titled “description”](#description)

Type: `String?`

Description of the role

##### tasks

[Section titled “tasks”](#tasks)

Type: [`ClusterTaskGroup`](#clustertaskgroup) · required

Which tasks are enabled for this cluster role

##### listeners

[Section titled “listeners”](#listeners)

Type: [`ClusterListenerGroup`](#clusterlistenergroup) · required

Which network listeners are enabled for this cluster role

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The ClusterRole object is available via the `urn:stalwart:jmap` capability.

### x:ClusterRole/get

[Section titled “x:ClusterRole/get”](#xclusterroleget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysClusterRoleGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterRole/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ClusterRole/set

[Section titled “x:ClusterRole/set”](#xclusterroleset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysClusterRoleCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterRole/set",          {            "create": {              "new1": {                "listeners": {                  "@type": "EnableAll"                },                "tasks": {                  "@type": "EnableAll"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysClusterRoleUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterRole/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysClusterRoleDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterRole/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ClusterRole/query

[Section titled “x:ClusterRole/query”](#xclusterrolequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysClusterRoleQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ClusterRole/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:ClusterRole/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get ClusterRole id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create ClusterRole \  --field 'tasks={"@type":"EnableAll"}' \  --field 'listeners={"@type":"EnableAll"}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query ClusterRolestalwart-cli query ClusterRole --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update ClusterRole id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete ClusterRole --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### ClusterTaskGroup

[Section titled “ClusterTaskGroup”](#clustertaskgroup)

Defines which cluster tasks are enabled for a cluster role.

`EnableAll`: Enable all tasks. No additional fields.
`DisableAll`: Disable all tasks. No additional fields.
`EnableSome`: Enable some tasks. Carries the fields of [`ClusterTaskGroupProperties`](#clustertaskgroupproperties).
`DisableSome`: Disable some tasks. Carries the fields of [`ClusterTaskGroupProperties`](#clustertaskgroupproperties).

#### ClusterTaskGroupProperties

[Section titled “ClusterTaskGroupProperties”](#clustertaskgroupproperties)

Specifies which tasks are enabled or disabled.

##### taskTypes

[Section titled “taskTypes”](#tasktypes)

Type: [`ClusterTaskType`](#clustertasktype)`[]`

Tasks to enable or disable for this group

### ClusterListenerGroup

[Section titled “ClusterListenerGroup”](#clusterlistenergroup)

Defines which network listeners are enabled for a cluster role.

`EnableAll`: Enable all network listeners. No additional fields.
`DisableAll`: Disable all network listeners. No additional fields.
`EnableSome`: Enable some network listeners. Carries the fields of [`ClusterListenerGroupProperties`](#clusterlistenergroupproperties).
`DisableSome`: Disable some network listeners. Carries the fields of [`ClusterListenerGroupProperties`](#clusterlistenergroupproperties).

#### ClusterListenerGroupProperties

[Section titled “ClusterListenerGroupProperties”](#clusterlistenergroupproperties)

Specifies which listeners are enabled or disabled.

##### listenerIds

[Section titled “listenerIds”](#listenerids)

Type: `Id<`[[ref/object/network-listener|`NetworkListener`]]`>[]`

List of network listeners to enable or disable for this group

## Enums

[Section titled “Enums”](#enums)

### ClusterTaskType

[Section titled “ClusterTaskType”](#clustertasktype)
ValueLabel`storeMaintenance`Store Maintenance`accountMaintenance`Account Maintenance`metricsCalculate`Calculate Metrics`metricsPush`Push Metrics`pushNotifications`Push Notifications`searchIndexing`Search Indexing`spamClassifierTraining`Spam Classifier Training`outboundMta`Outbound Email MTA`taskQueueProcessing`Task Queue Processing`taskScheduler`Task Scheduling