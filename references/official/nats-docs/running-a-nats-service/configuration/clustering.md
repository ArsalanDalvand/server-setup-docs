---
title: "Clustering"
source: "https://docs.nats.io/running-a-nats-service/configuration/clustering.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/clustering"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Clustering"
---
# Clustering

## NATS Server Clustering

NATS supports running each server in clustered mode. You can cluster servers together for high volume messaging systems and resiliency and high availability.

NATS servers achieve this by gossiping about and connecting to, all of the servers they know, thus dynamically forming a full mesh. Once clients [connect](/using-nats/developer/connecting/cluster.md) or [re-connect](/using-nats/developer/connecting/reconnect.md) to a particular server, they are informed about current cluster members. Because of this behavior, a cluster can grow, shrink and self heal. The full mesh does not necessarily have to be explicitly configured either.

Note that NATS clustered servers have a forwarding limit of one hop. This means that each `nats-server` instance will **only** forward messages that it has received **from a client** to the immediately adjacent `nats-server` instances to which it has routes. Messages received **from** a route will only be distributed to local clients.

For the cluster to successfully form a full mesh and NATS to function as intended and described throughout the documentation - temporary errors permitting - it is necessary that servers can connect to each other and that clients can connect to each server in the cluster.

## Cluster URLs

In addition to a port to listen for clients, `nats-server` listens on a "cluster" URL (the `-cluster` option). Additional `nats-server` servers can then add that URL to their `-routes` argument to join the cluster. These options can also be specified in a [config file](/running-a-nats-service/configuration/clustering/cluster_config.md), but only the command-line version is shown in this overview for simplicity.

## Running a Simple Cluster

Here is a simple cluster running on the same machine:

Server A - the 'seed server'

```bash
nats-server -p 4222 -cluster nats://localhost:4248 --cluster_name test-cluster
```

Server B

```shell
nats-server -p 5222 -cluster nats://localhost:5248 -routes nats://localhost:4248 --cluster_name test-cluster
```

Check the output of the server for the selected client and route ports.

Server C

```shell
nats-server -p 6222 -cluster nats://localhost:6248 -routes nats://localhost:4248 --cluster_name test-cluster
```

Check the output of the server for the selected client and route ports.

Each server has a client and cluster port specified. Servers with the routes option establish a route to the *seed server*. Because the clustering protocol gossips members of the cluster, all servers are able to discover other server in the cluster. When a server is discovered, the discovering server will automatically attempt to connect to it in order to form a *full mesh*. Typically only one instance of the server will run per machine, so you can reuse the client port (4222) and the cluster port (4248), and simply the route to the host/port of the seed server.

Similarly, clients connecting to any server in the cluster will discover other servers in the cluster. If the connection to the server is interrupted, the client will attempt to connect to all other known servers.

There is no explicit configuration for *seed server*. They simply serve as the starting point for server discovery by other members of the cluster as well as clients. As such these are the servers that clients have in their list of connect urls and cluster members have in their list of routes. They reduce configuration as not every server needs to be in these lists. But the ability for other server and clients to successfully connect depends on *seed server* running. If multiple *seed server* are used, they make use of the routes option as well, so they can establish routes to one another.

## Command Line Options

The following cluster options are supported:

```
--routes [rurl-1, rurl-2]     Routes to solicit and connect
--cluster nats://host:port    Cluster URL for solicited routes
```

When a NATS server routes to a specified URL, it will advertise its own cluster URL to all other servers in the route effectively creating a routing mesh to all other servers.

**Note:** when using the `-routes` option, you must also specify a `-cluster` option.

Clustering can also be configured using the server [config file](/running-a-nats-service/configuration/clustering/cluster_config.md).

## Three Server Cluster Example

The following example demonstrates how to run a cluster of 3 servers on the same host. We will start with the seed server and use the `-D` command line parameter to produce debug information.

```bash
nats-server -p 4222 -cluster nats://localhost:4248 -D
```

Alternatively, you could use a configuration file, let's call it `seed.conf`, with a content similar to this:

```
# Cluster Seed Node

listen: 127.0.0.1:4222
http: 8222

cluster {
  listen: 127.0.0.1:4248
}
```

And start the server like this:

```bash
nats-server -config ./seed.conf -D
```

This will produce an output similar to:

```
[83329] 2020/02/12 16:04:52.369039 [INF] Starting nats-server version 2.1.4
[83329] 2020/02/12 16:04:52.369130 [DBG] Go build version go1.13.6
[83329] 2020/02/12 16:04:52.369133 [INF] Git commit [not set]
[83329] 2020/02/12 16:04:52.369360 [INF] Starting http monitor on 127.0.0.1:8222
[83329] 2020/02/12 16:04:52.369436 [INF] Listening for client connections on 127.0.0.1:4222
[83329] 2020/02/12 16:04:52.369441 [INF] Server id is NDSGCS74MG5ZUMBOVWOUJ5S3HIOW
[83329] 2020/02/12 16:04:52.369443 [INF] Server is ready
[83329] 2020/02/12 16:04:52.369534 [INF] Listening for route connections on 127.0.0.1:4248
```

It is also possible to specify the hostname and port independently. At the minimum, the port is required. If you leave the hostname off it will bind to all the interfaces ('0.0.0.0').

```
cluster {
  host: 127.0.0.1
  port: 4248
}
```

Now let's start two more servers, each one connecting to the seed server.

```bash
nats-server -p 5222 -cluster nats://localhost:5248 -routes nats://localhost:4248 -D
```

When running on the same host, we need to pick different ports for the client connections `-p`, and for the port used to accept other routes `-cluster`. Note that `-routes` points to the `-cluster` address of the seed server (`localhost:4248`).

Here is the log produced. See how it connects and registers a route to the seed server (`...GzM`).

```
[83330] 2020/02/12 16:05:09.661047 [INF] Starting nats-server version 2.1.4
[83330] 2020/02/12 16:05:09.661123 [DBG] Go build version go1.13.6
[83330] 2020/02/12 16:05:09.661125 [INF] Git commit [not set]
[83330] 2020/02/12 16:05:09.661341 [INF] Listening for client connections on 0.0.0.0:5222
[83330] 2020/02/12 16:05:09.661347 [INF] Server id is NAABC2CKRVPZBIECMLZZA6L3PK
[83330] 2020/02/12 16:05:09.661349 [INF] Server is ready
[83330] 2020/02/12 16:05:09.662429 [INF] Listening for route connections on localhost:5248
[83330] 2020/02/12 16:05:09.662676 [DBG] Trying to connect to route on localhost:4248
[83330] 2020/02/12 16:05:09.663308 [DBG] 127.0.0.1:4248 - rid:1 - Route connect msg sent
[83330] 2020/02/12 16:05:09.663370 [INF] 127.0.0.1:4248 - rid:1 - Route connection created
[83330] 2020/02/12 16:05:09.663537 [DBG] 127.0.0.1:4248 - rid:1 - Registering remote route "NDSGCS74MG5ZUMBOVWOUJ5S3HIOW"
[83330] 2020/02/12 16:05:09.663549 [DBG] 127.0.0.1:4248 - rid:1 - Sent local subscriptions to route
```

From the seed's server log, we see that the route is indeed accepted:

```
[83329] 2020/02/12 16:05:09.663386 [INF] 127.0.0.1:62941 - rid:1 - Route connection created
[83329] 2020/02/12 16:05:09.663665 [DBG] 127.0.0.1:62941 - rid:1 - Registering remote route "NAABC2CKRVPZBIECMLZZA6L3PK"
[83329] 2020/02/12 16:05:09.663681 [DBG] 127.0.0.1:62941 - rid:1 - Sent local subscriptions to route
```

Finally, let's start the third server:

```bash
nats-server -p 6222 -cluster nats://localhost:6248 -routes nats://localhost:4248 -D
```

Again, notice that we use a different client port and cluster address, but still point to the same seed server at the address `nats://localhost:4248`:

```
[83331] 2020/02/12 16:05:12.838022 [INF] Listening for client connections on 0.0.0.0:6222
[83331] 2020/02/12 16:05:12.838029 [INF] Server id is NBE7SLUDLFIMHS2U6347N3DQEJ
[83331] 2020/02/12 16:05:12.838031 [INF] Server is ready
...
[83331] 2020/02/12 16:05:12.839203 [INF] Listening for route connections on localhost:6248
[83331] 2020/02/12 16:05:12.839453 [DBG] Trying to connect to route on localhost:4248
[83331] 2020/02/12 16:05:12.840112 [DBG] 127.0.0.1:4248 - rid:1 - Route connect msg sent
[83331] 2020/02/12 16:05:12.840198 [INF] 127.0.0.1:4248 - rid:1 - Route connection created
[83331] 2020/02/12 16:05:12.840324 [DBG] 127.0.0.1:4248 - rid:1 - Registering remote route "NDSGCS74MG5ZUMBOVWOUJ5S3HIOW"
[83331] 2020/02/12 16:05:12.840342 [DBG] 127.0.0.1:4248 - rid:1 - Sent local subscriptions to route
[83331] 2020/02/12 16:05:12.840717 [INF] 127.0.0.1:62946 - rid:2 - Route connection created
[83331] 2020/02/12 16:05:12.840906 [DBG] 127.0.0.1:62946 - rid:2 - Registering remote route "NAABC2CKRVPZBIECMLZZA6L3PK"
[83331] 2020/02/12 16:05:12.840915 [DBG] 127.0.0.1:62946 - rid:2 - Sent local subscriptions to route
```

First a route is created to the seed server (`...IOW`) and after that, a route from `...3PK` - which is the ID of the second server - is accepted.

The log from the seed server shows that it accepted the route from the third server:

```
[83329] 2020/02/12 16:05:12.840111 [INF] 127.0.0.1:62945 - rid:2 - Route connection created
[83329] 2020/02/12 16:05:12.840350 [DBG] 127.0.0.1:62945 - rid:2 - Registering remote route "NBE7SLUDLFIMHS2U6347N3DQEJ"
[83329] 2020/02/12 16:05:12.840363 [DBG] 127.0.0.1:62945 - rid:2 - Sent local subscriptions to route
```

And the log from the second server shows that it connected to the third.

```
[83330] 2020/02/12 16:05:12.840529 [DBG] Trying to connect to route on 127.0.0.1:6248
[83330] 2020/02/12 16:05:12.840684 [DBG] 127.0.0.1:6248 - rid:2 - Route connect msg sent
[83330] 2020/02/12 16:05:12.840695 [INF] 127.0.0.1:6248 - rid:2 - Route connection created
[83330] 2020/02/12 16:05:12.840814 [DBG] 127.0.0.1:6248 - rid:2 - Registering remote route "NBE7SLUDLFIMHS2U6347N3DQEJ"
[83330] 2020/02/12 16:05:12.840827 [DBG] 127.0.0.1:6248 - rid:2 - Sent local subscriptions to route
```

At this point, there is a full mesh cluster of NATS servers.

### Testing the Cluster

Now, the following should work: make a subscription to the first server (port 4222). Then publish to each server (ports 4222, 5222, 6222). You should be able to receive messages without problems.

Testing server A

```bash
nats sub -s "nats://127.0.0.1:4222" hello &
nats pub -s "nats://127.0.0.1:4222" hello world_4222
```

```
23:34:45 Subscribing on hello
23:34:45 Published 10 bytes to "hello"

[#1] Received on "hello"
world_4222
```

Testing server B

```shell
nats pub -s "nats://127.0.0.1:5222" hello world_5222
```

```
[#2] Received on "hello"
23:36:09 Published 10 bytes to "hello"
world_5222
```

Testing server C

```shell
nats pub -s "nats://127.0.0.1:6222" hello world_6222
```

```
23:38:40 Published 10 bytes to "hello"
[#3] Received on "hello"
world_6222
```

Testing using seed (i.e. A, B and C) server URLs

```shell
nats pub -s "nats://127.0.0.1:4222,nats://127.0.0.1:5222,nats://127.0.0.1:6222" hello whole_world
```

```
[#4] Received on "hello"
23:39:16 Published 11 bytes to "hello"
whole_world
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/clustering.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
