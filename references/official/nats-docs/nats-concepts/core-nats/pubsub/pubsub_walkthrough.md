---
title: "Pub/Sub Walkthrough"
source: "https://docs.nats.io/nats-concepts/core-nats/pubsub/pubsub_walkthrough.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/core-nats/pubsub/pubsub_walkthrough"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Pub/Sub Walkthrough"
---
# Pub/Sub Walkthrough

NATS is a [publish subscribe](/nats-concepts/core-nats/pubsub.md) messaging system [based on subjects](/nats-concepts/subjects.md). Subscribers listening on a subject receive messages published on that subject. If the subscriber is not actively listening on the subject, the message is not received. Subscribers can use the wildcard tokens such as `*` and `>` to match a single token or to match the tail of a subject.

## NATS Pub/Sub Walkthrough

This simple walkthrough demonstrates some ways in which subscribers listen on subjects, and publishers send messages on specific subjects.

![](/files/-LqMZgSl7CBR9-ZIe5Jh)

### Walkthrough prerequisites

If you have not already done so, you need to [install](/nats-concepts/what-is-nats/walkthrough_setup.md) the `nats` CLI Tool and optionally the nats-server on your machine.

#### 1. Create Subscriber 1

In a shell or command prompt session, start a client subscriber program.

```bash
nats sub <subject>
```

Here, `<subject>` is a subject to listen on. It helps to use unique and well thought-through subject strings because you need to ensure that messages reach the correct subscribers even when wildcards are used.

For example:

```bash
nats sub msg.test
```

You should see the message: *Listening on \[msg.test]*

#### 2. Create a Publisher and publish a message

In another shell or command prompt, create a NATS publisher and send a message.

```bash
nats pub <subject> <message>
```

Where `<subject>` is the subject name and `<message>` is the text to publish.

For example:

```bash
nats pub msg.test "NATS MESSAGE"
```

#### 3. Verify message publication and receipt

You'll notice that the publisher sends the message and prints: *Published \[msg.test] : 'NATS MESSAGE'*.

The subscriber receives the message and prints: *\[#1] Received on \[msg.test]: 'NATS MESSAGE'*.

If the receiver does not get the message, you'll need to check if you are using the same subject name for the publisher and the subscriber.

#### 4. Try publishing another message

```bash
nats pub msg.test "NATS MESSAGE 2"
```

You'll notice that the subscriber receives the message.\
Note that a message count is incremented each time your subscribing client receives a message on that subject.

#### 5. Create Subscriber 2

In a new shell or command prompt, start a new NATS subscriber.

```bash
nats sub msg.test
```

#### 6. Publish another message using the publisher client

```bash
nats pub msg.test "NATS MESSAGE 3"
```

Verify that both subscribing clients receive the message.

#### 7. Create Subscriber 3

In a new shell or command prompt session, create a new subscriber that listens on a different subject.

```bash
nats sub msg.test.new
```

#### 8. Publish another message

```bash
nats pub msg.test "NATS MESSAGE 4"
```

Subscriber 1 and Subscriber 2 receive the message, but Subscriber 3 does not. Why? Because Subscriber 3 is not listening on the message subject used by the publisher.

#### 9. Alter Subscriber 3 to use a wildcard

Change the last subscriber to listen on msg.\* and run it:

```bash
nats sub msg.*
```

Note: NATS supports the use of wildcard characters for message subscribers only. You cannot publish a message using a wildcard subject.

#### 10. Publish another message

```bash
nats pub msg.test "NATS MESSAGE 5"
```

This time, all three subscribing clients should receive the message.

Do try out a few more variations of substrings and wildcards to test your understanding.

## See Also

Publish-subscribe pattern with the NATS CLI

{% embed url="<https://www.youtube.com/watch?v=jLTVhP08Tq0>" %}
Publish-subscribe pattern - NATS CLI
{% endembed %}


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/core-nats/pubsub/pubsub_walkthrough.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
