---
title: "Load balancing"
source: "https://stalw.art/docs/cluster/configuration/load-balancing"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/configuration/load-balancing"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Load balancing"
---
# Load balancing

      

By default, Stalwart derives the server hostname from the underlying system hostname during installation. The hostname appears in protocol responses, logs, and internal identification. When Stalwart is deployed behind a load balancer, all backend nodes should use a consistent hostname that matches the public-facing identity of the service. This keeps client communications and protocol-level interactions uniform, particularly for protocols such as SMTP that expose the server hostname during the handshake.

The hostname is set on the [[ref/object/system-settings|SystemSettings]] singleton (found in the WebUI under  Settings ›  Network › Services,  Settings ›  Network › General) through the [[ref/object/system-settings#defaulthostname|`defaultHostname`]] field.

For example, with three Stalwart nodes (`mx1.example.org`, `mx2.example.org`, and `mx3.example.org`) behind a load balancer that publicly serves mail as `mx.example.org`, each node sets `defaultHostname` to the public identity:

```
{  "defaultHostname": "mx.example.org"}
```

All nodes then respond with the same hostname (`mx.example.org`), keeping clients consistent and avoiding confusion or misidentification.

Failing to align this setting leads to inconsistent SMTP banners, TLS certificates, and protocol-level responses, especially when multiple backend nodes serve the same clients in a round-robin or failover configuration.

Setting [[ref/object/system-settings#defaulthostname|`defaultHostname`]] once on the singleton is the correct approach in a load-balanced cluster. There is no per-node hostname override: the value supplied on the singleton applies across the cluster and is used in SMTP banners, authentication reports, and other protocol-level responses. Each node also has an operating-system hostname, but that value is read only to acquire a [[cluster/configuration/node-id|node-id lease]] and is not exposed to clients.