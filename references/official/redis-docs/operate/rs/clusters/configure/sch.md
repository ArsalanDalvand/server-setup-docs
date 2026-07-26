---
title: "Smart client handoffs"
source: "https://redis.io/docs/latest/operate/rs/clusters/configure/sch/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/configure/sch"
category: "operate"
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

Enable Smart client handoffs for your Redis Software cluster.

            Redis Software
          

Smart client handoffs (SCH) is a feature of Redis Cloud and Redis Software servers that lets them actively notify clients about planned server maintenance shortly before it happens. This lets a client reconnect or otherwise respond gracefully without significant interruptions in service.
See [[develop/clients/sch|Smart client handoffs]] for more information about SCH.

SCH is supported for Redis Software from v8.0.2 on, and OSS Cluster API from v8.0.16 on.
The degree of support for SCH depends on the specific upgrade method you use, as detailed in the table below.

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

To enable SCH on a Redis Software server, you must use the
[/v1/cluster](/docs/latest/operate/rs/references/rest-api/requests/cluster/#put-cluster)
REST API request to set the `client_maint_notifications` option to `true`.
The example below shows how to do this using the
[`curl`](https://curl.se/) command line utility:

```bash
curl -k -X PUT -H "accept: application/json" \
    -H "content-type: application/json" \
    -u "<username>:<password>" \
    -d '{ "client_maint_notifications": true }' \
    https://<host>:<port>/v1/cluster
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)