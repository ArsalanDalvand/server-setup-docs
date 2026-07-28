---
title: "nk"
source: "https://docs.nats.io/using-nats/nats-tools/nk.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/nats-tools/nk"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "nk"
---
# nk

`nk` is a command line tool that generates `nkeys`. NKeys are a highly secure public-key signature system based on [Ed25519](https://ed25519.cr.yp.to/).

With NKeys the server can verify identity without ever storing secrets on the server. The authentication system works by requiring a connecting client to provide its public key and digitally sign a challenge with its private key. The server generates a random challenge with every connection request, making it immune to playback attacks. The generated signature is validated a public key, thus proving the identity of the client. If the public key validation succeeds, authentication succeeds.

> NKey is an awesome replacement for token authentication, because a connecting client will have to prove it controls the private key for the authorized public key.

## Installing nk

To get started with NKeys, you’ll need the `nk` tool from <https://github.com/nats-io/nkeys/tree/master/nk> repository. If you have *go* installed, enter the following at a command prompt:

```bash
go install github.com/nats-io/nkeys/nk@latest
```

## Generating NKeys and Configuring the Server

To generate a *User* NKEY:

```shell
nk -gen user -pubout
```

```
SUACSSL3UAHUDXKFSNVUZRF5UHPMWZ6BFDTJ7M6USDXIEDNPPQYYYCU3VY
UDXU4RCSJNZOIQHZNWXHXORDPRTGNJAHAHFRGZNEEJCPQTT2M7NLCNF4
```

The first output line starts with the letter `S` for *Seed*. The second letter `U` stands for *User*. Seeds are private keys; you should treat them as secrets and guard them with care.

The second line starts with the letter `U` for *User*, and is a public key which can be safely shared.

To use `nkey` authentication, add a user, and set the `nkey` property to the public key of the user you want to authenticate. You are only required to use the public key and no other properties are required. Here is a snippet of configuration for the `nats-server`:

```
authorization: {
  users: [
    { nkey: UDXU4RCSJNZOIQHZNWXHXORDPRTGNJAHAHFRGZNEEJCPQTT2M7NLCNF4 }
  ]
}
```

To complete the end-to-end configuration and use an `nkey`, the [[running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth#client-configuration|client is configured]] to use the seed, which is the private key.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/nats-tools/nk.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
