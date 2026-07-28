---
title: "NKeys"
source: "https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "NKeys"
---
# NKeys

NKeys are a new, highly secure public-key signature system based on [Ed25519](https://ed25519.cr.yp.to/).

With NKeys the server can verify identities without ever storing or ever seeing private keys. The authentication system works by requiring a connecting client to provide its public key and digitally sign a challenge with its private key. The server generates a random challenge with every connection request, making it immune to playback attacks. The generated signature is validated against the provided public key, thus proving the identity of the client. If the public key is known to the server, authentication succeeds.

> NKey is an excellent replacement for token authentication because a connecting client will have to prove it controls the private key for the authorized public key.

To generate nkeys, you'll need the [`nk` tool](/using-nats/nats-tools/nk.md).

## Generating NKeys and Configuring the Server

To generate a *User* NKEY:

```shell
nk -gen user -pubout
```

```
SUACSSL3UAHUDXKFSNVUZRF5UHPMWZ6BFDTJ7M6USDXIEDNPPQYYYCU3VY
UDXU4RCSJNZOIQHZNWXHXORDPRTGNJAHAHFRGZNEEJCPQTT2M7NLCNF4
```

The first output line starts with the letter `S` for *Seed*. The second letter, `U` stands for *User*. Seeds are private keys; you should treat them as secrets and guard them with care.

The second line starts with the letter `U` for *User* and is a public key which can be safely shared.

To use nkey authentication, add a user, and set the `nkey` property to the public key of the user you want to authenticate:

```
authorization: {
  users: [
    { nkey: UDXU4RCSJNZOIQHZNWXHXORDPRTGNJAHAHFRGZNEEJCPQTT2M7NLCNF4 }
  ]
}
```

Note that the user section sets the `nkey` property (user/password/token properties are not needed). Add `permission` sections as required.

## Client Configuration

Now that you have a user nkey, let's configure a [client](/using-nats/developer/connecting/nkey.md) to use it for authentication. As an example, here are the connect options for the node client:

```javascript
const NATS = require('nats');
const nkeys = require('ts-nkeys');

const nkey_seed = ‘SUACSSL3UAHUDXKFSNVUZRF5UHPMWZ6BFDTJ7M6USDXIEDNPPQYYYCU3VY’;
const nc = NATS.connect({
  port: PORT,
  nkey: 'UDXU4RCSJNZOIQHZNWXHXORDPRTGNJAHAHFRGZNEEJCPQTT2M7NLCNF4',
  sigCB: function (nonce) {
    // client loads seed safely from a file
    // or some constant like `nkey_seed` defined in
    // the program
    const sk = nkeys.fromSeed(Buffer.from(nkey_seed));
    return sk.sign(nonce);
   }
});
...
```

The client provides a function that it uses to parse the seed (the private key) and sign the connection challenge.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/nkey_auth.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
