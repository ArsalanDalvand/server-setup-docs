---
title: "Security"
source: "https://docs.nats.io/nats-concepts/security.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/security"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Security"
---
# Security

NATS has a lot of security features:

* Connections can be [*encrypted* with TLS](/running-a-nats-service/configuration/securing_nats/tls.md)
* Client connections can be [*authenticated*](/running-a-nats-service/configuration/securing_nats/auth_intro.md) in many ways:
  * [Token Authentication](/running-a-nats-service/configuration/securing_nats/auth_intro/tokens.md)
  * [Username/Password credentials](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md)
  * [TLS Certificate](/running-a-nats-service/configuration/securing_nats/auth_intro/tls_mutual_auth.md)
  * [NKEY with Challenge](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md)
  * [Decentralized JWT Authentication/Authorization](/running-a-nats-service/configuration/securing_nats/auth_intro/jwt.md)
  * You can also integrate NATS with your existing authentication/authorization system or create your own custom authentication using the [Auth callout](/running-a-nats-service/configuration/securing_nats/auth_callout.md)
* Authenticated clients are identified as users and have a set of [*authorizations*](/running-a-nats-service/configuration/securing_nats/authorization.md)

You can use [accounts](/running-a-nats-service/configuration/securing_nats/accounts.md) for multi-tenancy: each account has its own independent 'subject namespace' and you control the import/export of both streams of messages and services between accounts, and any number of users that client applications can be authenticated as. The subjects or subject wildcards that a user is allowed to publish and/or subscribe to can be controlled either through server configuration or as part of signed JWTs.

JWT authentication/authorization administration is decentralized because each account private key holder can manage their users and their authorizations on their own, without the need for any configuration change on the NATS servers by minting their own JWTs and distributing them to the users. There is no need for the NATS server to ever store any user private keys as they only need to validate the signature chain of trust contained in the user JWT presented by the client application to validate that they have the proper public key for that user.

The JetStream persistence layer of NATS also provides [encryption at rest](/running-a-nats-service/nats_admin/jetstream_admin/encryption_at_rest.md).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/security.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
