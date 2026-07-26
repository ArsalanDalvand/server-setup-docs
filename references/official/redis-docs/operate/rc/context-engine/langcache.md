---
title: "Semantic caching with LangCache on Redis Cloud"
source: "https://redis.io/docs/latest/operate/rc/context-engine/langcache/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/context-engine/langcache"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Semantic caching with LangCache on Redis Cloud"
---
# Semantic caching with LangCache on Redis Cloud

Store LLM responses for AI applications in Redis Cloud.

            Redis Cloud
          

LangCache on Redis Cloud is currently available as a public preview. Features and behavior are subject to change.

LangCache is a semantic caching service available as a REST API that stores LLM responses for fast and cheaper retrieval, built on the Redis vector database. By using semantic caching, you can significantly reduce API costs and lower the average latency of your generative AI applications.

For more information about how LangCache works, see the [LangCache overview](/docs/latest/develop/ai/context-engine/langcache/).

## LLM cost reduction with LangCache

LangCache reduces your LLM costs by caching responses and avoiding repeated API calls. When a response is served from cache, you donât pay for output tokens. Input token costs are typically offset by embedding and storage costs.

For every cached response, you'll save the output token cost. To calculate your monthly savings with LangCache, you can use the following formula:

```bash
Est. monthly savings with LangCache = 
    (Monthly output token costs) Ã (Cache hit rate)
```

The more requests you serve from LangCache, the more you save, because youâre not paying to regenerate the output.

Hereâs an example:

Monthly LLM spend: $200
Percentage of output tokens in your spend: 60%
Cost of output tokens: $200 Ã 60% = $120
Cache hit rate: 50%
Estimated savings: $120 Ã 50% = $60/month

Note:
  The formula and numbers above provide a rough estimate of your monthly savings. Actual savings will vary depending on your usage.

You can also use the [LangCache savings calculator](https://redis.io/calculator/langcache/) to estimate your annual savings with LangCache.

## Get started with LangCache on Redis Cloud

To set up LangCache on Redis Cloud:

[Create a database](/docs/latest/operate/rc/databases/create-database/) on Redis Cloud.
[[operate/rc/context-engine/langcache/create-service|Create a LangCache service]] for your database on Redis Cloud.
[[operate/rc/context-engine/langcache/use-langcache|Use the LangCache API]] from your client app.

After you set up LangCache, you can [[operate/rc/context-engine/langcache/view-edit-cache|view and edit the cache]] and [[operate/rc/context-engine/langcache/monitor-cache|monitor the cache's performance]].

See also our [Redis LangCache setup](https://www.youtube.com/watch?v=UOGhMZlZLko)
tutorial video for advice on how to get started.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page