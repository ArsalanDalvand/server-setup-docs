---
title: "Installing, running and deploying a NATS Server"
source: "https://docs.nats.io/running-a-nats-service/introduction.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/introduction"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Installing, running and deploying a NATS Server"
---
# Installing, running and deploying a NATS Server

The NATS Server is highly optimized and its binary is very compact (less than 20 MB), it can run on any machine from a lowly Raspberry Pi to the largest of servers, in the cloud, on premise or at the edge, on bare metal, on VMs or in containers.

NATS Servers can cluster together to provide fault-tolerance and scalability. NATS servers can run as Leaf Nodes connecting to a cluster transparently proxying and expanding NATS and JetStream service to any location, be it a VPC, a VLAN, a remote office, a single machine at home or even an edge location with partial connectivity.

NATS clusters in different regions or cloud providers can be connected together by gateways that filter unneeded traffic and allow for disaster recovery and queued distributed processing with geo-location affinity.

Even better, if you have build your application with NATS you don't need to run your own server, cluster, or super-cluster and can simply leverage Synadia's [NGS](https://www.synadia.com/cloud), the global NATS super-cluster service provided by [Synadia](https://www.synadia.com?utm_source=nats_docs\&utm_medium=nats).

This section of the documentation explains how to install, deploy, run, configure, orchestrate, test and manage NATS servers.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/introduction.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
