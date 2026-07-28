---
title: "Queueing Walkthrough"
source: "https://docs.nats.io/nats-concepts/core-nats/queue/queues_walkthrough.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/core-nats/queue/queues_walkthrough"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Queueing Walkthrough"
---
# Queueing Walkthrough

NATS supports a form of load balancing using [queue groups](/nats-concepts/core-nats/queue.md). Subscribers register a queue group name. A single subscriber in the group is randomly selected to receive the message.

## Walkthrough prerequisites

If you have not already done so, you need to [install](/nats-concepts/what-is-nats/walkthrough_setup.md) the `nats` CLI tool and optionally the nats-server on your machine.

### 1. Start the first member of the queue group

The `nats reply` instances don't just subscribe to the subject but also automatically join a queue group (`"NATS-RPLY-22"` by default)

```bash
nats reply foo "service instance A Reply# {{Count}}"
```

### 2. Start a second member of the queue group

In a new window

```bash
nats reply foo "service instance B Reply# {{Count}}"
```

### 3. Start a third member of the queue group

In a new window

```bash
nats reply foo "service instance C Reply# {{Count}}"
```

### 4. Publish a NATS message

```bash
nats request foo "Simple request"
```

### 5. Verify message publication and receipt

You should see that only one of the my-queue group subscribers receives the message and replies to it, and you can also see which one of the available queue group subscribers processed the request from the reply message received (i.e. service instance A, B or C)

### 6. Publish another message

```bash
nats request foo "Another simple request"
```

You should see that a different queue group subscriber receives the message this time, chosen at random among the 3 queue group members.

You can also send any number of requests back-to-back. From the received messages, you'll see the distribution of those requests amongst the members of the queue group. For example: `nats request foo --count 10 "Request {{Count}}"`

### 7. Stop/start queue group members

You can at any time start yet another service instance, or stop one and see how the queue group automatically takes care of adding/removing those instances from the group.

## See Also

Queue groups using the NATS CLI

{% embed url="<https://youtu.be/jLTVhP08Tq0?t=101>" %}
Queue Groups NATS CLI
{% endembed %}


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/core-nats/queue/queues_walkthrough.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
