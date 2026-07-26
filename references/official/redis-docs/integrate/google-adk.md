---
title: "Redis with Google Agent Development Kit (ADK)"
source: "https://redis.io/docs/latest/integrate/google-adk/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/google-adk"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis with Google Agent Development Kit (ADK)"
---
# Redis with Google Agent Development Kit (ADK)

Build AI agents with persistent memory, semantic search, and caching using Redis and Google ADK.

[Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/) provides clean abstractions for building AI agents: interfaces for memory, sessions, tools, and callbacks. [adk-redis](https://github.com/redis-developer/adk-redis) implements these interfaces using Redis, giving agents persistent two-tier memory, semantic search for RAG, and response caching without requiring changes to agent logic.

## Architecture

adk-redis connects three backend systems to the ADK framework:

[Redis Agent Memory Server](https://github.com/redis/agent-memory-server) handles working memory (sessions), long-term memory (extracted facts), auto-summarization, and memory search.
[RedisVL](/docs/latest/develop/ai/redisvl/) (Redis Vector Library) powers the search tools and local semantic cache provider.
[LangCache](https://redis.io/langcache/) provides managed semantic caching with server-side embeddings.

## Prerequisites

Redis 8.4+ with vector search support
Agent Memory Server for memory and session services

```bash
# Start Redis
docker run -d --name redis -p 6379:6379 redis:8.4-alpine

# Start Agent Memory Server
docker run -d --name agent-memory-server -p 8088:8088 \
  -e REDIS_URL=redis://host.docker.internal:6379 \
  -e GEMINI_API_KEY=your-key \
  -e GENERATION_MODEL=gemini/gemini-2.5-flash \
  -e EMBEDDING_MODEL=gemini/text-embedding-004 \
  redislabs/agent-memory-server:latest \
  agent-memory api --host 0.0.0.0 --port 8088 --task-backend=asyncio
```

On Linux, `host.docker.internal` does not resolve by default. Use
`--network=host` plus `REDIS_URL=redis://127.0.0.1:6379`, or point
`REDIS_URL` at the Docker bridge gateway (typically
`redis://172.17.0.1:6379`).

## Installation

```bash
# Memory and session services (requires Agent Memory Server)
pip install adk-redis[memory]

# Search tools via RedisVL
pip install adk-redis[search]

# SQL-style search tool (sql-redis)
pip install adk-redis[sql]

# Managed semantic caching via LangCache
pip install adk-redis[langcache]

# Everything
pip install adk-redis[all]

# For the RedisVL MCP server (used with ADK's native McpToolset)
pip install 'redisvl[mcp]>=0.18.2'
```

## Quick start

Wire up Redis Agent Memory in a few lines:

```python
from google.adk import Agent
from google.adk.agents.callback_context import CallbackContext
from google.adk.runners import Runner
from adk_redis.sessions import (
    RedisWorkingMemorySessionService,
    RedisWorkingMemorySessionServiceConfig,
)
from adk_redis.memory import (
    RedisLongTermMemoryService,
    RedisLongTermMemoryServiceConfig,
)

session_service = RedisWorkingMemorySessionService(
    config=RedisWorkingMemorySessionServiceConfig(
        api_base_url="http://localhost:8088",
        default_namespace="my_app",
    )
)
memory_service = RedisLongTermMemoryService(
    config=RedisLongTermMemoryServiceConfig(
        api_base_url="http://localhost:8088",
        default_namespace="my_app",
    )
)

async def after_agent(callback_context: CallbackContext):
    await callback_context.add_session_to_memory()

agent = Agent(
    name="my_agent",
    model="gemini-2.5-flash",
    instruction="You are a helpful assistant with long-term memory.",
    after_agent_callback=after_agent,
)

runner = Runner(
    agent=agent,
    app_name="my_app",
    session_service=session_service,
    memory_service=memory_service,
)
```

## Capabilities

Capability
Description
Page

Redis Agent Memory
Working and long-term memory via framework services, REST tools, or MCP
[[integrate/google-adk/redis-agent-memory|Redis Agent Memory]]

Integration patterns
Framework-managed, LLM-controlled REST, and MCP tools
[[integrate/google-adk/integration-patterns|Integration patterns]]

Search tools
Vector, hybrid, text, range, and SQL search via RedisVL, plus the `rvl mcp` server over `McpToolset`
[[integrate/google-adk/search-tools|Search tools]]

Semantic caching
LLM response and tool result caching
[[integrate/google-adk/semantic-caching|Semantic caching]]

Examples
Nine complete examples covering all capabilities
[Examples](/docs/latest/integrate/google-adk/examples/)

## More info

[adk-redis on GitHub](https://github.com/redis-developer/adk-redis)
[adk-redis on PyPI](https://pypi.org/project/adk-redis/)
[Car dealership tutorial](https://redis.io/tutorials/build-a-car-dealership-agent-with-google-adk-and-redis-agent-memory/)
[Redis Agent Memory Server](https://github.com/redis/agent-memory-server)
[RedisVL documentation](/docs/latest/develop/ai/redisvl/)
[Google ADK documentation](https://google.github.io/adk-docs/)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page