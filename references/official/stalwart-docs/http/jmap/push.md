---
title: "Push notifications"
source: "https://stalw.art/docs/http/jmap/push"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/jmap/push"
category: "http"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Push notifications"
---
# Push notifications

      

Push notifications allow JMAP clients to receive updates almost immediately and stay in sync with data changes on the server. Stalwart supports two mechanisms:

Event source: clients hold a long-lived HTTP connection open and receive notifications directly from the server over a `text/event-stream` channel.
Push subscriptions: clients register an external push service URL with the server. Each state change produces an HTTP POST to that URL, and the push service is responsible for delivering the notification to the client.

Push and event-source settings are carried on the [[ref/object/jmap|Jmap]] singleton (found in the WebUI under  Settings ›  Network › JMAP › Limits,  Settings ›  Network › JMAP › Push,  Settings ›  Network › JMAP › WebSocket).

## Push Subscriptions

[Section titled “Push Subscriptions”](#push-subscriptions)

JMAP clients activate push subscriptions by registering a push service URL with the server. Each notification is sent to that URL over HTTPS. For additional privacy, clients can enable [web push encryption](https://datatracker.ietf.org/doc/html/rfc8291) by supplying Elliptic Curve Diffie-Hellman (ECDH) public keys during registration.

The push subsystem is tuned through the following fields:

[[ref/object/jmap#pushthrottle|`pushThrottle`]]: minimum time to wait between successive requests to the same push service. Default `"1s"`.
[[ref/object/jmap#pushmaxattempts|`pushMaxAttempts`]]: maximum number of delivery attempts before a notification is discarded. Default `3`.
[[ref/object/jmap#pushattemptwait|`pushAttemptWait`]]: time to wait between delivery attempts. Default `"1m"`.
[[ref/object/jmap#pushretrywait|`pushRetryWait`]]: time to wait between retries. Default `"1s"`.
[[ref/object/jmap#pushrequesttimeout|`pushRequestTimeout`]]: time after which a connection to the push service URL is considered timed out. Default `"10s"`.
[[ref/object/jmap#pushverifytimeout|`pushVerifyTimeout`]]: time to wait for the push service to verify a new subscription. Default `"1m"`.

## Event Source

[Section titled “Event Source”](#event-source)

JMAP clients that can hold transport connections open connect directly to Stalwart and receive push notifications through a `text/event-stream` resource, as described in the [event source specification](https://html.spec.whatwg.org/multipage/server-sent-events.html#server-sent-events). This is a long-running HTTP request during which the server can append data to the response without terminating it.

The event source endpoint is listed in the JMAP Session resource object (`/.well-known/jmap`) and is available at `/jmap/eventsource/?types={types}&closeafter={closeafter}&ping={ping}`.

To avoid overwhelming the client, the server groups updates and delivers them at a configurable cadence controlled by [[ref/object/jmap#eventsourcethrottle|`eventSourceThrottle`]]. Default `"1s"`.

## Conformed RFCs

[Section titled “Conformed RFCs”](#conformed-rfcs)

[RFC 5116 - An Interface and Algorithms for Authenticated Encryption (AEAD_AES_128_GCM)](https://datatracker.ietf.org/doc/html/rfc5116#section-5.1)
[RFC 8030 - Generic Event Delivery Using HTTP Push](https://datatracker.ietf.org/doc/html/rfc8030)
[RFC 8188 - Encrypted Content-Encoding for HTTP](https://datatracker.ietf.org/doc/html/rfc8188)
[RFC 8291 - Message Encryption for Web Push](https://datatracker.ietf.org/doc/html/rfc8291)