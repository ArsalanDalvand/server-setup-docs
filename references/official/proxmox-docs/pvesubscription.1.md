---
title: "pvesubscription(1)"
source: "https://pve.proxmox.com/pve-docs/pvesubscription.1.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "pvesubscription.1"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "pvesubscription(1)"
---
[â](#)

## NAME

pvesubscription - Proxmox VE Subscription Manager

## SYNOPSIS

pvesubscription <COMMAND> [ARGS] [OPTIONS] [](#cli_pvesubscription)

pvesubscription delete [](#cli_pvesubscription_delete)

Delete subscription key of this node.

pvesubscription get [](#cli_pvesubscription_get)

Read subscription info.

pvesubscription help [OPTIONS] [](#cli_pvesubscription_help)

Get help about specified command.

--extra-args <array>

Shows help for a specific command

--verbose <boolean>

Verbose output format.

pvesubscription set <key> [](#cli_pvesubscription_set)

Set subscription key.

<key>: \s*pve([1248])([cbsp])-[0-9a-f]{10}\s*

Proxmox VE subscription key

pvesubscription set-offline-key <data> [](#cli_pvesubscription_set-offline-key)

Internal use only! To set an offline key, use the package
proxmox-offline-mirror-helper instead.

<data>: <string>

A signed subscription info blob

pvesubscription update [OPTIONS] [](#cli_pvesubscription_update)

Update subscription info.

--force <boolean> (default = 0)

Always connect to server, even if local cache is still valid.

## DESCRIPTION

This tool is used to handle Proxmox VE subscriptions.

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