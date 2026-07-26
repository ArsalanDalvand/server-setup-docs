---
title: "Upgrade an existing Redis Software deployment"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/upgrading/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/upgrading"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Upgrade an existing Redis Software deployment"
---
# Upgrade an existing Redis Software deployment

How to upgrade a cluster's Redis Software version, database version, and operating system.

            Redis Software
          

## Upgrade Redis Software

To upgrade Redis Software:

Verify appropriate [[operate/rs/networking/port-configurations|network ports]] are either open or used by Redis Software.

Complete all [[operate/rs/installing-upgrading/upgrading/upgrade-cluster#upgrade-prerequisites|prerequisites]] before starting the upgrade.

Upgrade the software on all nodes of the cluster using one of the following methods:

[[operate/rs/installing-upgrading/upgrading/upgrade-cluster#in-place-upgrade|In-place upgrade]] - Directly upgrade Redis Software on each node in the cluster. Although this method is simpler than the rolling upgrade method, it might cause brief service interruptions as each node is upgraded.

[[operate/rs/installing-upgrading/upgrading/upgrade-cluster#rolling-upgrade|Rolling upgrade]] - Minimize downtime by adding new nodes with an updated Redis Software version to the cluster, one at a time, while keeping the rest of the cluster operational. This method is recommended for production environments that require continuous availability.

## Upgrade Redis database

[[operate/rs/installing-upgrading/upgrading/upgrade-database|Upgrade each database]] in the cluster or [[operate/rs/installing-upgrading/upgrading/upgrade-active-active|upgrade an Active-Active database]] to enable new features and important fixes.

## Upgrade operating system

To upgrade the cluster's operating system:

Complete all [[operate/rs/installing-upgrading/upgrading/upgrade-os#prerequisites|prerequisites]]  before starting the upgrade.

Use one of the following rolling upgrade methods:

[[operate/rs/installing-upgrading/upgrading/upgrade-os#extra-node-upgrade|Extra node method]] - Recommended if you have additional resources available.

[[operate/rs/installing-upgrading/upgrading/upgrade-os#replace-node-upgrade|Replace node method]] - Recommended if you cannot temporarily allocate additional resources.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page