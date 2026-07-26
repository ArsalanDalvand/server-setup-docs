---
title: "Client-side geographic failover"
source: "https://redis.io/docs/latest/develop/clients/failover/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/clients/failover"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Client-side geographic failover"
---
# Client-side geographic failover

Improve reliability using the failover/failback features of client libraries.

Some Redis client libraries support
[Client-side geographic failover](https://en.wikipedia.org/wiki/Failover)
to improve the availability of connections to Redis databases. Use this page
to get a general overview of the concepts and then see the documentation for
your client library to learn how to configure it for failover and failback.

## Supported client libraries

The table below lists the clients that support client-side geographic failover
along with their release state and available features.

Client
Basic failover
Pub/sub failover
OSS Cluster failover
Failback

[Jedis](/docs/latest/develop/clients/jedis/failover/)
Yes
No
No
Yes

[[develop/clients/redis-py/failover|redis-py]]
Yes  (Preview)
Yes
Yes
Yes

[Lettuce](/docs/latest/develop/clients/lettuce/failover/)
Yes (Preview)
Yes
No
Yes

## Concepts

You may have several [Active-Active databases](/docs/latest/operate/rs/databases/active-active/)
or independent Redis servers that are all suitable to serve your app.
Typically, you would prefer to use some database endpoints over others for a particular
instance of your app (perhaps the ones that are closest geographically to the app server
to reduce network latency). However, if the best endpoint is not available due
to a failure, it is generally better to switch to another, suboptimal endpoint
than to let the app fail completely.

Failover is the technique of actively checking for connection failures or
unacceptably slow connections and automatically switching to the best available endpoint
when they occur. This requires you to specify a list of endpoints to try, ordered by priority. The diagram below shows this process:

[](/docs/latest/images/failover/failover-client-reconnect.svg)

The complementary technique of failback then involves periodically checking the health
of all endpoints that have failed. If any endpoints recover, the failback mechanism
automatically switches the connection to the one with the highest priority.
This could potentially be repeated until the optimal endpoint is available again.

[](/docs/latest/images/failover/failover-client-failback.svg)

### Detecting connection problems

Redis clients use a [circuit breaker design pattern](https://en.wikipedia.org/wiki/Circuit_breaker_design_pattern) to detect connection problems.

The circuit breaker is a software component that tracks the sequence of recent
Redis connection attempts and commands, recording which ones have succeeded and
which have failed.
(Note that many command failures are caused by transient errors such as timeouts,
so before recording a failure, the first response should usually be just to retry
the command a few times.)

The status of the attempted command calls is kept in a "sliding window", which
is simply a buffer where the least recent item is dropped as each new
one is added. The buffer can be configured to have a fixed number of failures and/or a failure ratio (specified as a percentage), both based on a time window.

[](/docs/latest/images/failover/failover-sliding-window.svg)

When the number of failures in the window exceeds a configured
threshold, the circuit breaker declares the server to be unhealthy and triggers
a failover.

### Selecting a failover target

Since you may have multiple Redis servers available to fail over to, the client
lets you configure a list of endpoints to try, ordered by priority or
"weight". When a failover is triggered, the client selects the highest-weighted
endpoint that is still healthy and uses it for the temporary connection.

### Health checks

Given that the original endpoint had some geographical or other advantage
over the failover target, you will generally want to fail back to it as soon
as it recovers. In the meantime, another server might recover that is
still better than the current failover target, so it might be worth
failing back to that server even if it is not optimal.

Clients periodically run a "health check" on each server to see if it has recovered.
Several health check strategies are implemented in all clients:

Ping: This is the default strategy, which just sends a
[`PING`](/docs/latest/commands/ping/) command and ensures that it gives the
expected response.
Lag aware (Redis Software only): This strategy uses the
[REST API](/docs/latest/operate/rs/references/rest-api/) to check the
synchronization lag between a specific database and the others in the Active-Active
setup. If the lag is within a specified tolerance, the server is considered healthy.
Custom: You can implement your own health check strategy to use information
or take action that is specific to your application.

See the documentation for your client library for more information on how to
configure health checks.

You can also configure the client to run health checks on the current target
server during periods of inactivity, even if no failover has occurred. This can
help to detect problems even if your app is not actively using the server.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page