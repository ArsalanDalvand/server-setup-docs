---
title: "Prometheus metrics v2"
source: "https://redis.io/docs/latest/operate/rs/monitoring/metrics_stream_engine/prometheus-metrics-v2/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/monitoring/metrics_stream_engine/prometheus-metrics-v2"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Prometheus metrics v2"
---
# Prometheus metrics v2

V2 metrics available to Prometheus as of Redis Software version 7.8.2.

You can integrate Redis Software with Prometheus and tools such as [Grafana](/docs/latest/integrate/prometheus-with-redis-enterprise/), [Datadog](/docs/latest/integrate/datadog-with-redis-enterprise/), [Dynatrace](/docs/latest/integrate/dynatrace-with-redis-enterprise/), or [New Relic](/docs/latest/integrate/new-relic-with-redis-enterprise/) to create dashboards for important metrics.

The v2 metrics in the following tables are available as of Redis Software version 7.8.0. For help transitioning from v1 metrics to v2 PromQL, see [[operate/rs/monitoring/metrics_stream_engine/prometheus-metrics-v1-to-v2|Prometheus v1 metrics and equivalent v2 PromQL]].

The v2 scraping endpoint also exposes metrics for `node_exporter` version 1.8.1. For more information, see the [Prometheus node_exporter GitHub repository](https://github.com/prometheus/node_exporter).

## Database metrics

Metric
Type
Unit
Description

endpoint_accepted_connections
counter
count
Number of incoming accepted client connections

endpoint_client_connections
counter
count
Number of client connection establishment events

endpoint_client_disconnections
counter
count
Number of client disconnections initiated by the client

endpoint_client_connection_expired
counter
count
Total number of client connections with expired TTL (Time To Live)

endpoint_client_establishment_failures
counter
count
Number of client connections that failed to establish properly

endpoint_client_expiration_refresh
counter
count
Number of expiration time changes of clients

endpoint_client_tracking_off_requests
counter
count
Total number of `CLIENT TRACKING OFF` requests

endpoint_client_tracking_on_requests
counter
count
Total number of `CLIENT TRACKING ON` requests

endpoint_connections_rate
gauge
connections/second
The rate of incoming connections. Computed as `n_accepted / N` for the last interval where `n_accepted` is the number of accepted connections in this interval, and `N` is the interval in seconds.

endpoint_disconnected_cba_client
counter
count
Number of certificate-based clients disconnected

endpoint_disconnected_ldap_client
counter
count
Number of LDAP clients disconnected

endpoint_disconnected_user_password_client
counter
count
Number of user&password clients disconnected

endpoint_dispatch_failures
counter
count
Number of clients closed due to failure to be dispatched to workers

endpoint_disposed_commands_after_client_caching
counter
count
Total number of client caching commands that were disposed due to misuse

endpoint_egress
counter
bytes
Number of egress bytes

endpoint_egress_pending
counter
bytes
Number of send-pending bytes

endpoint_egress_pending_discarded
counter
bytes
Number of send-pending bytes that were discarded due to disconnection

endpoint_failed_cba_authentication
counter
count
Number of clients that failed certificate-based authentication

endpoint_failed_ldap_authentication
counter
count
Number of clients that failed LDAP authentication

endpoint_failed_user_password_authentication
counter
count
Number of clients that failed user password authentication

endpoint_ingress
counter
bytes
Number of ingress bytes

endpoint_longest_pipeline_histogram
histogram
count
Tracks the distribution of longest observed pipeline lengths, where a pipeline is a sequence of client commands sent without waiting for responses.

endpoint_other_requests
counter
count
Number of other requests

endpoint_other_requests_latency_histogram
histogram
microseconds
Latency (in Âµs) histogram of other commands

endpoint_other_requests_latency_histogram_bucket
histogram
microseconds
Latency histograms for commands other than read or write commands. Can be used to represent different latency percentiles.
p99.9 example:
`histogram_quantile(0.999, sum(rate(endpoint_other_requests_latency_histogram_bucket{cluster="$cluster", db="$db"}[$__rate_interval]) ) by (le, db))`

endpoint_other_responses
counter
count
Number of other responses

endpoint_ping_failures
gauge
count
Number of consecutive endpoint ping failures. Labels: endpoint_uid

endpoint_ping_failure_duration_seconds
gauge
seconds
Duration of ongoing endpoint failures (0 when healthy). Labels: endpoint_uid

endpoint_proxy_disconnections
counter
count
Number of client disconnections initiated by the proxy

endpoint_rate_limit_ok
gauge
â
Rate limit status based on the last 2 intervals.
0 = rate limit was recently exceeded
1 = rate limit was not recently exceeded

endpoint_rate_limit_overflows
counter
count
Total number of rate limit overflows

endpoint_read_requests
counter
count
Number of read requests

endpoint_read_requests_latency_histogram
histogram
microseconds
Latency (in Âµs) histogram of read commands

endpoint_read_requests_latency_histogram_bucket
histogram
microseconds
Latency histograms for read commands. Can be used to represent different latency percentiles.
p99.9 example:
`histogram_quantile(0.999, sum(rate(endpoint_read_requests_latency_histogram_bucket{cluster="$cluster", db="$db"}[$__rate_interval]) ) by (le, db))`

endpoint_read_responses
counter
count
Number of read responses

endpoint_successful_cba_authentication
counter
count
Number of clients that successfully authenticated with certificate-based authentication

endpoint_successful_ldap_authentication
counter
count
Number of clients that successfully authenticated with LDAP

endpoint_successful_user_password_authentication
counter
count
Number of clients that successfully authenticated with user&password

endpoint_write_requests
counter
count
Number of write requests

endpoint_write_requests_latency_histogram
histogram
microseconds
Latency (in Âµs) histogram of write commands

endpoint_write_requests_latency_histogram_bucket
histogram
microseconds
Latency histograms for write commands. Can be used to represent different latency percentiles.
p99.9 example:
`histogram_quantile(0.999, sum(rate(endpoint_write_requests_latency_histogram_bucket{cluster="$cluster", db="$db"}[$__rate_interval]) ) by (le, db))`

endpoint_write_responses
counter
count
Number of write responses

db_config
gauge
â
This is an information metric that holds database configuration within labels such as: db_name, db_version, db_port, tls_mode

db_status
gauge
â
This is a status metric that reports on various database statuses: 0 = active, 1 = active-change-pending, 2 = pending, 3 = import-pending, 4 = delete-pending, 5 = recovery, 99 = unknown

db_tags
gauge
â
Information metric that exposes database tags as labels; the value is always `1`. See [[operate/rs/monitoring/metrics_stream_engine/db-tags-in-metrics|Database tags in metrics]].

## Node metrics

Metric
Type
Unit
Description

node_available_flash_bytes
gauge
bytes
Available flash in the node (bytes)

node_available_flash_no_overbooking_bytes
gauge
bytes
Available flash in the node (bytes), without taking into account overbooking

node_available_memory_bytes
gauge
bytes
Amount of free memory in the node (bytes)

node_available_memory_no_overbooking_bytes
gauge
bytes
Available RAM in the node (bytes) without taking into account overbooking

node_bigstore_free_bytes
gauge
bytes
Sum of free space of back-end flash (used by flash database's [BigRedis]) on all cluster nodes (bytes); returned only when BigRedis is enabled

node_cert_expires_in_seconds
gauge
seconds
Certificate expiration (in seconds) per given node; read more about [certificates in Redis Software](/docs/latest/operate/rs/security/certificates/) and [monitoring certificates](/docs/latest/operate/rs/security/certificates/monitor-certificates/)

customer_managed_ine_certificates
gauge
â
Indicates whether customer-provided internode encryption certificates are in use
0=No
1=Yes

node_ephemeral_storage_avail_bytes
gauge
bytes
Disk space available to RLEC processes on configured ephemeral disk (bytes)

node_ephemeral_storage_free_bytes
gauge
bytes
Free disk space on configured ephemeral disk (bytes)

node_memory_MemFree_bytes
gauge
bytes
Free memory in the node (bytes)

node_persistent_storage_avail_bytes
gauge
bytes
Disk space available to RLEC processes on configured persistent disk (bytes)

node_persistent_storage_free_bytes
gauge
bytes
Free disk space on configured persistent disk (bytes)

node_provisional_flash_bytes
gauge
bytes
Amount of flash available for new shards on this node, taking into account overbooking, max Redis servers, reserved flash, and provision and migration thresholds (bytes)

node_provisional_flash_no_overbooking_bytes
gauge
bytes
Amount of flash available for new shards on this node, without taking into account overbooking, max Redis servers, reserved flash, and provision and migration thresholds (bytes)

node_provisional_memory_bytes
gauge
bytes
Amount of RAM that is available for provisioning to databases out of the total RAM allocated for databases

node_provisional_memory_no_overbooking_bytes
gauge
bytes
Amount of RAM that is available for provisioning to databases out of the total RAM allocated for databases, without taking into account overbooking

node_metrics_up
gauge
â
Node is part of the cluster and is connected (1 = connected, 0 = not connected)

dmc_ping_failures
gauge
count
Number of consecutive DMC ping failures

dmc_ping_failure_duration_seconds
gauge
seconds
Duration of ongoing DMC failures (0 when healthy)

## Cluster metrics

Metric
Type
Unit
Description

generation{node=<node_uid>}
gauge
â
Current generation (state-version) number of the cluster watchdog on the specific node

has_quorum{node=<node_uid>, has_witness_disk=BOOL}
gauge
â
Has_quorum = 1
No quorum = 0

is_primary{node=<node_uid>}
gauge
â
primary = 1
secondary = 0

license_expiration_days
gauge
days
Number of days until the license expires

license_shards_limit
gauge
count
Total shard limit by the license by shard type (ram / flash)

total_live_nodes_count{node=<node_uid>}
gauge
count
Number of live nodes

total_nodes_count{node=<node_uid>}
gauge
count
Number of nodes

primary_selections_total{node=<node_uid>}
counter
count
Monotonic counter for each selection process that started

users_count
gauge
count
Current number of users on the cluster

## Replication metrics

Metric
Type
Unit
Description

database_syncer_config
gauge
â
Used as a placeholder for configuration labels

database_syncer_current_status
gauge
â
Syncer status for traffic; 0 = in-sync, 1 = out of sync

database_syncer_dst_connectivity_state
gauge
â
Destination connectivity state (1 = connected, 0 = disconnected)

database_syncer_dst_connectivity_state_ms
gauge
milliseconds
Destination connectivity state duration

database_syncer_dst_lag
gauge
milliseconds
Lag in milliseconds between the syncer and the destination

database_syncer_dst_repl_offset
gauge
bytes
Offset of the last command acknowledged

database_syncer_flush_counter
gauge
count
Number of destination flushes

database_syncer_ingress_bytes
gauge
bytes
Number of bytes read from source shard

database_syncer_ingress_bytes_decompressed
gauge
bytes
Number of bytes read from source shard

database_syncer_internal_state
gauge
â
Internal state of the syncer

database_syncer_lag_ms
gauge
milliseconds
Lag time between the source and the destination for traffic in milliseconds

database_syncer_rdb_size
gauge
bytes
The source's RDB size in bytes to be transferred during the syncing phase

database_syncer_rdb_transferred
gauge
bytes
Number of bytes transferred from the source's RDB during the syncing phase

database_syncer_src_connectivity_state
gauge
â
Source connectivity state (1 = connected, 0 = disconnected)

database_syncer_src_connectivity_state_ms
gauge
milliseconds
Source connectivity state duration

database_syncer_src_repl_offset
gauge
bytes
Last known source offset

database_syncer_state
gauge
â
Internal state of the shard syncer

database_syncer_syncer_repl_offset
gauge
bytes
Offset of the last command handled by the syncer

database_syncer_total_requests
gauge
count
Number of destination writes

database_syncer_total_responses
gauge
count
Number of destination writes acknowledged

## Shard metrics

Metric
Type
Unit
Description

redis_server_active_defrag_running
gauge
percent
Automatic memory defragmentation current aggressiveness (% cpu)

redis_server_allocator_active
gauge
bytes
Total used memory, including external fragmentation

redis_server_allocator_allocated
gauge
bytes
Total allocated memory

redis_server_allocator_resident
gauge
bytes
Total resident memory (RSS)

redis_server_aof_last_cow_size
gauge
bytes
Last AOFR, CopyOnWrite memory

redis_server_aof_rewrite_in_progress
gauge
â
Indicates whether an AOF rewrite is in progress (1 = in progress, 0 = otherwise)

redis_server_aof_rewrites
gauge
count
Number of AOF rewrites this process executed

redis_server_aof_delayed_fsync
gauge
count
Number of times an AOF fsync caused delays in the main Redis thread (inducing latency); this can indicate that the disk is slow or overloaded

redis_server_blocked_clients
gauge
count
Count the clients waiting on a blocking call

redis_server_connected_clients
gauge
count
Number of client connections to the specific shard

redis_server_connected_slaves
gauge
count
Number of connected replicas

redis_server_db_avg_ttl
gauge
milliseconds
Average TTL of all volatile keys

redis_server_db0_avg_ttl
gauge
milliseconds
Average TTL of all volatile keys. Deprecated.

redis_server_db_keys
gauge
count
Total key count.

redis_server_db0_keys
gauge
count
Total key count. Deprecated.

redis_server_evicted_keys
gauge
count
Keys evicted so far (since restart)

redis_server_expire_cycle_cpu_milliseconds
gauge
milliseconds
The cumulative amount of time spent on active expiry cycles

redis_server_expired_keys
gauge
count
Keys expired so far since restart

redis_server_forwarding_state
gauge
â
Shard forwarding state (1 = on, 0 = off)

redis_server_hashes_items_under_1M
gauge
count
Number of hash keys with under 1 million elements

redis_server_hashes_items_1M_to_8M
gauge
count
Number of hash keys with an element count between 1 million and 8 million

redis_server_hashes_items_over_8M
gauge
count
Number of hash keys with over 8 million elements

redis_server_keys_trimmed
gauge
count
The number of keys that were trimmed in the current or last resharding process

redis_server_keyspace_read_hits
gauge
count
Number of read operations accessing an existing keyspace

redis_server_keyspace_read_misses
gauge
count
Number of read operations accessing a non-existing keyspace

redis_server_keyspace_write_hits
gauge
count
Number of write operations accessing an existing keyspace

redis_server_keyspace_write_misses
gauge
count
Number of write operations accessing a non-existing keyspace

redis_server_lists_items_under_1M
gauge
count
Number of list keys with under 1 million elements

redis_server_lists_items_1M_to_8M
gauge
count
Number of list keys with an element count between 1 million and 8 million

redis_server_lists_items_over_8M
gauge
count
Number of list keys with over 8 million elements

redis_server_master_link_status
gauge
â
Indicates whether the replica is connected to its master (1 = up, 2 = down, 3 = none, 99 = unknown)

redis_server_master_repl_offset
gauge
bytes
Number of bytes sent to replicas by the shard; calculate the throughput for a time period by comparing the value at different times

redis_server_master_sync_in_progress
gauge
â
The primary shard is synchronizing (1 true; 0 false)

redis_server_max_process_mem
gauge
bytes
Current memory limit configured by redis_mgr according to node free memory

redis_server_maxmemory
gauge
bytes
Current memory limit configured by redis_mgr according to database memory limits. 

To calculate the percent memory usage:
`sum by (cluster,db)(redis_server_used_memory{role="master"}) / (avg by(cluster,db)(db_memory_limit_bytes) / max by(cluster,db)(db_replication_factor))`

redis_server_mem_aof_buffer
gauge
bytes
Current size of AOF buffer

redis_server_mem_clients_normal
gauge
bytes
Current memory used for input and output buffers of non-replica clients

redis_server_mem_clients_slaves
gauge
bytes
Current memory used for input and output buffers of replica clients

redis_server_mem_fragmentation_ratio
gauge
ratio
Memory fragmentation ratio (1.3 means 30% overhead)

redis_server_mem_not_counted_for_evict
gauge
bytes
Portion of used_memory (in bytes) that's not counted for eviction and OOM error

redis_server_mem_replication_backlog
gauge
bytes
Size of replication backlog

redis_server_slave_buffer
gauge
bytes
Reports the effective replica output buffer hard limit for a shard, derived from the database slave_buffer configuration. For slave_buffer=auto, the hard limit is calculated as used_memory * auto_slavebuf_ratio / 100, then bounded by auto_slavebuf_min and auto_slavebuf_max.

redis_server_module_fork_in_progress
gauge
â
A binary value that indicates if there is an active fork spawned by a module (1) or not (0)

namedprocess_namegroup_cpu_seconds_total
counter
seconds
Shard process CPU usage in seconds

namedprocess_namegroup_thread_cpu_seconds_total
counter
seconds
Shard main thread CPU time spent in seconds

namedprocess_namegroup_open_filedesc
gauge
count
Shard number of open file descriptors

namedprocess_namegroup_memory_bytes
gauge
bytes
Shard memory size in bytes

namedprocess_namegroup_oldest_start_time_seconds
gauge
timestamp seconds
Shard start time of the process since unix epoch in seconds

redis_server_rdb_bgsave_in_progress
gauge
â
Indicates whether bgsave is currently in progress (1 = in progress, 0 = otherwise)

redis_server_rdb_last_cow_size
gauge
bytes
Last bgsave (or SYNC fork) used CopyOnWrite memory

redis_server_rdb_saves
gauge
count
Total count of bgsaves since the process was restarted (including replica fullsync and persistence)

redis_server_sets_items_under_1M
gauge
count
Number of set keys with under 1 million elements

redis_server_sets_items_1M_to_8M
gauge
count
Number of set keys with an element count between 1 million and 8 million

redis_server_sets_items_over_8M
gauge
count
Number of set keys with over 8 million elements

redis_server_repl_touch_bytes
gauge
bytes
Number of bytes sent to replicas as TOUCH commands by the shard as a result of a READ command that was processed; calculate the throughput for a time period by comparing the value at different times

redis_server_total_commands_processed
gauge
count
Number of commands processed by the shard; calculate the number of commands for a time period by comparing the value at different times

redis_server_total_connections_received
gauge
count
Number of connections received by the shard; calculate the number of connections for a time period by comparing the value at different times

redis_server_total_net_input_bytes
gauge
bytes
Number of bytes received by the shard; calculate the throughput for a time period by comparing the value at different times

redis_server_total_net_output_bytes
gauge
bytes
Number of bytes sent by the shard; calculate the throughput for a time period by comparing the value at different times

redis_server_up
gauge
â
Shard is up and running (1 = up, 0 = down)

redis_server_strings_sizes_under_128M
gauge
count
Number of string keys with a memory size under 128 megabytes

redis_server_strings_sizes_128M_to_512M
gauge
count
Number of string keys with a memory size between 128 and 512 megabytes

redis_server_strings_sizes_over_512M
gauge
count
Number of string keys with a memory size over 512 megabytes

redis_server_used_memory
gauge
bytes
Memory used by shard (in BigRedis this includes flash) (bytes)

redis_server_zsets_items_under_1M
gauge
count
Number of sorted set keys with under 1 million elements

redis_server_zsets_items_1M_to_8M
gauge
count
Number of sorted set keys with an element count between 1 million and 8 million

redis_server_zsets_items_over_8M
gauge
count
Number of sorted set keys with over 8 million elements

redis_server_search_gc_bytes_collected
gauge
bytes
The total amount of memory freed by the garbage collectors from indexes in the shard's memory in bytes. [4](#tnote-4)

redis_server_search_bytes_collected
gauge
bytes
The total amount of memory freed by the garbage collectors from indexes in the shard's memory in bytes. Deprecated in 8.0 (renamed redis_server_search_gc_bytes_collected), but still available in older versions. [1](#tnote-1)

redis_server_search_gc_marked_deleted_vectors
gauge
count
The number of vectors marked as deleted in the vector indexes that have not yet been cleaned. [4](#tnote-4)

redis_server_search_marked_deleted_vectors
gauge
count
The number of vectors marked as deleted in the vector indexes that have not yet been cleaned. Deprecated in 8.0 (renamed redis_server_search_gc_marked_deleted_vectors), but still available in older versions. [1](#tnote-1)

redis_server_search_gc_total_cycles
gauge
count
The total number of garbage collection cycles executed. [4](#tnote-4)

redis_server_search_total_cycles
gauge
count
The total number of garbage collection cycles executed. Deprecated in 8.0 (renamed redis_server_search_gc_total_cycles), but still available in older versions. [1](#tnote-1)

redis_server_search_gc_total_docs_not_collected_by_gc
gauge
count
The number of documents marked as deleted, whose memory has not yet been freed by the garbage collector. [4](#tnote-4)

redis_server_search_total_docs_not_collected_by_gc
gauge
count
The number of documents marked as deleted, whose memory has not yet been freed by the garbage collector. Deprecated in 8.0 (renamed redis_server_search_gc_total_docs_not_collected_by_gc), but still available in older versions. [1](#tnote-1)

redis_server_search_gc_total_ms_run
gauge
milliseconds
The total duration of all garbage collection cycles in the shard, measured in milliseconds. [4](#tnote-4)

redis_server_search_total_ms_run
gauge
milliseconds
The total duration of all garbage collection cycles in the shard, measured in milliseconds. Deprecated in 8.0 (renamed redis_server_search_gc_total_ms_run), but still available in older versions. [1](#tnote-1)

redis_server_search_cursors_internal_idle
gauge
count
The total number of coordinator cursors that are currently holding pending results in the shard. [4](#tnote-4)

redis_server_search_cursors_user_idle
gauge
count
The total number of cursors that were explicitly requested by users, that are currently holding pending results in the shard. [4](#tnote-4)

redis_server_search_global_idle
gauge
count
The total number of user and internal cursors currently holding pending results in the shard. Deprecated in 8.0 (split into redis_server_search_cursors_internal_idle and redis_server_search_cursors_user_idle), but still available in older versions. [1](#tnote-1)

redis_server_search_cursors_internal_active
gauge
count
The total number of coordinator cursors in the shard, either holding pending results or actively executing `FT.CURSOR READ`. [4](#tnote-4)

redis_server_search_cursors_user_active
gauge
count
The total number of user cursors in the shard, either holding pending results or actively executing `FT.CURSOR READ`. [4](#tnote-4)

redis_server_search_global_total
gauge
count
The total number of user and internal cursors in the shard, either holding pending results or actively executing `FT.CURSOR READ`. Deprecated in 8.0 (split into redis_server_search_cursors_internal_active and redis_server_search_cursors_user_active), but still available in older versions. [1](#tnote-1)

redis_server_search_number_of_indexes
gauge
count
Total number of indexes in the shard [1](#tnote-1)

redis_server_search_number_of_active_indexes
gauge
count
The total number of indexes running a background indexing and/or background query processing operation. Background indexing refers to vector ingestion process, or in-progress background indexer. [1](#tnote-1)

redis_server_search_total_num_docs_in_indexes
gauge
count
The total number of documents currently indexed across all indexes in the shard. [2](#tnote-2)

redis_server_search_number_of_active_indexes_running_queries
gauge
count
Total count of indexes currently running a background query process. [1](#tnote-1)

redis_server_search_number_of_active_indexes_indexing
gauge
count
Total count of indexes currently undergoing a background indexing process. Background indexing refers to vector ingestion process, or in-progress background indexer. This metric is limited by the number of WORKER threads allocated for writing operations + the number of indexes. [1](#tnote-1)

redis_server_search_total_active_write_threads
gauge
count
Total count of background write (indexing) processes currently running in the shard. Background indexing refers to vector ingestion process, or in-progress background indexer. This metric is limited by the number of threads allocated for writing operations. [1](#tnote-1)

redis_server_search_fields_text_Text
gauge
count
The total number of `TEXT` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_text_Sortable
gauge
count
The total number of `SORTABLE TEXT` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_text_NoIndex
gauge
count
The total number of `NOINDEX TEXT` fields across all indexes in the shard; i.e., used for sorting only but not indexed. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_text_IndexErrors
gauge
count
The total number of indexing failures caused by attempts to index a document containing a `TEXT` field. This field appears only if `TEXT` fields exist. [1](#tnote-1)

redis_server_search_fields_numeric_Numeric
gauge
count
The total number of `NUMERIC` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_numeric_Sortable
gauge
count
The total number of `SORTABLE NUMERIC` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_numeric_NoIndex
gauge
count
The total number of `NOINDEX NUMERIC` fields across all indexes in the shard, which are used for sorting only but not indexed. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_numeric_IndexErrors
gauge
count
The total number of indexing failures caused by attempts to index a document containing a `NUMERIC` field. This field appears only if `NUMERIC` fields exist. [1](#tnote-1)

redis_server_search_fields_tag_Tag
gauge
count
The total number of `TAG` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_tag_Sortable
gauge
count
The total number of `SORTABLE TAG` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_tag_NoIndex
gauge
count
The total number of `NOINDEX TAG` fields across all indexes in the shard; i.e., used for sorting only but not indexed. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_tag_CaseSensitive
gauge
count
The total number of `CASESENSITIVE TAG` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_tag_IndexErrors
gauge
count
The total number of indexing failures caused by attempts to index a document containing a `TAG` field. This field appears only if `TAG` fields exist. [1](#tnote-1)

redis_server_search_fields_geo_Geo
gauge
count
The total number of `GEO` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_geo_Sortable
gauge
count
The total number of `SORTABLE GEO` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_geo_NoIndex
gauge
count
The total number of `NOINDEX GEO` fields across all indexes in the shard; i.e., used for sorting only but not indexed. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_geo_IndexErrors
gauge
count
The total number of indexing failures caused by attempts to index a document containing a `GEO` field. This field appears only if `GEO` fields exist. [1](#tnote-1)

redis_server_search_fields_vector_Vector
gauge
count
The total number of `VECTOR` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_vector_Flat
gauge
count
The total number of `FLAT VECTOR` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_vector_HNSW
gauge
count
The total number of `HNSW VECTOR` fields across all indexes in the shard. This field appears only if its value is larger than 0. [1](#tnote-1)

redis_server_search_fields_vector_SVS_VAMANA
gauge
count
The total number of `SVS-VAMANA VECTOR` fields across all indexes in the shard. This field appears only if its value is larger than 0. [4](#tnote-4)

redis_server_search_fields_vector_SVS_VAMANA_Compressed
gauge
count
The total number of `SVS-VAMANA VECTOR` fields with `COMPRESSION` enabled (e.g., `LVQ8`) across all indexes in the shard. This field appears only if its value is larger than 0. [4](#tnote-4)

redis_server_search_fields_vector_IndexErrors
gauge
count
The total number of indexing failures caused by attempts to index a document containing a `VECTOR` field. This field appears only if `VECTOR` fields exist. [1](#tnote-1)

redis_server_search_fields_geoshape_Geoshape
gauge
count
The total number of `GEOSHAPE` fields across all indexes in the shard. This field appears only if its value is larger than 0. [2](#tnote-2)

redis_server_search_fields_geoshape_Sortable
gauge
count
The total number of `SORTABLE GEOSHAPE` fields across all indexes in the shard. This field appears only if its value is larger than 0. [2](#tnote-2)

redis_server_search_fields_geoshape_NoIndex
gauge
count
The total number of `NOINDEX GEOSHAPE` fields across all indexes in the shard; i.e., used for sorting only but not indexed. This field appears only if its value is larger than 0. [2](#tnote-2)

redis_server_search_fields_geoshape_IndexErrors
gauge
count
The total number of indexing failures caused by attempts to index a document containing a `GEOSHAPE` field. This field appears only if `GEOSHAPE` fields exist. [2](#tnote-2)

redis_server_search_total_indexing_ops_text_fields
gauge
count
The total number of indexing operations performed on `TEXT` fields across all indexes in the shard. [2](#tnote-2)

redis_server_search_total_indexing_ops_tag_fields
gauge
count
The total number of indexing operations performed on `TAG` fields across all indexes in the shard. [2](#tnote-2)

redis_server_search_total_indexing_ops_numeric_fields
gauge
count
The total number of indexing operations performed on `NUMERIC` fields across all indexes in the shard. [2](#tnote-2)

redis_server_search_total_indexing_ops_geo_fields
gauge
count
The total number of indexing operations performed on `GEO` fields across all indexes in the shard. [2](#tnote-2)

redis_server_search_total_indexing_ops_geoshape_fields
gauge
count
The total number of indexing operations performed on `GEOSHAPE` fields across all indexes in the shard. [2](#tnote-2)

redis_server_search_total_indexing_ops_vector_fields
gauge
count
The total number of indexing operations performed on `VECTOR` fields across all indexes in the shard. [2](#tnote-2)

redis_server_search_used_memory_indexes
gauge
bytes
The total memory allocated by all indexes in the shard in bytes. [1](#tnote-1)

redis_server_search_smallest_memory_index
gauge
bytes
The memory usage of the index with the smallest memory usage in the shard in bytes. [1](#tnote-1)

redis_server_search_largest_memory_index
gauge
bytes
The memory usage of the index with the largest memory usage in the shard in bytes. [1](#tnote-1)

redis_server_search_total_indexing_time
gauge
milliseconds
The total time spent on indexing operations, excluding the background indexing of vectors in the `HNSW` graph. [1](#tnote-1)

redis_server_search_used_memory_vector_index
gauge
bytes
The total memory usage of all vector indexes in the shard. [1](#tnote-1)

redis_server_search_total_queries_processed
gauge
count
The total number of successful query executions (When using cursors, not counting reading from existing cursors) in the shard. [1](#tnote-1)

redis_server_search_total_query_commands
gauge
count
The total number of successful query command executions (including `FT.SEARCH`, `FT.AGGREGATE`, and `FT.CURSOR READ`). [1](#tnote-1)

redis_server_search_total_query_execution_time_ms
gauge
milliseconds
The cumulative execution time of all query commands, including `FT.SEARCH`, `FT.AGGREGATE`, and `FT.CURSOR READ`, measured in ms. [1](#tnote-1)

redis_server_search_total_active_queries
gauge
count
The total number of background queries currently being executed in the shard, excluding `FT.CURSOR READ`. [1](#tnote-1)

redis_server_search_total_coord_dispatch_time_ms
gauge
milliseconds
The cumulative time the coordinator spent building and dispatching query commands (`FT.SEARCH`, `FT.AGGREGATE`, and `FT.HYBRID`) to the shards, measured in ms. [4](#tnote-4)

redis_server_search_errors_indexing_failures
gauge
count
The total number of indexing failures recorded across all indexes in the shard. [1](#tnote-1)

redis_server_search_errors_for_index_with_max_failures
gauge
count
The number of indexing failures in the index with the highest count of failures. [1](#tnote-1)

redis_server_search_OOM_indexing_failures_indexes_count
gauge
count
The count of indexes that experienced out-of-memory (OOM) failures during background indexing. [2](#tnote-2)

redis_server_search_shard_total_query_errors_syntax
gauge
count
The total number of query syntax errors occurred in the shard. [2](#tnote-2)

redis_server_search_shard_total_query_errors_arguments
gauge
count
The total number of queries in the shard that failed due to missing or invalid arguments. [2](#tnote-2)

redis_server_search_shard_total_query_errors_timeout
gauge
count
The total number of query timeout errors occurred in the shard (when timeout policy is 'fail'). [2](#tnote-2)

redis_server_search_shard_total_query_warnings_timeout
gauge
count
The total number of query timeout warnings occurred in the shard (when timeout policy is 'return partial results'). [2](#tnote-2)

redis_server_search_shard_total_query_errors_oom
gauge
count
The total number of query out-of-memory errors occurred in the shard. [4](#tnote-4)

redis_server_search_shard_total_query_warnings_oom
gauge
count
The total number of query out-of-memory warnings occurred in the shard. [4](#tnote-4)

redis_server_search_shard_total_query_warnings_max_prefix_expansions
gauge
count
The total number of max prefix expansion warnings occurred in the shard. [2](#tnote-2)

redis_server_search_coord_total_query_errors_syntax
gauge
count
The total number of query syntax errors occurred at the coordinator. [2](#tnote-2)

redis_server_search_coord_total_query_errors_arguments
gauge
count
The total number of query argument errors encountered by the shard's coordinator. [2](#tnote-2)

redis_server_search_coord_total_query_errors_timeout
gauge
count
The total number of query timeout errors encountered by the shard's coordinator (when timeout policy is 'fail'). [2](#tnote-2)

redis_server_search_coord_total_query_warnings_timeout
gauge
count
The total number of query timeout warnings encountered by the shard's coordinator (when timeout policy is 'return partial results'). [2](#tnote-2)

redis_server_search_coord_total_query_errors_oom
gauge
count
The total number of query out-of-memory errors encountered by the shard's coordinator. [4](#tnote-4)

redis_server_search_coord_total_query_warnings_oom
gauge
count
The total number of query out-of-memory warnings encountered by the shard's coordinator. [4](#tnote-4)

redis_server_search_coord_total_query_warnings_max_prefix_expansions
gauge
count
The total number of max prefix expansion warnings encountered by the shard's coordinator. [2](#tnote-2)

redis_server_search_uv_threads_running_queries
gauge
count
The number of I/O threads currently handling query distribution to shards in cluster environments. [2](#tnote-2)

redis_server_search_uv_threads_running_topology_update
gauge
count
The number of UV threads currently running topology updates. [3](#tnote-3)

redis_server_search_active_worker_threads
gauge
count
The number of active worker threads. [2](#tnote-2)

redis_server_search_active_coord_threads
gauge
count
The number of active coordinator threads. [2](#tnote-2)

redis_server_search_workers_low_priority_pending_jobs
gauge
count
The number of pending low-priority jobs in worker threads, such as vector background indexing, graph updates, and vector garbage collection in graph-based indexes. [2](#tnote-2)

redis_server_search_workers_high_priority_pending_jobs
gauge
count
The number of pending high-priority jobs in worker threads, such as query execution. [2](#tnote-2)

redis_server_search_workers_admin_priority_pending_jobs
gauge
count
The number of pending admin-priority jobs in worker threads, such as threadpool configuration changes. [3](#tnote-3)

redis_server_search_coord_high_priority_pending_jobs
gauge
count
The number of pending jobs in the coordinator thread queue. Coordinator threads only have a high-priority queue and are primarily used for query distribution. [2](#tnote-2)

shard_ping_failures
gauge
count
Number of consecutive ping failures for a shard. Labels: shard_uid, role

shard_ping_failure_duration_seconds
gauge
seconds
Duration of ongoing failures (0 when healthy). Labels: shard_uid, role

 Available since RediSearch 2.6.
 Available since RediSearch 2.8.
 Available since RediSearch 2.10.
 Available since RediSearch 8.0.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page