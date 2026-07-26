---
title: "Plan a Flex deployment for Redis Software"
source: "https://redis.io/docs/latest/operate/rs/flex/plan/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/flex/plan"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Plan a Flex deployment for Redis Software"
---
# Plan a Flex deployment for Redis Software

Hardware requirements, sizing guidelines, best practices, and limitations for Flex databases on Redis Software.

            Redis Software
          

Review the following hardware requirements, sizing guidelines, best practices, and known limitations before you deploy a Redis Software cluster for Flex databases.

## Checklist

Before you deploy Flex, verify:

```
- [ ] You have locally attached NVMe or SSD dedicated to Flex data and keys.
- [ ] The flash capacity is greater than the total provisioned database size.
- [ ] Your working set is much smaller than the total dataset and fits a high-RAM hit-rate pattern.
```

## Version requirements

To create Flex databases, you need:

Redis Software cluster version 8.0.2-17 or later

Redis database version 8.2 or later

## Hardware requirements

When planning a Flex deployment, consider the following flash drive requirements:

Flash storage must be locally attached. Network-attached storage (NAS), storage area networks (SAN), or solutions such as AWS Elastic Block Storage (EBS) are not supported.

Flash storage must be dedicated to Flex database data and keys. It should not be used for durability, binaries, or persistence.

For the best performance, the SSDs should be NVMe based. NVMe Gen 5 or Gen 4 is recommended. Gen 3 is also supported, but not recommended for best performance.

Flash storage must be greater than the total provisioned database size to account for write buffers, space amplification, and more.

See the general Redis Software [[operate/rs/installing-upgrading/install/plan-deployment/hardware-requirements|hardware requirements]] for additional requirements.

## Sizing guidelines

Size your Redis Flex databases based on data volume, shard count, RAM-to-flash ratio, and CPU allocation to balance capacity, throughput, and latency.

### Shard size

Flex databases consist of shardsâindependent data partitions that handle a portion of the dataset and request load. Each shard runs as a self-contained Redis process with dedicated memory, CPU, and flash resources.

Use these standard building blocks to plan capacity:

50 GB per shard

Allocate 1 vCPU per shard

For example, a 1 TB dataset requires 20 shards, with 50 GB per shard.

### CPU allocation

Throughput capacity scales with both CPU cores and the RAM-to-flash ratio. At minimum, allocate 1 vCPU per 50 GB shard.

### RAM-to-flash ratio

The RAM-to-flash ratio directly affects throughput and latency:

More RAM increases throughput and lowers latency.

Less RAM lowers throughput and increases latency, but reduces cost.

Actual performance can vary based on your data model, commands, and network latency.

## Feature and data type compatibility

Data type/feature
Flex support

[Active-Active databases](/docs/latest/operate/rs/databases/active-active/)
âNot supported

[JSON](/docs/latest/develop/data-types/json/)
â Supported

[Probabilistic data structures](/docs/latest/develop/data-types/#probabilistic-data-types)
â Supported

[Redis Search](/docs/latest/develop/ai/search-and-query/)
âNot supported

Standard [Redis data types](/docs/latest/develop/data-types/)
â Supported

[Time series](/docs/latest/develop/data-types/timeseries/)
âNot supported

## Best practices

Store small keys and values when possible. Avoid objects larger than 10 KB, which can reduce performance because the entire value moves between RAM and flash.

Start with a balanced ratio, such as 20% RAM to 80% flash, and monitor latency and throughput metrics.

Increase RAM percentage if 99th-percentile latency exceeds your target or cache hit rates drop.

Decrease RAM percentage if memory utilization is high but latency remains stable to lower cost and improve efficiency.

Re-evaluate the ratio of RAM to flash periodically as dataset size and access patterns evolve.

## Known limitations

Flex databases cannot store keys or values larger than 4GB in flash storage. Larger keys or values will be stored in RAM only, and warnings will appear in the Redis logs:

```sh
# WARNING: key too big for disk driver, size: 4703717276, key: subactinfo:htable
```

## Next steps

[[operate/rs/flex/get-started|Get started]]: Configure Flex on your cluster.
[[operate/rs/flex/scale|Scale your deployment]]: Learn scaling strategies.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page