---
title: "Http"
source: "https://stalw.art/docs/ref/object/http"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/http"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Http"
---
# Http

      

Configures HTTP server settings including rate limiting, CORS, and security headers.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › HTTP › General Settings ›  Network › HTTP › Security

## Fields

[Section titled “Fields”](#fields)

##### rateLimitAuthenticated

[Section titled “rateLimitAuthenticated”](#ratelimitauthenticated)

Type: [`Rate`](#rate)`?` · default: `{"count":1000,"period":"1m"}`

Specifies the request rate limit for authenticated users

##### rateLimitAnonymous

[Section titled “rateLimitAnonymous”](#ratelimitanonymous)

Type: [`Rate`](#rate)`?` · default: `{"count":100,"period":"1m"}`

Specifies the request rate limit for unauthenticated users

##### allowedEndpoints

[Section titled “allowedEndpoints”](#allowedendpoints)

Type: [`Expression`](#expression) · default: `{"else":"200"}`

An expression that determines whether access to an endpoint is allowed. The expression should an HTTP status code (200, 403, etc.)

Available variables: [[ref/expression/variable/http-variable|`HttpVariable`]].

##### enableHsts

[Section titled “enableHsts”](#enablehsts)

Type: `Boolean` · default: `false`

Specifies whether to enable HTTP Strict Transport Security for the HTTP server.

##### usePermissiveCors

[Section titled “usePermissiveCors”](#usepermissivecors)

Type: `Boolean` · default: `false`

Specifies whether to allow all origins in the CORS policy for the HTTP server

##### responseHeaders

[Section titled “responseHeaders”](#responseheaders)

Type: `Map<String, String>`

Additional headers to include in HTTP responses

##### useXForwarded

[Section titled “useXForwarded”](#usexforwarded)

Type: `Boolean` · default: `false`

Specifies whether to use the Forwarded or X-Forwarded-For header to determine the client’s IP address

##### redirectRoot

[Section titled “redirectRoot”](#redirectroot)

Type: `String?` · default: `"/account"`

The URL to redirect users to when they access the root path of the HTTP server. If not set, the server will return a 404 Not Found response.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Http singleton is available via the `urn:stalwart:jmap` capability.

### x:Http/get

[Section titled “x:Http/get”](#xhttpget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysHttpGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Http/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Http/set

[Section titled “x:Http/set”](#xhttpset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysHttpUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Http/set",          {            "update": {              "singleton": {                "redirectRoot": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Http
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Http --field redirectRoot='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Rate

[Section titled “Rate”](#rate)

Defines a rate limit as a count over a time period.

##### count

[Section titled “count”](#count)

Type: `UnsignedInt` · default: `0` · min: 1 · max: 1000000

Count

##### period

[Section titled “period”](#period)

Type: `Duration` · default: `"0s"` · min: 1

Period

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

[[ref/expression/variable/http-variable|`HttpVariable`]] (Variables)