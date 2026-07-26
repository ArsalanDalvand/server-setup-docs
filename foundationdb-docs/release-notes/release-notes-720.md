---
title: "Release Notes"
source: "https://apple.github.io/foundationdb/release-notes/release-notes-720.html"
source_format: official-rst
language: "en"
doc_slug: "release-notes/release-notes-720"
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

7.2.0
======

Features
--------
- Read-aware Data Distribution feature is developed for balance the read bytes bandwidth among storage servers.

Performance
-----------

Reliability
-----------

Fixes
-----

* In ``fdbcli``, integer options are now expressed as integers rather than byte strings (e.g. ``option on TIMEOUT 1000``). [(PR #7571) ](https://github.com/apple/foundationdb/pull/7571)
* Fixed the bug in ``ConflictingKeysImpl::getRange`` which happens when an underlying conflicting range contains the read range. Added additional test coverage for validating random ``getRange`` results from special keys. [(PR #7597) ](https://github.com/apple/foundationdb/pull/7597)
* Fixed the bug in ``SpecialKeyRangeAsyncImpl::getRange`` that the local cache is updated incorrectly after a cross-module read range if it touched more than one ``SpecialKeyRangeAsyncImpl`` in resolving key selectors. Extended the ``SpecialKeySpaceCorrectness`` workload to catch the bug. [(PR #7671) ](https://github.com/apple/foundationdb/pull/7671)

Status
------

Bindings
--------

Other Changes
-------------

Earlier release notes
---------------------
* [[release-notes/release-notes-710|7.1 (API Version 710)]]
* [[release-notes/release-notes-700|7.0 (API Version 700)]]
* [[release-notes/release-notes-630|6.3 (API Version 630)]]
* [[release-notes/release-notes-620|6.2 (API Version 620)]]
* [[release-notes/release-notes-610|6.1 (API Version 610)]]
* [[release-notes/release-notes-600|6.0 (API Version 600)]]
* [[release-notes/release-notes-520|5.2 (API Version 520)]]
* [[release-notes/release-notes-510|5.1 (API Version 510)]]
* [[release-notes/release-notes-500|5.0 (API Version 500)]]
* [[release-notes/release-notes-460|4.6 (API Version 460)]]
* [[release-notes/release-notes-450|4.5 (API Version 450)]]
* [[release-notes/release-notes-440|4.4 (API Version 440)]]
* [[release-notes/release-notes-430|4.3 (API Version 430)]]
* [[release-notes/release-notes-420|4.2 (API Version 420)]]
* [[release-notes/release-notes-410|4.1 (API Version 410)]]
* [[release-notes/release-notes-400|4.0 (API Version 400)]]
* [[release-notes/release-notes-300|3.0 (API Version 300)]]
* [[release-notes/release-notes-200|2.0 (API Version 200)]]
* [[release-notes/release-notes-100|1.0 (API Version 100)]]
* [[release-notes/release-notes-023|Beta 3 (API Version 23)]]
* [[release-notes/release-notes-022|Beta 2 (API Version 22)]]
* [[release-notes/release-notes-021|Beta 1 (API Version 21)]]
* [[release-notes/release-notes-016|Alpha 6 (API Version 16)]]
* [[release-notes/release-notes-014|Alpha 5 (API Version 14)]]
