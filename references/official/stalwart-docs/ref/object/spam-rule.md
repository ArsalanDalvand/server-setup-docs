---
title: "SpamRule"
source: "https://stalw.art/docs/ref/object/spam-rule"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-rule"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamRule"
---
# SpamRule

      

Defines a spam filter rule for message classification.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › Rules

## Fields

[Section titled “Fields”](#fields)

SpamRule is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Any"

[Section titled “@type: "Any"”](#type-any)

Any

##### condition

[Section titled “condition”](#condition)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-generic-variable|`SpamGenericVariable`]].

##### name

[Section titled “name”](#name)

Type: `String` · read-only

Short name for the rule

##### description

[Section titled “description”](#description)

Type: `String?`

Description for the rule

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Whether to enable this rule

##### priority

[Section titled “priority”](#priority)

Type: `Integer` · default: `500` · max: 99999 · min: -99999

The priority of the rule

### @type: "Url"

[Section titled “@type: "Url"”](#type-url)

URL

##### condition

[Section titled “condition”](#condition-1)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-url-variable|`SpamUrlVariable`]].

##### name

[Section titled “name”](#name-1)

Type: `String` · read-only

Short name for the rule

##### description

[Section titled “description”](#description-1)

Type: `String?`

Description for the rule

##### enable

[Section titled “enable”](#enable-1)

Type: `Boolean` · default: `true`

Whether to enable this rule

##### priority

[Section titled “priority”](#priority-1)

Type: `Integer` · default: `500` · max: 99999 · min: -99999

The priority of the rule

### @type: "Domain"

[Section titled “@type: "Domain"”](#type-domain)

Domain

##### condition

[Section titled “condition”](#condition-2)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-generic-variable|`SpamGenericVariable`]].

##### name

[Section titled “name”](#name-2)

Type: `String` · read-only

Short name for the rule

##### description

[Section titled “description”](#description-2)

Type: `String?`

Description for the rule

##### enable

[Section titled “enable”](#enable-2)

Type: `Boolean` · default: `true`

Whether to enable this rule

##### priority

[Section titled “priority”](#priority-2)

Type: `Integer` · default: `500` · max: 99999 · min: -99999

The priority of the rule

### @type: "Email"

[Section titled “@type: "Email"”](#type-email)

E-mail

##### condition

[Section titled “condition”](#condition-3)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-email-variable|`SpamEmailVariable`]].

##### name

[Section titled “name”](#name-3)

Type: `String` · read-only

Short name for the rule

##### description

[Section titled “description”](#description-3)

Type: `String?`

Description for the rule

##### enable

[Section titled “enable”](#enable-3)

Type: `Boolean` · default: `true`

Whether to enable this rule

##### priority

[Section titled “priority”](#priority-3)

Type: `Integer` · default: `500` · max: 99999 · min: -99999

The priority of the rule

### @type: "Ip"

[Section titled “@type: "Ip"”](#type-ip)

IP

##### condition

[Section titled “condition”](#condition-4)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]].

##### name

[Section titled “name”](#name-4)

Type: `String` · read-only

Short name for the rule

##### description

[Section titled “description”](#description-4)

Type: `String?`

Description for the rule

##### enable

[Section titled “enable”](#enable-4)

Type: `Boolean` · default: `true`

Whether to enable this rule

##### priority

[Section titled “priority”](#priority-4)

Type: `Integer` · default: `500` · max: 99999 · min: -99999

The priority of the rule

### @type: "Header"

[Section titled “@type: "Header"”](#type-header)

Header

##### condition

[Section titled “condition”](#condition-5)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-header-variable|`SpamHeaderVariable`]].

##### name

[Section titled “name”](#name-5)

Type: `String` · read-only

Short name for the rule

##### description

[Section titled “description”](#description-5)

Type: `String?`

Description for the rule

##### enable

[Section titled “enable”](#enable-5)

Type: `Boolean` · default: `true`

Whether to enable this rule

##### priority

[Section titled “priority”](#priority-5)

Type: `Integer` · default: `500` · max: 99999 · min: -99999

The priority of the rule

### @type: "Body"

[Section titled “@type: "Body"”](#type-body)

Body

##### condition

[Section titled “condition”](#condition-6)

Type: [`Expression`](#expression) · required

Expression that returns the tag to assign to the message.

Available variables: [[ref/expression/variable/spam-generic-variable|`SpamGenericVariable`]].

##### name

[Section titled “name”](#name-6)

Type: `String` · read-only

Short name for the rule

##### description

[Section titled “description”](#description-6)

Type: `String?`

Description for the rule

##### enable

[Section titled “enable”](#enable-6)

Type: `Boolean` · default: `true`

Whether to enable this rule

##### priority

[Section titled “priority”](#priority-6)

Type: `Integer` · default: `500` · max: 99999 · min: -99999

The priority of the rule

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamRule object is available via the `urn:stalwart:jmap` capability.

### x:SpamRule/get

[Section titled “x:SpamRule/get”](#xspamruleget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysSpamRuleGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamRule/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamRule/set

[Section titled “x:SpamRule/set”](#xspamruleset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysSpamRuleCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamRule/set",          {            "create": {              "new1": {                "@type": "Any",                "condition": {                  "else": "Example",                  "match": {}                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysSpamRuleUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamRule/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysSpamRuleDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamRule/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamRule/query

[Section titled “x:SpamRule/query”](#xspamrulequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysSpamRuleQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamRule/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:SpamRule/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamRule id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create SpamRule/Any \  --field 'condition={"else":"Example","match":{}}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query SpamRulestalwart-cli query SpamRule --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update SpamRule id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete SpamRule --ids id1
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

[[ref/expression/variable/spam-generic-variable|`SpamGenericVariable`]] (Variables)
[[ref/expression/variable/spam-url-variable|`SpamUrlVariable`]] (Variables)
[[ref/expression/variable/spam-email-variable|`SpamEmailVariable`]] (Variables)
[[ref/expression/variable/spam-ip-variable|`SpamIpVariable`]] (Variables)
[[ref/expression/variable/spam-header-variable|`SpamHeaderVariable`]] (Variables)