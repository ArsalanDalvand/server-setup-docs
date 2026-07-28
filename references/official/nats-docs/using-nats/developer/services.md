---
title: "Building Services"
source: "https://docs.nats.io/using-nats/developer/services.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/services"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Building Services"
---
# Building Services

Recently we have agreed upon an [initial specification](https://github.com/nats-io/nats-architecture-and-design/blob/main/adr/ADR-32.md) for a services protocol so that we can add first-class services support to NATS clients and support this in our tooling. This services protocol is an agreement between clients and tooling and doesn't require any special functionality from the NATS server or JetStream.

To check if the NATS client in your favorite language supports the new services API, make sure you check the docs and GitHub repository for that client. The services API is relatively new and not all clients may support it yet.

To see the services API in action in different languages, take a look at the [NATS By Example](https://natsbyexample.com/examples/services/intro/go) samples.

## Concepts

There are a few high level concepts in the services API worth understanding before you start developing your own services.

### Service

The service is the highest level abstraction and refers to a group of logically related functionality. Services are required to have names and versions that conform to the [semver](https://semver.org/#is-there-a-suggested-regular-expression-regex-to-check-a-semver-string) rules. Services are discoverable within a NATS system.

### Endpoint

A service endpoint is the entity with which clients interact. You can think of an endpoint as a single operation within a service. All services must have at least 1 endpoint.

### Group

A group is a collection of endpoints. These are optional and can provide a logical association between endpoints as well as an optional common subject prefix for all endpoints.

## Service Operations

The services API supports 3 operations for discoverability and observability. While the NATS client will take care of responding on these subjects, it is still the developer's responsibility to respond to requests made the service's actual endpoints.

* `PING` - Requests made on the `$SRV.PING.>` subject gather replies from running services. This facilitates service listing by tooling.
* `STATS` - Requests made on the `$SRV.STATS.>` subject query statistics from services. Available stats include total requests, total errors, and total processing time.
* `INFO` - Requests made on the `$SRV.INFO.>` subject obtain the service definition and metadata, including groups, endpoints, etc.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/developer/services.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
