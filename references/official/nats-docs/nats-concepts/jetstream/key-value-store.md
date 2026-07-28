---
title: "Key/Value Store"
source: "https://docs.nats.io/nats-concepts/jetstream/key-value-store.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/jetstream/key-value-store"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Key/Value Store"
---
# Key/Value Store

JetStream, the persistence layer of NATS, not only allows for the higher qualities of service and features associated with 'streaming', but it also enables some functionalities not found in messaging systems.

One such feature is the Key/Value store functionality, which allows client applications to create `buckets` and use them as immediately (as opposed to eventually) consistent, persistent [associative arrays](https://en.wikipedia.org/wiki/Associative_array) (or maps). Note that this is an abstraction on top of the Stream functionality. Buckets are materialized as Streams (with a name starting with `KV_`), everything you can do with a bucket you can do with a Stream, but you ultimately have more functionality and flexibility and control when using the Stream functionality directly.

Do note, while we do guarantee immediate consistency when it comes to [monotonic writes](https://jepsen.io/consistency/models/monotonic-writes) and [monotonic reads](https://jepsen.io/consistency/models/monotonic-reads). We don't guarantee [read your writes](https://jepsen.io/consistency/models/read-your-writes) at this time, as reads through *direct get* requests may be served by followers or mirrors. More consistent results can be achieved by sending get requests to the underlying stream leader of the Key/Value store.

* [Walkthrough](/nats-concepts/jetstream/key-value-store/kv_walkthrough.md)
* [Details](/using-nats/developer/develop_jetstream/kv.md)

## Managing a Key Value store

1. Create a bucket, which corresponds to a stream in the underlying storage. Define KV/Stream limits as appropriate
2. Use the operation below.

## Map style operations

You can use KV buckets to perform the typical operations you would expect from an immediately consistent key/value store:

* put: associate a value with a key
* get: retrieve the value associated with a key
* delete: clear any value associated with a key
* purge: clear all the values associated with all keys
* keys: get a copy of all of the keys (with a value or operation associated with it)

## Atomic operations used for locking and concurrency control

* create: associate the value with a key only if there is currently no value associated with that key (i.e. compare to null and set)
* update: compare and set (aka compare and swap) the value for a key

## Limiting size, TTL etc.

You can set limits for your buckets, such as:

* the maximum size of the bucket
* the maximum size for any single value
* a TTL: how long the store will keep values for

## Treating the Key Value store as a message stream

Finally, you can even do things that typically can not be done with a Key/Value Store:

* watch: watch for changes happening for a key, which is similar to subscribing (in the publish/subscribe sense) to the key: the watcher receives updates due to put or delete operations on the key pushed to it in real-time as they happen
* watch all: watch for all the changes happening on all the keys in the bucket
* history: retrieve a history of the values (and delete operations) associated with each key over time (by default the history of buckets is set to 1, meaning that only the latest value/operation is stored)

## Notes

A valid key can contain the following characters: `a-z`, `A-Z`, `0-9`, `_`, `-`, `.`, `=` and `/`, i.e. it can be a dot-separated list of tokens (which means that you can then use wildcards to match hierarchies of keys when watching a bucket). The value can be any byte array.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/jetstream/key-value-store.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
