---
title: "qmrestore(1)"
source: "https://pve.proxmox.com/pve-docs/qmrestore.1.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "qmrestore.1"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "qmrestore(1)"
---
[â](#)

## NAME

qmrestore - Restore QemuServer vzdump Backups

## SYNOPSIS

qmrestore help [](#cli_qmrestore_help)

qmrestore <archive> <vmid> [OPTIONS] [](#cli_qmrestore)

Restore QemuServer vzdump backups.

<archive>: <string>

The backup file. You can pass - to read from standard input.

<vmid>: <integer> (100 - 999999999)

The (unique) ID of the VM.

--bwlimit <number> (0 - N)

Override I/O bandwidth limit (in KiB/s).

--force <boolean>

Allow to overwrite existing VM.

--ha-managed <boolean> (default = 0)

Add the VM as a HA resource after it was restored.

--live-restore <boolean>

Start the VM immediately from the backup and restore in background. PBS only.

--pool <string>

Add the VM to the specified pool.

--start <boolean> (default = 0)

Start VM after it was restored successfully.

--storage <storage ID>

Default storage.

--unique <boolean>

Assign a unique random ethernet address.

## DESCRIPTION

Restore the QemuServer vzdump backup archive to virtual machine
vmid. Volumes are allocated on the original storage if there is no
storage specified.

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