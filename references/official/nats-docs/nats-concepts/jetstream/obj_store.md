---
title: "Object Store"
source: "https://docs.nats.io/nats-concepts/jetstream/obj_store.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/jetstream/obj_store"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Object Store"
---
# Object Store

JetStream, the persistence layer of NATS, not only allows for the higher qualities of service and features associated with 'streaming', but it also enables some functionalities not found in messaging systems.

One such feature is the Object store functionality, which allows client applications to create `buckets` (corresponding to streams) that can store a set of files. Files are stored and transmitted in chunks, allowing files of arbitrary size to be transferred safely over the NATS infrastructure.

**Note:** Object store is not a distributed storage system. All files in a bucket will need to fit on the target file system.

* [Walkthrough](/nats-concepts/jetstream/obj_store/obj_walkthrough.md)
* [Details](/using-nats/developer/develop_jetstream/object.md)

## Basic Capabilities

The Object Store implements a chunking mechanism, allowing you to for example store and retrieve files (i.e. the object) of any size by associating them with a path or file name as the key.

* `add` a `bucket` to hold the files.
* `put` Add a file to the bucket
* `get` Retrieve the file and store it to a designated location
* `del` Delete a file

## Advanced Capabilities

* `watch` Subscribe to changes in the bucket. Will receive notifications on successful `put` and `del` operations.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/jetstream/obj_store.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
