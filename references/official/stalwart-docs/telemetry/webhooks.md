---
title: "Webhooks"
source: "https://stalw.art/docs/telemetry/webhooks"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/webhooks"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Webhooks"
---
# Webhooks

      

Webhooks deliver real-time notifications about server events by posting to an HTTP endpoint of the operator’s choice. They let external systems react to activity in the server without polling; typical uses include recording deliveries into an analytics pipeline, flagging authentication anomalies to a SIEM, or triggering automated workflows on message ingestion.

## Configuration

[Section titled “Configuration”](#configuration)

Each webhook is represented by a [[ref/object/web-hook|WebHook]] object (found in the WebUI under  Settings ›  Telemetry › Webhooks). Relevant fields are:

[[ref/object/web-hook#url|`url`]]: endpoint to which `POST` requests are sent.
[[ref/object/web-hook#events|`events`]] and [[ref/object/web-hook#eventspolicy|`eventsPolicy`]]: list of events and how to interpret it. With `eventsPolicy` set to `include`, only the listed events trigger the webhook; with `exclude`, all events except the listed ones trigger it. Event identifiers are documented on the [[telemetry/events#event-types|Events]] page.
[[ref/object/web-hook#timeout|`timeout`]]: maximum time to wait for a response. Default `"30s"`.
[[ref/object/web-hook#throttle|`throttle`]]: minimum interval between batches. Events that fall within the same window are grouped. Default `"1s"`.
[[ref/object/web-hook#discardafter|`discardAfter`]]: duration after which an undelivered webhook request is discarded. Default `"5m"`.
[[ref/object/web-hook#signaturekey|`signatureKey`]]: optional HMAC key used to sign each request body. The signature is base64-encoded and carried in the `X-Signature` header. The field is a `SecretKeyOptional` with variants `None`, `Value`, `EnvironmentVariable`, and `File`.
[[ref/object/web-hook#httpauth|`httpAuth`]]: HTTP authentication used by the request. A nested type with variants `Unauthenticated`, `Basic`, and `Bearer`.
[[ref/object/web-hook#httpheaders|`httpHeaders`]]: additional HTTP headers to include on each request.
[[ref/object/web-hook#allowinvalidcerts|`allowInvalidCerts`]]: whether to permit requests to endpoints with an invalid TLS certificate. Default `false`.
[[ref/object/web-hook#lossy|`lossy`]]: whether to drop events when the endpoint is unreachable or persistently failing. When `false`, events accumulate until delivery succeeds or [[ref/object/web-hook#discardafter|`discardAfter`]] elapses.
[[ref/object/web-hook#enable|`enable`]]: whether the webhook is active.
[[ref/object/web-hook#level|`level`]]: minimum severity of events delivered to this endpoint.

## API documentation

[Section titled “API documentation”](#api-documentation)

### Response object

[Section titled “Response object”](#response-object)

The main object in the webhook response is `WebhookEvents`, which contains a list of `WebhookEvent` objects:

```
{    "events": [        {            "id": "12345",            "createdAt": "2023-06-21T14:55:00Z",            "type": "auth.success",            "data": {}        }    ]}
```

Each `WebhookEvent` contains the following fields:

`id` (String): Unique identifier for the event.
`createdAt` (RFC3339 timestamp): Timestamp when the event was created.
`type` (WebhookType): Type of the [[telemetry/events#event-types|event]].
`data` (WebhookPayload): Detailed data associated with the event.

### Data payload

[Section titled “Data payload”](#data-payload)

The `data` payload is a nested object that contains additional information about the event. The structure of the `data` object varies depending on the event type and it may contain any of the supported [[telemetry/events#key-types|keys]].

## Example

[Section titled “Example”](#example)

The equivalent of a webhook firing on `auth.success` and `store.ingest`, signed with a shared secret, with a custom header and Basic authentication:

```
{  "url": "https://example.com/webhook",  "events": ["auth.success", "store.ingest"],  "eventsPolicy": "include",  "timeout": "30s",  "throttle": "1s",  "signatureKey": {"@type": "Value", "secret": "my-secret-key"},  "httpHeaders": {"X-My-Header": "my-value"},  "httpAuth": {    "@type": "Basic",    "username": "account",    "secret": {"@type": "Value", "secret": "password"}  },  "allowInvalidCerts": false,  "enable": true}
```

Each entry in [[ref/object/web-hook#events|`events`]] selects a single event from the EventType enum; wildcard patterns are not supported, so every event to be delivered must be listed explicitly.