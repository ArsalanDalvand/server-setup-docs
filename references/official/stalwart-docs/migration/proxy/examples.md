---
title: "Overview"
source: "https://stalw.art/docs/migration/proxy/examples"
source_format: official-html-to-markdown
language: "en"
doc_slug: "migration/proxy/examples"
category: "migration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

The pages in this section present complete, working configurations for the three scenarios the proxy is designed for. Each is a self-contained TOML file with the surrounding context explained, and each can be adapted by substituting real hostnames, addresses, ports and certificate paths.

[[migration/proxy/examples/dovecot|Migrating from Dovecot]] fronts a legacy Dovecot and Postfix stack alongside a new Stalwart server, routing per account with file mappings and using `XCLIENT` forwarding for the legacy backend.
[[migration/proxy/examples/stalwart-upgrade|Upgrading Stalwart]] places an older and a newer Stalwart deployment behind the proxy, cutting accounts over one at a time and routing both mail and HTTP/JMAP traffic with PROXY protocol forwarding.
[[migration/proxy/examples/cluster|Cluster cache-locality routing]] uses the proxy in front of a Stalwart cluster to pin each account to a consistent node, keeping that node’s caches warm.

The general principles behind these configurations are covered under [[migration/proxy/destinations|Destinations]], [[migration/proxy/listeners|Listeners]], [[migration/proxy/mappings|Mappings]] and [[migration/proxy/routing|Routing]]; the examples show how the pieces fit together.