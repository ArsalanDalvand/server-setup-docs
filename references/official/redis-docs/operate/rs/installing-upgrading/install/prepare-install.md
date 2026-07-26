---
title: "Prepare to install Redis Software"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/prepare-install/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install/prepare-install"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Prepare to install Redis Software"
---
# Prepare to install Redis Software

Prepare to install Redis Software.

            Redis Software
          

Before you install Redis Software:

[[operate/rs/installing-upgrading/install/prepare-install/download-install-package|Download an installation package]].

[[operate/rs/installing-upgrading/install/manage-installation-questions|View installation questions]] and optionally prepare answers before installation.

Review the [[operate/rs/security|security considerations]] for your deployment.

Check that you have root-level access to each node, either directly or with `sudo`.

Check that all [[operate/rs/installing-upgrading/install/prepare-install/port-availability|required ports are available]].

[[operate/rs/installing-upgrading/configuring/linux-swap|Turn off Linux swap]] on all cluster nodes.

If you require the `redislabs` UID (user ID) and GID (group ID) numbers to be the same on all the nodes, create the `redislabs` user and group with the required numbers on each node.

If you want to use Auto Tiering for your databases, see [[operate/rs/installing-upgrading/install/install-on-linux#auto-tiering-installation|Auto Tiering installation]].

## Next steps

View [installation script options](/docs/latest/operate/rs/installing-upgrading/install/install-script/) before starting the installation.

[[operate/rs/installing-upgrading/install|Install Redis Software]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page