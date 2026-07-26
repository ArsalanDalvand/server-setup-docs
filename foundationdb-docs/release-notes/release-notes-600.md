---
title: "Release Notes"
source: "https://apple.github.io/foundationdb/release-notes/release-notes-600.html"
source_format: official-rst
language: "en"
doc_slug: "release-notes/release-notes-600"
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

6.0.18
======

Fixes
-----

* Backup metadata could falsely indicate that a backup is not usable. [(PR #1007) ](https://github.com/apple/foundationdb/pull/1007)
* Blobstore request failures could cause backup expire and delete operations to skip some files. [(PR #1007) ](https://github.com/apple/foundationdb/pull/1007)
* Blobstore request failures could cause restore to fail to apply some files. [(PR #1007) ](https://github.com/apple/foundationdb/pull/1007)
* Storage servers with large amounts of data would pause for a short period of time after rebooting. [(PR #1001) ](https://github.com/apple/foundationdb/pull/1001)
* The client library could leak memory when a thread died. [(PR #1011) ](https://github.com/apple/foundationdb/pull/1011)

Features
--------

* Added the ability to specify versions as version-days ago from latest log in backup. [(PR #1007) ](https://github.com/apple/foundationdb/pull/1007)

6.0.17
======

Fixes
-----

* Existing backups did not make progress when upgraded to 6.0.16. [(PR #962) ](https://github.com/apple/foundationdb/pull/962)

6.0.16
======

Performance
-----------

* Added a new backup folder scheme which results in far fewer kv range folders. [(PR #939) ](https://github.com/apple/foundationdb/pull/939)

Fixes
-----

* Blobstore REST client attempted to create buckets that already existed. [(PR #923) ](https://github.com/apple/foundationdb/pull/923)
* DNS would fail if IPv6 responses were received. [(PR #945) ](https://github.com/apple/foundationdb/pull/945)
* Backup expiration would occasionally fail due to an incorrect assert.  [(PR #926) ](https://github.com/apple/foundationdb/pull/926)

6.0.15
======

Features
--------

* Added support for asynchronous replication to a remote DC with processes in a single cluster. This improves on the asynchronous replication offered by fdbdr because servers can fetch data from the remote DC if all replicas have been lost in one DC.
* Added support for synchronous replication of the transaction log to a remote DC. This remote DC does not need to contain any storage servers, meaning you need much fewer servers in this remote DC.
* The TLS plugin is now statically linked into the client and server binaries and no longer requires a separate library. [(Issue #436) ](https://github.com/apple/foundationdb/issues/436)
* TLS peer verification now supports verifiying on Subject Alternative Name. [(Issue #514) ](https://github.com/apple/foundationdb/issues/514)
* TLS peer verification now supports suffix matching by field. [(Issue #515) ](https://github.com/apple/foundationdb/issues/515)
* TLS certificates are automatically reloaded after being updated. [6.0.5] [(Issue #505) ](https://github.com/apple/foundationdb/issues/505)
* Added the ``fileconfigure`` command to fdbcli, which configures a database from a JSON document. [6.0.10] [(PR #713) ](https://github.com/apple/foundationdb/pull/713)
* Backup-to-blobstore now accepts a "bucket" URL parameter for setting the bucket name where backup data will be read/written.  [6.0.15] [(PR #914) ](https://github.com/apple/foundationdb/pull/914)

Performance
-----------

* Transaction logs do not copy mutations from previous generations of transaction logs. [(PR #339) ](https://github.com/apple/foundationdb/pull/339)
* Load balancing temporarily avoids communicating with storage servers that have fallen behind.
* Avoid assigning storage servers responsibility for keys they do not have.
* Clients optimistically assume the first leader reply from a coordinator is correct. [(PR #425) ](https://github.com/apple/foundationdb/pull/425)
* Network connections are now closed after no interface needs the connection. [6.0.1] [(Issue #375) ](https://github.com/apple/foundationdb/issues/375)
* Significantly improved the CPU efficiency of copy mutations to transaction logs during recovery. [6.0.2] [(PR #595) ](https://github.com/apple/foundationdb/pull/595)
* Significantly improved the CPU efficiency of generating status on the cluster controller. [6.0.11] [(PR #758) ](https://github.com/apple/foundationdb/pull/758)
* Reduced CPU cost of truncating files that are being cached. [6.0.12] [(PR #816) ](https://github.com/apple/foundationdb/pull/816)
* Significantly reduced master recovery times for clusters with large amounts of data. [6.0.14] [(PR #836) ](https://github.com/apple/foundationdb/pull/836)
* Reduced read and commit latencies for clusters which are processing transactions larger than 1MB. [6.0.14] [(PR #851) ](https://github.com/apple/foundationdb/pull/851)
* Significantly reduced recovery times when executing rollbacks on the memory storage engine. [6.0.14] [(PR #821) ](https://github.com/apple/foundationdb/pull/821)
* Clients update their key location cache much more efficiently after storage server reboots. [6.0.15] [(PR #892) ](https://github.com/apple/foundationdb/pull/892)
* Tuned multiple resolver configurations to do a better job balancing work between each resolver. [6.0.15] [(PR #911) ](https://github.com/apple/foundationdb/pull/911)

Fixes
-----

* Not all endpoint failures were reported to the failure monitor.
* Watches registered on a lagging storage server would take a long time to trigger.
* The cluster controller would not start a new generation until it recovered its files from disk.
* Under heavy write load, storage servers would occasionally pause for ~100ms. [6.0.2] [(PR #597) ](https://github.com/apple/foundationdb/pull/597)
* Storage servers were not given time to rejoin the cluster before being marked as failed. [6.0.2] [(PR #592) ](https://github.com/apple/foundationdb/pull/592)
* Incorrect accounting of incompatible connections led to occasional assertion failures. [6.0.3] [(PR #616) ](https://github.com/apple/foundationdb/pull/616)
* A client could fail to connect to a cluster when the cluster was upgraded to a version compatible with the client. This affected upgrades that were using the multi-version client to maintain compatibility with both versions of the cluster. [6.0.4] [(PR #637) ](https://github.com/apple/foundationdb/pull/637)
* A large number of concurrent read attempts could bring the database down after a cluster reboot. [6.0.4] [(PR #650) ](https://github.com/apple/foundationdb/pull/650)
* Automatic suppression of trace events which occur too frequently was happening before trace events were suppressed by other mechanisms. [6.0.4] [(PR #656) ](https://github.com/apple/foundationdb/pull/656)
* After a recovery, the rate at which transaction logs made mutations durable to disk was around 5 times slower than normal. [6.0.5] [(PR #666) ](https://github.com/apple/foundationdb/pull/666)
* Clusters configured to use TLS could get stuck spending all of their CPU opening new connections. [6.0.5] [(PR #666) ](https://github.com/apple/foundationdb/pull/666)
* A mismatched TLS certificate and key set could cause the server to crash. [6.0.5] [(PR #689) ](https://github.com/apple/foundationdb/pull/689)
* Sometimes a minority of coordinators would fail to converge after a new leader was elected. [6.0.6] [(PR #700) ](https://github.com/apple/foundationdb/pull/700)
* Calling status too many times in a 5 second interval caused the cluster controller to pause for a few seconds. [6.0.7] [(PR #711) ](https://github.com/apple/foundationdb/pull/711)
* TLS certificate reloading could cause TLS connections to drop until process restart. [6.0.9] [(PR #717) ](https://github.com/apple/foundationdb/pull/717)
* Watches polled the server much more frequently than intended. [6.0.10] [(PR #728) ](https://github.com/apple/foundationdb/pull/728)
* Backup and DR didn't allow setting certain knobs. [6.0.10] [(Issue #715) ](https://github.com/apple/foundationdb/issues/715)
* The failure monitor will become much less reactive after multiple successive failed recoveries. [6.0.10] [(PR #739) ](https://github.com/apple/foundationdb/pull/739)
* Data distribution did not limit the number of source servers for a shard. [6.0.10] [(PR #739) ](https://github.com/apple/foundationdb/pull/739)
* The cluster controller did not do locality aware reads when measuring status latencies. [6.0.12] [(PR #801) ](https://github.com/apple/foundationdb/pull/801)
* Storage recruitment would spin too quickly when the storage server responded with an error. [6.0.12] [(PR #801) ](https://github.com/apple/foundationdb/pull/801)
* Restoring a backup to the exact version a snapshot ends did not apply mutations done at the final version. [6.0.12] [(PR #787) ](https://github.com/apple/foundationdb/pull/787)
* Excluding a process that was both the cluster controller and something else would cause two recoveries instead of one. [6.0.12] [(PR #784) ](https://github.com/apple/foundationdb/pull/784)
* Configuring from ``three_datacenter`` to ``three_datacenter_fallback`` would cause a lot of unnecessary data movement. [6.0.12] [(PR #782) ](https://github.com/apple/foundationdb/pull/782)
* Very rarely, backup snapshots would stop making progress. [6.0.14] [(PR #837) ](https://github.com/apple/foundationdb/pull/837)
* Sometimes data distribution calculated the size of a shard incorrectly. [6.0.15] [(PR #892) ](https://github.com/apple/foundationdb/pull/892)
* Changing the storage engine configuration would not effect which storage engine was used by the transaction logs. [6.0.15] [(PR #892) ](https://github.com/apple/foundationdb/pull/892)
* On exit, fdbmonitor will only kill its child processes instead of its process group when run without the daemonize option. [6.0.15] [(PR #826) ](https://github.com/apple/foundationdb/pull/826)
* HTTP client used by backup-to-blobstore now correctly treats response header field names as case insensitive. [6.0.15] [(PR #904) ](https://github.com/apple/foundationdb/pull/904)
* Blobstore REST client was not following the S3 API in several ways (bucket name, date, and response formats). [6.0.15] [(PR #914) ](https://github.com/apple/foundationdb/pull/914)
* Data distribution could queue shard movements for restoring replication at a low priority. [6.0.15] [(PR #907) ](https://github.com/apple/foundationdb/pull/907)

Fixes only impacting 6.0.0+
---------------------------

* A cluster configured with usable_regions=2 did not limit the rate at which it could copy data from the primary DC to the remote DC. This caused poor performance when recovering from a DC outage. [6.0.5] [(PR #673) ](https://github.com/apple/foundationdb/pull/673)
* Configuring usable_regions=2 on a cluster with a large amount of data caused commits to pause for a few seconds. [6.0.5] [(PR #687) ](https://github.com/apple/foundationdb/pull/687)
* On clusters configured with usable_regions=2, status reported no replicas remaining when the primary DC was still healthy. [6.0.5] [(PR #687) ](https://github.com/apple/foundationdb/pull/687)
* Clients could crash when passing in TLS options. [6.0.5] [(PR #649) ](https://github.com/apple/foundationdb/pull/649)
* Databases with more than 10TB of data would pause for a few seconds after recovery. [6.0.6] [(PR #705) ](https://github.com/apple/foundationdb/pull/705)
* Configuring from usable_regions=2 to usable_regions=1 on a cluster with a large number of processes would prevent data distribution from completing. [6.0.12] [(PR #721) ](https://github.com/apple/foundationdb/pull/721) [(PR #739) ](https://github.com/apple/foundationdb/pull/739) [(PR #780) ](https://github.com/apple/foundationdb/pull/780)
* Fixed a variety of problems with ``force_recovery_with_data_loss``. [6.0.12] [(PR #801) ](https://github.com/apple/foundationdb/pull/801)
* The transaction logs would leak memory when serving peek requests to log routers. [6.0.12] [(PR #801) ](https://github.com/apple/foundationdb/pull/801)
* The transaction logs were doing a lot of unnecessary disk writes. [6.0.12] [(PR #784) ](https://github.com/apple/foundationdb/pull/784)
* The master will recover the transaction state store from local transaction logs if possible. [6.0.12] [(PR #801) ](https://github.com/apple/foundationdb/pull/801)
* A bug in status collection led to various workload metrics being missing and the cluster reporting unhealthy. [6.0.13] [(PR #834) ](https://github.com/apple/foundationdb/pull/834)
* Data distribution did not stop tracking certain unhealthy teams, leading to incorrect status reporting. [6.0.15] [(PR #892) ](https://github.com/apple/foundationdb/pull/892)
* Fixed a variety of problems related to changing between different region configurations. [6.0.15] [(PR #892) ](https://github.com/apple/foundationdb/pull/892) [(PR #907) ](https://github.com/apple/foundationdb/pull/907)
* fdbcli protects against configuration changes which could cause irreversible damage to a cluster. [6.0.15] [(PR #892) ](https://github.com/apple/foundationdb/pull/892) [(PR #907) ](https://github.com/apple/foundationdb/pull/907)
* Significantly reduced both client and server memory usage in clusters with large amounts of data and usable_regions=2. [6.0.15] [(PR #892) ](https://github.com/apple/foundationdb/pull/892)

Status
------

* The replication factor in status JSON is stored under ``redundancy_mode`` instead of ``redundancy.factor``. [(PR #492) ](https://github.com/apple/foundationdb/pull/492)
* The metric ``data_version_lag`` has been replaced by ``data_lag.versions`` and ``data_lag.seconds``. [(PR #521) ](https://github.com/apple/foundationdb/pull/521)
* Additional metrics for the number of watches and mutation count have been added and are exposed through status. [(PR #521) ](https://github.com/apple/foundationdb/pull/521)


Bindings
--------

* API version updated to 600. See the [[api-version-upgrade-guide-600|API version upgrade guide ]] for upgrade details.
* Several cases where functions in go might previously cause a panic now return a non-``nil`` error. [(PR #532) ](https://github.com/apple/foundationdb/pull/532)
* C API calls made on the network thread could be reordered with calls made from other threads. [6.0.2] [(Issue #518) ](https://github.com/apple/foundationdb/issues/518)
* The TLS_PLUGIN option is now a no-op and has been deprecated. [6.0.10] [(PR #710) ](https://github.com/apple/foundationdb/pull/710)
* Java: the [Versionstamp::getUserVersion() ](/javadoc/com/apple/foundationdb/tuple/Versionstamp.html#getUserVersion--) method did not handle user versions greater than ``0x00FF`` due to operator precedence errors. [6.0.11] [(Issue #761) ](https://github.com/apple/foundationdb/issues/761)
* Python: bindings didn't work with Python 3.7 because of the new ``async`` keyword. [6.0.13] [(Issue #830) ](https://github.com/apple/foundationdb/issues/830)
* Go: ``PrefixRange`` didn't correctly return an error if it failed to generate the range. [6.0.15] [(PR #878) ](https://github.com/apple/foundationdb/pull/878)
* Go: Add Tuple layer support for ``uint``, ``uint64``, and ``*big.Int`` integers up to 255 bytes. Integer values will be decoded into the first of ``int64``, ``uint64``, or ``*big.Int`` in which they fit. [(PR #915) ](https://github.com/apple/foundationdb/pull/915) [6.0.15]
* Ruby: Add Tuple layer support for integers up to 255 bytes. [(PR #915) ](https://github.com/apple/foundationdb/pull/915) [6.0.15]
* Python: bindings didn't work with Python 3.7 because of the new ``async`` keyword. [6.0.13] [(Issue #830) ](https://github.com/apple/foundationdb/issues/830)
* Go: ``PrefixRange`` didn't correctly return an error if it failed to generate the range. [6.0.15] [(PR #878) ](https://github.com/apple/foundationdb/pull/878)

Other Changes
-------------

* Does not support upgrades from any version older than 5.0.
* Normalized the capitalization of trace event names and attributes. [(PR #455) ](https://github.com/apple/foundationdb/pull/455)
* Various stateless processes now have a higher affinity for running on processes with unset process class, which may result in those roles changing location upon upgrade. See [[version-specific-upgrading]] for details. [(PR #526) ](https://github.com/apple/foundationdb/pull/526)
* Increased the memory requirements of the transaction log by 400MB. [6.0.5] [(PR #673) ](https://github.com/apple/foundationdb/pull/673)

Earlier release notes
---------------------
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
