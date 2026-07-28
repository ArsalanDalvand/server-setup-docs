---
title: "NATS and Kubernetes"
source: "https://docs.nats.io/running-a-nats-service/nats-kubernetes.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats-kubernetes"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "NATS and Kubernetes"
---
# NATS and Kubernetes

The recommended way to deploy NATS on Kubernetes is using [Helm](https://helm.sh/) with the official NATS Helm Chart.

## Helm repo

To register the NATS Helm chart run:

```sh
helm repo add nats https://nats-io.github.io/k8s/helm/charts/
```

## Config values

The default configuration values of the chart will deploy a single NATS server as a `StatefulSet` and a single replica [nats-box](https://github.com/nats-io/nats-box) `Deployment`.

The [ArtifactHub page](https://artifacthub.io/packages/helm/nats/nats) provides the list of Helm configuration values and examples for the current release.

*For tracking the development version, refer to the* [*source repo*](https://github.com/nats-io/k8s/tree/main/helm/charts/nats#nats-server)*.*

Once the desired configuration is created, install the chart:

```sh
helm install nats nats/nats
```

## Validate connectivity

Once the pods are up, validate by accessing the `nats-box` container and running a CLI command.

```sh
kubectl exec -it deployment/nats-box -- nats pub test hi
```

The output should indicate a successful publish to NATS.

```
16:17:00 Published 2 bytes to "test"
```

## Commercial Options

Synadia offers [Deploy for Kubernetes](https://www.synadia.com/deploy-for-kubernetes/), a self-service, bring-your-own Kubernetes deployment option that includes NATS and additional components.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats-kubernetes.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
