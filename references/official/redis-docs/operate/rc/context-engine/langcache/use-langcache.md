---
title: "Use the LangCache API on Redis Cloud"
source: "https://redis.io/docs/latest/operate/rc/context-engine/langcache/use-langcache/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/context-engine/langcache/use-langcache"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Use the LangCache API on Redis Cloud"
---
# Use the LangCache API on Redis Cloud

            Redis Cloud
          

LangCache on Redis Cloud is currently available as a public preview. Features and behavior are subject to change.

You can use the [LangCache API and SDK](/docs/latest/develop/ai/context-engine/langcache/api-examples/) from your client app to store and retrieve LLM, RAG, or agent responses.

To access the LangCache API, you need:

LangCache API base URL
LangCache service API key
Cache ID

For LangCache on Redis Cloud, the base URL and cache ID are available in the LangCache service's Configuration page in the [[operate/rc/context-engine/langcache/view-edit-cache#connectivity|Connectivity section]].

The LangCache API key is only available immediately after you create the LangCache service. If you lost this value, you will need to [[operate/rc/context-engine/langcache/view-edit-cache#replace-service-api-key|replace the service API key]] to be able to use the LangCache API.

When you call the API, you need to pass the LangCache API key in the `Authorization` header as a Bearer token and the Cache ID as the `cacheId` path parameter.

See the [LangCache API and SDK examples](/docs/latest/develop/ai/context-engine/langcache/api-examples/) for more information on how to use the LangCache API.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)