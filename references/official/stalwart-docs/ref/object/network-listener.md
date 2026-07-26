---
title: "NetworkListener"
source: "https://stalw.art/docs/ref/object/network-listener"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/network-listener"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "NetworkListener"
---
# NetworkListener

      

Defines a network listener for accepting incoming connections.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › Listeners

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `String` · read-only

Unique identifier for the listener

##### bind

[Section titled “bind”](#bind)

Type: `SocketAddr[]` · min items: 1

The addresses the listener will bind to

##### protocol

[Section titled “protocol”](#protocol)

Type: [`NetworkListenerProtocol`](#networklistenerprotocol) · default: `"smtp"`

The protocol used by the listener

##### overrideProxyTrustedNetworks

[Section titled “overrideProxyTrustedNetworks”](#overrideproxytrustednetworks)

Type: `IpMask[]`

Enable proxy protocol for connections from these networks

##### socketBacklog

[Section titled “socketBacklog”](#socketbacklog)

Type: `UnsignedInt?` · default: `1024` · min: 1

The maximum number of incoming connections that can be pending in the backlog queue

##### socketNoDelay

[Section titled “socketNoDelay”](#socketnodelay)

Type: `Boolean` · default: `true`

Whether the Nagle algorithm should be disabled for the socket

##### socketReceiveBufferSize

[Section titled “socketReceiveBufferSize”](#socketreceivebuffersize)

Type: `UnsignedInt?` · min: 1

The size of the buffer used for receiving data

##### socketReuseAddress

[Section titled “socketReuseAddress”](#socketreuseaddress)

Type: `Boolean` · default: `true`

Whether the socket can be bound to an address that is already in use by another socket

##### socketReusePort

[Section titled “socketReusePort”](#socketreuseport)

Type: `Boolean` · default: `true`

Whether multiple sockets can be bound to the same address and port

##### socketSendBufferSize

[Section titled “socketSendBufferSize”](#socketsendbuffersize)

Type: `UnsignedInt?` · min: 1

The size of the buffer used for sending data

##### socketTosV4

[Section titled “socketTosV4”](#sockettosv4)

Type: `UnsignedInt?` · min: 1

The type of service (TOS) value for the socket, which determines the priority of the traffic sent through the socket

##### socketTtl

[Section titled “socketTtl”](#socketttl)

Type: `UnsignedInt?` · min: 1

Time-to-live (TTL) value for the socket, which determines how many hops a packet can make before it is discarded

##### useTls

[Section titled “useTls”](#usetls)

Type: `Boolean` · default: `true`

Whether to enable TLS for this listener

##### tlsDisableCipherSuites

[Section titled “tlsDisableCipherSuites”](#tlsdisableciphersuites)

Type: [`TlsCipherSuite`](#tlsciphersuite)`[]`

Which cipher suites to disable

##### tlsDisableProtocols

[Section titled “tlsDisableProtocols”](#tlsdisableprotocols)

Type: [`TlsVersion`](#tlsversion)`[]`

Which TLS protocols to disable

##### tlsIgnoreClientOrder

[Section titled “tlsIgnoreClientOrder”](#tlsignoreclientorder)

Type: `Boolean` · default: `true`

Whether to ignore the client’s cipher order

##### tlsImplicit

[Section titled “tlsImplicit”](#tlsimplicit)

Type: `Boolean` · default: `false`

Whether to use implicit TLS

##### tlsTimeout

[Section titled “tlsTimeout”](#tlstimeout)

Type: `Duration?` · default: `"1m"`

TLS handshake timeout

##### maxConnections

[Section titled “maxConnections”](#maxconnections)

Type: `UnsignedInt?` · default: `8192` · min: 1

The maximum number of concurrent connections the listener will accept

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The NetworkListener object is available via the `urn:stalwart:jmap` capability.

### x:NetworkListener/get

[Section titled “x:NetworkListener/get”](#xnetworklistenerget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysNetworkListenerGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:NetworkListener/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:NetworkListener/set

[Section titled “x:NetworkListener/set”](#xnetworklistenerset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysNetworkListenerCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:NetworkListener/set",          {            "create": {              "new1": {                "bind": {},                "overrideProxyTrustedNetworks": {},                "tlsDisableCipherSuites": {},                "tlsDisableProtocols": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysNetworkListenerUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:NetworkListener/set",          {            "update": {              "id1": {                "bind": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysNetworkListenerDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:NetworkListener/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:NetworkListener/query

[Section titled “x:NetworkListener/query”](#xnetworklistenerquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysNetworkListenerQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:NetworkListener/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:NetworkListener/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get NetworkListener id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create NetworkListener \  --field 'bind={}' \  --field 'overrideProxyTrustedNetworks={}' \  --field 'tlsDisableCipherSuites={}' \  --field 'tlsDisableProtocols={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query NetworkListenerstalwart-cli query NetworkListener --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update NetworkListener id1 --field bind='{}'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete NetworkListener --ids id1
```

## Enums

[Section titled “Enums”](#enums)

### NetworkListenerProtocol

[Section titled “NetworkListenerProtocol”](#networklistenerprotocol)
ValueLabel`smtp`SMTP`lmtp`LMTP`http`HTTP`imap`IMAP4`pop3`POP3`manageSieve`ManageSieve

### TlsCipherSuite

[Section titled “TlsCipherSuite”](#tlsciphersuite)
ValueLabel`tls13-aes-256-gcm-sha384`TLS1.3 AES256 GCM SHA384`tls13-aes-128-gcm-sha256`TLS1.3 AES128 GCM SHA256`tls13-chacha20-poly1305-sha256`TLS1.3 CHACHA20 POLY1305 SHA256`tls-ecdhe-ecdsa-with-aes-256-gcm-sha384`ECDHE ECDSA AES256 GCM SHA384`tls-ecdhe-ecdsa-with-aes-128-gcm-sha256`ECDHE ECDSA AES128 GCM SHA256`tls-ecdhe-ecdsa-with-chacha20-poly1305-sha256`ECDHE ECDSA CHACHA20 POLY1305 SHA256`tls-ecdhe-rsa-with-aes-256-gcm-sha384`ECDHE RSA AES256 GCM SHA384`tls-ecdhe-rsa-with-aes-128-gcm-sha256`ECDHE RSA AES128 GCM SHA256`tls-ecdhe-rsa-with-chacha20-poly1305-sha256`ECDHE RSA CHACHA20 POLY1305 SHA256

### TlsVersion

[Section titled “TlsVersion”](#tlsversion)
ValueLabel`tls12`TLS version 1.2`tls13`TLS version 1.3