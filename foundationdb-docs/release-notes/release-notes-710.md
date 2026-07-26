---
title: "Release Notes"
source: "https://apple.github.io/foundationdb/release-notes/release-notes-710.html"
source_format: official-rst
language: "en"
doc_slug: "release-notes/release-notes-710"
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

7.1.61
======
* Same as 7.1.60 release with AVX enabled.

7.1.60
======
* Released with AVX disabled.
* Fixed a DR corruption bug due to a race condition. [(PR #11245) ](https://github.com/apple/foundationdb/pull/11245)
* Added Go tenanting support for 7.1. [(PR #11278) ](https://github.com/apple/foundationdb/pull/11278)
* Increased visibility of gray failure actions. [(PR #11314) ](https://github.com/apple/foundationdb/pull/11314)
* Increase visibility of CommitProxyTerminated events for failed_to_progress errors. [(PR #11316) ](https://github.com/apple/foundationdb/pull/11316)
* Fixed an infinite retry of GRV request bug. [(PR #11352) ](https://github.com/apple/foundationdb/pull/11352)
* Improved distributed consistency checker to continuously run by default and visibility of recruitment errors. [(PR #11351) ](https://github.com/apple/foundationdb/pull/11351)
* Fixed an assertion failure of cluster controller when waiting for recovery. [(PR #11398) ](https://github.com/apple/foundationdb/pull/11398)

7.1.59
======
* Same as 7.1.59 release with AVX enabled.

7.1.58
======
* Released with AVX disabled.
* Fixed a bug in consistency checker urgent mode. [(PR #11230) ](https://github.com/apple/foundationdb/pull/11230)

7.1.57
======
* Same as 7.1.56 release with AVX enabled.

7.1.56
======
* Released with AVX disabled.
* Added a knob DD_REMOVE_MAINTENANCE_ON_FAILURE to remove the maintenance mode if a failure outside of the maintenance zone happens. [(PR #11207) ](https://github.com/apple/foundationdb/pull/11207)
* Fixed start failure of single-threaded consistency checker. [(PR #11205) ](https://github.com/apple/foundationdb/pull/11205)
* Fixed false alarms of Sev40 TestFailure in consistency checker urgent mode. [(PR #11203) ](https://github.com/apple/foundationdb/pull/11203)
* Added a knob ABORT_ON_FAILURE to generate core dumps for failures. [(PR #11191) ](https://github.com/apple/foundationdb/pull/11191)
* Upgraded to RocksDB 8.10.0. [(PR #11176) ](https://github.com/apple/foundationdb/pull/11176)
* Fixed find toml in release-7.1 to avoid macos pr failures. [(PR #11190) ](https://github.com/apple/foundationdb/pull/11190)

7.1.55
======
* Same as 7.1.54 release with AVX enabled.

7.1.54
======
* Released with AVX disabled.
* Upgraded RocksDB to 8.6.7. [(PR #11160) ](https://github.com/apple/foundationdb/pull/11160)
* Enabled data distribution verbose logging by default. [(PR #11158) ](https://github.com/apple/foundationdb/pull/11158)
* Added logging for invalid mutations on storage servers. [(PR #11097) ](https://github.com/apple/foundationdb/pull/11097)
* Enhanced AsyncFileWriteChecker history.timestamp to have better millisecond precision. [(PR #11155) ](https://github.com/apple/foundationdb/pull/11155)
* Fixed checkall debug command to output all inconsistent keys. [(PR #11153) ](https://github.com/apple/foundationdb/pull/11153), [(PR #11146) ](https://github.com/apple/foundationdb/pull/11146), [(PR #11141) ](https://github.com/apple/foundationdb/pull/11141), and [(PR #11138) ](https://github.com/apple/foundationdb/pull/11138)
* Fixed unreachable processes in status json due to stale tester interfaces at cluster controller. [(PR #11149) ](https://github.com/apple/foundationdb/pull/11149)

7.1.53
======
* Same as 7.1.52 release with AVX enabled.

7.1.52
======
* Released with AVX disabled.
* Fixed testers from being overloaded by consistency checkers. [(PR #11127) ](https://github.com/apple/foundationdb/pull/11127)

7.1.51
======
* Same as 7.1.50 release with AVX enabled.

7.1.50
======
* Released with AVX disabled.
* Added urgent consistency checker mode that uses multiple tester processes. [(PR #11103) ](https://github.com/apple/foundationdb/pull/11103)
* Fixed checkall debug command when shard to check is large. [(PR #11105) ](https://github.com/apple/foundationdb/pull/11105)

7.1.49
======
* Same as 7.1.48 release with AVX enabled.

7.1.48
======
* Released with AVX disabled.
* Added consistency checker urgent mode. [(PR #11102) ](https://github.com/apple/foundationdb/pull/11102)
* Increased the number of keys that "fdbcli checkall" command can fetch per GetKeyValueRequest call. [(PR #11098) ](https://github.com/apple/foundationdb/pull/11098)

7.1.47
======
* Same as 7.1.46 release with AVX enabled.

7.1.46
======
* Released with AVX disabled.
* Added range-customizable consistency checker and fixed retriable errors during the check. [(PR #11100) ](https://github.com/apple/foundationdb/pull/11100) and [(PR #11096) ](https://github.com/apple/foundationdb/pull/11096)
* Added key ranges as filters for fdbdecode. [(PR #11099) ](https://github.com/apple/foundationdb/pull/11099)

7.1.45
======
* Same as 7.1.44 release with AVX enabled.

7.1.44
======
* Released with AVX disabled.
* Added distributed consistency checker. [(PR #11088) ](https://github.com/apple/foundationdb/pull/11088)
* Fixed the exclusions of localities that are not matching any process. [(PR #11034) ](https://github.com/apple/foundationdb/pull/11034)

7.1.43
======
* Same as 7.1.42 release with AVX enabled.

7.1.42
======
* Released with AVX disabled.
* Added redistribute fdbcli command to manually split shards. [(PR #10909) ](https://github.com/apple/foundationdb/pull/10909), [(PR #10936) ](https://github.com/apple/foundationdb/pull/10936), [(PR #10942) ](https://github.com/apple/foundationdb/pull/10942), [(PR #10905) ](https://github.com/apple/foundationdb/pull/10905), and [(PR #10958) ](https://github.com/apple/foundationdb/pull/10958)
* Fixed a MacOS linking issue for go bindings. [(PR #10924) ](https://github.com/apple/foundationdb/pull/10924)
* Added knobs to control backup retry delays for blob stores. [(PR #10947) ](https://github.com/apple/foundationdb/pull/10947)
* Fixed two use-after-free bugs for backup agents. [(PR #10951) ](https://github.com/apple/foundationdb/pull/10951)
* Added automatic range split for hot storage queue. [(PR #10932) ](https://github.com/apple/foundationdb/pull/10932)
* Fixed multiple bugs related to locality based exclusions. [(PR #10976) ](https://github.com/apple/foundationdb/pull/10976), [(PR #11008) ](https://github.com/apple/foundationdb/pull/11008), and [(PR #11025) ](https://github.com/apple/foundationdb/pull/11025)
* Fixed compaction rate limiter for RocksDB storage engine. [(PR #10988) ](https://github.com/apple/foundationdb/pull/10988)
* Added a perpetual wiggle option to have multiple storage servers in rebalance state during wiggling. [(PR #10995) ](https://github.com/apple/foundationdb/pull/10995)
* Fixed exclude status of machines in status json when not all processes are excluded. [(PR #10996) ](https://github.com/apple/foundationdb/pull/10996) and [(PR #11006) ](https://github.com/apple/foundationdb/pull/11006)

7.1.41
======
* Same as 7.1.40 release with AVX enabled.

7.1.40
======
* Released with AVX disabled.
* Removed storageWiggleID from storage metadata if the storage server is not wiggling. [(PR #10913) ](https://github.com/apple/foundationdb/pull/10913)
* Augmented storage team selection to be aware of storage queue sizes. [(PR #10905) ](https://github.com/apple/foundationdb/pull/10905)
* Fixed the proxy setting for backup agents. [(PR #10903) ](https://github.com/apple/foundationdb/pull/10903)
* Added an option to set perpetual_storage_wiggle_engine to none. [(PR #10881) ](https://github.com/apple/foundationdb/pull/10881)

7.1.39
======
* Same as 7.1.38 release with AVX enabled.

7.1.38
======
* Released with AVX disabled.
* Added locality check on reading perpetualStorageWiggleIDPrefix key when DD restarts. [(PR #10864) ](https://github.com/apple/foundationdb/pull/10864)
* Added perpetual wiggle wait based on data balance of the cluster. [(PR #10865) ](https://github.com/apple/foundationdb/pull/10865)
* Added rocksdb options to delete old rocksdb logs. [(PR #10872) ](https://github.com/apple/foundationdb/pull/10872)
* Added knob to guard the gray failure rejection during TLog recovery. [(PR #10852) ](https://github.com/apple/foundationdb/pull/10852)
* Added knob RESOLVE_PREFER_IPV4_ADDR to prefer IPv4 addresses. [(PR #10826) ](https://github.com/apple/foundationdb/pull/10826)
* Added perpetual_storage_wiggle_engine config to support storage migration with perpetual wiggle. [(PR #10790) ](https://github.com/apple/foundationdb/pull/10790)
* Fixed the return code for perpetual wiggle configure command. [(PR #10795) ](https://github.com/apple/foundationdb/pull/10795)
* Fixed a compatibility issue of s3 backup. [(PR #10774) ](https://github.com/apple/foundationdb/pull/10774)
* Added proxy to backup agent via global variable. [(PR #10875) ](https://github.com/apple/foundationdb/pull/10875)

7.1.37
======
* Same as 7.1.36 release with AVX enabled.

7.1.36
======
* Released with AVX disabled.
* Added consistency check for rocksdb only [(PR #10751) ](https://github.com/apple/foundationdb/pull/10751)
* Fixed grv queue stats when requests are dropped [(PR #10753) ](https://github.com/apple/foundationdb/pull/10753)

7.1.35
======
* Same as 7.1.34 release with AVX enabled.

7.1.34
======
* Released with AVX disabled.
* Fixed a high GRV latency issue when many storage servers are recruited.  [(PR #10688) ](https://github.com/apple/foundationdb/pull/10688)
* Fixed a single key deletion bug when ROCKSDB_SINGLEKEY_DELETES_ON_CLEARRANGE is enabled. [(PR #10672) ](https://github.com/apple/foundationdb/pull/10672) and [(PR #10676) ](https://github.com/apple/foundationdb/pull/10676)
* Added degraded and disconnected peer recovery to gray failure detection. [(PR #10565) ](https://github.com/apple/foundationdb/pull/10565)
* Fixed RocksDB engine to avoid read timeout checks for eager reads and system keys. [(PR #10500) ](https://github.com/apple/foundationdb/pull/10500)
* Fixed backup to work with s3 compatible service.  [(PR #10369) ](https://github.com/apple/foundationdb/pull/10369)
* Fixed data loss during multiple range restores. [(PR #10424) ](https://github.com/apple/foundationdb/pull/10424)
* Updated RocksDB to version 8.1.1. [(PR #10268) ](https://github.com/apple/foundationdb/pull/10268)

7.1.33
======
* Same as 7.1.32 release with AVX enabled.

7.1.32
======
* Released with AVX disabled.
* Increased MAX_STORAGE_COMMIT_TIME to reduce storage server IO timeout errors. [(PR #10116) ](https://github.com/apple/foundationdb/pull/10116)
* Fixed a CPU spinning issue on DR destination clusters. [(PR #10114) ](https://github.com/apple/foundationdb/pull/10114)
* Fixed a inconsistent read issue when using RocksDB engine. [(PR #10060) ](https://github.com/apple/foundationdb/pull/10060)
* Fixed a storage server failure issue and added client backoff when commit proxy is overloaded for key location requests. [(PR #10007) ](https://github.com/apple/foundationdb/pull/10007)
* Added gray failure detection of disconnected remote log routers. [(PR #9933) ](https://github.com/apple/foundationdb/pull/9933)

7.1.31
======
* Same as 7.1.30 release with AVX enabled.

7.1.30
======
* Released with AVX disabled.
* Fixed storage server finishedQueries metric when using getMappedRange. [(PR #9785) ](https://github.com/apple/foundationdb/pull/9785)
* Fixed unnecessary transaction system recovery when excluding the servers that are already excluded/failed. [(PR #9809) ](https://github.com/apple/foundationdb/pull/9809) and [(PR #9878) ](https://github.com/apple/foundationdb/pull/9878)
* Fixed the exclusion of stateless processes by skipping the free capacity check. [(PR #9789) ](https://github.com/apple/foundationdb/pull/9789) and [(PR #9769) ](https://github.com/apple/foundationdb/pull/9769)
* Fixed an issue where the new worker cannot get ServerDBInfo update. [(PR #9778) ](https://github.com/apple/foundationdb/pull/9778)
* Added RocksDB bloom filter knobs. [(PR #9770) ](https://github.com/apple/foundationdb/pull/9770)
* Upgraded RocksDB to version 7.10.2. [(PR #9829) ](https://github.com/apple/foundationdb/pull/9829)
* Fixed an issue where ExclusionSafetyCheckRequest could be blocked forever. [(PR #9871) ](https://github.com/apple/foundationdb/pull/9871)
* Fixed fdbserver not able to join the cluster if the majority of coordinators in its connection string have failed. [(PR #9883) ](https://github.com/apple/foundationdb/pull/9883)
* Fixed stuck data movement when moving to removed a storage server. [(PR #9904) ](https://github.com/apple/foundationdb/pull/9904)

7.1.29
======
* Same as 7.1.28 release with AVX enabled.

7.1.28
======
* Released with AVX disabled.
* Changed log router to detect slow peeks and to automatically switch DC for peeking. [(PR #9640) ](https://github.com/apple/foundationdb/pull/9640)
* Added multiple prefix filter support for fdbdecode. [(PR #9483) ](https://github.com/apple/foundationdb/pull/9483), [(PR #9489) ](https://github.com/apple/foundationdb/pull/9489), [(PR #9511) ](https://github.com/apple/foundationdb/pull/9511), and [(PR #9560) ](https://github.com/apple/foundationdb/pull/9560)
* Enhanced fdbbackup query command to estimate data processing from a specific snapshot to a target version. [(PR #9506) ](https://github.com/apple/foundationdb/pull/9506)
* Improved PTree insertion and erase performance for storage servers. [(PR #9508) ](https://github.com/apple/foundationdb/pull/9508)
* Added exclude to fdbcli's configure command to prevent faulty TLogs from affecting recovery. [(PR #9404) ](https://github.com/apple/foundationdb/pull/9404)
* Fixed getMappedRange metrics. [(PR #9331) ](https://github.com/apple/foundationdb/pull/9331)

7.1.27
======
* Same as 7.1.26 release with AVX enabled.

7.1.26
======
* Released with AVX disabled.
* Added detection of disconnection to satellite TLog in gray failure detection. [(PR #9107) ](https://github.com/apple/foundationdb/pull/9107)
* Fixed (non)empty peeks stats in TLogMetrics. [(PR #9074) ](https://github.com/apple/foundationdb/pull/9074)
* Fixed a data distribution bug where exclusions can become stuck because DD cannot build new teams. [(PR #9035) ](https://github.com/apple/foundationdb/pull/9035)
* Added FoundationDB version to ProcessMetrics. [(PR #9037) ](https://github.com/apple/foundationdb/pull/9037)
* Removed RocksDB read iterator destruction from the commit path. [(PR #8971) ](https://github.com/apple/foundationdb/pull/8971)
* Added determinstic degraded server selection in gray failure detection. [(PR #9001) ](https://github.com/apple/foundationdb/pull/9001)
* Fixed an interger overflow bug that causes fetching backup files to fail. [(PR #8996) ](https://github.com/apple/foundationdb/pull/8996)
* Fixed a log router race condition that blocks remote tlogs forever. [(PR #8966) ](https://github.com/apple/foundationdb/pull/8966)
* Fixed a backup worker assertion failure. [(PR #8887) ](https://github.com/apple/foundationdb/pull/8887)
* Upgraded RocksDB to 7.7.3 version. [(PR #8880) ](https://github.com/apple/foundationdb/pull/8880)
* Added byte limit for index prefetch. [(PR #8802) ](https://github.com/apple/foundationdb/pull/8802)
* Added storage server read range bytes metrics. [(PR #8724) ](https://github.com/apple/foundationdb/pull/8724)
* Added counters for single key clear requests. [(PR #8792) ](https://github.com/apple/foundationdb/pull/8792)
* Added more RocksDB knobs. [(PR #8713) ](https://github.com/apple/foundationdb/pull/8713), [(PR #8862) ](https://github.com/apple/foundationdb/pull/8862), and [(PR #9165) ](https://github.com/apple/foundationdb/pull/9165)
* Added a new network option "retain_client_library_copies" to keep the client library copies. [(PR #8740) ](https://github.com/apple/foundationdb/pull/8740)
* Fixed a transaction_too_old error on storage servers when version vector is enabled. [(PR #8710) ](https://github.com/apple/foundationdb/pull/8710)

7.1.25
======
* Same as 7.1.24 release with AVX enabled.

7.1.24
======
* Released with AVX disabled.
* Fixed a transaction log data corruption bug. [(PR #8525) ](https://github.com/apple/foundationdb/pull/8525), [(PR #8562) ](https://github.com/apple/foundationdb/pull/8562), and [(PR #8647) ](https://github.com/apple/foundationdb/pull/8647)
* Fixed a rare data race in transaction logs when PEEK_BATCHING_EMPTY_MSG is enabled. [(PR #8660) ](https://github.com/apple/foundationdb/pull/8660)
* Fixed a heap-use-after-free bug in cluster controller.  [(PR #8683) ](https://github.com/apple/foundationdb/pull/8683)
* Changed consistency check to report all corruptions. [(PR #8571) ](https://github.com/apple/foundationdb/pull/8571)
* Fixed a rare storage server crashing bug after recovery. [(PR #8468) ](https://github.com/apple/foundationdb/pull/8468)
* Added client knob UNLINKONLOAD_FDBCLIB to control deletion of external client libraries. [(PR #8434) ](https://github.com/apple/foundationdb/pull/8434)
* Updated the default peer latency degradation percentile to 0.5. [(PR #8370) ](https://github.com/apple/foundationdb/pull/8370)
* Made exclusion less pessimistic when warning about low space usage. [(PR #8347) ](https://github.com/apple/foundationdb/pull/8347) 
* Added storage server readrange and update latency metrics. [(PR #8353) ](https://github.com/apple/foundationdb/pull/8353)
* Increased the default PEER_DEGRADATION_CONNECTION_FAILURE_COUNT value to 5s. [(PR #8336) ](https://github.com/apple/foundationdb/pull/8336)
* Increased RocksDB block cache size. [(PR #8274) ](https://github.com/apple/foundationdb/pull/8274)

7.1.23
======
* Same as 7.1.22 release with AVX enabled.

7.1.22
======
* Released with AVX disabled.
* Added new latency samples for GetValue, GetRange, QueueWait, and VersionWait in storage servers. [(PR #8215) ](https://github.com/apple/foundationdb/pull/8215)
* Fixed a rare partial data write for TLogs. [(PR #8210) ](https://github.com/apple/foundationdb/pull/8210)
* Added HTTP proxy support for backup agents. [(PR #8193) ](https://github.com/apple/foundationdb/pull/8193)
* Fixed a memory bug of secondary queries in index prefetch. [(PR #8195) ](https://github.com/apple/foundationdb/pull/8195), [(PR #8190) ](https://github.com/apple/foundationdb/pull/8190)
* Introduced STORAGE_SERVER_REBOOT_ON_IO_TIMEOUT knob to recreate SS at io_timeout errors. [(PR #8123) ](https://github.com/apple/foundationdb/pull/8123)
* Fixed two TLog stopped bugs and a CC leader replacement bug. [(PR #8081) ](https://github.com/apple/foundationdb/pull/8081)
* Added back RecoveryAvailable trace event for status's seconds_since_last_recovered field. [(PR #8068) ](https://github.com/apple/foundationdb/pull/8068)

7.1.21
======
* Same as 7.1.20 release with AVX enabled.

7.1.20
======
* Released with AVX disabled.
* Fixed missing localities for fdbserver that can cause cross DC calls among storage servers. [(PR #7995) ](https://github.com/apple/foundationdb/pull/7995)
* Removed extremely spammy trace event in FetchKeys and fixed transaction_profiling_analyzer.py. [(PR #7934) ](https://github.com/apple/foundationdb/pull/7934)
* Fixed bugs when GRV proxy returns an error. [(PR #7860) ](https://github.com/apple/foundationdb/pull/7860)

7.1.19
======
* Same as 7.1.18 release with AVX enabled.

7.1.18
======
* Released with AVX disabled.
* Added knobs for the minimum and the maximum of the Ratekeeper's default priority. [(PR #7820) ](https://github.com/apple/foundationdb/pull/7820)
* Fixed bugs in ``getRange`` of the special key space. [(PR #7778) ](https://github.com/apple/foundationdb/pull/7778), [(PR #7720) ](https://github.com/apple/foundationdb/pull/7720)
* Added debug ID for secondary queries in index prefetching. [(PR #7755) ](https://github.com/apple/foundationdb/pull/7755)
* Changed hostname resolving to prefer IPv6 addresses. [(PR #7750) ](https://github.com/apple/foundationdb/pull/7750)
* Added more transaction debug events for prefetch queries. [(PR #7732) ](https://github.com/apple/foundationdb/pull/7732)

7.1.17
======
* Same as 7.1.16 release with AVX enabled.

7.1.16
======
* Released with AVX disabled.
* Fixed a crash bug when cluster controller shuts down. [(PR #7706) ](https://github.com/apple/foundationdb/pull/7706)
* Fixed a storage server failure when getReadVersion returns an error. [(PR #7688) ](https://github.com/apple/foundationdb/pull/7688)
* Fixed unbounded status json generation. [(PR #7680) ](https://github.com/apple/foundationdb/pull/7680)
* Fixed ScopeEventFieldTypeMismatch error for TLogMetrics. [(PR #7640) ](https://github.com/apple/foundationdb/pull/7640)
* Added getMappedRange latency metrics. [(PR #7632) ](https://github.com/apple/foundationdb/pull/7632)
* Fixed a version vector performance bug due to not updating client side tag cache. [(PR #7616) ](https://github.com/apple/foundationdb/pull/7616)
* Fixed DiskReadSeconds and DiskWriteSeconds calculation in ProcessMetrics. [(PR #7609) ](https://github.com/apple/foundationdb/pull/7609)
* Added Rocksdb compression and data size stats. [(PR #7596) ](https://github.com/apple/foundationdb/pull/7596)

7.1.15
======
* Same as 7.1.14 release with AVX enabled.

7.1.14
======
* Released with AVX disabled.
* Fixed a high commit latency bug when there are data movement. [(PR #7548) ](https://github.com/apple/foundationdb/pull/7548)
* Fixed the primary locality on the sequencer by obtaining it from cluster controller. [(PR #7535) ](https://github.com/apple/foundationdb/pull/7535)
* Added StorageEngine type to StorageMetrics trace events. [(PR #7546) ](https://github.com/apple/foundationdb/pull/7546)
* Improved hasIncompleteVersionstamp performance in Java binding to use iteration rather than stream processing. [(PR #7559) ](https://github.com/apple/foundationdb/pull/7559)

7.1.13
======
* Same as 7.1.12 release with AVX enabled.

7.1.12
======
* Released with AVX disabled.
* Optimized out the version vector specific code on the client when version vector is disabled. [(PR #7528) ](https://github.com/apple/foundationdb/pull/7528)
* Added pipelining for secondary queries in index prefetch. [(PR #7507) ](https://github.com/apple/foundationdb/pull/7507)
* Fixed a connection failure bug when using DNS names. [(PR #7478) ](https://github.com/apple/foundationdb/pull/7478)
* Fixed delays in version advancement that can be larger than knob MAX_COMMIT_BATCH_INTERVAL. [(PR #7518) ](https://github.com/apple/foundationdb/pull/7518)
* Removed explicit degraded peer recovery in gray failure detection since this may be false positive. [(PR #7466) ](https://github.com/apple/foundationdb/pull/7466)
* Fixed undefined behavior from accessing field of uninitialized object. [(PR #7430) ](https://github.com/apple/foundationdb/pull/7430)

7.1.11
======
* Same as 7.1.10 release with AVX enabled.

7.1.10
======
* Released with AVX disabled.
* Fixed a sequencer crash when DC ID is a string. [(PR #7393) ](https://github.com/apple/foundationdb/pull/7393)
* Fixed a client performance regression by removing unnecessary transaction initialization. [(PR #7365) ](https://github.com/apple/foundationdb/pull/7365)
* Safely removed fdb_transaction_get_range_and_flat_map C API. [(PR #7379) ](https://github.com/apple/foundationdb/pull/7379)
* Fixed an unknown error bug when hostname resolving fails. [(PR #7380) ](https://github.com/apple/foundationdb/pull/7380)

7.1.9
=====
* Same as 7.1.8 release with AVX enabled.

7.1.8
=====
* Released with AVX disabled.
* Fixed a performance regression in network run loop.  [(PR #7342) ](https://github.com/apple/foundationdb/pull/7342)
* Added RSS bytes for processes in status json output and corrected available_bytes calculation. [(PR #7348) ](https://github.com/apple/foundationdb/pull/7348)
* Added versionstamp support in tuples. [(PR #7313) ](https://github.com/apple/foundationdb/pull/7313)
* Fixed some spammy trace events. [(PR #7300) ](https://github.com/apple/foundationdb/pull/7300)
* Avoided a memory corruption bug by disabling streaming peeks. [(PR #7288) ](https://github.com/apple/foundationdb/pull/7288)
* Fixed a hang bug in fdbcli exclude command. [(PR #7268) ](https://github.com/apple/foundationdb/pull/7268)
* Fixed an issue that a remote TLog blocks peeks. [(PR #7255) ](https://github.com/apple/foundationdb/pull/7255)
* Fixed a connection issue using hostnames. [(PR #7264) ](https://github.com/apple/foundationdb/pull/7264)
* Added support of the reboot command in go bindings. [(PR #7270) ](https://github.com/apple/foundationdb/pull/7270)
* Fixed several issues in profiling special keys using GlobalConfig. [(PR #7120) ](https://github.com/apple/foundationdb/pull/7120)
* Fixed a stuck transaction system bug due to inconsistent recovery transaction version. [(PR #7261) ](https://github.com/apple/foundationdb/pull/7261)
* Fixed an unknown_error crash due to not resolving hostnames. [(PR #7254) ](https://github.com/apple/foundationdb/pull/7254)
* Fixed a heap-use-after-free bug. [(PR #7250) ](https://github.com/apple/foundationdb/pull/7250)
* Fixed a performance issue that remote TLogs are sending too many pops to log routers. [(PR #7235) ](https://github.com/apple/foundationdb/pull/7235)
* Fixed an issue that SharedTLogs are not displaced and leaking disk space. [(PR #7246) ](https://github.com/apple/foundationdb/pull/7246)
* Fixed an issue that coordinatorsKey does not store DNS names. [(PR #7203) ](https://github.com/apple/foundationdb/pull/7203)
* Fixed a sequential execution issue for fdbcli kill, suspend, and expensive_data_check commands. [(PR #7211) ](https://github.com/apple/foundationdb/pull/7211)

7.1.7
=====
* Same as 7.1.6 release with AVX enabled.

7.1.6
=====
* Released with AVX disabled.
* Fixed a fdbserver crash when given invalid knob name. [(PR #7189) ](https://github.com/apple/foundationdb/pull/7189)
* Fixed a storage server bug that read data after its failure. [(PR #7217) ](https://github.com/apple/foundationdb/pull/7217)

7.1.5
=====
* Fixed a fdbcli kill bug that was not killing in parallel. [(PR #7150) ](https://github.com/apple/foundationdb/pull/7150)
* Fixed a bug that prevents a peer from sending messages on a previously incompatible connection. [(PR #7124) ](https://github.com/apple/foundationdb/pull/7124)
* Added rocksdb throttling counters to trace event. [(PR #7096) ](https://github.com/apple/foundationdb/pull/7096)
* Added a backtrace before throwing serialization_failed. [(PR #7155) ](https://github.com/apple/foundationdb/pull/7155)

7.1.4
=====
* Fixed a bug that prevents client from connecting to a cluster. [(PR #7060) ](https://github.com/apple/foundationdb/pull/7060)
* Fixed a performance bug that overloads Resolver CPU. [(PR #7068) ](https://github.com/apple/foundationdb/pull/7068)
* Optimized storage server performance for "get range and flat map" feature. [(PR #7078) ](https://github.com/apple/foundationdb/pull/7078)
* Optimized both Proxy performance and Resolver (when version vector is enabled) performance. [(PR #7076) ](https://github.com/apple/foundationdb/pull/7076)
* Fixed a key size limit bug when using tenants. [(PR #6986) ](https://github.com/apple/foundationdb/pull/6986)
* Fixed operation_failed thrown incorrectly from transactions. [(PR #6993) ](https://github.com/apple/foundationdb/pull/6993)
* Fixed a version vector bug when GRV cache is used. [(PR #7057) ](https://github.com/apple/foundationdb/pull/7057)
* Fixed orphaned storage server due to force recovery. [(PR #7028) ](https://github.com/apple/foundationdb/pull/7028)
* Fixed a bug that a storage server reads stale cluster ID. [(PR #7026) ](https://github.com/apple/foundationdb/pull/7026)
* Fixed a storage server exclusion status bug that affects wiggling. [(PR #6984) ](https://github.com/apple/foundationdb/pull/6984)
* Fixed a bug that relocate shard tasks move data to a removed team. [(PR #7023) ](https://github.com/apple/foundationdb/pull/7023)
* Fixed recruitment thrashing when there are temporarily multiple cluster controllers. [(PR #7001) ](https://github.com/apple/foundationdb/pull/7001)
* Fixed change feed deletion due to multiple sources race. [(PR #6987) ](https://github.com/apple/foundationdb/pull/6987)
* Fixed TLog crash if more TLogs are absent than the replication factor. [(PR #6991) ](https://github.com/apple/foundationdb/pull/6991)
* Added hostname DNS resolution logic for cluster connection string. [(PR #6998) ](https://github.com/apple/foundationdb/pull/6998)
* Fixed a limit bug in indexPrefetch. [(PR #7005) ](https://github.com/apple/foundationdb/pull/7005)

7.1.3
=====
* Added logging measuring commit compute duration. [(PR #6906) ](https://github.com/apple/foundationdb/pull/6906)
* RocksDb used aggregated property metrics for pending compaction bytes. [(PR #6867) ](https://github.com/apple/foundationdb/pull/6867)
* Fixed a perpetual wiggle bug that would not react to a pause. [(PR #6933) ](https://github.com/apple/foundationdb/pull/6933)
* Fixed a crash of data distributor. [(PR #6938) ](https://github.com/apple/foundationdb/pull/6938)
* Added new c libs to client package. [(PR #6921) ](https://github.com/apple/foundationdb/pull/6921)
* Fixed a bug that prevents a cluster from fully recovered state after taking a snapshot. [(PR #6892) ](https://github.com/apple/foundationdb/pull/6892)

7.1.2
=====
* Fixed failing upgrades due to non-persisted initial cluster version. [(PR #6864) ](https://github.com/apple/foundationdb/pull/6864)
* Fixed a client load balancing bug because ClientDBInfo may be unintentionally not set. [(PR #6878) ](https://github.com/apple/foundationdb/pull/6878)
* Fixed stuck LogRouter due to races of multiple PeekStream requests. [(PR #6870) ](https://github.com/apple/foundationdb/pull/6870)
* Fixed a client-side infinite loop due to provisional GRV Proxy ID not set in GetReadVersionReply. [(PR #6849) ](https://github.com/apple/foundationdb/pull/6849)

7.1.1
=====
* Added new c libs to client package. [(PR #6828) ](https://github.com/apple/foundationdb/pull/6828)

7.1.0
=====

Features
--------
* Added ``USE_GRV_CACHE`` transaction option to allow read versions to be locally cached on the client side for latency optimizations. [(PR #5725) ](https://github.com/apple/foundationdb/pull/5725) [(PR #6664) ](https://github.com/apple/foundationdb/pull/6664)
* Added "get range and flat map" feature with new APIs (see Bindings section). Storage servers are able to generate the keys in the queries based on another query. With this, upper layer can push some computations down to FDB, to improve latency and bandwidth when read. [(PR #5609) ](https://github.com/apple/foundationdb/pull/5609), [(PR #6181) ](https://github.com/apple/foundationdb/pull/6181), etc..

Performance
-----------

Reliability
-----------

Fixes
-----

Status
------
* Added ``cluster.storage_wiggler`` field report storage wiggle stats [(PR #6219) ](https://github.com/apple/foundationdb/pull/6219)

Bindings
--------
* C: Added ``fdb_transaction_get_range_and_flat_map`` function to support running queries based on another query in one request. [(PR #5609) ](https://github.com/apple/foundationdb/pull/5609)
* Java: Added ``Transaction.getRangeAndFlatMap`` function to support running queries based on another query in one request. [(PR #5609) ](https://github.com/apple/foundationdb/pull/5609)

Other Changes
-------------
* OpenTracing support is now deprecated in favor of OpenTelemetry tracing, which will be enabled in a future release. [(PR #6478) ](https://github.com/apple/foundationdb/pull/6478/files)
* Changed ``memory`` option to limit resident memory instead of virtual memory. Added a new ``memory_vsize`` option if limiting virtual memory is desired. [(PR #6719) ](https://github.com/apple/foundationdb/pull/6719)
* Change ``perpetual storage wiggle`` to wiggle the storage servers based on their created time. [(PR #6219) ](https://github.com/apple/foundationdb/pull/6219)

Earlier release notes
---------------------
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
