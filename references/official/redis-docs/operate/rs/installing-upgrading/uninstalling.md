---
title: "Uninstall Redis Software"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/uninstalling/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/uninstalling"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Uninstall Redis Software"
---
# Uninstall Redis Software

            Redis Software
          

Use the script `rl_uninstall.sh` to uninstall Redis Software and remove its files from a node. The script also deletes all Redis data and configuration from the node.

The uninstall script does not remove the node from the cluster, but the node's status changes to down. For node removal instructions, see [Remove a cluster node](/docs/latest/operate/rs/clusters/remove-node/).

## Uninstall Redis Software

To uninstall Redis Software from a cluster node:

Navigate to the script's location, which is in `/opt/redislabs/bin/` by default.

Run the uninstall script as the root user:

```sh
sudo ./rl_uninstall.sh
```

When you run the uninstall script on a node, it only uninstalls Redis Software from that node. To uninstall Redis Software for the entire cluster, run the uninstall script on each cluster node.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page