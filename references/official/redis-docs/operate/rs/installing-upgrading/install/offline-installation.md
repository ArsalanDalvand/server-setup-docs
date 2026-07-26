---
title: "Offline installation"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/offline-installation/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install/offline-installation"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Offline installation"
---
# Offline installation

If you install Redis Software on a machine with no internet connection, you need to perform two tasks manually.

            Redis Software
          

By default, the installation process requires an internet connection to
enable installing dependency packages and for [[operate/rs/clusters/configure/sync-clocks|synchronizing the
operating system clock]] against an NTP server.

If you install Redis Software on a machine without an
internet connection, you need to perform two tasks manually.

## Install required dependency packages

When you install Redis Software on a machine that is not connected to the internet, the installation process fails and displays an error message informing you it failed to automatically install dependencies. Review the installation steps in the console to see which missing dependencies the process attempted to install. Install all these dependency packages and then run the installation again.

## Set up NTP time synchronization

At the end of the installation, the process asks if you want to set up NTP time synchronization. If you choose `Yes` while you are not connected to the internet, the action fails and displays the appropriate error message, but the installation completes successfully. Despite the successful completion of the installation, you still have to configure all nodes for [[operate/rs/clusters/configure/sync-clocks|NTP time synchronization]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page