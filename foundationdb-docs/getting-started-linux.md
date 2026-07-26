---
title: "Getting Started on Linux"
source: "https://apple.github.io/foundationdb/getting-started-linux.html"
source_format: official-rst
language: "en"
doc_slug: "getting-started-linux"
category: "foundationdb"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Getting Started on Linux"
---
.. _getting-started-linux:

########################
Getting Started on Linux
########################

.. include:: guide-common.rst.inc

This guide walks through installing a locally accessible FoundationDB server that is suitable for development on Linux.

To install an externally accessible FoundationDB cluster on one or more machines, see [building-cluster](building-cluster.html).

First steps
===========

* Validate your system meets the [[system-requirements]].

* Download the FoundationDB packages for your system from [[downloads|downloads]].

* Before upgrading from a previous version of FoundationDB, see [[upgrading-foundationdb]].

Installing or upgrading FoundationDB packages
=============================================

.. warning:: |upgrade-client-server-warning|

To install on **Ubuntu** use the dpkg command:

.. parsed-literal::

    user@host$ sudo dpkg -i |package-deb-clients| \\
    |package-deb-server|

To install on **RHEL/CentOS 6** use the rpm command:

.. parsed-literal::

    user@host$ sudo rpm -Uvh |package-rpm-clients| \\
    |package-rpm-server|

To install on **RHEL/CentOS 7** use the rpm command:

.. parsed-literal::

    user@host$ sudo rpm -Uvh |package-rpm-clients| \\
    |package-rpm-server|

|simple-installation-mode-warnings|

|networking-clarification|
	
Testing your FoundationDB installation
======================================

To verify that the local FoundationDB database is operational, open the command line interface (``fdbcli``) and use the status command. ::

  user@host$ fdbcli
  Using cluster file `/etc/foundationdb/fdb.cluster'.

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

* Install the APIs for [[api-ruby|Ruby]], [[api-python|Python]], [Java ](javadoc/index.html) or [Go ](https://godoc.org/github.com/apple/foundationdb/bindings/go/src/fdb) if you intend to use those languages. The [[api-c|C]] API was installed along with the ``foundationdb-clients`` package above.
* See [[tutorials|tutorials]] for samples of developing applications with FoundationDB.
* See [[developer-guide|developer-guide]] for information of interest to developers, including common design patterns and performance considerations.
* See [[administration|administration]] for detailed administration information.
* See [[known-limitations|known-limitations]] of the system.
* See [building-cluster](building-cluster.html) for step-by-step instructions on converting your local single-machine cluster to an externally visible cluster of one or more machines.

