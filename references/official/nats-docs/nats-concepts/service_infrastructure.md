---
title: "NATS Service Infrastructure"
source: "https://docs.nats.io/nats-concepts/service_infrastructure.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/service_infrastructure"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "NATS Service Infrastructure"
---
# NATS Service Infrastructure

NATS is a client/server system in the fact that you have 'NATS client applications' (applications using one of the NATS client libraries) that connect to 'NATS servers' that provide the NATS service. The NATS servers work together to provide a NATS service infrastructure to their client applications.

NATS is extremely flexible and scalable and allows the service infrastructure to be as small as a single process running locally on your local machine and as large as an 'Internet of NATS' of Leaf Nodes, and Leaf Node clusters all interconnected in a secure way over a global shared NATS super-cluster.

Regardless of the size and complexity of the NATS service infrastructure being used, the only configuration needed by the client applications being the location (NATS URLs) of one or more NATS servers and depending on the required security, their credentials.

Note that if your application is written in Golang then you even have the option of embedding the NATS server functionality into the application itself (however you need to then configure your application instances with nats-server configuration information).

You do not actually need to run your NATS service infrastructure, instead you can instead make use of a public NATS infrastructure offered by a NATS Service Provider such as [Synadia Cloud](https://www.synadia.com/cloud?utm_source=nats_docs\&utm_medium=nats), think of Synadia Cloud as being an 'Internet of NATS' (literally an "InterNATS") and of Synadia as being an "InterNATS Service Provider".

## The Evolution of your NATS service infrastructure

You will typically start by running a single instance of nats-server on your local development machine, and have your applications connect to it while you do your application development and local testing.

Next you will probably want to start testing and running those applications and servers in a VPC, or a region or in some on-prem location, so you will deploy either single NATS server or clusters of NATS servers in your VPCs/regions/on-prem/etc... locations and in each location have the applications connect their local nats-server or nats-server cluster. You can then connect those local nats-servers or local nats-server clusters together by making them leaf nodes connecting to a 'backbone' cluster or super-cluster, or by connecting them directly together via gateway connections.

If you have many client applications (e.g., applications deployed on end-user devices all over the Internet, or for example many IoT devices) or many servers in a lot of locations you will then scale your NATS service infrastructure by deploying clusters of NATS servers in multiple locations and multiple cloud providers and VPCs. You will then need to connect those clusters into a global super-cluster and then devise a scheme to intelligently direct your client applications to the right 'closest' NATS server cluster.

## Running your own NATS service infrastructure

You can deploy and run your own NATS service infrastructure of nats-server instances, composed of servers, clusters of servers, super-cluster and leaf node NATS servers.

### Virtualization and containerization considerations

If using Kubernetes we recommend you use the [Helm charts](https://github.com/nats-io/k8s/tree/main/helm/charts/nats).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/service_infrastructure.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
