---
title: "Listeners"
source: "https://stalw.art/docs/server/listener"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/listener"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Listeners"
---
# Listeners

      

Stalwart accepts incoming TCP connections through one or more listeners. Each listener is represented by a [[ref/object/network-listener|NetworkListener]] object (found in the WebUI under  Settings ›  Network › Listeners), and any number of listeners can coexist on the server.

## Bind settings

[Section titled “Bind settings”](#bind-settings)

The addresses and ports a listener accepts connections on are set through [[ref/object/network-listener#bind|`bind`]], which takes a list of `host:port` entries. A single listener can bind to multiple IPv4 and IPv6 endpoints:

```
{  "name": "smtp",  "protocol": "smtp",  "bind": ["192.0.2.1:25", "203.0.113.9:25", "[2001:db8::2]:25"]}
```

To bind on every interface, use an unspecified address such as `[::]:25`.

## Protocol

[Section titled “Protocol”](#protocol)

Each listener selects its application protocol through [[ref/object/network-listener#protocol|`protocol`]]. The supported values are `smtp`, `lmtp`, `http`, `imap`, `pop3`, and `manageSieve`. For example, an SMTP listener on port 25 alongside an IMAP listener on port 143:

```
[  {"name": "smtp", "protocol": "smtp", "bind": ["[::]:25"]},  {"name": "imap", "protocol": "imap", "bind": ["[::]:143"]}]
```

## Maximum connections

[Section titled “Maximum connections”](#maximum-connections)

The server-wide cap on concurrent connections is set through [[ref/object/system-settings#maxconnections|`maxConnections`]] on the [[ref/object/system-settings|SystemSettings]] singleton, and defaults to `8192`. Each NetworkListener also exposes its own [[ref/object/network-listener#maxconnections|`maxConnections`]] field so that a specific listener (for example an implicit-TLS submissions endpoint) can be capped independently of the server default:

```
{  "name": "submissions",  "protocol": "smtp",  "bind": ["[::]:465"],  "tlsImplicit": true,  "maxConnections": 1024}
```

## TCP socket options

[Section titled “TCP socket options”](#tcp-socket-options)

Socket-level behaviour is configured per listener through the following fields on [[ref/object/network-listener|NetworkListener]]:

[[ref/object/network-listener#socketreuseaddress|`socketReuseAddress`]]: whether the socket can be bound to an address that is already in use. Default `true`.
[[ref/object/network-listener#socketreuseport|`socketReusePort`]]: whether multiple sockets can be bound to the same address and port, useful for certain load-balancing scenarios. Default `true`.
[[ref/object/network-listener#socketbacklog|`socketBacklog`]]: maximum number of pending incoming connections. Default `1024`.
[[ref/object/network-listener#socketttl|`socketTtl`]]: time-to-live value for outgoing packets.
[[ref/object/network-listener#socketsendbuffersize|`socketSendBufferSize`]]: size of the send buffer in bytes.
[[ref/object/network-listener#socketreceivebuffersize|`socketReceiveBufferSize`]]: size of the receive buffer in bytes.
[[ref/object/network-listener#sockettosv4|`socketTosV4`]]: type-of-service (TOS) value for IPv4 traffic, used to signal traffic priority.
[[ref/object/network-listener#socketnodelay|`socketNoDelay`]]: whether the Nagle algorithm should be disabled. Default `true`.

## TLS on a listener

[Section titled “TLS on a listener”](#tls-on-a-listener)

Listener-level TLS is controlled by [[ref/object/network-listener#usetls|`useTls`]], [[ref/object/network-listener#tlsimplicit|`tlsImplicit`]], [[ref/object/network-listener#tlstimeout|`tlsTimeout`]], [[ref/object/network-listener#tlsdisableprotocols|`tlsDisableProtocols`]], [[ref/object/network-listener#tlsdisableciphersuites|`tlsDisableCipherSuites`]], and [[ref/object/network-listener#tlsignoreclientorder|`tlsIgnoreClientOrder`]]. Each NetworkListener carries its own TLS fields and is configured independently: implicit TLS on a submissions endpoint is set by flipping [[ref/object/network-listener#tlsimplicit|`tlsImplicit`]] on that listener alone, without touching other listeners. See [[server/tls|TLS overview]] for the list of supported protocols and cipher suites.

## Example

[Section titled “Example”](#example)

The following listeners define an SMTP server on port 25, an explicit-TLS submission endpoint on port 587, an implicit-TLS submissions endpoint on port 465, and an HTTP management endpoint on port 8080:

```
[  {"name": "smtp", "protocol": "smtp", "bind": ["[::]:25"]},  {"name": "submission", "protocol": "smtp", "bind": ["[::]:587"]},  {"name": "submissions", "protocol": "smtp", "bind": ["[::]:465"], "tlsImplicit": true},  {"name": "management", "protocol": "http", "bind": ["127.0.0.1:8080"]}]
```