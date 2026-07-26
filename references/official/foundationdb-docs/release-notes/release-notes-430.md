---
title: "Release Notes"
source: "https://apple.github.io/foundationdb/release-notes/release-notes-430.html"
source_format: official-rst
language: "en"
doc_slug: "release-notes/release-notes-430"
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

4.3.0
=====
    
Features
--------

* Improved DR thoughput by having mutations copied into the DR database before applying them.
* Renamed db_agent to dr_agent.
* Added more detailed DR and backup active task detail into layer status.

Fixes
-----

* Backup seconds behind did not update in continuous mode.
* DR layer status did not report correctly.
* The Java bindings had an incorrectly named native extension on Linux.
* DR status would crash if called before a DR had been started.
* Changed the blob restore read pattern to work around blob store issues.
* External clients do not load environment variable options.

Bindings
--------

* API version updated to 430. There are no behavior changes in this API version. See the [[api-version-upgrade-guide-430|API version upgrade guide ]] for upgrade details.

Earlier release notes
---------------------
* [4.2 (API Version 420)](release-notes-420.html)
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
