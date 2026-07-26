---
title: "pvedaemon(8)"
source: "https://pve.proxmox.com/pve-docs/pvedaemon.8.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "pvedaemon.8"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "pvedaemon(8)"
---
[â](#)

## NAME

pvedaemon - PVE API Daemon

## SYNOPSIS

pvedaemon <COMMAND> [ARGS] [OPTIONS] [](#cli_pvedaemon)

pvedaemon help [OPTIONS] [](#cli_pvedaemon_help)

Get help about specified command.

--extra-args <array>

Shows help for a specific command

--verbose <boolean>

Verbose output format.

pvedaemon restart [](#cli_pvedaemon_restart)

Restart the daemon (or start if not running).

pvedaemon start [OPTIONS] [](#cli_pvedaemon_start)

Start the daemon.

--debug <boolean> (default = 0)

Debug mode - stay in foreground

pvedaemon status [](#cli_pvedaemon_status)

Get daemon status.

pvedaemon stop [](#cli_pvedaemon_stop)

Stop the daemon.

## DESCRIPTION

This daemon exposes the whole Proxmox VE API on 127.0.0.1:85. It runs as
root and has permission to do all privileged operations.

The daemon listens to a local address only, so you cannot access
it from outside. The pveproxy daemon exposes the API to the outside
world.

## Number of Workers

pvedaemon delegates handling of incoming requests to worker processes. By
default, pvedaemon spawns 3 worker processes, which is sufficient for most
workloads. For automation-heavy workloads that issue a huge volume of API
requests and that experience slow request handling or timeouts, the number of
worker processes can be increased by setting MAX_WORKERS in
/etc/default/pvedaemon, for example:

```bash
MAX_WORKERS=5
```

Note that a higher number of worker processes may result in higher CPU usage.
The number of worker processes must be greater than 0 and smaller than 128.

The same setting exists for [[pveproxy.8.html#pveproxy_max_workers|pveproxy]].

## Copyright and Disclaimer

Copyright © 2007-2022 Proxmox Server Solutions GmbH

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public
License along with this program.  If not, see
[https://www.gnu.org/licenses/](https://www.gnu.org/licenses/)