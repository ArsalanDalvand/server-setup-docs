---
title: "Getting Started on macOS"
source: "https://apple.github.io/foundationdb/getting-started-mac.html"
source_format: official-rst
language: "en"
doc_slug: "getting-started-mac"
category: "foundationdb"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Getting Started on macOS"
---
.. _getting-started-mac:

########################
Getting Started on macOS
########################

.. include:: guide-common.rst.inc

This guide walks through installing a locally accessible FoundationDB server that is suitable for development on macOS.

.. note:: |platform-not-supported-for-production|

First steps
===========

* Validate that your system has

  * x86-64 processor architecture
  * 4 GB RAM (per process)
  * macOS 10.7 or newer

* Download the FoundationDB packages for your system from [[downloads|downloads]].

* Before upgrading from a previous version of FoundationDB, see [[upgrading-foundationdb]].

Installing or upgrading FoundationDB
====================================

To begin installation, double-click on |package-mac|. Follow the instructions and select the components that you want to install. 

Client-only installation
------------------------

By default, the FoundationDB installer installs the binaries required to run both clients and a local development server. If you don't intend to run the FoundationDB server on your machine, you can deselect the "FoundationDB Server" option. Copy the [[foundationdb-cluster-file|cluster file ]] from a server or client in the cluster you want to connect to and place it in ``/usr/local/etc/foundationdb/``.

Other considerations
--------------------

|simple-installation-mode-warnings|

|networking-clarification|
	
Testing your FoundationDB installation
======================================

To verify that the local FoundationDB database is operational, open the command line interface (``fdbcli``) and use the status command. ::

  host:~ user$ fdbcli
  Using cluster file `/usr/local/etc/foundationdb/fdb.cluster'.

  The database is available.

  Welcome to the fdbcli. For help, type `help'.
  fdb> status

  Configuration:
    Redundancy mode        - single
    Storage engine         - memory
    Coordinators           - 1

  Cluster:
    FoundationDB processes - 1
    Machines               - 1
    Memory availability    - 4.1 GB per process on machine with least available
    Fault Tolerance        - 0 machines
    Server time            - Thu Mar 15 14:41:34 2018
    
  Data:
    Replication health     - Healthy
    Moving data            - 0.000 GB
    Sum of key-value sizes - 8 MB
    Disk space used        - 103 MB

  Operating space:
    Storage server         - 1.0 GB free on most full server
    Transaction log        - 1.0 GB free on most full server

  Workload:
    Read rate              - 2 Hz
    Write rate             - 0 Hz
    Transactions started   - 2 Hz
    Transactions committed - 0 Hz
    Conflict rate          - 0 Hz

  Backup and DR:
    Running backups        - 0
    Running DRs            - 0

  Client time: Thu Mar 15 14:41:34 2018

If these steps were successful you have installed and validated FoundationDB. You can now start using the database!

.. note:: If the database is not operational the ``status`` command will provide diagnostic information to help you resolve the issue.

Managing the FoundationDB service
==================================

* See [[administration-running-foundationdb]].
* See [[administration-removing]].

Next steps
==========

* Install the APIs for [[api-ruby|Ruby]], [Java ](javadoc/index.html), or [Go ](https://godoc.org/github.com/apple/foundationdb/bindings/go/src/fdb) if you intend to use those languages.  [[api-python|Python]] and [[api-c|C]] APIs were installed using the FoundationDB installer above.
* See [[jozve/server-setup-docs/references/official/foundationdb-docs/tutorials|tutorials]] for samples of developing applications with FoundationDB.
* See [[developer-guide|developer-guide]] for information of interest to developers, including common design patterns and performance considerations.
* See [[administration|administration]] for detailed administration information.
* See [[known-limitations|known-limitations]] of the system.

