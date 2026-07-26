---
title: "Redis Cloud changelog (August 2023)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2023/august-2023/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2023/august-2023"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (August 2023)"
---
# Redis Cloud changelog (August 2023)

New features, enhancements, and other changes added to Redis Cloud during August 2023.

            Redis Cloud
          

This changelog lists new features, enhancements, and other changes added to Redis Cloud during August 2023.

## New features

### Redis 7.2 flexible opt-in

You can opt in to Redis 7.2 on [Flexible subscriptions](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/). Redis 7.2 introduces several changes to existing Redis commands; see the [[operate/rc/changelog/2023/june-2023#redis-72-breaking-changes|list of breaking changes]] published in June's changelog for more details.

### Triggers and functions preview

A preview of [triggers and functions](/docs/latest/operate/oss_and_stack/stack-with-enterprise/deprecated-features/triggers-and-functions/) (previously known as RedisGears) is now available in the following regions:

AWS Asia Pacific - Singapore (`ap-southeast-1`)
GCP Asia Pacific - Tokyo (`asia-northeast1`)

To use it, [create a fixed subscription](/docs/latest/operate/rc/databases/create-database/create-essentials-database/) in one of these regions. Then, [create your database](/docs/latest/operate/rc/databases/create-database/) and set the database Type to Redis and select Triggers and Functions in the drop-down. Or, set the database Type to Redis Stack to get all of our advanced capabilities.

If you'd like to use triggers and functions with a [Flexible subscription](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/), contact [support](https://redis.com/company/support/).

For more information about triggers and functions, see the [triggers and functions documentation](/docs/latest/operate/oss_and_stack/stack-with-enterprise/deprecated-features/triggers-and-functions/).

Note:
  Triggers and functions is discontinued as of [[operate/rc/changelog/2024/may-2024|May 2024]].

### Maintenance windows

You can now [[operate/rc/subscriptions/maintenance/set-maintenance-windows|set manual maintenance windows]] if you want to control when Redis can perform [[operate/rc/subscriptions/maintenance|maintenance]] for a Flexible subscription.

## Known issues

#### Invalid ACL rule causes failed state machine

Applying an invalid ACL rule to a database may cause a failed state machine. If this happens, use the [`PUT /v1/acl/redisRules/{aclRedisRuleId}`](https://api.redislabs.com/v1/swagger-ui/index.html#/Access%20Control%20List/updateRedisRule) API call to update the rule, and then delete it if necessary.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page