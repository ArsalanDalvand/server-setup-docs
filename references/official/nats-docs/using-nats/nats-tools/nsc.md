---
title: "nsc"
source: "https://docs.nats.io/using-nats/nats-tools/nsc.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/nats-tools/nsc"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "nsc"
---
# nsc

NATS account configurations are built using the `nsc` tool. The NSC tool allows you to:

* Create and edit Operators, Accounts, Users
* Manage publish and subscribe permissions for Users
* Define Service and Stream exports from an account
* Reference Service and Streams from another account
* Generate Activation tokens that grants access to a private service or stream
* Generate User credential files
* Describe Operators, Accounts, Users, and Activations
* Push and pull account JWTs to an account JWTs server

## Installation

Installing `nsc` is easy:

```shell
curl -L https://raw.githubusercontent.com/nats-io/nsc/master/install.py | python
```

> Additional ways of installing nsc are described at [nsc's github repository](https://github.com/nats-io/nsc#install)

The script will download the latest version of `nsc` and install it into your system.

In case NSC is not initialized already do `nsc init`

Output of `tree -L 2 nsc/`

```
nsc/
├── accounts
│   ├── nats
│   └── nsc.json
└── nkeys
    ├── creds
    └── keys
5 directories, 1 file
```

**IMPORTANT**: `nsc` version 2.2.0 has been released. This version of nsc only supports `nats-server` v2.2.0 and `nats-account-server` v1.0.0. For more information please refer to the [nsc 2.2.0 release notes](https://github.com/nats-io/nsc/releases/tag/2.2.0).

## Tutorials

You can find various task-oriented tutorials to working with the tool here:

* [Basic Usage](/using-nats/nats-tools/nsc/basics.md)
* [Configuring Account Streams Import/Export](/using-nats/nats-tools/nsc/streams.md)
* [Configuring Account Services Import/Export](/using-nats/nats-tools/nsc/services.md)
* [Signing Keys](/using-nats/nats-tools/nsc/signing_keys.md)
* [Revoking Users or Activations](/using-nats/nats-tools/nsc/revocation.md)
* [Working with Managed Operators](/using-nats/nats-tools/nsc/managed.md)

## Tool Documentation

For more specific browsing of the tool syntax, check out the `nsc` tool documentation. It can be found within the tool itself:

```shell
nsc help
```

Or an online version [here](https://nats-io.github.io/nsc).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/nats-tools/nsc.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
