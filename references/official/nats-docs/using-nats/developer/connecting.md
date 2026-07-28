---
title: "Connecting"
source: "https://docs.nats.io/using-nats/developer/connecting.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Connecting"
---
# Connecting

In order for a NATS client application to connect to the NATS service, and then subscribe or publish messages to subjects, it needs to be able to be configured with the details of how to connect to the NATS service infrastructure and of how to authenticate with it.

## NATS URL

1. A 'NATS URL' is a string (in a URL format) that specifies the IP address and port where the NATS server(s) can be reached, and what kind of connection to establish:
   * TLS encrypted *only* TCP connection (i.e. NATS URLs starting with `tls://...`)
   * TLS encrypted if the server is configured for it or plain un-encrypted TCP connection otherwise (i.e. NATS URLs starting with `nats://...`)
   * Websocket connection (i.e. NATS URLs starting with `ws://...`)

### Connecting to clusters

Note that when connecting to a NATS service infrastructure with clusters there is more than one URL and the application should allow for more than one URL to be specified in its NATS connect call (typically you pass a comma separated list of URLs as the URL, e.g. `"nats://server1:port1,nats://server2:port2"`).

When connecting to a cluster it is best to provide the complete set of 'seed' URLs for the cluster.

## Authentication details

1. If required: authentication details for the application to identify itself with the NATS server(s). NATS supports multiple authentication schemes:
   * [Username/Password credentials](/using-nats/developer/connecting/userpass.md) (which can be passed as part of the NATS URL)
   * [Decentralized JWT Authentication/Authorization](/using-nats/developer/connecting/creds.md) (where the application is configured with the location of 'credentials file' containing the JWT and private Nkey)
   * [Token Authentication](/using-nats/developer/connecting/token.md#connecting-with-a-token) (where the application is configured with a Token string)
   * [TLS Certificate](/using-nats/developer/connecting/tls.md#connecting-with-tls-and-verify-client-identity) (where the client is configured to use a client TLS certificate and the servers are configured to map the TLS client certificates to users defined in the server configuration)
   * [NKEY with Challenge](/using-nats/developer/connecting/nkey.md) (where the client is configured with a Seed and User NKeys)

### Runtime configuration

Your application should expose a way to be configured at run time with the NATS URL(s) to use. If you want to use a secure infrastructure, the application must provide for the definition of either the credentials file (.creds) to use, or the means to encode the token, or Nkey, in the URL(s).

## Connection Options

Besides the connectivity and security details, there are numerous options for a NATS connection ranging from [timeouts](/using-nats/developer/connecting/reconnect.md#connection-timeout-attributes) to [reconnect settings](/using-nats/developer/connecting/reconnect.md#reconnection-attributes) to setting [asynchronous error and connection event callback handlers](/using-nats/developer/connecting/reconnect.md#advisories) in your application.

## See Also

WebSocket and NATS

{% embed url="<https://www.youtube.com/watch?v=AbAR9zgJnjY>" %}
WebSocket and NATS | Hello World
{% endembed %}

NATS WebSockets and React

{% embed url="<https://www.youtube.com/watch?v=XS_Q0i6orSk>" %}
NATS WebSockets and React
{% endembed %}


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/developer/connecting.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
