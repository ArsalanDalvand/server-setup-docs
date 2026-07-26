---
title: "File locations"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/plan-deployment/file-locations/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install/plan-deployment/file-locations"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "File locations"
---
# File locations

Redis Software file installation locations.

            Redis Software
          

Warning:
  To ensure that Redis Software functions properly, be careful with the files in the application directories. If you modify or delete the application files, Redis Software might not work as expected.

## Application directories

The directories that Redis Software installs into are:

Path
Description

/opt/redislabs
Main installation directory for all Redis Software binaries

/opt/redislabs/bin
Binaries for all the utilities for command-line access and management, such as [`rladmin`](/docs/latest/operate/rs/references/cli-utilities/rladmin/) or [`redis-cli`](/docs/latest/operate/rs/references/cli-utilities/redis-cli/)

/opt/redislabs/config
System configuration files

/opt/redislabs/lib
System library files

/opt/redislabs/sbin
System binaries for tweaking provisioning

## Configuration and data directories

The default directories that Redis Software uses for data and metadata are:

Path
Description

/var/opt/redislabs
Default storage location for the cluster data, system logs, backups, and ephemeral, persisted data

/var/opt/redislabs/log
System logs for Redis Software

/var/opt/redislabs/run
Socket files for Redis Software

/etc/opt/redislabs
Default location for cluster manager configuration and certificates

/tmp
The /tmp filesystem is for temporary files and should be sized according to this formula: `<number-of-cluster-nodes> * <size-of-/var/opt/redislabs/log-filesystem>`

You can change these file locations for:

[[operate/rs/clusters/new-cluster-setup|Ephemeral and persistence storage]] during cluster setup
[Socket files](/docs/latest/operate/rs/installing-upgrading/configuring/change-location-socket-files/) after cluster setup

Note:
  To specify custom paths for ephemeral and persistent storage during cluster setup, you must install Redis Software using [custom installation directories](/docs/latest/operate/rs/installing-upgrading/install/customize-install-directories/). If you use the default installation, you cannot specify custom storage paths and will receive a "path not mounted" error.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page