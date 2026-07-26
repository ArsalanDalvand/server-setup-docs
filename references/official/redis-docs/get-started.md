---
title: "Open Source"
source: "https://redis.io/docs/latest/get-started/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "get-started"
category: "redis"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Open Source"
---
# Open Source

Get started with Redis Open Source

Redis is an [[develop/get-started/data-store|in-memory data store]] used by millions of developers as a cache, [[develop/get-started/vector-database|vector database]], [[develop/get-started/document-database|document database]], [streaming engine](/docs/latest/develop/data-types/streams/), and message broker. Redis has built-in replication and different levels of [[operate/oss_and_stack/management/persistence|on-disk persistence]]. It supports complex [data types](/docs/latest/develop/data-types/) (for example, strings, hashes, lists, sets, sorted sets, and JSON), with atomic operations defined on those data types.

Since 2009, the Redis open source project has inspired an enthusiastic and active community of users and contributors. The Redis core source repository is hosted under [https://github.com/redis/redis](https://github.com/redis/redis) along with many of the client libraries. See the [[operate/oss_and_stack|Redis Open Source]] page for more details and links.

You can install Redis from source or from an executable/distribution for your OS.

Install Redis on Linux using [APT](/docs/latest/operate/oss_and_stack/install/install-stack/apt/), [[operate/oss_and_stack/install/install-stack/rpm|RPM]], or [[operate/oss_and_stack/install/install-stack/snap|Snap]]
[[operate/oss_and_stack/install/install-stack/homebrew|Install Redis on macOS]]
[[operate/oss_and_stack/install/install-stack/windows|Run Redis on Windows using Docker]]
[[operate/oss_and_stack/install/install-stack/docker|Run Redis on Docker]]
[[operate/oss_and_stack/install/build-stack|Install Redis from Source]]
[[operate/oss_and_stack/install/archive/install-stack|Install Redis with Redis Stack and Redis Insight]]

## Use cases

The following quick start guides will show you how to use Redis for the following specific purposes:

[[develop/get-started/data-store|Data structure store]]
[[develop/get-started/document-database|Document database]]
[[develop/get-started/vector-database|Vector database]]
[[develop/get-started/redis-in-ai|AI agents and chatbots]]
[[develop/get-started/rag|Retrieval Augmented Generation (RAG) with Redis]]

## Data integration tools, libraries, and frameworks

[[develop/clients|Client API libraries]]
[[integrate/redis-data-integration|Redis Data Integration]]
[Redis vector library for Python](/docs/latest/develop/ai/redisvl/)
[[integrate/amazon-bedrock|Redis Cloud with Amazon Bedrock]]
[[integrate/redisom-for-net|Object-mapping for .NET]]
[[integrate/spring-framework-cache|Spring Data Redis for Java]]

You can find a complete list of integrations on the [[integrate|integrations and frameworks hub]].

To learn more, refer to the [[develop|develop with Redis]] documentation.

## Deployment options

You can deploy Redis with the following methods:

As a service by using [[operate/rc|Redis Cloud]], the fastest way to deploy Redis on your preferred cloud platform.
By installing [[operate/rs|Redis Enterprise Software]] in an on-premises data center or on Cloud infrastructure.
On a variety Kubernetes distributions by using the [Redis Enterprise operator for Kubernetes](/docs/latest/operate/kubernetes/).

The following guides will help you to get started with your preferred deployment method.

Get started with [[operate/rc|Redis Cloud]] by creating a database:

The [[operate/rc/rc-quickstart|Redis Cloud quick start]] helps you create a free database.  (Start here if you're new.)
[Create an Essentials database](/docs/latest/operate/rc/databases/create-database/create-essentials-database/) with a memory limit up to 12 GB.
[Create a Pro database](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/) that suits your workload and offers seamless scaling.

Install a [[operate/rs|Redis Enterprise Software]] cluster:

[Redis Enterprise on Linux quick start](/docs/latest/operate/rs/installing-upgrading/quickstarts/redis-enterprise-software-quickstart/)
[[operate/rs/installing-upgrading/quickstarts/docker-quickstart|Redis Enterprise on Docker quick start]]
[Get started with Redis Enterprise's Active-Active feature](/docs/latest/operate/rs/databases/active-active/get-started/)
[[operate/rs/installing-upgrading|Install and upgrade Redis Enterprise]]

Leverage [Redis Enterprise for Kubernetes](/docs/latest/operate/kubernetes/) to simply deploy a Redis Enterprise cluster on Kubernetes:

[Deploy Redis Enterprise for Kubernetes](/docs/latest/operate/kubernetes/deployment/quick-start/)
[Deploy Redis Enterprise for Kubernetes with OpenShift](/docs/latest/operate/kubernetes/deployment/openshift/)

To learn more, refer to the [[operate|Redis products]] documentation.

## Provisioning and observability tools

[Pulumi provider for Redis Cloud](/docs/latest/integrate/pulumi-provider-for-redis-cloud/)
[Terraform provider for Redis Cloud](/docs/latest/integrate/terraform-provider-for-redis-cloud/)
[Prometheus and Grafana with Redis Cloud](/docs/latest/integrate/prometheus-with-redis-cloud/)
[Prometheus and Grafana with Redis Enterprise](/docs/latest/integrate/prometheus-with-redis-enterprise/)

You can find a complete list of integrations on the [[integrate|libraries and tools hub]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page