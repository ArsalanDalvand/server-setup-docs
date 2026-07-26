---
title: "pve-ha-lrm(8)"
source: "https://pve.proxmox.com/pve-docs/pve-ha-lrm.8.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "pve-ha-lrm.8"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "pve-ha-lrm(8)"
---
[â](#)

## NAME

pve-ha-lrm - PVE Local Resource Manager Daemon

## SYNOPSIS

pve-ha-lrm <COMMAND> [ARGS] [OPTIONS] [](#cli_pve-ha-lrm)

pve-ha-lrm help [OPTIONS] [](#cli_pve-ha-lrm_help)

Get help about specified command.

--extra-args <array>

Shows help for a specific command

--verbose <boolean>

Verbose output format.

pve-ha-lrm start [OPTIONS] [](#cli_pve-ha-lrm_start)

Start the daemon.

--debug <boolean> (default = 0)

Debug mode - stay in foreground

pve-ha-lrm status [](#cli_pve-ha-lrm_status)

Get daemon status.

pve-ha-lrm stop [](#cli_pve-ha-lrm_stop)

Stop the daemon.

## DESCRIPTION

This is the Local Resource Manager Daemon.

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