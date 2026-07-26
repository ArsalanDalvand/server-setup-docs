---
title: "MtaRoute"
source: "https://stalw.art/docs/ref/object/mta-route"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-route"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaRoute"
---
# MtaRoute

      

Defines a routing rule for outbound message delivery.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Outbound › Routes

## Fields

[Section titled “Fields”](#fields)

MtaRoute is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Mx"

[Section titled “@type: "Mx"”](#type-mx)

Remote Delivery (MX)

##### ipLookupStrategy

[Section titled “ipLookupStrategy”](#iplookupstrategy)

Type: [`MtaIpStrategy`](#mtaipstrategy) · default: `"v4ThenV6"`

IP resolution strategy for MX hosts

##### maxMultihomed

[Section titled “maxMultihomed”](#maxmultihomed)

Type: `UnsignedInt` · default: `2` · min: 1

For multi-homed remote servers, it is the maximum number of IP addresses to try on each delivery attempt

##### maxMxHosts

[Section titled “maxMxHosts”](#maxmxhosts)

Type: `UnsignedInt` · default: `5` · min: 1

Maximum number of MX hosts to try on each delivery attempt

##### name

[Section titled “name”](#name)

Type: `String` · read-only

Short identifier for the route

##### description

[Section titled “description”](#description)

Type: `String?`

A short description of the route, which can be used to identify it in the list of routes

### @type: "Relay"

[Section titled “@type: "Relay"”](#type-relay)

Relay Host

##### address

[Section titled “address”](#address)

Type: `String` · required

The address of the remote SMTP server, which can be an IP address or a domain name

##### authSecret

[Section titled “authSecret”](#authsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

The secret to use when authenticating with the remote server

##### authUsername

[Section titled “authUsername”](#authusername)

Type: `String?`

The username to use when authenticating with the remote server

##### port

[Section titled “port”](#port)

Type: `UnsignedInt` · default: `25` · max: 65535 · min: 1

The port number of the remote server, which is typically 25 for SMTP and 11200 for LMTP

##### protocol

[Section titled “protocol”](#protocol)

Type: [`MtaProtocol`](#mtaprotocol) · default: `"smtp"`

The protocol to use when delivering messages to the remote server, which can be either SMTP or LMTP

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Whether to allow connections to servers with invalid TLS certificates

##### implicitTls

[Section titled “implicitTls”](#implicittls)

Type: `Boolean` · default: `false`

Whether to use TLS encryption for all connections to the remote server

##### name

[Section titled “name”](#name-1)

Type: `String` · read-only

Short identifier for the route

##### description

[Section titled “description”](#description-1)

Type: `String?`

A short description of the route, which can be used to identify it in the list of routes

### @type: "Local"

[Section titled “@type: "Local"”](#type-local)

Local Delivery

##### name

[Section titled “name”](#name-2)

Type: `String` · read-only

Short identifier for the route

##### description

[Section titled “description”](#description-2)

Type: `String?`

A short description of the route, which can be used to identify it in the list of routes

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaRoute object is available via the `urn:stalwart:jmap` capability.

### x:MtaRoute/get

[Section titled “x:MtaRoute/get”](#xmtarouteget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaRouteGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaRoute/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaRoute/set

[Section titled “x:MtaRoute/set”](#xmtarouteset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaRouteCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaRoute/set",          {            "create": {              "new1": {                "@type": "Mx"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaRouteUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaRoute/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaRouteDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaRoute/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaRoute/query

[Section titled “x:MtaRoute/query”](#xmtaroutequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaRouteQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaRoute/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MtaRoute/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaRoute id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaRoute/Mx
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaRoutestalwart-cli query MtaRoute --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaRoute id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaRoute --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret)

Type: `String` · required · secret

Password or secret value

#### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

#### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

## Enums

[Section titled “Enums”](#enums)

### MtaIpStrategy

[Section titled “MtaIpStrategy”](#mtaipstrategy)
ValueLabel`v4ThenV6`IPv4 then IPv6`v6ThenV4`IPv6 then IPv4`v4Only`IPv4 Only`v6Only`IPv6 Only

### MtaProtocol

[Section titled “MtaProtocol”](#mtaprotocol)
ValueLabel`smtp`SMTP`lmtp`LMTP