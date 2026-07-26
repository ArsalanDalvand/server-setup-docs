---
title: "Role"
source: "https://stalw.art/docs/ref/object/role"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/role"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Role"
---
# Role

      

Defines a named set of permissions that can be assigned to accounts, groups, or tenants.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Directory › Roles

## Fields

[Section titled “Fields”](#fields)

##### description

[Section titled “description”](#description)

Type: `String` · required

Description of the role

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this role belongs to

##### roleIds

[Section titled “roleIds”](#roleids)

Type: `Id<`[[ref/object/role|`Role`]]`>[]`

List of roles this role extends

##### enabledPermissions

[Section titled “enabledPermissions”](#enabledpermissions)

Type: [[ref/permissions|`Permission`]]`[]`

List of permissions that are explicitly enabled.

##### disabledPermissions

[Section titled “disabledPermissions”](#disabledpermissions)

Type: [[ref/permissions|`Permission`]]`[]`

List of permissions that are explicitly disabled, even if they would be inherited through other roles or groups. This takes precedence over enabled permissions.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Role object is available via the `urn:stalwart:jmap` capability.

### x:Role/get

[Section titled “x:Role/get”](#xroleget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysRoleGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Role/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Role/set

[Section titled “x:Role/set”](#xroleset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysRoleCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Role/set",          {            "create": {              "new1": {                "description": "Example",                "disabledPermissions": {},                "enabledPermissions": {},                "roleIds": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysRoleUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Role/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysRoleDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Role/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Role/query

[Section titled “x:Role/query”](#xrolequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysRoleQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Role/query",          {            "filter": {              "description": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Role/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`description`text`memberTenantId`id of Tenant

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Role id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Role \  --field description=Example \  --field 'roleIds={}' \  --field 'enabledPermissions={}' \  --field 'disabledPermissions={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Rolestalwart-cli query Role --where description=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Role id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Role --ids id1
```