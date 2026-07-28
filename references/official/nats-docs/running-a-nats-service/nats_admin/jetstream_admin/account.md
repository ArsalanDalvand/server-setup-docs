---
title: "Account Information"
source: "https://docs.nats.io/running-a-nats-service/nats_admin/jetstream_admin/account.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin/jetstream_admin/account"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Account Information"
---
# Account Information

## Account Information

JetStream is multi-tenant so you will need to check that your account is enabled for JetStream and is not limited. You can view your limits as follows:

```shell
nats account info
```

```
Connection Information:
               Client ID: 8
               Client IP: 127.0.0.1
                     RTT: 178.545µs
       Headers Supported: true
         Maximum Payload: 1.0 MiB
           Connected URL: nats://localhost:4222
       Connected Address: 127.0.0.1:4222
     Connected Server ID: NCCOHA6ONXJOGAEZP4WPU4UJ3IQP2VVXEPRKTQCGBCW4IL4YYW4V4KKL
JetStream Account Information:
           Memory: 0 B of 5.7 GiB
          Storage: 0 B of 11 GiB
          Streams: 0 of Unlimited
   Max Consumers: unlimited
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin/jetstream_admin/account.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
