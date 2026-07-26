---
title: "Redis Cloud shared responsibility model"
source: "https://redis.io/docs/latest/operate/rc/security/shared-responsibility-model/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/shared-responsibility-model"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud shared responsibility model"
---
# Redis Cloud shared responsibility model

            Redis Cloud
          

The security of all Redis Cloud deployments is a shared responsibility. Redis, the public cloud providers (Amazon Web Services [AWS], Google Cloud, and Microsoft Azure), and our customers all help ensure the security of these deployments.

## Redis responsibility

Redis Cloud's offerings are managed by Redis and deployed on AWS, Azure, and Google Cloud infrastructure.

Redis is responsible for the software that runs Redis Cloud. This includes the patching and maintenance of
the operating systems that Redis is deployed on as well as the patching and maintenance of Redis Cloud.

## Cloud provider responsibility

The public cloud provider hosting your Redis Cloud databases is responsible for the physical security of their data centers and
the security of the network, storage, servers, and virtualization that form the core infrastructure of your deployment.

Amazon, Microsoft, and Google public clouds embrace a wide range of security best practices and compliance standards. Compliance informationâincluding audits, attestations, and certifications about resources hostedâcan be found on these compliance pages:

[AWS Compliance](https://aws.amazon.com/compliance/)
[Google Cloud Compliance](https://cloud.google.com/security/compliance)
[Azure Compliance](https://azure.microsoft.com/en-us/overview/trusted-cloud/compliance/)

## Customer responsibility

Customers are responsible for the security configurations in their Redis databases and the Redis Cloud console. Customers must understand and implement the Redis Cloud security features and best practices.

Customers are also responsible for the applications built on Redis and the data they store in Redis. Customers determine the cloud provider, region, and availability zone of their deployments.

Customers understand that Redis Cloud Essentials plans are deployed to a multi-tenant infrastructure. Redis Cloud Pro plans are deployed to a single-tenant infrastructure dedicated to one specific customer.

## Continue learning with Redis University

See the [Redis Cloud security overview course](https://university.redis.io/course/0b1hkhejlitg3x) to learn more.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page