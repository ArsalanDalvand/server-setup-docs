---
title: "qmeventd(8)"
source: "https://pve.proxmox.com/pve-docs/qmeventd.8.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "qmeventd.8"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "qmeventd(8)"
---
[â](#)

## NAME

qmeventd - PVE QEMU Eventd Daemon

## SYNOPSIS

qmeventd [-f] [-v] PATH

-v

Turn on verbose messages

-f

Do not daemonize server

PATH

The path to listen on

This service is usually started and managed using systemd toolset. The
service is called qmeventd.

```bash
systemctl start qmeventd
```

```bash
systemctl stop qmeventd
```

```bash
systemctl status qmeventd
```

## DESCRIPTION

qmeventd is a daemon that listens on PATH for incoming connections from
a qemu qmp socket, and waits for SHUTDOWN events. When a client then
disconnects, it executes /usr/sbin/qm cleanup. This makes it easy
to clean up leftover tap devices, vgpus, etc.

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