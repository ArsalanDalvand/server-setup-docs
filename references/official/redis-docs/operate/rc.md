---
title: "Redis Cloud"
source: "https://redis.io/docs/latest/operate/rc/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud"
---
# Redis Cloud

The fastest way to set up Redis - a fully managed Redis database on major public cloud services.

            Redis Cloud
          

[Redis Cloud](https://redis.io/cloud/) is a fully managed database-as-a-service that brings the speed and reliability of Redis to the cloud, offering seamless scalability and high availability for modern applications.

With Redis Cloud, you get all of the features of Redis Software, including:

[[develop|Redis]] and [Redis Stack](/docs/latest/operate/oss_and_stack/stack-with-enterprise/) support
Linear scalability
Instant failover, backups, and recovery
Predictable performance
24/7 monitoring and support

[Try Redis Cloud](https://redis.io/try-free/) to set up your free 30MB database.

## Get started

Use the [[operate/rc/rc-quickstart|Quick start]] to learn how to create your free database.

[[operate/rc/rc-quickstart#using-rediscli|Connect with redis-cli]]
[[operate/rc/rc-quickstart#using-redis-client|Connect with Redis client]]
[[operate/rc/rc-quickstart#using-redisinsight|Connect with Redis Insight]]

You can also use [Vercel's Redis Cloud integration](https://vercel.com/marketplace/redis-cloud) or [Heroku's Redis Cloud add-on](https://elements.heroku.com/addons/rediscloud) to quickly add a Redis database to your project.

## Databases

Create and manage [Redis databases](/docs/latest/operate/rc/databases/) in the cloud.

[Create database](/docs/latest/operate/rc/databases/create-database/)
[View and edit databases](/docs/latest/operate/rc/databases/view-edit-database/)
[Monitor performance](/docs/latest/operate/rc/databases/monitor-performance/)
[Manage databases](/docs/latest/operate/rc/databases/configuration/)
[[commands|Redis commands]] & [[operate/rc/compatibility|compatibility]]

## Subscriptions

Learn about the [[operate/rc/subscriptions|types of subscriptions]].

[[operate/rc/subscriptions/view-essentials-subscription|View and upgrade Essentials plan]]
[[operate/rc/subscriptions/view-essentials-subscription/essentials-plan-details|Essentials plans]]
[[operate/rc/subscriptions/view-pro-subscription|View and edit Pro plan]]

## Accounts & settings

Manage Redis Cloud [[operate/rc/accounts|accounts and settings]].

[[operate/rc/billing-and-payments|Billing and payments]]
[Manage marketplace integrations](/docs/latest/operate/rc/cloud-integrations/)

## Security

Manage [[operate/rc/security|secure connections]] to cloud databases.

[[operate/rc/security/access-control/access-management|Access management]] for Redis Cloud console security and account management
[[operate/rc/security/database-security|Cloud database security]]
[[operate/rc/security/access-control/multi-factor-authentication|Multi-factor authentication]]
[[operate/rc/security/access-control/saml-sso|Single sign-on]] and [[operate/rc/security/access-control/social-login|social login]]
[[operate/rc/security/access-control/data-access-control|Data access control]]

## REST API

Use the [[operate/rc/api|REST API]] to manage Redis Cloud databases and subscriptions.

[[operate/rc/api/get-started|Get started with the REST API]]
REST API [[operate/rc/api/api-reference|reference]] & [examples](/docs/latest/operate/rc/api/examples/)
[`redisctl`](https://github.com/redis/redisctl) â a CLI tool that wraps the Redis Cloud and Redis Software APIs for terminal-based management

## Migrate to Redis Cloud

Follow the step-by-step guide for your source environment:

[ElastiCache to Redis Cloud](https://redis.io/tutorials/migration/elasticache-to-redis-cloud/) â offline and live migration from AWS ElastiCache
[Memorystore to Redis Cloud](https://redis.io/tutorials/migration/memorystore-to-redis-cloud/) â offline and live migration from Google Cloud Memorystore
[Open source Redis to Redis Cloud](https://redis.io/tutorials/migration/redis-open-source-to-redis-cloud/) â migrate from a self-hosted Redis instance

## Migrate to Azure Managed Redis

[ElastiCache to Azure Managed Redis (AMR)](https://redis.io/tutorials/learn/migration/elasti-cache-to-azure-managed-redis/) â move your workload from AWS to Azure
[Memorystore to Azure Managed Redis (AMR)](https://redis.io/tutorials/learn/migration/memorystore-to-azure-managed-redis/) â move your workload from Google Cloud to Azure

## Related info

[[operate/rs|Redis Software]]
[[develop|Develop with Redis]]
[Redis Stack](/docs/latest/operate/oss_and_stack/stack-with-enterprise/)
[[glossary|Glossary]]

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page