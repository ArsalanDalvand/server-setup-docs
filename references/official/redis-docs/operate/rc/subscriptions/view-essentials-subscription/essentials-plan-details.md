---
title: "Redis Cloud Essentials plans"
source: "https://redis.io/docs/latest/operate/rc/subscriptions/view-essentials-subscription/essentials-plan-details/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/subscriptions/view-essentials-subscription/essentials-plan-details"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud Essentials plans"
---
# Redis Cloud Essentials plans

Provides detailed information on Redis Cloud Essentials plans.

            Redis Cloud
          

Redis Cloud Essentials is cost-efficient and designed for low-throughput scenarios. It supports a range of availability, persistence, and backup options.

[Essentials plan pricing](https://redis.io/pricing/#monthly) scales according to the memory size of the database defined in the subscription.  Additional limits also apply, as shown in the tables below.

The 30 MB Essentials plan is free; it's designed for learning and building test projects. It gives you enough space to learn Redis concepts and develop application prototypes.

Our paid plans start at 250 MB and offer extra features, like high availability and backups (both daily and instant). They are great for bigger projects or production environments that require increased storage, reliability, and other features to support your operational needs.

Essentials plans have bandwidth and throughput limits described in the tables below. We will notify you before limiting your database usage if you exceed these limits.

If you need additional resources, you can [[operate/rc/subscriptions/view-essentials-subscription#upgrade-plan|upgrade your subscription]] at any time.

## Current plans

These plans are currently offered for all new and upgraded Essentials subscriptions (updated December 2025).

### Redis Cloud Essentials plans

DBÂ size[1](#table-note-1)
30Â MBÂ (Free)
250 MB
1 GB
2.5 GB
5 GB
12 GB

Concurrent
connections
per database
30
256
1024
2500
5000
10000

CIDR
 allow rules
1
4
4-8
4-8
4-16
4-32

Monthly
 total network
 bandwidth[2](#table-note-2)
5Â GB
100Â GB
200Â GB
400Â GB
800Â GB
2000Â GB

Maximum
 throughput[3](#table-note-3)
100Â ops/sec
1000Â ops/sec
2000Â ops/sec
4000Â ops/sec
8000Â ops/sec
16000Â ops/sec

Database size includes replication where applicable. See [High availability cost impact](/docs/latest/operate/rc/databases/configuration/high-availability/#dataset-size) for more information.

The monthly total network bandwidth limit applies to the entire plan. All databases in the plan share the allocated bandwidth limit.

Assumes request size of 1 KiB. Maximums are capped by actual MB/s reached. To find the MB/s limit, divide the Maximum throughput by 1024.

### Redis Cloud Essentials plans with Redis Flex

DBÂ size[1](#table-note-1-flex)
1 GB
2.5 GB
5 GB
12 GB
25 GB
50 GB
100 GB

Concurrent
connections
per database
1024
2500
5000
10000
10000
10000
10000

CIDR
 allow rules
4-8
4-8
4-16
4-32
4-32
4-32
4-32

Monthly
 total network
 bandwidth
50Â GB
100Â GB
200Â GB
500Â GB
1000Â GB
2000Â GB
4000Â GB

Maximum
 throughput[2](#table-note-2-flex)
200Â ops/sec
500Â ops/sec
1000Â ops/sec
2400Â ops/sec
5000Â ops/sec
10000Â ops/sec
20000Â ops/sec

Database size includes replication where applicable. See [High availability cost impact](/docs/latest/operate/rc/databases/configuration/high-availability/#dataset-size) for more information.

Assumes request size of 1 KiB. Maximums are capped by actual MB/s reached. To find the MB/s limit, divide the Maximum throughput by 1024.

## Previous plans

These plans were available before November 2023.

MaxÂ DBÂ sizeÂ [1](#table-note-1-legacy)
30 MB
100 MB
250 MB
500 MB
1 GB
2.5 GB
5 GB
10 GB

Concurrent
connections
per database
30
256
256
512
1024
2500
5000
10000

Monthly
 total network
 bandwidth[2](#table-note-2-legacy)
5Â GB
50Â GB
100Â GB
150Â GB
200Â GB
400Â GB
800Â GB
2000Â GB

Maximum
 throughput[3](#table-note-3-legacy)
100Â ops/sec
500Â ops/sec
1000Â ops/sec
1500Â ops/sec
2000Â ops/sec
4000Â ops/sec
8000Â ops/sec
16000Â ops/sec

Database size includes replication. See [High availability cost impact](/docs/latest/operate/rc/databases/configuration/high-availability/#dataset-size) for more information.

The monthly total network bandwidth limit applies to the entire plan. All databases in the plan share the allocated bandwidth limit.

Assumes request size of 1 KiB. Maximums are capped by actual MB/s reached. To find the MB/s limit, divide the Maximum throughput by 1024.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page