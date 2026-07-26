---
title: "pvestatd(8)"
source: "https://pve.proxmox.com/pve-docs/pvestatd.8.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "pvestatd.8"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "pvestatd(8)"
---
[â](#)

## NAME

pvestatd - PVE Status Daemon

## SYNOPSIS

pvestatd <COMMAND> [ARGS] [OPTIONS] [](#cli_pvestatd)

pvestatd help [OPTIONS] [](#cli_pvestatd_help)

Get help about specified command.

--extra-args <array>

Shows help for a specific command

--verbose <boolean>

Verbose output format.

pvestatd restart [](#cli_pvestatd_restart)

Restart the daemon (or start if not running).

pvestatd start [OPTIONS] [](#cli_pvestatd_start)

Start the daemon.

--debug <boolean> (default = 0)

Debug mode - stay in foreground

pvestatd status [](#cli_pvestatd_status)

Get daemon status.

pvestatd stop [](#cli_pvestatd_stop)

Stop the daemon.

## DESCRIPTION

This daemon queries the status of VMs, storages and containers at
regular intervals. The result is sent to all nodes in the cluster.

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