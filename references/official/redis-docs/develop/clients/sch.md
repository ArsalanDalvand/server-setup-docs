---
title: "Smart client handoffs"
source: "https://redis.io/docs/latest/develop/clients/sch/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/clients/sch"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Smart client handoffs"
---
# Smart client handoffs

Learn how to avoid disruptions during Redis server maintenance.

Smart client handoffs (SCH) is a feature of Redis Cloud and
Redis Software servers that lets them actively notify clients
about planned server maintenance shortly before it happens. This
lets a client reconnect or otherwise respond gracefully without significant
interruptions in service.

SCH is primarily useful when server software or hardware upgrades
are required. It provides two main features to help the
client avoid disruptions in service during the maintenance period:

Relaxed timeouts: Upgrades tend to impact the general performance of the server.
Advance notification of the upgrade lets a client adjust its command
timeouts to take this into account and avoid aborting commands too soon.
Pre-handoffs: Upgrades also involve migrating
Redis shards to new nodes, which inevitably disconnects clients from
existing nodes. However, with some advance warning of the disconnection,
a client can buffer commands, connect to a new node, and then resume
the buffered commands without aborting any of them. As a result, users
see no disruption in service. These transparent reconnections to new endpoints
are known as pre-handoffs.

Note:
  SCH does not work with blocking connections.
These include connections used for blocking operations like
[`BLPOP`](/docs/latest/commands/blpop/) and also
[[develop/pubsub|pub/sub]] subscriptions.
All non-blocking operations are safe to use with SCH.

## SCH support in Redis client libraries

SCH is enabled automatically on the client side during connection
if you select the [[develop/reference/protocol-spec#resp-versions|RESP3]]
protocol, which is a requirement for SCH. However, you can
configure some parameters, such as the timeouts to use
during maintenance.

The table below lists the Redis client libraries that support SCH,
and the versions that added support for basic connections and
[OSS Cluster API](/docs/latest/operate/rs/databases/configure/oss-cluster-api/) connections.

Client
Basic connection
OSS Cluster API
Client-side geographic failover

[[develop/clients/redis-py/connect#connect-using-smart-client-handoffs-sch|redis-py]]
v7.0.0
v7.2.0
Disabled

[node-redis](/docs/latest/develop/clients/nodejs/connect/#connect-using-smart-client-handoffs-sch)
v5.9.0
v5.11.0
Disabled

[Lettuce](/docs/latest/develop/clients/lettuce/connect/#connect-using-smart-client-handoffs-sch)
v7.0.0
-
Disabled

[go-redis](/docs/latest/develop/clients/go/connect/#connect-using-smart-client-handoffs-sch)
v9.16.0
v9.18.0
Disabled

Note:
  SCH is currently disabled when a client is configured for
[[develop/clients/failover|Client-side geographic failover]].
Integration of the two features is planned for a future release.

## SCH support in Redis server products

### Redis Cloud

SCH is fully supported and enabled by default on Redis Cloud, except when you
are using one of the following options:

[[operate/rc/security/aws-privatelink|AWS PrivateLink]]
[[operate/rc/security/private-service-connect|Google Cloud Private Service Connect]]

These services don't currently allow for pre-handoffs, but you still get the
benefit of relaxed timeouts during database version upgrades. All other
configurations have full support for both relaxed timeouts and pre-handoffs.

### Redis Software

You must enable SCH explicitly on self-managed Redis Software servers by using the
[v1/cluster](/docs/latest/operate/rs/references/rest-api/requests/cluster/)
REST API request to set the `client_maint_notifications` option to `true`.
The example below shows how to do this using the
[`curl`](https://curl.se/) command line utility:

```bash
curl -k -X PUT -H "accept: application/json" \
    -H "content-type: application/json" \
    -u "[email protected]:test123" \
    -d '{ "client_maint_notifications": true }' \
    https://localhost:9443/v1/cluster
```

Redis Software uses relaxed timeouts to avoid command failures during
database version upgrades. SCH is supported for Redis Software from v8.0.2 on,
and OSS Cloud and OSS Cluster API from v8.0.16 on.

The degree of support for pre-handoffs depends on
the specific upgrade method you use, as detailed in the table below.

Upgrade method
SCH support
Expected behavior

[[operate/rs/installing-upgrading/upgrading/upgrade-cluster#rolling-upgrade|Rolling upgrade]]
Full
New nodes and old ones removed sequentially. SCH pre-handoffs and relaxed timeouts greatly reduce disruptions during the upgrade.

[[operate/rs/installing-upgrading/upgrading/upgrade-cluster#in-place-upgrade|In-place upgrade]]
Partial
Relaxed timeouts reduce errors but there are no pre-handoffs. Disconnections occur when processes are replaced during the upgrade, so clients should rely on auto-reconnect, which will cause brief lapses in service.

[Maintenance mode](/docs/latest/operate/rs/clusters/maintenance-mode/)
Full
SCH is fully supported during hardware or OS patching operations. Pre-handoffs and relaxed timeouts minimize application impact.

### Redis Enterprise for Kubernetes

SCH is not currently supported for [Kubernetes](/docs/latest/operate/kubernetes/) clusters.

### Redis Open Source

SCH is not currently supported for [[operate/oss_and_stack|Redis Open Source]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page