---
title: "Monitor a LangCache service"
source: "https://redis.io/docs/latest/operate/rc/context-engine/langcache/monitor-cache/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/context-engine/langcache/monitor-cache"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Monitor a LangCache service"
---
# Monitor a LangCache service

            Redis Cloud
          

LangCache on Redis Cloud is currently available as a public preview. Features and behavior are subject to change.

You can monitor a LangCache service's performance from the Metrics tab of the service's page.

[](/docs/latest/images/rc/langcache-metrics.png)

The Metrics tab provides a series of graphs showing performance data for your LangCache service.

You can switch between hourly, daily, and weekly stats using the Hour, Day, and Week buttons at the top of the page. Each graph also includes minimum, average, maximum, and latest values.

## LangCache metrics reference

### Cache hit ratio

The percentage of requests that were successfully served from the cache without needing to call the LLM API. A healthy cache will generally show an increasing hit ratio over time as it becomes more populated by cached responses.

To optimize your cache hit ratio:

Tune similarity thresholds to capture more semantically related queries.
Analyze recurring query patterns to fine-tune your embedding strategies.
Test different embedding models to understand their impact on cache hit rates.

A higher cache hit ratio does not always mean better performance. If the cache is too lenient in its similarity matching, it may return irrelevant responses, leading to a higher cache hit rate but poorer overall performance.

### Cache search requests

The number of read attempts against the cache at the specified time. This metric can help you understand the load on your cache and identify periods of high or low activity.

### Cache latency

The average time to process a cache lookup request. This metric can help you identify performance bottlenecks and optimize your cache configuration.

Cache latency is highly dependent on embedding model performance, since the cache must generate embeddings for each request in order to compare them to the cached responses.

High cache latency may indicate one of the following:

Inefficient embedding generation from the embedding provider
Large cache requiring longer comparison times
Network latency between the cache and embedding provider
Resource constraints

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page