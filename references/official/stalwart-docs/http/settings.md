---
title: "Settings"
source: "https://stalw.art/docs/http/settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/settings"
category: "http"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Settings"
---
# Settings

      

Stalwart includes a built-in HTTP server that handles [[http/jmap|JMAP]] and [[http/webdav|WebDAV]] requests. HTTP server behaviour is configured on the [[ref/object/http|Http]] singleton (found in the WebUI under  Settings ›  Network › HTTP › General,  Settings ›  Network › HTTP › Security).

## Response headers

[Section titled “Response headers”](#response-headers)

Additional HTTP headers returned with every response are configured through [[ref/object/http#responseheaders|`responseHeaders`]], a map of header name to header value. Typical uses include setting security policies, caching behaviour, or adding custom identification headers.

For example, to advertise a `Server` header and a cache-control policy:

```
{  "responseHeaders": {    "Cache-Control": "max-age=3600",    "Server": "Stalwart"  }}
```

Administrators should exercise caution when overriding headers: certain values can affect the behaviour of clients or intermediary systems. Consult the relevant specifications before introducing custom headers.

## Use Forwarded IP

[Section titled “Use Forwarded IP”](#use-forwarded-ip)

When Stalwart runs behind a reverse proxy such as Cloudflare or Amazon CloudFront, the client IP address must be read from the `Forwarded` or `X-Forwarded-For` HTTP header rather than from the socket source address, which would otherwise report the proxy’s address. This is controlled by [[ref/object/http#usexforwarded|`useXForwarded`]]; setting the flag to `true` instructs the server to trust those headers. The flag should not be enabled when the [[server/reverse-proxy/proxy-protocol|proxy protocol]] is in use.

Care must be taken when enabling this feature. It should only be used when Stalwart is behind a trusted proxy, because untrusted sources can forge these headers and mislead logging or access control decisions. When no proxy is present, [[ref/object/http#usexforwarded|`useXForwarded`]] must remain `false` to prevent malicious clients from spoofing their source IP address.

The base URL advertised in well-known responses (for example `/.well-known/jmap` and OAuth discovery metadata) is derived automatically from the listener address and server hostname; no explicit configuration field is required.