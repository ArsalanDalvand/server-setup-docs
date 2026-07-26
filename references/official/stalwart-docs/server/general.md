---
title: "General"
source: "https://stalw.art/docs/server/general"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/general"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "General"
---
# General

      

General server settings are grouped on the [[ref/object/system-settings|SystemSettings]] singleton (found in the WebUI under  Settings ›  Network › Services,  Settings ›  Network › General). This page covers the hostname, the CPU thread pool, and IP address allow / block lists. Per-listener concurrency is described on the [[server/listener|Listeners]] page.

## Server hostname

[Section titled “Server hostname”](#server-hostname)

The default server hostname is used in SMTP `EHLO` commands, in message headers, and in reports. It is configured through [[ref/object/system-settings#defaulthostname|`defaultHostname`]] on the SystemSettings singleton. If the field is left unset, the server falls back to the hostname reported by the operating system.

## Thread pool size

[Section titled “Thread pool size”](#thread-pool-size)

Stalwart runs CPU-intensive tasks on a dedicated thread pool. The pool size is controlled by [[ref/object/system-settings#threadpoolsize|`threadPoolSize`]] on the SystemSettings singleton. When left unset, the server allocates one worker per CPU core detected at start-up. Raising the value above the number of available cores does not generally improve throughput.

## IP address blocking

[Section titled “IP address blocking”](#ip-address-blocking)

IP addresses and ranges can be rejected at connection time. Each blocked entry is represented by a [[ref/object/blocked-ip|BlockedIp]] record (found in the WebUI under  Settings ›  Security › Blocked IPs); the [[ref/object/blocked-ip#address|`address`]] field accepts a single IP address or a CIDR mask such as `203.0.113.0/24`, and [[ref/object/blocked-ip#expiresat|`expiresAt`]] can be set to make the block temporary.

When [[server/auto-ban|auto-banning]] is enabled, the server creates BlockedIp records automatically for sources that exceed the configured thresholds. Manual entries can be managed from the [[management/webui|WebUI]] or the [[management/cli|CLI]].

## IP address allow list

[Section titled “IP address allow list”](#ip-address-allow-list)

IP addresses and ranges that should bypass rate limits and auto-banning are represented by [[ref/object/allowed-ip|AllowedIp]] records (found in the WebUI under  Settings ›  Security › Allowed IPs). The [[ref/object/allowed-ip#address|`address`]] field accepts a single IP address or a CIDR mask, and [[ref/object/allowed-ip#expiresat|`expiresAt`]] can be used for time-bounded allowances. The localhost addresses `127.0.0.1` and `::1` are allowed by default to prevent accidental lockouts.

## Run as user

[Section titled “Run as user”](#run-as-user)

On Linux systems, Stalwart requires the `CAP_NET_BIND_SERVICE` capability to bind to privileged ports (those below 1024). On older systems that do not support this capability, Stalwart can be started as `root` and then drop privileges to a non-privileged account. The target user and group are selected through the `RUN_AS_USER` and `RUN_AS_GROUP` environment variables.