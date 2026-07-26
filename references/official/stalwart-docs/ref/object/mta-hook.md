---
title: "MtaHook"
source: "https://stalw.art/docs/ref/object/mta-hook"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-hook"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaHook"
---
# MtaHook

      

Defines an MTA hook endpoint for message processing.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Filters › MTA Hooks

## Fields

[Section titled “Fields”](#fields)

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Whether Stalwart should connect to a hook server that has an invalid TLS certificate

##### enable

[Section titled “enable”](#enable)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Expression that determines whether to enable this hook

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### maxResponseSize

[Section titled “maxResponseSize”](#maxresponsesize)

Type: `Size` · default: `52428800`

Maximum size, in bytes, of a response that Stalwart will accept from this MTA Hook server

##### tempFailOnError

[Section titled “tempFailOnError”](#tempfailonerror)

Type: `Boolean` · default: `true`

Whether to respond with a temporary failure (typically a 4xx SMTP status code) when Stalwart encounters an error while communicating with this MTA Hook server

##### stages

[Section titled “stages”](#stages)

Type: [`MtaStage`](#mtastage)`[]` · default: `["data"]`

Which SMTP stages to run this hook on

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"30s"`

Maximum amount of time that Stalwart will wait for a response from this hook server

##### url

[Section titled “url”](#url)

Type: `Uri` · required

URL of the hook endpoint

##### httpAuth

[Section titled “httpAuth”](#httpauth)

Type: [`HttpAuth`](#httpauth) · required

The type of HTTP authentication to use

##### httpHeaders

[Section titled “httpHeaders”](#httpheaders)

Type: `Map<String, String>`

Additional headers to include in HTTP requests

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaHook object is available via the `urn:stalwart:jmap` capability.

### x:MtaHook/get

[Section titled “x:MtaHook/get”](#xmtahookget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaHookGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaHook/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaHook/set

[Section titled “x:MtaHook/set”](#xmtahookset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaHookCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaHook/set",          {            "create": {              "new1": {                "httpAuth": {                  "@type": "Unauthenticated"                },                "httpHeaders": {},                "url": "https://example.com"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaHookUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaHook/set",          {            "update": {              "id1": {                "allowInvalidCerts": false              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaHookDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaHook/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaHook/query

[Section titled “x:MtaHook/query”](#xmtahookquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaHookQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaHook/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaHook id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaHook \  --field url=https://example.com \  --field 'httpAuth={"@type":"Unauthenticated"}' \  --field 'httpHeaders={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaHook
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaHook id1 --field allowInvalidCerts=false
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaHook --ids id1
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

### HttpAuth

[Section titled “HttpAuth”](#httpauth-1)

Defines the HTTP authentication method to use for HTTP requests.

`Unauthenticated`: Anonymous. No additional fields.
`Basic`: Basic Authentication. Carries the fields of [`HttpAuthBasic`](#httpauthbasic).
`Bearer`: Bearer Token. Carries the fields of [`HttpAuthBearer`](#httpauthbearer).

#### HttpAuthBasic

[Section titled “HttpAuthBasic”](#httpauthbasic)

HTTP Basic authentication credentials.

##### username

[Section titled “username”](#username)

Type: `String` · required

Username for HTTP Basic Authentication

##### secret

[Section titled “secret”](#secret)

Type: [`SecretKey`](#secretkey) · required

Password for HTTP Basic Authentication

##### SecretKey

[Section titled “SecretKey”](#secretkey)

A secret value provided directly, from an environment variable, or from a file.

`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

##### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret-1)

Type: `String` · required · secret

Password or secret value

##### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

##### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

#### HttpAuthBearer

[Section titled “HttpAuthBearer”](#httpauthbearer)

HTTP Bearer token authentication.

##### bearerToken

[Section titled “bearerToken”](#bearertoken)

Type: [`SecretKey`](#secretkey) · required

Bearer token for HTTP Bearer Authentication

## Enums

[Section titled “Enums”](#enums)

### MtaStage

[Section titled “MtaStage”](#mtastage)
ValueLabel`connect`Connect`ehlo`EHLO`auth`AUTH`mail`MAIL FROM`rcpt`RCPT TO`data`DATA

## Expression references

[Section titled “Expression references”](#expression-references)

The following expression contexts are used by fields on this page:

[[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]] (Variables)