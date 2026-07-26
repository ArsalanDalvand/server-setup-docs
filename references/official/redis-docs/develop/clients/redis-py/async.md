---
title: "Asynchronous operations with redis-py"
source: "https://redis.io/docs/latest/develop/clients/redis-py/async/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/clients/redis-py/async"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Asynchronous operations with redis-py"
---
# Asynchronous operations with redis-py

Use redis-py with asyncio for non-blocking Redis access

`redis-py` provides an asyncio-compatible API under the
[`redis.asyncio`](https://redis.readthedocs.io/en/stable/examples/asyncio_examples.html)
namespace. It mirrors the synchronous client API, so most code patterns
translate directly â you `await` commands instead of calling them.

Use the async client for I/O-bound workloads, for integration with async web
frameworks (such as [[integrate/fastapi|FastAPI]], [Starlette](https://www.starlette.io/), [aiohttp](https://docs.aiohttp.org/en/stable/), or [Sanic](https://sanic.dev/en/), or when you need
to run many concurrent Redis operations from a single process. For simple
scripts, CPU-bound work, or codebases without an existing event loop, the
synchronous client is usually a better choice.

The examples on the other pages in this section use the synchronous client,
but you can translate any of them to async by following the rules in
[Translating sync examples](#translating-sync-examples) below.

## Basic connection

Import the async client from `redis.asyncio` and `await` each command.
Construction is synchronous and doesn't open a connection â the pool
establishes one lazily the first time you issue a command. Call `aclose()`
when you're done to release the underlying socket.

Language:

                        Python
                    

Foundational: Connect to Redis with the async client and run a basic SET/GET

```python
import redis.asyncio as redis

async def basic_example():
    r = redis.Redis(host='localhost', port=6379, decode_responses=True)
    await r.set('foo', 'bar')
    value = await r.get('foo')
    print(value)
    # bar
    await r.aclose()

asyncio.run(basic_example())
```

```python

```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

The recommended pattern is to use the client as an async context manager,
which ensures `aclose()` runs even if an exception is raised:

Language:

                        Python
                    

Foundational: Use the async client as a context manager for automatic cleanup

```python
async def context_example():
    async with redis.Redis(
        host='localhost', port=6379, decode_responses=True
    ) as r:
        await r.set('foo', 'bar')
        value = await r.get('foo')
        print(value)
        # bar

asyncio.run(context_example())
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

## Connection pools

For production usage, you should manage connections with a connection pool
rather than opening and closing them individually.
See [[develop/clients/pools-and-muxing|Connection pools and multiplexing]]
for more information about how this works.

A `Redis` client instance already creates and manages its own connection
pool internally, so in a long-running async application the recommended
pattern is to create a single client at startup, share it across requests
and tasks, and close it at shutdown. Avoid creating a new `Redis()` per
request â it defeats pooling and pays the connection cost on every call.

Language:

                        Python
                    

Foundational: Create and share a single async client across the app

```python
async def shared_client_example():
    # Create one client at startup. It owns its own internal pool,
    # which is sized by max_connections.
    r = redis.Redis(
        host='localhost', port=6379, decode_responses=True,
        max_connections=10,
    )
    try:
        # Share `r` across every request and task for the app's lifetime.
        await r.set('foo', 'bar')
        value = await r.get('foo')
        print(value)
        # bar
    finally:
        # Close the client at shutdown to release pooled connections.
        await r.aclose()

asyncio.run(shared_client_example())
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

Tune `max_connections` to the maximum number of concurrent Redis operations
you expect from the process. If you'd rather block on pool exhaustion than
raise an error, construct the client with a `BlockingConnectionPool`.

Note:
  Don't share a single `ConnectionPool` across multiple `Redis(connection_pool=...)`
instances. Closing any one of those clients also closes the shared pool,
which silently invalidates the connections held by every other client using
it. Share the `Redis` client object instead.

## Awaiting commands

Every command method on the async client returns a coroutine â each call
must be `await`ed. Forgetting `await` returns a coroutine object instead of
a result, which is the most common async mistake to watch for.

Because each command is a coroutine, you can run several concurrently with
`asyncio.gather()`:

Language:

                        Python
                    

Run several Redis commands concurrently with asyncio.gather

```python
async def gather_example():
    async with redis.Redis(
        host='localhost', port=6379, decode_responses=True
    ) as r:
        await r.mset({'a': '1', 'b': '2', 'c': '3'})
        a, b, c = await asyncio.gather(
            r.get('a'),
            r.get('b'),
            r.get('c'),
        )
        print(a, b, c)
        # 1 2 3

asyncio.run(gather_example())
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

Each in-flight command consumes one pooled connection while it's executing,
so size `max_connections` to match your peak concurrency. (If you instantiate
the client with `single_connection_client=True`, all commands serialize
through a single connection instead.)

## Pipelines and transactions

Pipelines and transactions work the same way as in the synchronous client
(see [[develop/clients/redis-py/transpipe|Pipelines and transactions]]
for the conceptual background). The only difference is that you create the
pipeline inside an `async with` block and `await pipe.execute()`.

Language:

                        Python
                    

Foundational: Execute commands in an async pipeline

```python
async def pipeline_example():
    async with redis.Redis(
        host='localhost', port=6379, decode_responses=True
    ) as r:
        async with r.pipeline(transaction=True) as pipe:
            pipe.set('a', '1')
            pipe.set('b', '2')
            pipe.get('a')
            pipe.get('b')
            results = await pipe.execute()
            print(results)
            # [True, True, '1', '2']

asyncio.run(pipeline_example())
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

`WATCH`/`MULTI`/`EXEC` for optimistic locking also has an async form.
`watch()` and `execute()` are coroutines; `multi()` remains synchronous
because it only toggles internal pipeline state.

Language:

                        Python
                    

Optimistic locking with an async pipeline and WATCH

```python
from redis.exceptions import WatchError

async def watch_example():
    async with redis.Redis(
        host='localhost', port=6379, decode_responses=True
    ) as r:
        await r.set('counter', '0')
        async with r.pipeline(transaction=True) as pipe:
            while True:
                try:
                    await pipe.watch('counter')
                    current = int(await pipe.get('counter'))
                    pipe.multi()
                    pipe.set('counter', str(current + 1))
                    await pipe.execute()
                    break
                except WatchError:
                    continue
        print(await r.get('counter'))
        # 1

asyncio.run(watch_example())
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

## Pub/Sub

The async pub/sub object follows the same shape as the sync version. Call
`await pubsub.subscribe(...)` to register channels, then iterate messages
with `async for message in pubsub.listen():`. Use the `PubSub` object as an
async context manager (`async with r.pubsub() as pubsub:`) or call
`await pubsub.aclose()` explicitly to release the connection.

Language:

                        Python
                    

Subscribe and receive messages with the async pub/sub API

```python
async def pubsub_example():
    async with redis.Redis(
        host='localhost', port=6379, decode_responses=True
    ) as r:
        async with r.pubsub() as pubsub:
            await pubsub.subscribe('channel-1')

            async def reader():
                async for message in pubsub.listen():
                    if message['type'] == 'message':
                        print(message['data'])
                        # hello
                        break

            reader_task = asyncio.create_task(reader())
            await asyncio.sleep(0.1)
            await r.publish('channel-1', 'hello')
            await reader_task

asyncio.run(pubsub_example())
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

A single `PubSub` object isn't safe to share across tasks â give each
consuming task its own subscription.

## Cluster connections

To connect to a Redis cluster asynchronously, import `RedisCluster` from
`redis.asyncio.cluster`. The API matches the synchronous cluster client
(see [[develop/clients/redis-py/connect#connect-to-a-redis-cluster|Connect to a Redis cluster]]),
with `await` in front of each command.

Language:

                        Python
                    

Foundational: Connect to a Redis cluster with the async client

```python
from redis.asyncio.cluster import RedisCluster

async def cluster_example():
    rc = RedisCluster(host='localhost', port=16379, decode_responses=True)
    await rc.set('foo', 'bar')
    value = await rc.get('foo')
    print(value)
    # bar
    await rc.aclose()

asyncio.run(cluster_example())
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

## Cleanup and lifecycle

Always close clients and pools when you're done:

Use `async with Redis(...) as r:` whenever the client's lifetime fits a
single scope.
For longer-lived clients, call `await r.aclose()` explicitly. (The older
`close()` method is deprecated.)
For frameworks with startup/shutdown hooks â for example [[integrate/fastapi|FastAPI]]'s
`lifespan` â create the client or pool at startup and close it at
shutdown so connections aren't leaked between process restarts.

## Cancellation and timeouts

You can cancel an in-flight command by wrapping it in `asyncio.wait_for()`
(or `asyncio.timeout()` on Python 3.11+). If the command is canceled
mid-flight, `redis-py` disconnects the underlying connection to avoid
response/request misalignment on subsequent reads. The next command
transparently picks up a new connection from the pool.

Language:

                        Python
                    

Apply an asyncio timeout to a Redis command

```python
async def timeout_example():
    async with redis.Redis(
        host='localhost', port=6379, decode_responses=True
    ) as r:
        try:
            # BLPOP blocks waiting for a value, so the timeout reliably fires.
            await asyncio.wait_for(r.blpop('empty-queue'), timeout=0.1)
        except asyncio.TimeoutError:
            print('command canceled')

asyncio.run(timeout_example())
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

A `Redis` client instance is safe to share across tasks (the pool handles
concurrency), but stateful objects derived from it â pipelines and pub/sub
subscriptions â are not. Give each task its own pipeline or `PubSub`.

## Translating sync examples

To adapt any synchronous example elsewhere in this guide to the async
client, apply these rules:

Replace `import redis` with `import redis.asyncio as redis`.
Wrap the example in an `async def` function and call it with
`asyncio.run(...)`.
Add `await` in front of every command call. (Exception: buffered pipeline
commands such as `pipe.set(...)` stay un-awaited; only `pipe.watch(...)`,
any reads issued before `pipe.multi()`, and `pipe.execute()` are awaited.)
Replace `with r.pipeline(...) as pipe:` with
`async with r.pipeline(...) as pipe:`, and `await pipe.execute()`.
Replace `r.close()` with `await r.aclose()`, or use
`async with redis.Redis(...) as r:` as a context manager.

## More information

The [`redis-py` asyncio examples](https://redis.readthedocs.io/en/stable/examples/asyncio_examples.html)
on Read the Docs cover further patterns.
See [[develop/clients/redis-py/error-handling|Error handling]] and
[[develop/clients/redis-py/failover|Client-side geographic failover]] for
resiliency patterns that apply to both sync and async clients.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page