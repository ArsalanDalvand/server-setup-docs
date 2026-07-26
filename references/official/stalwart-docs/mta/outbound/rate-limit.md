---
title: "Rate limits"
source: "https://stalw.art/docs/mta/outbound/rate-limit"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/outbound/rate-limit"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Rate limits"
---
# Rate limits

      

Rate limiting controls the flow of connections and messages by restricting the number of operations allowed over a specific time period. In Stalwart, rate limiting ensures that email traffic is managed efficiently, preventing resource overuse and protecting the server from abuse such as spam or denial-of-service attacks.

Stalwart enforces rate limits on both inbound and outbound connections. Inbound limits apply to connections from remote servers on standard ports (such as 25 and 465) and help regulate the flow of incoming messages. Outbound limits apply to connections initiated by the queue when delivering messages to remote hosts, and help manage outgoing traffic volume so that external systems are not overwhelmed.

In distributed environments, rate limiters are shared across the whole cluster and stored in the default [[storage/in-memory|in-memory store]] for low-latency access and efficient synchronisation across nodes.

## Inbound limits

[Section titled “Inbound limits”](#inbound-limits)

Inbound rate limits regulate the flow of incoming connections and messages from remote servers. They mitigate spam campaigns, denial-of-service attempts, and routing loops.

When an inbound rate limit is exceeded, additional messages are rejected with a `451 4.4.5 Rate limit exceeded, try again later.` error. This temporary rejection prevents server overload while signalling the remote sender to retry later. Rejection continues until the rate-limiter window resets.

Inbound rate limits are defined as [[ref/object/mta-inbound-throttle|MtaInboundThrottle]] objects (found in the WebUI under  Settings ›  MTA › Rates & Quotas › Inbound Rate Limits).

## Outbound limits

[Section titled “Outbound limits”](#outbound-limits)

Outbound rate limits regulate the flow of connections and messages initiated by Stalwart to deliver mail to remote systems. Unlike inbound rate limits, outbound rate limiting does not reject or discard messages: when the limit is reached, affected messages remain in the queue until the window resets, at which point delivery resumes according to the configured retry schedule.

Outbound rate limits are defined as [[ref/object/mta-outbound-throttle|MtaOutboundThrottle]] objects (found in the WebUI under  Settings ›  MTA › Rates & Quotas › Outbound Rate Limits).

## Settings

[Section titled “Settings”](#settings)

Both MtaInboundThrottle and MtaOutboundThrottle expose the same core fields:

[[ref/object/mta-outbound-throttle#enable|`enable`]]: whether the throttle is active. Default `true`.
[[ref/object/mta-outbound-throttle#rate|`rate`]]: the rate limit itself, a `Rate` object with `count` (requests allowed) and `period` (time window).
[[ref/object/mta-outbound-throttle#key|`key`]]: optional list of context variables that scope the limit (see [Groups](#groups) below).
[[ref/object/mta-outbound-throttle#match|`match`]]: optional expression that determines whether the limit applies in a given context.
[[ref/object/mta-outbound-throttle#description|`description`]]: short description used for identification (required on MtaOutboundThrottle, read-only on MtaInboundThrottle).

A throttle with an empty `key` list applies globally. For example, an outbound throttle allowing at most 100 outgoing messages per second across all destinations:

```
{  "enable": true,  "description": "Global outbound cap",  "key": [],  "match": {"else": "true"},  "rate": {"count": 100, "period": "1s"}}
```

### Groups

[Section titled “Groups”](#groups)

The `key` field scopes a limit to specific combinations of context variables. Available keys differ between inbound and outbound throttles.

For outbound throttles ([[ref/object/mta-outbound-throttle#mtaoutboundthrottlekey|MtaOutboundThrottleKey]]):

`remoteIp`: remote IP address.
`localIp`: local IP address (only available when a [[mta/outbound/connection#source-ip|source IP]] is configured).
`mx`: remote MX hostname.
`sender`: return path specified in `MAIL FROM`.
`senderDomain`: domain component of the return path.
`rcptDomain`: domain component of the recipient address.

For inbound throttles ([[ref/object/mta-inbound-throttle#mtainboundthrottlekey|MtaInboundThrottleKey]]):

`listener`: identifier of the listener that received the connection.
`remoteIp`: IP address of the client.
`localIp`: local IP address.
`authenticatedAs`: username used for authentication.
`heloDomain`: domain supplied in the `EHLO` command.
`sender`: return path specified in `MAIL FROM`.
`senderDomain`: domain component of the return path.
`rcpt`: recipient address specified in `RCPT TO`.
`rcptDomain`: domain component of the recipient address.

For example, an outbound throttle limiting to 25 messages per hour per recipient domain:

```
{  "enable": true,  "description": "Per recipient domain throttle",  "key": ["rcptDomain"],  "match": {"else": "true"},  "rate": {"count": 25, "period": "1h"}}
```

And an inbound throttle limiting a single sending domain to 6 messages per minute to any given recipient:

```
{  "enable": true,  "key": ["senderDomain", "rcpt"],  "match": {"else": "true"},  "rate": {"count": 6, "period": "1m"}}
```

### Conditional limits

[Section titled “Conditional limits”](#conditional-limits)

The `match` expression can restrict a throttle to specific contexts. For example, an outbound throttle imposing a per-sender-per-recipient-domain limit only when mail is destined for IP `192.0.2.20`:

```
{  "enable": true,  "description": "Throttle deliveries routed via 192.0.2.20",  "key": ["sender", "rcptDomain"],  "match": {"else": "remote_ip == '192.0.2.20'"},  "rate": {"count": 100, "period": "1h"}}
```

And an inbound throttle imposing a per-recipient limit only for connections from `192.0.2.25`:

```
{  "enable": true,  "key": ["rcpt"],  "match": {"else": "remote_ip == '192.0.2.25'"},  "rate": {"count": 5, "period": "1h"}}
```