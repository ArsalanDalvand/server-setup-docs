---
title: "Release Notes"
source: "https://apple.github.io/foundationdb/release-notes/release-notes-420.html"
source_format: official-rst
language: "en"
doc_slug: "release-notes/release-notes-420"
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

4.2.1
=====

Fixes
-----

* The Java bindings had an incorrectly named native extension on Linux.

4.2.0
=====
    
Features
--------

* A new utility allows [Backups](/backups.html) of a cluster directly into another FoundationDB database. It is designed also to support asynchronous replication and disaster recovery.
* A new version of the [Java bindings ](../javadoc-completable/index.html)  adds support for Java 8 Completable Futures.
* Information on the versions of connected clients has been added to [Machine-Readable Status](/mr-status.html).
* Information on the status of running backups has been added to [Machine-Readable Status](/mr-status.html).

Bindings
--------

* API version updated to 420. There are no behavior changes in this API version. See the [[api-version-upgrade-guide-420|API version upgrade guide ]] for upgrade details.

Earlier release notes
---------------------
* [4.1 (API Version 410)](release-notes-410.html)
* [4.0 (API Version 400)](release-notes-400.html)
* [3.0 (API Version 300)](release-notes-300.html)
* [2.0 (API Version 200)](release-notes-200.html)
* [1.0 (API Version 100)](release-notes-100.html)
* [Beta 3 (API Version 23)](release-notes-023.html)
* [Beta 2 (API Version 22)](release-notes-022.html)
* [Beta 1 (API Version 21)](release-notes-021.html)
* [Alpha 6 (API Version 16)](release-notes-016.html)
* [Alpha 5 (API Version 14)](release-notes-014.html)
