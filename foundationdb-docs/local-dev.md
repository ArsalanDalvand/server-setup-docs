---
title: "begin local development"
source: "https://apple.github.io/foundationdb/local-dev.html"
source_format: official-rst
language: "en"
doc_slug: "local-dev"
category: "foundationdb"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "begin local development"
---
#################
Local Development 
#################

Download the FoundationDB package
=================================

[[downloads|Download the FoundationDB package]] for macOS (FoundationDB-\*.pkg) onto your local development machine.

Install the FoundationDB binaries
=================================

By default, the FoundationDB installer installs the binaries required to run both a client and a local development server. Begin installation by double-clicking on the downloaded FoundationDB package and following the displayed instructions.

For more details on installing on macOS, see [[getting-started-mac|getting-started-mac]].

If you later wish to remove FoundationDB from a machine, follow the instruction for [[administration-removing|uninstalling ]].

Check the status of the local database
======================================

You can verify the status of the local database with the following the command, which will show basic statistics::

    fdbcli --exec status

Basic tutorial
==============

Here's a [[class-scheduling|tutorial]] that begins with "Hello world" code for connecting to the database and then walks through the basics of reading and writing data with transactions.
