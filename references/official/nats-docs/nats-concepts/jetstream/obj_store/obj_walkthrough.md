---
title: "Object Store Walkthrough"
source: "https://docs.nats.io/nats-concepts/jetstream/obj_store/obj_walkthrough.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/jetstream/obj_store/obj_walkthrough"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Object Store Walkthrough"
---
# Object Store Walkthrough

If you are running a local `nats-server` stop it and restart it with JetStream enabled using `nats-server -js` (if that's not already done)

You can then check that JetStream is enabled by using

```shell
nats account info
```

Which should output something like:

```
Connection Information:

               Client ID: 6
               Client IP: 127.0.0.1
                     RTT: 64.996µs
       Headers Supported: true
         Maximum Payload: 1.0 MiB
           Connected URL: nats://127.0.0.1:4222
       Connected Address: 127.0.0.1:4222
     Connected Server ID: ND2XVDA4Q363JOIFKJTPZW3ZKZCANH7NJI4EJMFSSPTRXDBFG4M4C34K

JetStream Account Information:

           Memory: 0 B of Unlimited
          Storage: 0 B of Unlimited
          Streams: 0 of Unlimited
        Consumers: 0 of Unlimited
```

If you see the below instead then JetStream is *not* enabled

```
JetStream Account Information:

   JetStream is not supported in this account
```

## Creating an Object Store bucket

Just like you need to create streams before you can use them you need to first create an Object Store bucket

```shell
nats object add myobjbucket
```

which outputs

```
myobjbucket Object Store Status

         Bucket Name: myobjbucket
            Replicas: 1
                 TTL: unlimitd
              Sealed: false
                Size: 0 B
  Backing Store Kind: JetStream
    JetStream Stream: OBJ_myobjbucket
```

## Putting a file in the bucket

```shell
nats object put myobjbucket ~/Movies/NATS-logo.mov
```

```
1.5 GiB / 1.5 GiB [====================================================================================]

Object information for myobjbucket > /Users/jnmoyne/Movies/NATS-logo.mov

               Size: 1.5 GiB
  Modification Time: 14 Apr 22 00:34 +0000
             Chunks: 12,656
             Digest: sha-256 8ee0679dd1462de393d81a3032d71f43d2bc89c0c8a557687cfe2787e926
```

## Putting a file in the bucket by providing a name

By default the full file path is used as a key. Provide the key explicitly (e.g. a relative path ) with `--name`

```shell
nats object put --name /Movies/NATS-logo.mov myobjbucket ~/Movies/NATS-logo.mov
```

```
1.5 GiB / 1.5 GiB [====================================================================================]

Object information for myobjbucket > /Movies/NATS-logo.mov

               Size: 1.5 GiB
  Modification Time: 14 Apr 22 00:34 +0000
             Chunks: 12,656
             Digest: sha-256 8ee0679dd1462de393d81a3032d71f43d2bc89c0c8a557687cfe2787e926
```

## Listing the objects in a bucket

```shell
nats object ls myobjbucket
```

```
╭───────────────────────────────────────────────────────────────────────────╮
│                              Bucket Contents                              │
├─────────────────────────────────────┬─────────┬───────────────────────────┤
│ Name                                │ Size    │ Time                      │
├─────────────────────────────────────┼─────────┼───────────────────────────┤
│ /Users/jnmoyne/Movies/NATS-logo.mov │ 1.5 GiB │ 2022-04-13T17:34:55-07:00 │
│ /Movies/NATS-logo.mov               │ 1.5 GiB │ 2022-04-13T17:35:41-07:00 │
╰─────────────────────────────────────┴─────────┴───────────────────────────╯
```

## Getting an object from the bucket

```shell
nats object get myobjbucket ~/Movies/NATS-logo.mov
```

```
1.5 GiB / 1.5 GiB [====================================================================================]

Wrote: 1.5 GiB to /Users/jnmoyne/NATS-logo.mov in 5.68s average 279 MiB/s
```

## Getting an object from the bucket with a specific output path

By default, the file will be stored relative to the local path under its name (not the full path). To specify an output path use `--output`

```shell
nats object get myobjbucket --output /temp/Movies/NATS-logo.mov /Movies/NATS-logo.mov
```

```
1.5 GiB / 1.5 GiB [====================================================================================]

Wrote: 1.5 GiB to /temp/Movies/NATS-logo.mov in 5.68s average 279 MiB/s
```

## Removing an object from the bucket

```shell
nats object rm myobjbucket ~/Movies/NATS-logo.mov
```

```
? Delete 1.5 GiB byte file myobjbucket > /Users/jnmoyne/Movies/NATS-logo.mov? Yes
Removed myobjbucket > /Users/jnmoyne/Movies/NATS-logo.mov
myobjbucket Object Store Status

         Bucket Name: myobjbucket
            Replicas: 1
                 TTL: unlimitd
              Sealed: false
                Size: 16 MiB
  Backing Store Kind: JetStream
    JetStream Stream: OBJ_myobjbucket
```

## Getting information about the bucket

```shell
nats object info myobjbucket
```

```
myobjbucket Object Store Status

         Bucket Name: myobjbucket
            Replicas: 1
                 TTL: unlimitd
              Sealed: false
                Size: 1.6 GiB
  Backing Store Kind: JetStream
    JetStream Stream: OBJ_myobjbucket
```

## Watching for changes to a bucket

```shell
nats object watch myobjbucket
```

```
[2022-04-13 17:51:28] PUT myobjbucket > /Users/jnmoyne/Movies/NATS-logo.mov: 1.5 GiB bytes in 12,656 chunks
[2022-04-13 17:53:27] DEL myobjbucket > /Users/jnmoyne/Movies/NATS-logo.mov
```

### Sealing a bucket

You can seal a bucket, meaning that no further changes are allowed on that bucket

```shell
nats object seal myobjbucket
```

```
? Really seal Bucket myobjbucket, sealed buckets can not be unsealed or modified Yes
myobjbucket has been sealed
myobjbucket Object Store Status

         Bucket Name: myobjbucket
            Replicas: 1
                 TTL: unlimitd
              Sealed: true
                Size: 1.6 GiB
  Backing Store Kind: JetStream
    JetStream Stream: OBJ_myobjbucket
```

## Deleting a bucket

Using `nats object rm myobjbucket` will delete the bucket and all the files stored in it.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/jetstream/obj_store/obj_walkthrough.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
