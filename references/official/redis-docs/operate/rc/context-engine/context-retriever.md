---
title: "Redis Context Retriever on Redis Cloud"
source: "https://redis.io/docs/latest/operate/rc/context-engine/context-retriever/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/context-engine/context-retriever"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Context Retriever on Redis Cloud"
---
# Redis Context Retriever on Redis Cloud

Expose schema-first retrieval tools from your Redis Cloud data to AI agents.

            Redis Cloud
          

Redis Context Retriever on Redis Cloud is currently available in preview. Features and behavior are subject to change.

Redis Context Retriever helps teams expose operational context to AI agents through schema-first retrieval. It models the entities, fields, keys, and relationships that matter to an agent workflow, then presents that context through a governed tool surface the agent can call at runtime. Context Retriever helps an AI agent understand what business objects exist, how they connect, and which paths are safe to use.

When you set up Redis Context Retriever, you model the objects that matter to your agent workflow and connect the relationships between them. You can do this through the UI, the [Context Surfaces Python Client](https://pypi.org/project/redis-context-retriever/), or the `ctxctl` CLI (available when you install the Python client). Context Retriever uses those relationships to automatically create and deploy retrieval tools from your entity model.

When an agent needs context during execution, it calls the MCP tools Context Retriever exposes. Instead of guessing which tool to use or generating SQL, the agent follows the defined entity paths and gets back structured, live, operational context.

For more details, see the [Redis Context Retriever overview](/docs/latest/develop/ai/context-engine/context-retriever/).

## Get started with Context Retriever on Redis Cloud

To set up a Redis Context Retriever on Redis Cloud, you need a database on Redis Cloud that already has relevant data. If you use a relational database, use [[operate/rc/rdi|Redis Data Integration (RDI)]] to ingest data into a Redis Cloud database.

When you have a database, [[operate/rc/context-engine/context-retriever/create-service|Create a context retriever service]] for your database on Redis Cloud.

After you set up Context Retriever, you can [[operate/rc/context-engine/context-retriever/view-service|view your service]]. See the [Context Surfaces Python Client](https://pypi.org/project/redis-context-retriever/) for more information on how to call your tools.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page