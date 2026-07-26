---
title: "Access Control"
source: "https://stalw.art/docs/http/access-control"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/access-control"
category: "http"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Access Control"
---
# Access Control

      

Stalwart provides a flexible access control mechanism for the HTTP server. Rules can restrict access by IP address, resource path, method name, listener identity, and other request attributes, so that sensitive services can be exposed only to the clients and listeners that require them.

## Configuration

[Section titled “Configuration”](#configuration)

Access rules are carried by the [[ref/object/http#allowedendpoints|`allowedEndpoints`]] expression on the [[ref/object/http|Http]] singleton (found in the WebUI under  Settings ›  Network › HTTP › General,  Settings ›  Network › HTTP › Security). The expression is evaluated for each incoming request; it must return an HTTP status code. A result of `200` permits the request, while any other value denies it and the server responds with the returned status. By default the expression evaluates to `200`, allowing every endpoint.

## Examples

[Section titled “Examples”](#examples)

### Restriction by IP address

[Section titled “Restriction by IP address”](#restriction-by-ip-address)

The following rule restricts access to `/api/*` to requests originating from `127.0.0.1`:

```
{  "allowedEndpoints": {    "match": [      {"if": "starts_with(url_path, '/api') && remote_ip != '127.0.0.1'", "then": "404"}    ],    "else": "200"  }}
```

### Restriction by IP range

[Section titled “Restriction by IP range”](#restriction-by-ip-range)

The following rule allows public access to `/robots.txt` and `/.well-known/*`; all other requests are denied unless they originate from the `192.168.1.*` network.

```
{  "allowedEndpoints": {    "match": [      {"if": "starts_with(remote_ip, '192.168.1.') || contains(['robots.txt', '.well-known'], split(url_path, '/')[1])", "then": "200"}    ],    "else": "400"  }}
```

### Restriction by listener

[Section titled “Restriction by listener”](#restriction-by-listener)

Two HTTP listeners can be defined on the [[ref/object/network-listener|NetworkListener]] object (found in the WebUI under  Settings ›  Network › Listeners): a `private-http` listener bound to localhost, and a `public-http` listener bound to all interfaces. HTTP requests arriving through `private-http` are unrestricted, while requests coming from `public-http` are only allowed for the `/jmap`, `/robots.txt`, and `/.well-known/*` endpoints.

The rule on the Http singleton:

```
{  "allowedEndpoints": {    "match": [      {"if": "listener == 'private-http' || contains(['jmap', 'robots.txt', '.well-known'], split(url_path, '/')[1])", "then": "200"}    ],    "else": "404"  }}
```

### Restriction by endpoint and method

[Section titled “Restriction by endpoint and method”](#restriction-by-endpoint-and-method)

The following rule disables JMAP access unless the request is an `OPTIONS` request:

```
{  "allowedEndpoints": {    "match": [      {"if": "!starts_with(url, '/jmap') || method == 'OPTIONS'", "then": "200"}    ],    "else": "400"  }}
```