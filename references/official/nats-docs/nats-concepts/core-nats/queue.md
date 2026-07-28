---
title: "Queue Groups"
source: "https://docs.nats.io/nats-concepts/core-nats/queue.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/core-nats/queue"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Queue Groups"
---
# Queue Groups

When subscribers register themselves to receive messages from a publisher, the 1:N fan-out pattern of messaging ensures that any message sent by a publisher, reaches all subscribers that have registered. NATS provides an additional feature named "queue", which allows subscribers to register themselves as part of a queue. Subscribers that are part of a queue, form the "queue group".

## How queue groups function

As an example, consider message delivery occurring in the 1:N pattern to all subscribers based on the subject name (delivery happens even to subscribers that are not part of a queue group). If a subscriber is registered based on a queue name, it will always receive messages it is subscribed to, based on the subject name. However, if more subscribers are added to the same queue name, they become a queue group, and only one randomly chosen subscriber of the queue group will consume a message each time a message is received by the queue group. Such distributed queues are a built-in load balancing feature that NATS provides.

**Advantages**

* Ensures application fault tolerance
* Workload processing can be scaled up or down
* Scale your consumers up or down without duplicate messages
* No extra configuration required
* Queue groups are defined by the application and their queue subscribers, rather than the server configuration

Queue group names follow the same naming rules as [subjects](/nats-concepts/subjects.md). Foremost, they are case sensitive and cannot contain whitespace. Consider structuring queue groups hierarchically using a period `.`. Some server functionalities like [queue permissions](/running-a-nats-service/configuration/securing_nats/authorization.md#queue-permissions) can use [wildcard matching](/nats-concepts/subjects.md#wildcards) on them.

Queue subscribers are ideal for scaling services. Scale up is as simple as running another application, scale down is terminating the application with a signal that drains the in flight requests. This flexibility and lack of any configuration changes makes NATS an excellent service communication technology that can work with all platform technologies.

### No responder

When a request is made to a service (request/reply) and the NATS Server knows there are no services available (since there are no client applications currently subscribing to the subject in a queue-group) the server will send a “no-responders” protocol message back to the requesting client which will break from blocking API calls. This allows applications to react immediately. This further enables building a highly responsive system at scale, even in the face of application failures and network partitions.

## Stream as a queue

With [JetStream](/nats-concepts/jetstream.md) a stream can also be used as a queue by setting the retention policy to `WorkQueuePolicy` and leveraging [`pull` consumers](/nats-concepts/jetstream/consumers.md) to get easy horizontal scalability of the processing (or using an explicit ack push consumer with a queue group of subscribers).

![](/files/-LqMZeqHRi1BPHIRKAMs)

### Queuing geo-affinity

When connecting to a globally distributed NATS super-cluster, there is an automatic service geo-affinity due to the fact that a service request message will only be routed to another cluster (i.e. another region) if there are no listeners on the cluster available to handle the request locally.

### Tutorial

Try NATS queue subscriptions on your own, using a live server by walking through the [queueing walkthrough](/nats-concepts/core-nats/queue/queues_walkthrough.md).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/core-nats/queue.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
