---
title: "Release Notes"
source: "https://apple.github.io/foundationdb/release-notes/release-notes-400.html"
source_format: official-rst
language: "en"
doc_slug: "release-notes/release-notes-400"
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

4.0.2
=====

Fixes
-----

* Streaming mode ``EXACT`` was ignoring the ``target_bytes`` parameter.

Java
----

* Added a ``toString`` method to the Tuple class.

4.0.1
=====

Fdbcli
------

* Added a ``configure auto`` command which will recommend a setting for proxies and logs (not resolvers) along with machine class changes.
* Added a ``setclass`` command which can change a process's machine class from ``fdbcli``.

Performance
-----------

* Improved the recovery speed of the transaction subsystem.
* Improved the stability of the transaction rate under saturating workloads.
* Made the transaction log more memory efficient.

Features
--------

* Added support for Versionstamp atomic operations.

Fixes
-----

* It was not safe to allocate multiple directories concurrently in the same transaction in the directory layer.

Bindings
--------

* API version updated to 400. See the [[api-version-upgrade-guide-400|API version upgrade guide ]] for upgrade details.

Java
----

* Changed the package for the Java bindings from com.foundationdb to com.apple.cie.foundationdb.

Python
------

* Tuple support for integers up to 255 bytes.

Other changes
-------------

* Added detailed metric logging available through Scope.
* An optional configuration parameter has been added that allows you to specify a seed cluster file.

Earlier release notes
---------------------
* [3.0 (API Version 300)](release-notes-300.html)
* [2.0 (API Version 200)](release-notes-200.html)
* [1.0 (API Version 100)](release-notes-100.html)
* [Beta 3 (API Version 23)](release-notes-023.html)
* [Beta 2 (API Version 22)](release-notes-022.html)
* [Beta 1 (API Version 21)](release-notes-021.html)
* [Alpha 6 (API Version 16)](release-notes-016.html)
* [Alpha 5 (API Version 14)](release-notes-014.html)
