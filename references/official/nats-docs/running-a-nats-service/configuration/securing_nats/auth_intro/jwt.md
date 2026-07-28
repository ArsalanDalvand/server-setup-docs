---
title: "Decentralized JWT Authentication/Authorization"
source: "https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/jwt.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/securing_nats/auth_intro/jwt"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Decentralized JWT Authentication/Authorization"
---
# Decentralized JWT Authentication/Authorization

## Decentralized JWT Authentication/Authorization

With other authentication mechanisms, configuration for identifying a user and [Account](/running-a-nats-service/configuration/securing_nats/accounts.md), is in the server configuration file. JWT authentication leverages [JSON Web Tokens (JWT)](https://jwt.io/) to describe the various entities supported. When a client connects, servers verify the authenticity of the request using [NKeys](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md), download account information and validate a trust chain. Users are not directly tracked by the server, but rather verified as and belonging to an [Account](/running-a-nats-service/configuration/securing_nats/accounts.md). This enables the management of users, without requiring server configuration updates.

Effectively, JWTs improve accounts and provide for a **distributed configuration paradigm**. Previously each user (or client) needed to be known and authorized a priori in the server’s configuration requiring an administrator to modify and update server configurations. These chores are eliminated. User creation can even be performed by different entities altogether.

> Note: This scheme improves [accounts](/running-a-nats-service/configuration/securing_nats/accounts.md). Functionalities like [isolation](/running-a-nats-service/configuration/securing_nats/accounts.md) or defining [exports/imports](/running-a-nats-service/configuration/securing_nats/accounts.md#exporting-and-importing) between accounts remain! It moves configuration of accounts, exports/imports or users and their permissions away from the server into several trusted [JSON Web Token (JWT)](https://jwt.io/) that are managed separately, therefore removing the need to configure these entities in each and every server. It furthermore adds functionalities like expiration and revocation fore decentralized account management

### JSON Web Tokens

[JSON Web Tokens (JWT)](https://jwt.io/) are an open and industry standard [RFC7519](https://tools.ietf.org/html/rfc7519) method for representing claims securely between two parties.

Claims are a fancy way of asserting information on a *subject*. In this context, a *subject* is the entity being described (not a messaging subject). Standard JWT claims are typically digitally signed and verified.

NATS further restricts JWTs by requiring that JWTs be:

* Digitally signed *always* and only using [Ed25519](https://ed25519.cr.yp.to/).
* NATS adopts the convention that all *Issuer* and *Subject* fields in a JWT claim must be a public [NKEY](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md).
* *Issuer* and *Subject* must match specific roles depending on the claim [NKeys](https://github.com/nats-io/nkeys).

#### NKey Roles

[NKeys](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md) Roles are:

* Operators
* Accounts
* Users

Roles are hierarchical and form a chain of trust. Operators issue Accounts which in turn issue Users. Servers trust specific Operators. If an account is issued by an operator that is trusted, account users are trusted.

### The Authentication Process

When a *User* connects to a server, it presents a JWT issued by its *Account*. The user proves its identity by signing a server-issued cryptographic challenge with its private key. The signature verification validates that the signature is attributable to the user's public key. Next, the server retrieves the associated account JWT that issued the user. It verifies the *User* issuer matches the referenced account. Finally, the server checks that a trusted *Operator* - one the server is configured with - issued the *Account*, completing the trust chain verification.

### The Authorization Process

From an authorization point of view, the account provides information on messaging subjects that are imported from other accounts (including any ancillary related authorization) as well as messaging subjects exported to other accounts. Accounts can also bear limits, such as the maximum number of connections they may have. A user JWT can express restrictions on the messaging subjects to which it can publish or subscribe.

When a new user is added to an account, the account configuration need not change, as each user can and should have its own user JWT that can be verified by simply resolving its parent account.

### JWTs and Privacy

One crucial detail to keep in mind is that while in other systems JWTs are used as sessions or proof of authentication, NATS JWTs are only used as configuration describing:

* the public ID of the entity
* the public ID of the entity that issued it
* capabilities of the entity

Authentication is a public key cryptographic process — a client signs a nonce proving identity while the trust chain and configuration provides the authorization.

The server is never aware of private keys but can verify that a signer or issuer indeed matches a specified or known public key.

Lastly, all NATS JWTs (Operators, Accounts, Users and others) are expected to be signed using the [Ed25519](https://ed25519.cr.yp.to/) algorithm. If they are not, they are rejected by the system.

### Decentralized Authentication and Authorization - Configuration and nsc

There is very little to configure on the nats-server to enable operator JWT security, once the servers have been initially configured the authentication and authorization tasks are typically done by using the `nsc` administration tool locally and synchronizing with the account resolvers built into the nats-server.

Configuration is broken up into separate steps. Depending on organizational needs these are performed by the same or different entities.

Practically, JWT configuration is done using the [`nsc` tool](/using-nats/nats-tools/nsc.md). It can be set up to issue [NKeys](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md) and corresponding JWTs for all [nkey roles](#nkey-roles): Operator/Account/User ([Example usage](/using-nats/nats-tools/nsc/basics.md#creating-an-operator-account-and-user)). Despite Account and User creation not happening in server configuration, this model is a centralized authentication and authorization setup.

Provided institutional trust, it is also possible to use nsc to import account or user public [NKeys](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md) and issue corresponding JWTs. This way an operator can issue account JWTs and a separate entity can issue JWTs for user associated with it's account. Neither entity has to be aware of the other's private Nkey. This not only allows users to be configured some place other than servers, but also by different organizations altogether. Say administrators of a NATS installation controlling operators, issuing account JWTs to individual prod/dev teams managing their own user. This is a fully decentralized authorization setup!

With an Operator JWT in place, the server needs to be configured to trust it by specifying `operator`. Furthermore the server needs a way to obtain account JWTs. This done by either defaulting to the resolver specified in the operator jwt or by manually specifying the [resolver](/running-a-nats-service/configuration/securing_nats/auth_intro/jwt/resolver.md). Depending on your configuration an [account server](/using-nats/nats-tools/nsc/basics.md#account-server-configuration) needs to be in place

> It is possible to [mix](/running-a-nats-service/configuration/securing_nats/auth_intro/jwt/jwt_nkey_auth.md) JWT and [NKEY](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md)/[Account](/running-a-nats-service/configuration/securing_nats/accounts.md) based Authentication/Authorization.

## Managing JWT authentication

A lot more information is available in the [In Depth Guide](/running-a-nats-service/nats_admin/security/jwt.md).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/jwt.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
