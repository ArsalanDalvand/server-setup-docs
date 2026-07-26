---
title: "SpamDnsblServer"
source: "https://stalw.art/docs/ref/object/spam-dnsbl-server"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-dnsbl-server"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamDnsblServer"
---
# SpamDnsblServer

      

Defines a DNSBL server used for spam filtering lookups.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › DNSBL › Servers

## Fields

[Section titled “Fields”](#fields)

SpamDnsblServer is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Any"

[Section titled “@type: "Any"”](#type-any)

Any

##### tag

[Section titled “tag”](#tag)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### zone

[Section titled “zone”](#zone)

Type: [`Expression`](#expression) · required

Expression that returns the DNS zone to query.

Available variables: [[ref/expression/variable/spam-generic-variable|`SpamGenericVariable`]].

##### name

[Section titled “name”](#name)

Type: `String` · read-only

A unique name for this DNSBL server configuration

##### description

[Section titled “description”](#description)

Type: `String?`

Description for the DNSBL server

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Whether to enable this DNSBL server

### @type: "Url"

[Section titled “@type: "Url"”](#type-url)

URL

##### tag

[Section titled “tag”](#tag-1)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### zone

[Section titled “zone”](#zone-1)

Type: [`Expression`](#expression) · required

Expression that returns the DNS zone to query.

Available variables: [[ref/expression/variable/spam-url-variable|`SpamUrlVariable`]].

##### name

[Section titled “name”](#name-1)

Type: `String` · read-only

A unique name for this DNSBL server configuration

##### description

[Section titled “description”](#description-1)

Type: `String?`

Description for the DNSBL server

##### enable

[Section titled “enable”](#enable-1)

Type: `Boolean` · default: `true`

Whether to enable this DNSBL server

### @type: "Domain"

[Section titled “@type: "Domain"”](#type-domain)

Domain

##### tag

[Section titled “tag”](#tag-2)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### zone

[Section titled “zone”](#zone-2)

Type: [`Expression`](#expression) · required

Expression that returns the DNS zone to query.

Available variables: [[ref/expression/variable/spam-generic-variable|`SpamGenericVariable`]].

##### name

[Section titled “name”](#name-2)

Type: `String` · read-only

A unique name for this DNSBL server configuration

##### description

[Section titled “description”](#description-2)

Type: `String?`

Description for the DNSBL server

##### enable

[Section titled “enable”](#enable-2)

Type: `Boolean` · default: `true`

Whether to enable this DNSBL server

### @type: "Email"

[Section titled “@type: "Email"”](#type-email)

E-mail

##### tag

[Section titled “tag”](#tag-3)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### zone

[Section titled “zone”](#zone-3)

Type: [`Expression`](#expression) · required

Expression that returns the DNS zone to query.

Available variables: [[ref/expression/variable/spam-email-variable|`SpamEmailVariable`]].

##### name

[Section titled “name”](#name-3)

Type: `String` · read-only

A unique name for this DNSBL server configuration

##### description

[Section titled “description”](#description-3)

Type: `String?`

Description for the DNSBL server

##### enable

[Section titled “enable”](#enable-3)

Type: `Boolean` · default: `true`

Whether to enable this DNSBL server

### @type: "Ip"

[Section titled “@type: "Ip"”](#type-ip)

IP

##### tag

[Section titled “tag”](#tag-4)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### zone

[Section titled “zone”](#zone-4)

Type: [`Expression`](#expression) · required

Expression that returns the DNS zone to query.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### name

[Section titled “name”](#name-4)

Type: `String` · read-only

A unique name for this DNSBL server configuration

##### description

[Section titled “description”](#description-4)

Type: `String?`

Description for the DNSBL server

##### enable

[Section titled “enable”](#enable-4)

Type: `Boolean` · default: `true`

Whether to enable this DNSBL server

### @type: "Header"

[Section titled “@type: "Header"”](#type-header)

Header

##### tag

[Section titled “tag”](#tag-5)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### zone

[Section titled “zone”](#zone-5)

Type: [`Expression`](#expression) · required

Expression that returns the DNS zone to query.

Available variables: [[ref/expression/variable/spam-header-variable|`SpamHeaderVariable`]].

##### name

[Section titled “name”](#name-5)

Type: `String` · read-only

A unique name for this DNSBL server configuration

##### description

[Section titled “description”](#description-5)

Type: `String?`

Description for the DNSBL server

##### enable

[Section titled “enable”](#enable-5)

Type: `Boolean` · default: `true`

Whether to enable this DNSBL server

### @type: "Body"

[Section titled “@type: "Body"”](#type-body)

Body

##### tag

[Section titled “tag”](#tag-6)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### zone

[Section titled “zone”](#zone-6)

Type: [`Expression`](#expression) · required

Expression that returns the DNS zone to query.

Available variables: [[ref/expression/variable/spam-generic-variable|`SpamGenericVariable`]].

##### name

[Section titled “name”](#name-6)

Type: `String` · read-only

A unique name for this DNSBL server configuration

##### description

[Section titled “description”](#description-6)

Type: `String?`

Description for the DNSBL server

##### enable

[Section titled “enable”](#enable-6)

Type: `Boolean` · default: `true`

Whether to enable this DNSBL server

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamDnsblServer object is available via the `urn:stalwart:jmap` capability.

### x:SpamDnsblServer/get

[Section titled “x:SpamDnsblServer/get”](#xspamdnsblserverget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysSpamDnsblServerGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamDnsblServer/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamDnsblServer/set

[Section titled “x:SpamDnsblServer/set”](#xspamdnsblserverset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysSpamDnsblServerCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamDnsblServer/set",          {            "create": {              "new1": {                "@type": "Any",                "tag": {                  "else": "Example",                  "match": {}                },                "zone": {                  "else": "Example",                  "match": {}                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysSpamDnsblServerUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamDnsblServer/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysSpamDnsblServerDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamDnsblServer/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamDnsblServer/query

[Section titled “x:SpamDnsblServer/query”](#xspamdnsblserverquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysSpamDnsblServerQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamDnsblServer/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:SpamDnsblServer/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamDnsblServer id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create SpamDnsblServer/Any \  --field 'tag={"else":"Example","match":{}}' \  --field 'zone={"else":"Example","match":{}}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query SpamDnsblServerstalwart-cli query SpamDnsblServer --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update SpamDnsblServer id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete SpamDnsblServer --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Expression

[Section titled “Expression”](#expression)

A conditional expression with match rules and a default value.

##### match

[Section titled “match”](#match)

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

## Expression references

[Section titled “Expression references”](#expression-references)

The following expression contexts are used by fields on this page:

[[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]] (Variables)
[[ref/expression/variable/spam-generic-variable|`SpamGenericVariable`]] (Variables)
[[ref/expression/variable/spam-url-variable|`SpamUrlVariable`]] (Variables)
[[ref/expression/variable/spam-email-variable|`SpamEmailVariable`]] (Variables)
[[ref/expression/variable/spam-header-variable|`SpamHeaderVariable`]] (Variables)