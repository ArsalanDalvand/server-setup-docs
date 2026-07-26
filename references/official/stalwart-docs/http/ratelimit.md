---
title: "Rate limiting"
source: "https://stalw.art/docs/http/ratelimit"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ratelimit"
category: "http"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Rate limiting"
---
# Rate limiting

      

Rate limiting restricts how often a client can repeat an action within a given time period. Applied to the HTTP server, it mitigates brute-force attacks and reduces overall load by capping the number of requests that any single source can issue in a short window.

Stalwart tracks the IP address of each incoming HTTP request and measures the number of requests received over the configured window. Once a client exceeds the configured rate, further requests from that source are rejected until the window elapses.

HTTP rate limits are configured on the [[ref/object/http|Http]] singleton (found in the WebUI under  Settings ›  Network › HTTP › General,  Settings ›  Network › HTTP › Security) through two separate fields, one for anonymous traffic and one for authenticated users. Each field is a `Rate` value: a count over a duration.

## Anonymous limits

[Section titled “Anonymous limits”](#anonymous-limits)

The [[ref/object/http#ratelimitanonymous|`rateLimitAnonymous`]] field sets the request rate allowed to an anonymous IP address. The default is 100 requests per minute.

```
{  "rateLimitAnonymous": {"count": 100, "period": "1m"}}
```

## Authenticated limits

[Section titled “Authenticated limits”](#authenticated-limits)

The [[ref/object/http#ratelimitauthenticated|`rateLimitAuthenticated`]] field sets the request rate allowed to an authenticated user. The default is 1000 requests per minute.

```
{  "rateLimitAuthenticated": {"count": 1000, "period": "1m"}}
```