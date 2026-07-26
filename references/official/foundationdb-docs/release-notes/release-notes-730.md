---
title: "Release Notes"
source: "https://apple.github.io/foundationdb/release-notes/release-notes-730.html"
source_format: official-rst
language: "en"
doc_slug: "release-notes/release-notes-730"
category: "release-notes"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Release Notes"
---
.. _release-notes:

#############
Release Notes
#############

7.3.79
======
* Same as 7.3.78 release with AVX enabled.

7.3.78
======
* Fixed stale peer connections for client facing roles (storage server, commit proxy, GRV proxy) so clients actively drain connections to dead processes instead of retrying them indefinitely. [(PR #13367) ](https://github.com/apple/foundationdb/pull/13367) and [(PR #13643) ](https://github.com/apple/foundationdb/pull/13643)
* Added an admission control pipeline for Data Distribution that caps the number of relocations DD runs simultaneously (default 1000), applied to relocations of all priorities including operator-triggered excludes, to prevent DD from being overwhelmed. [(PR #13243) ](https://github.com/apple/foundationdb/pull/13243) and [(PR #13402) ](https://github.com/apple/foundationdb/pull/13402)
* Added exponential backoff with jitter and a retry limit for transaction_too_old in finishMoveKeys, fixing a retry storm that could clog the event loop when Data Distribution is under stress at startup. [(PR #12991) ](https://github.com/apple/foundationdb/pull/12991) and [(PR #13154) ](https://github.com/apple/foundationdb/pull/13154)
* Fixed a race in removeKeysFromFailedServer where concurrently awaiting the unassign and reassign writes for an excluded server let krmSetRangeCoalescing's read-modify-write observe stale state, producing an incorrect serverKeys assignment. [(PR #13662) ](https://github.com/apple/foundationdb/pull/13662)
* Added a background-refreshed DNS cache for coordinator addresses so a spike in upstream DNS latency no longer risks cluster controller leadership loss or repeated recovery failures. [(PR #13066) ](https://github.com/apple/foundationdb/pull/13066)
* Made status json generation return partial results before the client side timeout instead of returning nothing when the underlying transactions are slow. [(PR #13068) ](https://github.com/apple/foundationdb/pull/13068)
* Fixed bounce_impact.can_clean_bounce incorrectly reporting false in status json even on a fully recovered cluster, which had been blocking clean bounces. [(PR #13343) ](https://github.com/apple/foundationdb/pull/13343)
* Enabled SNI in the TLS handshake, fixing connectivity issues with some S3-compatible blob storage backends such as GCS. [(PR #13089) ](https://github.com/apple/foundationdb/pull/13089)
* Fixed a crash during encrypted restores run as a different user from a directory it cannot access, and removed unneeded encryption key and cache handling from backup expire and describe. [(PR #13111) ](https://github.com/apple/foundationdb/pull/13111)
* Changed encrypted backup/restore to store and thread the encryption block size per backup instead of relying on a process wide knob, and removed the knob. [(PR #13126) ](https://github.com/apple/foundationdb/pull/13126)
* Fixed spurious read failures on small or last file blocks by clipping read-ahead cache reads to the file size, affecting some blob storage backends. [(PR #13293) ](https://github.com/apple/foundationdb/pull/13293)

7.3.77
======
* Same as 7.3.76 release with AVX enabled.

7.3.76
======
* Fixed peer disconnect detection in waitValueOrSignal so dead connections are detected immediately instead of hanging indefinitely. [(PR #12935) ](https://github.com/apple/foundationdb/pull/12935)
* Made getTeamByServers O(1) in time which was sometimes causing Data Distributor (DD) to get stuck during initialization when shard_encode_location_metadata was enabled. [(PR #12938) ](https://github.com/apple/foundationdb/pull/12938)
* Added miscellaneous observability improvements for CPU and memory usage, DD startup, and S3 backup subsystems. [(PR #12937) ](https://github.com/apple/foundationdb/pull/12937), [(PR #12913) ](https://github.com/apple/foundationdb/pull/12913), and [(PR #12997) ](https://github.com/apple/foundationdb/pull/12997)

7.3.75
======
* Same as 7.3.74 release with AVX enabled.

7.3.74
======
* Update rocksdb version to 8.11.5. [(PR #12690) ](https://github.com/apple/foundationdb/pull/12690)

7.3.73
======
* Same as 7.3.72 release with AVX enabled.

7.3.72
======
* Added backup encryption support and related status/CLI improvements. [(PR #12580) ](https://github.com/apple/foundationdb/pull/12580), [(PR #12590) ](https://github.com/apple/foundationdb/pull/12590), [(PR #12621) ](https://github.com/apple/foundationdb/pull/12621), and [(PR #12658) ](https://github.com/apple/foundationdb/pull/12658)
* Fixed a backup decode range file bug when interpreting keys and values. [(PR #12399) ](https://github.com/apple/foundationdb/pull/12399)

7.3.71
======
* Same as 7.3.71 release with AVX enabled.

7.3.70
======
* Fixed handleTssMismatches crashes. [(PR #12377) ](https://github.com/apple/foundationdb/pull/12377)
* Improved TLS handshake mechanism. [(PR #12348) ](https://github.com/apple/foundationdb/pull/12348)
* Fixed restarting test failures. [(PR #12324) ](https://github.com/apple/foundationdb/pull/12324)
* Suppressed PingLatency events. [(PR #12316) ](https://github.com/apple/foundationdb/pull/12316)
* Fixed "Unknown error" when configuring regions. [(PR #12313) ](https://github.com/apple/foundationdb/pull/12313)
* Replaced '...' with actual random data in simulation workloads. [(PR #12273) ](https://github.com/apple/foundationdb/pull/12273)
* Added TLS performance counters. [(PR #12278) ](https://github.com/apple/foundationdb/pull/12278) and [(PR #12296) ](https://github.com/apple/foundationdb/pull/12296)

7.3.69
======
* Same as 7.3.68 release with AVX enabled.

7.3.68
======
* Fixed a rare race that can cause recovery to be stuck. [(PR #12213) ](https://github.com/apple/foundationdb/pull/12213)
* Backported various go binding and documentation changes. [(PR #12146) ](https://github.com/apple/foundationdb/pull/12146)

7.3.67
======
* Same as 7.3.66 release with AVX enabled.

7.3.66
======
* Fixed a restore speed regression. [(PR #12088) ](https://github.com/apple/foundationdb/pull/12088)
* Added per DC check for fdbcli's checkall command. [(PR #12089) ](https://github.com/apple/foundationdb/pull/12089)
* Added periodical flushes of old column families for Sharded RocksDB engine. [(PR #12123) ](https://github.com/apple/foundationdb/pull/12123)

7.3.65
======
* Same as 7.3.64 release with AVX enabled.

7.3.64
======
* Fixed fdb-kubernetes-monitor to only log an error when there is one. [(PR #12112) ](https://github.com/apple/foundationdb/pull/12112)
* Fixed distributed conconsistency checker tester to always accept new requests from ConsistencyCheckUrgent role. [(PR #12001) ](https://github.com/apple/foundationdb/pull/12001)
* Added RocksDB compaction knobs and more metrics. [(PR #12016) ](https://github.com/apple/foundationdb/pull/12016), [(PR #11993) ](https://github.com/apple/foundationdb/pull/11993), [(PR #12054) ](https://github.com/apple/foundationdb/pull/12054), and [(PR #12055) ](https://github.com/apple/foundationdb/pull/12055)
* Fixed a rare restore bug due to a race condition. [(PR #12041) ](https://github.com/apple/foundationdb/pull/12041)
* Fixed backup reporting on snapshot restorable state and continuous log end version. [(PR #12038) ](https://github.com/apple/foundationdb/pull/12038) and [(PR #12069) ](https://github.com/apple/foundationdb/pull/12069)

7.3.63
======
* Same as 7.3.62 release with AVX enabled.

7.3.62
======
* Reverted backup agents dry run request changes since 7.3.49 release. [(PR #11965) ](https://github.com/apple/foundationdb/pull/11965)

7.3.61
======
* Same as 7.3.60 release with AVX enabled.

7.3.60
======
* Fixed a potential corruption of the destination storage server when shard_encode_location_metadata is enabled. [(PR #11934) ](https://github.com/apple/foundationdb/pull/11934)
* Paused storage wiggle when all storage servers do not have enough available space. [(PR #11911) ](https://github.com/apple/foundationdb/pull/11911)
* Updated Sharded RocksDB knobs. [(PR #11936) ](https://github.com/apple/foundationdb/pull/11936)
* Fixed storage migration to consider the perpetualStorageEngine setting. [(PR #11940) ](https://github.com/apple/foundationdb/pull/11940)
* Improved the coverage of location metadata auditing. [(PR #11897) ](https://github.com/apple/foundationdb/pull/11897)

7.3.59
======
* Same as 7.3.58 release with AVX enabled.

7.3.58
======
* Updated fdbmonitor to only delay the restart of fdbserver if the process exited with an exit code other than 0. [(PR #11812) ](https://github.com/apple/foundationdb/pull/11812)
* Added knob to pause perpetual storage wiggle when TSS count target is met. [(PR #11824) ](https://github.com/apple/foundationdb/pull/11824)
* Updated exclusion code to reduce the requests that are made by locality-based exclusions. [(PR #11848) ](https://github.com/apple/foundationdb/pull/11848)
* Added multiple improvements to reduce the false positive rate of gray failure triggered recoveries. [(PR #11850) ](https://github.com/apple/foundationdb/pull/11850), [(PR #11852) ](https://github.com/apple/foundationdb/pull/11852), [(PR #11885) ](https://github.com/apple/foundationdb/pull/11885)
* Added a knob to use direct IO for ShardedRocks storage engine. [(PR #11891) ](https://github.com/apple/foundationdb/pull/11891)
* Added a knob to compact based on number of range deletions in file for ShardedRocks storage engine. [(PR #11890) ](https://github.com/apple/foundationdb/pull/11890)
* Updated RocksDB force flush interval based on the last flush time. [(PR #11845) ](https://github.com/apple/foundationdb/pull/11845)

7.3.57
======
* Same as 7.3.56 release with AVX enabled.

7.3.56
======
* Added an option to force flush if RocksCb flush does not happen within a time interval. [(PR #11792) ](https://github.com/apple/foundationdb/pull/11792)
* Changed RocksDB histogram sample rate to 1. [(PR #11794) ](https://github.com/apple/foundationdb/pull/11794)
* Updated TLS input handling to allow multiple Criteria per NID. [(PR #11763) ](https://github.com/apple/foundationdb/pull/11763)
* Added Sharded RocksDB knobs and metrics for bloom filters. [(PR #11785) ](https://github.com/apple/foundationdb/pull/11785)
* Fixed a backup agent crash bug when receiving an invalid token error from S3. [(PR #11774) ](https://github.com/apple/foundationdb/pull/11774)
* Updated ROCKSDB_MEMTABLE_MAX_RANGE_DELETIONS Knob to prevent OOMs of RocksDB storage servers [(PR #11739) ](https://github.com/apple/foundationdb/pull/11739)

7.3.55
======
* Same as 7.3.54 release with AVX enabled.

7.3.54
======
* Addressed urgent consistency checker related issues. [(PR #11736) ](https://github.com/apple/foundationdb/pull/11736)
* Addressed a downgrade related incompatibility issue between 7.3 patch releases. [(PR #11732) ](https://github.com/apple/foundationdb/pull/11732)
* Added knob LOG_CONNECTION_ATTEMPTS_ENABLED to log all incoming connections. [(PR #11713) ](https://github.com/apple/foundationdb/pull/11713)

7.3.53
======
* Same as 7.3.52 release with AVX enabled.

7.3.52
======
* Improved Sharded Rocksdb to use a single iterator pool for all physical shards. [(PR #11694) ](https://github.com/apple/foundationdb/pull/11694)
* Changed the default values of various Sharded Rocksdb related knobs. [(PR #11706) ](https://github.com/apple/foundationdb/pull/11706)
* Removed CC_PAUSE_HEALTH_MONITOR knob. [(PR #11701) ](https://github.com/apple/foundationdb/pull/11701)
* Addressed protocol incompatibility issues between 7.3 patch releases. [(PR #11697) ](https://github.com/apple/foundationdb/pull/11697), [(PR #11705) ](https://github.com/apple/foundationdb/pull/11705)

7.3.51
======
* Same as 7.3.50 release with AVX enabled.

7.3.50
======
* Converted an assertion for Sharded Rocksdb in storage server to error logs. [(PR #11622) ](https://github.com/apple/foundationdb/pull/11622)
* Fixed backup agents from retrying uploading in the presence of S3 token errors. [(PR #11602) ](https://github.com/apple/foundationdb/pull/11602)
* Fixed an inconsistent location metadata issue and invalid shard IDs generated by seed shard servers when SHARD_ENCODE_LOCATION_METADATA is enabled. [(PR #11640) ](https://github.com/apple/foundationdb/pull/11640) and [(PR #11647) ](https://github.com/apple/foundationdb/pull/11647)
* Enabled direct_io for RocksDB storage engine and wiggle knobs. [(PR #11636) ](https://github.com/apple/foundationdb/pull/11636)
* Added knobs for caching index blocks for Sharded RocksDB. [(PR #11649) ](https://github.com/apple/foundationdb/pull/11649) and [(PR #11680) ](https://github.com/apple/foundationdb/pull/11680)
* Added various data distributor knobs for performance tuning. [(PR #11668) ](https://github.com/apple/foundationdb/pull/11668) and [(PR #11665) ](https://github.com/apple/foundationdb/pull/11665)
* Increased the minimum age to wiggle to avoid re-wiggling migrated storage servers. [(PR #11684) ](https://github.com/apple/foundationdb/pull/11684)
* Added various version vector improvements. [(PR #11590) ](https://github.com/apple/foundationdb/pull/11590), [(PR #11599) ](https://github.com/apple/foundationdb/pull/11599), [(PR #11605) ](https://github.com/apple/foundationdb/pull/11605), [(PR #11608) ](https://github.com/apple/foundationdb/pull/11608), [(PR #11620) ](https://github.com/apple/foundationdb/pull/11620), [(PR #11626) ](https://github.com/apple/foundationdb/pull/11626) and [(PR #11631) ](https://github.com/apple/foundationdb/pull/11631)
* Removed upgrade tests before FDB 6.3. [(PR #11662) ](https://github.com/apple/foundationdb/pull/11662)

7.3.49
======
* Same as 7.3.48 release with AVX enabled.

7.3.48
======
* Added various fdb-kubernetes-monitor improvements. [(PR #11456) ](https://github.com/apple/foundationdb/pull/11456), [(PR #11462) ](https://github.com/apple/foundationdb/pull/11462), and [(PR #11488) ](https://github.com/apple/foundationdb/pull/11488)
* Added more debug information for the fdbcli's checkall command. [(PR #11477) ](https://github.com/apple/foundationdb/pull/11477)
* Improved distributed consistency checker and updated the documentation. [(PR #11496) ](https://github.com/apple/foundationdb/pull/11496)
* Fixed a known committed version computation when version vector unicast is enabled. [(PR #11520) ](https://github.com/apple/foundationdb/pull/11520)
* Added PeerAddress to all PeerAddr/Peer TraceEvents. [(PR #11521) ](https://github.com/apple/foundationdb/pull/11521)
* Added a dynamic knob CC_PAUSE_HEALTH_MONITOR to disable gray failure recoveries. [(PR #11526) ](https://github.com/apple/foundationdb/pull/11526)
* Changed the default base Docker image to RockyLinux9. [(PR #11549) ](https://github.com/apple/foundationdb/pull/11549)
* Fixed a storage server crash when there are conflicting physical shard moves. [(PR #11485) ](https://github.com/apple/foundationdb/pull/11485)
* Fixed a rare cluster controller crash when serving status requests. [(PR #11580) ](https://github.com/apple/foundationdb/pull/11580)

7.3.47
======
* Same as 7.3.46 release with AVX enabled.

7.3.46
======
* Added exponential backoff when restarting fdbserver processes in fdb-kubernetes-monitor. [(PR #11453) ](https://github.com/apple/foundationdb/pull/11453)

7.3.45
======
* Same as 7.3.44 release with AVX enabled.

7.3.44
======
* Fixed negative free disk space in RkUpdate metrics. [(PR #11413) ](https://github.com/apple/foundationdb/pull/11413)
* Corrected the path where the fdb-kubernetes-monitor copies the binary into when running in sidecar mode. [(PR #11439) ](https://github.com/apple/foundationdb/pull/11439)

7.3.43
======
* Same as 7.3.42 release with AVX enabled.

7.3.42
======
* Fixed a segfault when tlog encounters a platform_error. [(PR #11406) ](https://github.com/apple/foundationdb/pull/11406)
* Fixed an assertion failure of cluster controller when waiting for recovery. [(PR #11402) ](https://github.com/apple/foundationdb/pull/11402)
* Fixed a global config bug that causes client latency metrics to be missing after upgrade. [(PR #11400) ](https://github.com/apple/foundationdb/pull/11400)
* Added a knob for shard merge parallelism and added logs for max shard size. [(PR #11389) ](https://github.com/apple/foundationdb/pull/11389)
* Updated the RocksDB memtable max range deletions knob. [(PR #11387) ](https://github.com/apple/foundationdb/pull/11387)
* Improved visibility when Sharded RocksDB takes a long time to commit. [(PR #11362) ](https://github.com/apple/foundationdb/pull/11362)
* Fixed data move trigger for rebalancing storage queue. [(PR #11375) ](https://github.com/apple/foundationdb/pull/11375)
* Fixed an infinite retry of GRV request bug. [(PR #11353) ](https://github.com/apple/foundationdb/pull/11353)
* Improved distributed consistency checker to continuously run by default and visibility of recruitment errors. [(PR #11349) ](https://github.com/apple/foundationdb/pull/11349)
* Fixed add-prefix and remove-prefix for fdbrestore. [(PR #11344) ](https://github.com/apple/foundationdb/pull/11344)
* Fixed a crash of data distributor when taking a snapshot. [(PR #11341) ](https://github.com/apple/foundationdb/pull/11341)
* Increased visibility of gray failure actions. [(PR #11324) ](https://github.com/apple/foundationdb/pull/11324)
* Increased visibility of CommitProxyTerminated events for failed_to_progress errors. [(PR #11315) ](https://github.com/apple/foundationdb/pull/11315)

7.3.41
======
* Same as 7.3.40 release with AVX enabled.

7.3.40
======
* Upgraded RocksDB to version 8.11.4. [(PR #11327) ](https://github.com/apple/foundationdb/pull/11327)

7.3.39
======
* Same as 7.3.38 release with AVX enabled.

7.3.38
======
* Fixed the detection of private mutations in version vector. [(PR #11279) ](https://github.com/apple/foundationdb/pull/11279)
* Added accumulative checksum feature. [(PR #11281) ](https://github.com/apple/foundationdb/pull/11281) and [(PR #11289) ](https://github.com/apple/foundationdb/pull/11289)
* Added Go tenanting support. [(PR #11299) ](https://github.com/apple/foundationdb/pull/11299)
* Added RocksDB caching knobs. [(PR #11312) ](https://github.com/apple/foundationdb/pull/11312)
* Added RocksDB metrics in status json. [(PR #11320) ](https://github.com/apple/foundationdb/pull/11320)
* Various Sharded RocksDB improvements. [(PR #11332) ](https://github.com/apple/foundationdb/pull/11332)


7.3.37
======
* Same as 7.3.36 release with AVX enabled.

7.3.36
======
* Fixed a DR corruption issue where destination cluster gets no mutations. [(PR #11246) ](https://github.com/apple/foundationdb/pull/11246)
* Added rocksdb direct_io knobs. [(PR #11267) ](https://github.com/apple/foundationdb/pull/11267)

7.3.35
======
* Same as 7.3.34 release with AVX enabled.

7.3.34
======
* Added storage-queue-aware load balancer for data distributor. [(PR #11195) ](https://github.com/apple/foundationdb/pull/11195)
* Added a checksum field in MutationRef. [(PR #11193) ](https://github.com/apple/foundationdb/pull/11193)
* Abort processes when abnormal shutdown is initiated to enable coredumps. [(PR #11198) ](https://github.com/apple/foundationdb/pull/11198)
* Fixed fdbcli's checkall debug command. [(PR #11208) ](https://github.com/apple/foundationdb/pull/11208)
* Added knobs for enabling RocksDB in-memory checksums for data structures. [(PR #11214) ](https://github.com/apple/foundationdb/pull/11214)
* Fixed calculation of EmptyMessageRatio when version vector was enabled. [(PR #11227) ](https://github.com/apple/foundationdb/pull/11227)
* Added consistency checker urgent mode. [(PR #11228) ](https://github.com/apple/foundationdb/pull/11228)
* Disabled compaction compaction for newly added shard and fixed block cache usage reporting. [(PR #11247) ](https://github.com/apple/foundationdb/pull/11247)
* Fixed setting perpetual_storage_wiggle_engine is considered as wrongly configured. [(PR #11252) ](https://github.com/apple/foundationdb/pull/11252)
* Added a max range deletions knob before flush. [(PR #11243) ](https://github.com/apple/foundationdb/pull/11243)

7.3.33
======
* Same as 7.3.32 release with AVX enabled.

7.3.32
======
* Enabled data distributor verbose tracing by default. [(PR #11159) ](https://github.com/apple/foundationdb/pull/11159)
* Added RocksDB file checksum knobs. [(PR #11171) ](https://github.com/apple/foundationdb/pull/11171)
* Fixed a regression that caused rebalance data moves to be scheduled at a much lower frequency. [(PR #11167) ](https://github.com/apple/foundationdb/pull/11167)
* Added throttling of RocksDB flushes when memtable layers exceed a limit. [(PR #11182) ](https://github.com/apple/foundationdb/pull/11182)
* Added a trace event when a log router cannot find its primary peek location. [(PR #11180) ](https://github.com/apple/foundationdb/pull/11180)
* Upgraded RocksDB version to 8.10.0. [(PR #11175) ](https://github.com/apple/foundationdb/pull/11175)
* Added periodical logging for RocksDB compaction reasons. [(PR #11186) ](https://github.com/apple/foundationdb/pull/11186)

7.3.31
======
* Same as 7.3.30 release with AVX disabled.

7.3.30
======
* Fixed an issue in Ratekeeper that could cause StorageQueueInfo loss. [(PR #11124) ](https://github.com/apple/foundationdb/pull/11124)
* Fixed checkall command for large shards. [(PR #11121) ](https://github.com/apple/foundationdb/pull/11121)

7.3.29
======
* Same as 7.3.28 release with AVX disabled.

7.3.28
======
* Fixed a race condition in kvstorerockddb when accessing latencySample. [(PR #11114) ](https://github.com/apple/foundationdb/pull/11114)
* Added support for physical shard move. [(PR #11086) ](https://github.com/apple/foundationdb/pull/11086)
* Disabled CPU based team selection in rebalance data move. [(PR #11110) ](https://github.com/apple/foundationdb/pull/11110)


7.3.27
======
* Same as 7.3.26 release with AVX disabled.

7.3.26
======
* Updated RocskDB version to 8.6.7. [(PR #11043) ](https://github.com/apple/foundationdb/pull/11043)
* Changed RocksDB rate limiter to all IO. [(PR #11016) ](https://github.com/apple/foundationdb/pull/11016)
* Added ``fdb_c_apiversion.g.h`` to OSX package. [(PR #11042) ](https://github.com/apple/foundationdb/pull/11042)
* Added write traffic metrics to ddMetricsGetRange. [(PR #10998) ](https://github.com/apple/foundationdb/pull/10998)
* Fixed several locality-based exclusion bugs. [(PR #11024) ](https://github.com/apple/foundationdb/pull/11024), [(PR #11007) ](https://github.com/apple/foundationdb/pull/11007), and [(PR #11005) ](https://github.com/apple/foundationdb/pull/11005)
* Fixed the null pointer issue in proxy setup. [(PR #11039) ](https://github.com/apple/foundationdb/pull/11039)

7.3.25
======
* Same as 7.3.24 release with AVX enabled.

7.3.24
======
* Released with AVX disabled.
* Added support for large shard. [(PR #10965) ](https://github.com/apple/foundationdb/pull/10965)
* Fixed perpetual wiggle locality match regex. [(PR #10973) ](https://github.com/apple/foundationdb/pull/10973)
* Added a knob to throttle perpetual wiggle data move. [(PR #10957) ](https://github.com/apple/foundationdb/pull/10957)

7.3.23
======
* Same as 7.3.22 release with AVX enabled.

7.3.22
======
* No code change, only version bumped.

7.3.21
======
* Same as 7.3.20 release with AVX enabled.

7.3.20
======
* Added data move throttling for perpetual wiggle. [(PR #10957) ](https://github.com/apple/foundationdb/pull/10957)
* Fixed AuditStorage to check all DC replicas. [(PR #10966) ](https://github.com/apple/foundationdb/pull/10966)
* Added large shards support. [(PR #10965) ](https://github.com/apple/foundationdb/pull/10965)
* Fixed bugs for locality-based exclusion. [(PR #10946) ](https://github.com/apple/foundationdb/pull/10946)
* Fixed various memory-related bugs. [(PR #10952) ](https://github.com/apple/foundationdb/pull/10952) and [(PR #10969) ](https://github.com/apple/foundationdb/pull/10969)
* Fixed perpetual wiggling locality match regex. [(PR #10972) ](https://github.com/apple/foundationdb/pull/10972)

7.3.19
======
* Same as 7.3.18 release with AVX enabled.

7.3.18
======
* Released with AVX disabled.
* Changed Event to use std::latch from c++20. [(PR #10929) ](https://github.com/apple/foundationdb/pull/10929)
* Added support for preinstalled libfmt. [(PR #10929) ](https://github.com/apple/foundationdb/pull/10929)
* Changed perpetual_storage_wiggle_locality database option to take a list of localities. [(PR #10928) ](https://github.com/apple/foundationdb/pull/10928)
* Fixed the trailing newline in c++filt output for Implib.so. [(PR #10921) ](https://github.com/apple/foundationdb/pull/10921)
* Stopped tracking a storage server after its removal. [(PR #10921) ](https://github.com/apple/foundationdb/pull/10921)
* Fixed Ratekeeper for not accounting dropped requests. [(PR #10921) ](https://github.com/apple/foundationdb/pull/10921)
* Fixed a memory leak of cluster controller's status json invocation. [(PR #10921) ](https://github.com/apple/foundationdb/pull/10921)
* Fixed cluster controller from issuing many point reads for storage metadata. [(PR #10906) ](https://github.com/apple/foundationdb/pull/10906)
* Fixed multiple issues with AuditStorage. [(PR #10895) ](https://github.com/apple/foundationdb/pull/10895)
* Disabled storage server read sampling by default. [(PR #10899) ](https://github.com/apple/foundationdb/pull/10899)

7.3.17
======
* Same as 7.3.16 release with AVX enabled.

7.3.16
======
* Released with AVX disabled.
* Added location_metadata fdbcli to query shard locations and assignements. [(PR #10428) ](https://github.com/apple/foundationdb/pull/10428)
* Added degraded/disconnected peer recovery in gray failure. [(PR #10541) ](https://github.com/apple/foundationdb/pull/10541)
* Added replica and metadata audit support. [(PR #10631) ](https://github.com/apple/foundationdb/pull/10631)
* Added a SecurityMode for data distributor where data movements are not allowed but auditStorage is enabled. [(PR #10660) ](https://github.com/apple/foundationdb/pull/10660)
* Remove SS entries from RateKeeper once it is down. [(PR #10681) ](https://github.com/apple/foundationdb/pull/10681/files)
* Added the support of manual compaction for Sharded RocksDB. [(PR #10815) ](https://github.com/apple/foundationdb/pull/10838)

7.3.0
=====

Fixes
-----
* Fixed a consistency scan infinite looping without progress bug when a storage server is removed. [(PR #9154) ](https://github.com/apple/foundationdb/pull/9154)
* Fixed a backup worker assertion failure. [(PR #8886) ](https://github.com/apple/foundationdb/pull/8886)
* Fixed a DD stuck issue when the remote data center is dead. [(PR #9338) ](https://github.com/apple/foundationdb/pull/9338)
* Exclude command will not perform a write if the addresses being excluded are already excluded. [(PR #9873) ](https://github.com/apple/foundationdb/pull/9873)
* ConsistencyCheck should finish after complete scan than failing on first mismatch. [(PR #8539) ](https://github.com/apple/foundationdb/pull/8539)

Bindings
--------
* Allow Ruby bindings to run on arm64. [(PR #9575) ](https://github.com/apple/foundationdb/pull/9575)

Performance
-----------
* Improvements on physical shard creation to reduce shard count. [(PR #9067) ](https://github.com/apple/foundationdb/pull/9067)
* Older TLog generations are garbage collected as soon as they are no longer needed. [(PR #10289) ](https://github.com/apple/foundationdb/pull/10289)

Reliability
-----------
* Gray failure will monitor satellite TLog disconnections.
* Storage progress is logged during the slow recovery. [(PR #9041) ](https://github.com/apple/foundationdb/pull/9041)
* Added a new network option fail_incompatible_client. If the option is set, transactions are failing with fail_incompatible_client in case of an attempt to connect to a cluster without providing a compatible client library

Status
------

Other Changes
-------------

*  Added MonotonicTime field, based on system clock, to CommitDebug
   trace events, for accurate timing.

*  Added a new function fdb_database_get_client_status providing a
   client-side connection status information in json format.

*  Added a new network option retain_client_library_copies to avoid
   deleting the temporary library copies after completion of the
   process. This may be useful in various debugging and profiling
   scenarios.

*  Added a new network option trace_initialize_on_setup to enable client
   traces already on fdb_setup_network, so that traces do not get lost
   on client configuration issues

*  TraceEvents related to TLS handshake, new connections, and tenant
   access by authorization token are no longer subject to suppression or
   throttling, using an internal “AuditedEvent” TraceEvent
   classification

*  Usage of authorization token is logged as part of AuditedEvent, with
   5-second suppression time window for duplicate entries (suppression
   time window is controlled by AUDIT_TIME_WINDOW flow knob)

Earlier release notes
---------------------
* [[release-notes-720|7.2 (API Version 720)]]
* [[release-notes-710|7.1 (API Version 710)]]
* [[release-notes-700|7.0 (API Version 700)]]
* [[release-notes-630|6.3 (API Version 630)]]
* [[release-notes-620|6.2 (API Version 620)]]
* [[release-notes-610|6.1 (API Version 610)]]
* [[release-notes-600|6.0 (API Version 600)]]
* [[release-notes-520|5.2 (API Version 520)]]
* [[release-notes-510|5.1 (API Version 510)]]
* [[release-notes-500|5.0 (API Version 500)]]
* [[release-notes-460|4.6 (API Version 460)]]
* [[release-notes-450|4.5 (API Version 450)]]
* [[release-notes-440|4.4 (API Version 440)]]
* [[release-notes-430|4.3 (API Version 430)]]
* [[release-notes-420|4.2 (API Version 420)]]
* [[release-notes-410|4.1 (API Version 410)]]
* [[release-notes-400|4.0 (API Version 400)]]
* [[release-notes-300|3.0 (API Version 300)]]
* [[release-notes-200|2.0 (API Version 200)]]
* [[release-notes-100|1.0 (API Version 100)]]
* [[release-notes-023|Beta 3 (API Version 23)]]
* [[release-notes-022|Beta 2 (API Version 22)]]
* [[release-notes-021|Beta 1 (API Version 21)]]
* [[release-notes-016|Alpha 6 (API Version 16)]]
* [[release-notes-014|Alpha 5 (API Version 14)]]
