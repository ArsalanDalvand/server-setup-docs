---
title: "Shell interface for the Proxmox VE API"
source: "https://pve.proxmox.com/pve-docs/chapter-pvesh.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "chapter-pvesh"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "Shell interface for the Proxmox VE API"
---
[â](#)

The Proxmox VE management tool (pvesh) allows to directly invoke API
function, without using the REST/HTTPS server.

Only root is allowed to do that.

## EXAMPLES

Get the list of nodes in my cluster

```bash
# pvesh get /nodes
```

Get a list of available options for the datacenter

```bash
# pvesh usage cluster/options -v
```

Set the HTMl5 NoVNC console as the default console for the datacenter

```bash
# pvesh set cluster/options -console html5
```