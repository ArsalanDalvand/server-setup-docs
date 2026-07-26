---
title: "Release Notes"
source: "https://apple.github.io/foundationdb/release-notes/release-notes-014.html"
source_format: official-rst
language: "en"
doc_slug: "release-notes/release-notes-014"
category: "release-notes"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Release Notes"
---
#############
Release Notes
#############

FoundationDB Alpha 5
====================

Language support
----------------

* FoundationDB now supports [[api-ruby|Ruby]]

* FoundationDB now supports Node.js

* FoundationDB now supports [Java ](/javadoc/index.html) and other JVM languages.

.. _alpha-5-rel-notes-features:

Features
--------

* A new [backup](/backups.html) system allows scheduled backups of a snapshot of the FoundationDB database to an external filesystem.
	
* [[docs/index|Integrated HTML documentation]]

* [[snapshot isolation|Snapshot reads ]] allow API clients to selectively relax FoundationDB's strong isolation guarantee. Appropriate use can of them can reduce [[conflict-ranges]] but makes reasoning about concurrency harder.

* [[streaming-mode-python|Streaming modes ]] allow API clients to adjust how FoundationDB transfers data for range reads for improved performance.

* Client APIs automatically detect the appropriate network interface (local address) when connecting to a cluster, and will look for a [[default-cluster-file]].

Compatibility
-------------

* Tuples encoded with prior alpha versions are incompatible with the tuple layer in Alpha 5.

* Databases created with Alpha 4 will be compatible. (See [[upgrading-from-older-versions|Upgrading from older versions ]] for upgrade instructions)

* Databases created before Alpha 4 will be incompatible. (See [[upgrading-from-older-versions|Upgrading from older versions ]] for details)

Changes to all APIs
-------------------

* The API version has been updated to 14.

* [[snapshot isolation|Snapshot reads ]] (see [[alpha-5-rel-notes-features|Features ]], above).

* [[streaming-mode-python|Streaming modes ]] (see [[alpha-5-rel-notes-features|Features ]], above).

* Automatic network interface detection (see [[alpha-5-rel-notes-features|Features ]], above).

* The tuple layer supports unicode strings (encoded as UTF-8), has a more compact encoding, and is not compatible with data from prior versions.

* Reversed range reads are now exposed through a separate parameter rather than via a negative ``limit``.

* Extensible options are now exposed at the network, cluster, database and transaction levels. The parameters to :c:func:`fdb_setup_network` and :py:func:`fdb.init` have been replaced by network options.

* Option enumerations are available in a machine-readable format for the benefit of third-party language binding developers.

Python API changes
------------------

* :py:func:`fdb.open` can be called with no parameters to use the [[default-cluster-file]].

* Waiting on a Future object has changed from ``.get()`` to :py:meth:`.wait() <fdb.Future.wait>`

* Reversed range reads can by specified by passing a slice object with a -1 step.

* The convenience read methods on :py:class:`fdb.Database` are now transactional.

C API changes
-------------

* Byte limits exposed in :c:func:`fdb_transaction_get_range`.  These are not currently exposed by any of the higher level clients (and usually streaming modes should be preferred).

* :c:func:`fdb_future_get_keyvalue_array` returns an explicit flag indicating whether there is more data in the range beyond the limits passed to :c:func:`fdb_transaction_get_range`.
 
* ``fdb_transaction_get_range_selector`` has been eliminated - :c:func:`fdb_transaction_get_range` always takes key selectors.
