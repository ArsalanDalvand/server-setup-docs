---
title: "Authentication"
source: "https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/securing_nats/auth_intro"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Authentication"
---
# Authentication

NATS authentication is multi-level. All of the security modes have an [*accounts*](/running-a-nats-service/configuration/securing_nats/auth_intro.md) level with [*users*](#user-configuration-map) belonging to those accounts. The decentralized JWT Authentication also has an *operator* to which the accounts belong.

Each account has its own independent subject namespace: a message published on subject 'foo' in one account will not be seen by subscribers to 'foo' in other accounts. Accounts can however define exports and imports of subject(s) streams as well as expose request-reply services between accounts. Users within an account will share the same subject namespace but can be restricted to only be able to publish-subscribe to specific subjects.

## Authentication Methods

The NATS server provides various ways of authenticating clients:

* [Token Authentication](/running-a-nats-service/configuration/securing_nats/auth_intro/tokens.md)
* [Plain Text Username/Password credentials](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md#plain-text-passwords)
* [Bcrypted Username/Password credentials](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md#bcrypted-passwords)
* [TLS Certificate](/running-a-nats-service/configuration/securing_nats/auth_intro/tls_mutual_auth.md)
* [NKEY with Challenge](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md)
* [Decentralized JWT Authentication/Authorization](/running-a-nats-service/configuration/securing_nats/auth_intro/jwt.md)

Authentication deals with allowing a NATS client to connect to the server. Except for JWT authentication, authentication and authorization are configured in the `authorization` section of the configuration. With JWT authentication the account and user information are stored in the [resolver](/running-a-nats-service/configuration/securing_nats/auth_intro/jwt/resolver.md) rather than in the server configuration file.

## Authorization Map

The `authorization` block provides *authentication* configuration as well as [*authorization*](/running-a-nats-service/configuration/securing_nats/authorization.md):

| Property                                                                                                      | Description                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| [`token`](/running-a-nats-service/configuration/securing_nats/auth_intro/tokens.md)                           | Specifies a global token that can be used to authenticate to the server (exclusive of user and password)                              |
| [`user`](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md#single-user)     | Specifies a single *global* user name for clients to the server (exclusive of token)                                                  |
| [`password`](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md)             | Specifies a single *global* password for clients to the server (exclusive of `token`)                                                 |
| [`users`](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md#multiple-users) | A list of [user configuration](#user-configuration-map) maps. For multiple username and password credentials, specify a `users` list. |
| [`timeout`](/running-a-nats-service/configuration/securing_nats/auth_intro/auth_timeout.md)                   | Maximum number of seconds to wait for client authentication                                                                           |
| [`auth_callout`](/running-a-nats-service/configuration/securing_nats/auth_callout.md)                         | Enables the auth callout extension                                                                                                    |

## User Configuration Map

A `user` configuration map specifies credentials and permissions options for a single user:

| Property                                                                                          | Description                                                                                                                                                                                                |
| ------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`user`](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md)     | username for client authentication. (Can also be a user for [tls authentication](/running-a-nats-service/configuration/securing_nats/auth_intro/tls_mutual_auth.md#mapping-client-certificates-to-a-user)) |
| [`password`](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md) | password for the user entry                                                                                                                                                                                |
| [`nkey`](/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md)             | public nkey identifying an user                                                                                                                                                                            |
| [`permissions`](/running-a-nats-service/configuration/securing_nats/authorization.md)             | permissions map configuring subjects accessible to the user                                                                                                                                                |


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
