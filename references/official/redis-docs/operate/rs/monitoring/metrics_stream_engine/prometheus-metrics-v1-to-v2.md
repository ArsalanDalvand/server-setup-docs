---
title: "Transition from Prometheus v1 to Prometheus v2"
source: "https://redis.io/docs/latest/operate/rs/monitoring/metrics_stream_engine/prometheus-metrics-v1-to-v2/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/monitoring/metrics_stream_engine/prometheus-metrics-v1-to-v2"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Transition from Prometheus v1 to Prometheus v2"
---
# Transition from Prometheus v1 to Prometheus v2

Transition from v1 metrics to v2 PromQL equivalents.

You can integrate Redis Software with Prometheus and tools such as [Grafana](/docs/latest/integrate/prometheus-with-redis-enterprise/), [Datadog](/docs/latest/integrate/datadog-with-redis-enterprise/), [Dynatrace](/docs/latest/integrate/dynatrace-with-redis-enterprise/), or [New Relic](/docs/latest/integrate/new-relic-with-redis-enterprise/) to create dashboards for important metrics.

As of Redis Software version 7.8.2, [PromQL (Prometheus Query Language)](https://prometheus.io/docs/prometheus/latest/querying/basics/) metrics are available. V1 metrics are deprecated but still available.

To transition from v1 metrics to v2 metrics, you need to change the `metrics_path` in your Prometheus configuration file from `/` to `/v2` to use the new scraping endpoint.

Here's an example of the updated scraping configuration in `prometheus.yml`:

```yaml
scrape_configs:
  # Scrape Redis Software
  - job_name: redis-enterprise
    scrape_interval: 30s
    scrape_timeout: 30s
    metrics_path: /v2
    scheme: https
    tls_config:
      insecure_skip_verify: true
    static_configs:
      - targets: ["<cluster_name>:8070"]
```

You can scrape both v1 and v2 endpoints simultaneously during the transition period to prepare dashboards and ensure a smooth transition.

You can use the following tables to transition from v1 metrics to equivalent v2 PromQL. For a list of all available v2 metrics, see [[operate/rs/monitoring/metrics_stream_engine/prometheus-metrics-v2|Prometheus metrics v2]].

## Database metrics

V1Â metric
Equivalent V2 PromQL
Description

bdb_avg_latency
`(sum by (db)(irate(endpoint_read_requests_latency_histogram_sum{db=""$db""}[$__rate_interval]) + irate(endpoint_write_requests_latency_histogram_sum{db=""$db""}[$__rate_interval]) + irate(endpoint_other_requests_latency_histogram_sum{db=""$db""}[$__rate_interval])))/(sum by (db)(irate(endpoint_read_requests{db=""$db""}[$__rate_interval]) + irate(endpoint_write_requests{db=""$db""}[$__rate_interval]) + irate(endpoint_other_requests{db=""$db""}[$__rate_interval])))/1000000`
Average latency of operations on the database (seconds); returned only when there is traffic

bdb_avg_latency_max
`avg(histogram_quantile(1, sum by (le, db) (irate(endpoint_read_requests_latency_histogram_bucket{db=""$db""}[$__rate_interval]) + irate(endpoint_write_requests_latency_histogram_bucket{db=""$db""}[$__rate_interval]) + irate(endpoint_other_requests_latency_histogram_bucket{db=""$db""}[$__rate_interval])))) / 1000000`
Highest value of average latency of operations on the database (seconds); returned only when there is traffic

bdb_avg_read_latency
`(sum(irate(endpoint_read_requests_latency_histogram_sum{db=""$db""}[$__rate_interval]))/sum(irate(endpoint_read_requests{db=""$db""}[$__rate_interval])))/1000000`
Average latency of read operations (seconds); returned only when there is traffic

bdb_avg_read_latency_max
`histogram_quantile(1, sum by (le) (irate(endpoint_read_requests_latency_histogram_bucket{db=""$db""}[$__rate_interval]))) / 1000000`
Highest value of average latency of read operations (seconds); returned only when there is traffic

bdb_avg_write_latency
`(sum(irate(endpoint_write_requests_latency_histogram_sum{db=""$db""}[$__rate_interval]))/sum(irate(endpoint_write_requests{db=""$db""}[$__rate_interval])))/1000000`
Average latency of write operations (seconds); returned only when there is traffic

bdb_avg_write_latency_max
`histogram_quantile(1, sum by (le) (irate(endpoint_write_requests_latency_histogram_bucket{db=""$db""}[$__rate_interval]))) / 1000000`
Highest value of average latency of write operations (seconds); returned only when there is traffic

bdb_bigstore_shard_count
`sum((sum(label_replace(label_replace(namedprocess_namegroup_thread_count{groupname=~"redis-\d+", threadname=~"(speedb|rocksdb).*"}, "redis", "$1", "groupname", "redis-(\d+)"), "driver", "$1", "threadname", "(speedb|rocksdb).*")) by (redis, driver) > bool 0) * on (redis) group_left(db) redis_server_up) by (db, driver)`
Shard count by database and by storage engine (driver - rocksdb / speedb); Only for databases with Auto Tiering enabled

bdb_conns
`sum(endpoint_client_connections{cluster="$cluster", db="$db"} - endpoint_client_disconnections{cluster="$cluster", db="$db"} - endpoint_proxy_disconnections{cluster="$cluster", db="$db"})`
Number of client connections to database

bdb_egress_bytes
`sum by(db) (irate(endpoint_egress{db="$db"}[$__rate_interval]))`
Rate of outgoing network traffic from the database (bytes/sec)

bdb_egress_bytes_max
`max_over_time (sum by(db) (irate(endpoint_egress{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of outgoing network traffic from the database (bytes/sec)

bdb_evicted_objects
`sum by (db) (irate(redis_server_evicted_keys{role="master"}[$__rate_interval]))`
Rate of key evictions from database (evictions/sec)

bdb_evicted_objects_max
`sum by (db) (irate(redis_server_evicted_keys{role="master"}[$__rate_interval]))`
Highest value of the rate of key evictions from database (evictions/sec)

bdb_expired_objects
`sum by (db) (irate(redis_server_expired_keys{role="master"}[$__rate_interval]))`
Rate keys expired in database (expirations/sec)

bdb_expired_objects_max
`sum by (db) (irate(redis_server_expired_keys{role="master"}[$__rate_interval]))`
Highest value of the rate keys expired in database (expirations/sec)

bdb_fork_cpu_system
`sum by (db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="system"}[$__rate_interval]))`
% cores utilization in system mode for all Redis shard fork child processes of this database

bdb_fork_cpu_system_max
`sum by (db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="system"}[$__rate_interval]))`
Highest value of % cores utilization in system mode for all Redis shard fork child processes of this database

bdb_fork_cpu_user
`sum by (db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="user"}[$__rate_interval]))`
% cores utilization in user mode for all Redis shard fork child processes of this database

bdb_fork_cpu_user_max
`sum by (db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="user"}[$__rate_interval]))`
Highest value of % cores utilization in user mode for all Redis shard fork child processes of this database

bdb_ingress_bytes
`sum by(db) (irate(endpoint_ingress{db="$db"}[$__rate_interval]))`
Rate of incoming network traffic to database (bytes/sec)

bdb_ingress_bytes_max
`max_over_time (sum by(db) (irate(endpoint_ingress{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of incoming network traffic to database (bytes/sec)

bdb_instantaneous_ops_per_sec
`sum by(db) (redis_server_instantaneous_ops_per_sec)`
Request rate handled by all shards of database (ops/sec)

bdb_main_thread_cpu_system
`sum by(db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="system", threadname=~"redis-server.*"}[$__rate_interval]))`
% cores utilization in system mode for all Redis shard main threads of this database

bdb_main_thread_cpu_system_max
`sum by(db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="system", threadname=~"redis-server.*"}[$__rate_interval]))`
Highest value of % cores utilization in system mode for all Redis shard main threads of this database

bdb_main_thread_cpu_user
`sum by(irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="user", threadname=~"redis-server.*"}[$__rate_interval]))`
% cores utilization in user mode for all Redis shard main threads of this database

bdb_main_thread_cpu_user_max
`sum by(irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="user", threadname=~"redis-server.*"}[$__rate_interval]))`
Highest value of % cores utilization in user mode for all Redis shard main threads of this database

bdb_mem_frag_ratio
`avg(redis_server_mem_fragmentation_ratio)`
RAM fragmentation ratio (RSS / allocated RAM)

bdb_mem_size_lua
`sum by(db) (redis_server_used_memory_lua)`
Redis lua scripting heap size (bytes)

bdb_memory_limit
`db_memory_limit_bytes`
Configured RAM limit for the database

bdb_monitor_sessions_count
`sum by(db) (endpoint_monitor_sessions_count)`
Number of clients connected in monitor mode to the database

bdb_no_of_keys
`sum by (db) (redis_server_db_keys{role="master"})`
Number of keys in database

bdb_other_req
`sum by(db) (irate(endpoint_other_requests{db="$db"}[$__rate_interval]))`
Rate of other (non read/write) requests on the database (ops/sec)

bdb_other_req_max
`max_over_time (sum by(db) (irate(endpoint_other_requests{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of other (non read/write) requests on the database (ops/sec)

bdb_other_res
`sum by(db) (irate(endpoint_other_responses{db="$db"}[$__rate_interval]))`
Rate of other (non read/write) responses on the database (ops/sec)

bdb_other_res_max
`max_over_time (sum by(db) (irate(endpoint_other_responses{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of other (non read/write) responses on the database (ops/sec)

bdb_pubsub_channels
`sum by(db) (redis_server_pubsub_channels)`
Count the pub/sub channels with subscribed clients

bdb_pubsub_channels_max
`sum by(db) (redis_server_pubsub_channels)`
Highest value of count the pub/sub channels with subscribed clients

bdb_pubsub_patterns
`sum by(db) (redis_server_pubsub_patterns)`
Count the pub/sub patterns with subscribed clients

bdb_pubsub_patterns_max
`sum by(db) (redis_server_pubsub_patterns)`
Highest value of count the pub/sub patterns with subscribed clients

bdb_read_hits
`sum by (db) (irate(redis_server_keyspace_read_hits{role="master"}[$__rate_interval]))`
Rate of read operations accessing an existing key (ops/sec)

bdb_read_hits_max
`sum by (db) (irate(redis_server_keyspace_read_hits{role="master"}[$__rate_interval]))`
Highest value of the rate of read operations accessing an existing key (ops/sec)

bdb_read_misses
`sum by (db) (irate(redis_server_keyspace_read_misses{role="master"}[$__rate_interval]))`
Rate of read operations accessing a non-existing key (ops/sec)

bdb_read_misses_max
`sum by (db) (irate(redis_server_keyspace_read_misses{role="master"}[$__rate_interval]))`
Highest value of the rate of read operations accessing a non-existing key (ops/sec)

bdb_read_req
`sum by (db) (irate(endpoint_read_requests{db="$db"}[$__rate_interval]))`
Rate of read requests on the database (ops/sec)

bdb_read_req_max
`max_over_time (sum by(db) (irate(endpoint_read_requests{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of read requests on the database (ops/sec)

bdb_read_res
`sum by (db) (irate(endpoint_read_responses{"$db"}[$__rate_interval]))`
Rate of read responses on the database (ops/sec)

bdb_read_res_max
`max_over_time (sum by(db) (irate(endpoint_read_responses{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of read responses on the database (ops/sec)

bdb_shard_cpu_system
`sum by(db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="system", role="master"}[$__rate_interval]))`
% cores utilization in system mode for all Redis shard processes of this database

bdb_shard_cpu_system_max
`sum by(db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="system", role="master"}[$__rate_interval]))`
Highest value of % cores utilization in system mode for all Redis shard processes of this database

bdb_shard_cpu_user
`sum by(db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="user", role="master"}[$__rate_interval]))`
% cores utilization in user mode for the Redis shard process

bdb_shard_cpu_user_max
`sum by(db) (irate(namedprocess_namegroup_thread_cpu_seconds_total{mode="user", role="master"}[$__rate_interval]))`
Highest value of % cores utilization in user mode for the Redis shard process

bdb_shards_used
`sum((sum(label_replace(label_replace(label_replace(namedprocess_namegroup_thread_count{groupname=~"redis-\d+"}, "redis", "$1", "groupname", "redis-(\d+)"), "shard_type", "flash", "threadname", "(bigstore).*"), "shard_type", "ram", "shard_type", "")) by (redis, shard_type) > bool 0) * on (redis) group_left(db) redis_server_up) by (db, shard_type)`
Used shard count by database and by shard type (ram / flash)

bdb_total_connections_received
`sum by(db) (irate(endpoint_client_connections{db="$db"}[$__rate_interval]))`
Rate of new client connections to database (connections/sec)

bdb_total_connections_received_max
`max_over_time (sum by(db) (irate(endpoint_client_connections{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of new client connections to database (connections/sec)

bdb_total_req
`sum by (db) (irate(endpoint_read_requests{db=""$db""}[$__rate_interval]) + irate(endpoint_write_requests{db=""$db""}[$__rate_interval]) + irate(endpoint_other_requests{db=""$db""}[$__rate_interval]))`
Rate of all requests on the database (ops/sec)

bdb_total_req_max
`max_over_time(sum by (db) (irate(endpoint_read_requests{db=""$db""}[$__rate_interval]) + irate(endpoint_write_requests{db=""$db""}[$__rate_interval]) + irate(endpoint_other_requests{db=""$db""}[$__rate_interval])) [$__range:])`
Highest value of the rate of all requests on the database (ops/sec)

bdb_total_res
`sum by (db) (irate(endpoint_read_responses{db=""$db""}[$__rate_interval]) + irate(endpoint_write_responses{db=""$db""}[$__rate_interval]) + irate(endpoint_other_responses{db=""$db""}[$__rate_interval]))`
Rate of all responses on the database (ops/sec)

bdb_total_res_max
`max_over_time(sum by (db) (irate(endpoint_read_responses{db=""$db""}[$__rate_interval]) + irate(endpoint_write_responses{db=""$db""}[$__rate_interval]) + irate(endpoint_other_responses{db=""$db""}[$__rate_interval])) [$__range:])`
Highest value of the rate of all responses on the database (ops/sec)

bdb_up
`min by(db) (redis_server_up)`
Database is up and running

bdb_used_memory
`sum by (db) (redis_server_used_memory)`
Memory used by database (in BigRedis this includes flash) (bytes)

bdb_write_hits
`sum by (db) (irate(redis_server_keyspace_write_hits{role="master"}[$__rate_interval]))`
Rate of write operations accessing an existing key (ops/sec)

bdb_write_hits_max
`sum by (db) (irate(redis_server_keyspace_write_hits{role="master"}[$__rate_interval]))`
Highest value of the rate of write operations accessing an existing key (ops/sec)

bdb_write_misses
`sum by (db) (irate(redis_server_keyspace_write_misses{role="master"}[$__rate_interval]))`
Rate of write operations accessing a non-existing key (ops/sec)

bdb_write_misses_max
`sum by (db) (irate(redis_server_keyspace_write_misses{role="master"}[$__rate_interval]))`
Highest value of the rate of write operations accessing a non-existing key (ops/sec)

bdb_write_req
`sum by (db) (irate(endpoint_write_requests{db="$db"}[$__rate_interval]))`
Rate of write requests on the database (ops/sec)

bdb_write_req_max
`max_over_time(sum by (db) (irate(endpoint_write_requests{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of write requests on the database (ops/sec)

bdb_write_res
`sum by(db) (irate(endpoint_write_responses{db="$db"}[$__rate_interval]))`
Rate of write responses on the database (ops/sec)

bdb_write_res_max
`max_over_time(sum by (db) (irate(endpoint_write_responses{db="$db"}[$__rate_interval]))[$__range:])`
Highest value of the rate of write responses on the database (ops/sec)

no_of_expires
`sum by(db) (redis_server_db_expires{role="master"})`
Current number of volatile keys in the database

## Node metrics

V1Â metric
Equivalent V2 PromQL
Description

node_available_flash
`node_available_flash_bytes`
Available flash in the node (bytes)

node_available_flash_no_overbooking
`node_available_flash_no_overbooking_bytes`
Available flash in the node (bytes), without taking into account overbooking

node_available_memory
`node_available_memory_bytes`
Amount of free memory in the node (bytes) that is available for database provisioning

node_available_memory_no_overbooking
`node_available_memory_no_overbooking_bytes`
Available RAM in the node (bytes) without taking into account overbooking

node_avg_latency
`sum by (proxy) (irate(endpoint_acc_latency[$__rate_interval])) / sum by (proxy) (irate(endpoint_total_started_res[$__rate_interval]))`
Average latency of requests handled by endpoints on the node in milliseconds; returned only when there is traffic

node_bigstore_free
`node_bigstore_free_bytes`
Sum of free space of back-end flash (used by flash database's [BigRedis]) on all cluster nodes (bytes); returned only when BigRedis is enabled

node_bigstore_iops
`node_flash_reads_total + node_flash_writes_total`
Rate of I/O operations against back-end flash for all shards which are part of a flash-based database (BigRedis) in the cluster (ops/sec); returned only when BigRedis is enabled

node_bigstore_kv_ops
`sum by (node) (irate(redis_server_big_io_dels[$__rate_interval]) + irate(redis_server_big_io_reads[$__rate_interval]) + irate(redis_server_big_io_writes[$__rate_interval]))`
Rate of value read/write operations against back-end flash for all shards which are part of a flash-based database (BigRedis) in the cluster (ops/sec); returned only when BigRedis is enabled

node_bigstore_throughput
`sum by (node) (irate(redis_server_big_io_read_bytes[$__rate_interval]) + irate(redis_server_big_io_write_bytes[$__rate_interval]))`
Throughput I/O operations against back-end flash for all shards which are part of a flash-based database (BigRedis) in the cluster (bytes/sec); returned only when BigRedis is enabled

node_cert_expiration_seconds
`node_cert_expires_in_seconds`
Certificate expiration (in seconds) per given node; read more about [certificates in Redis Software](/docs/latest/operate/rs/security/certificates/) and [monitoring certificates](/docs/latest/operate/rs/security/certificates/monitor-certificates/)

node_conns
`sum by (node) (endpoint_client_connections)`
Number of clients connected to endpoints on node

node_cpu_idle
`avg by (node) (irate(node_cpu_seconds_total{mode="idle"}[$__rate_interval]))`
CPU idle time portion (0-1, multiply by 100 to get percent)

node_cpu_idle_max
N/A
Highest value of CPU idle time portion (0-1, multiply by 100 to get percent)

node_cpu_idle_median
N/A
Average value of CPU idle time portion (0-1, multiply by 100 to get percent)

node_cpu_idle_min
N/A
Lowest value of CPU idle time portion (0-1, multiply by 100 to get percent)

node_cpu_system
`avg by (node) (irate(node_cpu_seconds_total{mode="system"}[$__rate_interval]))`
CPU time portion spent in the kernel (0-1, multiply by 100 to get percent)

node_cpu_system_max
N/A
Highest value of CPU time portion spent in the kernel (0-1, multiply by 100 to get percent)

node_cpu_system_median
N/A
Average value of CPU time portion spent in the kernel (0-1, multiply by 100 to get percent)

node_cpu_system_min
N/A
Lowest value of CPU time portion spent in the kernel (0-1, multiply by 100 to get percent)

node_cpu_user
`avg by (node) (irate(node_cpu_seconds_total{mode="user"}[$__rate_interval]))`
CPU time portion spent by user-space processes (0-1, multiply by 100 to get percent)

node_cpu_user_max
N/A
Highest value of CPU time portion spent by user-space processes (0-1, multiply by 100 to get percent)

node_cpu_user_median
N/A
Average value of CPU time portion spent by user-space processes (0-1, multiply by 100 to get percent)

node_cpu_user_min
N/A
Lowest value of CPU time portion spent by user-space processes (0-1, multiply by 100 to get percent)

node_cur_aof_rewrites
`sum by (cluster, node) (redis_server_aof_rewrite_in_progress)`
Number of AOF rewrites that are currently performed by shards on this node

node_egress_bytes
`irate(node_network_transmit_bytes_total{device="<interface>"}[$__rate_interval])`
Rate of outgoing network traffic to node (bytes/sec)

node_egress_bytes_max
N/A
Highest value of the rate of outgoing network traffic to node (bytes/sec)

node_egress_bytes_median
N/A
Average value of the rate of outgoing network traffic to node (bytes/sec)

node_egress_bytes_min
N/A
Lowest value of the rate of outgoing network traffic to node (bytes/sec)

node_ephemeral_storage_avail
`node_ephemeral_storage_avail_bytes`
Disk space available to RLEC processes on configured ephemeral disk (bytes)

node_ephemeral_storage_free
`node_ephemeral_storage_free_bytes`
Free disk space on configured ephemeral disk (bytes)

node_free_memory
`node_memory_MemFree_bytes`
Free memory in the node (bytes)

node_ingress_bytes
`irate(node_network_receive_bytes_total{device="<interface>"}[$__rate_interval])`
Rate of incoming network traffic to node (bytes/sec)

node_ingress_bytes_max
N/A
Highest value of the rate of incoming network traffic to node (bytes/sec)

node_ingress_bytes_median
N/A
Average value of the rate of incoming network traffic to node (bytes/sec)

node_ingress_bytes_min
N/A
Lowest value of the rate of incoming network traffic to node (bytes/sec)

node_persistent_storage_avail
`node_persistent_storage_avail_bytes`
Disk space available to RLEC processes on configured persistent disk (bytes)

node_persistent_storage_free
`node_persistent_storage_free_bytes`
Free disk space on configured persistent disk (bytes)

node_provisional_flash
`node_provisional_flash_bytes`
Amount of flash available for new shards on this node, taking into account overbooking, max Redis servers, reserved flash, and provision and migration thresholds (bytes)

node_provisional_flash_no_overbooking
`node_provisional_flash_no_overbooking_bytes`
Amount of flash available for new shards on this node, without taking into account overbooking, max Redis servers, reserved flash, and provision and migration thresholds (bytes)

node_provisional_memory
`node_provisional_memory_bytes`
Amount of RAM that is available for provisioning to databases out of the total RAM allocated for databases

node_provisional_memory_no_overbooking
`node_provisional_memory_no_overbooking_bytes`
Amount of RAM that is available for provisioning to databases out of the total RAM allocated for databases, without taking into account overbooking

node_total_req
`sum by (cluster, node) (irate(endpoint_read_requests[$__rate_interval]) +  irate(endpoint_write_requests[$__rate_interval]) +  irate(endpoint_other_requests[$__rate_interval]) )`
Request rate handled by endpoints on node (ops/sec)

node_up
`node_metrics_up`
Node is part of the cluster and is connected

## Cluster metrics

V1Â metric
Equivalent V2 PromQL
Description

cluster_shards_limit
`license_shards_limit`
Total shard limit by the license by shard type (ram / flash)

## Proxy metrics

V1Â metric
Equivalent V2 PromQL
Description

listener_acc_latency
N/A
Accumulative latency (sum of the latencies) of all types of commands on the database. For the average latency, divide this value by listener_total_res

listener_acc_latency_max
N/A
Highest value of accumulative latency of all types of commands on the database

listener_acc_other_latency
N/A
Accumulative latency (sum of the latencies) of commands that are a type "other" on the database. For the average latency, divide this value by listener_other_res

listener_acc_other_latency_max
N/A
Highest value of accumulative latency of commands that are a type "other" on the database

listener_acc_read_latency
N/A
Accumulative latency (sum of the latencies) of commands that are a type "read" on the database. For the average latency, divide this value by listener_read_res

listener_acc_read_latency_max
N/A
Highest value of accumulative latency of commands that are a type "read" on the database

listener_acc_write_latency
N/A
Accumulative latency (sum of the latencies) of commands that are a type "write" on the database. For the average latency, divide this value by listener_write_res

listener_acc_write_latency_max
N/A
Highest value of accumulative latency of commands that are a type "write" on the database

listener_auth_cmds
N/A
Number of memcached AUTH commands sent to the database

listener_auth_cmds_max
N/A
Highest value of the number of memcached AUTH commands sent to the database

listener_auth_errors
N/A
Number of error responses to memcached AUTH commands

listener_auth_errors_max
N/A
Highest value of the number of error responses to memcached AUTH commands

listener_cmd_flush
N/A
Number of memcached FLUSH_ALL commands sent to the database

listener_cmd_flush_max
N/A
Highest value of the number of memcached FLUSH_ALL commands sent to the database

listener_cmd_get
N/A
Number of memcached GET commands sent to the database

listener_cmd_get_max
N/A
Highest value of the number of memcached GET commands sent to the database

listener_cmd_set
N/A
Number of memcached SET commands sent to the database

listener_cmd_set_max
N/A
Highest value of the number of memcached SET commands sent to the database

listener_cmd_touch
N/A
Number of memcached TOUCH commands sent to the database

listener_cmd_touch_max
N/A
Highest value of the number of memcached TOUCH commands sent to the database

listener_conns
N/A
Number of clients connected to the endpoint

listener_egress_bytes
N/A
Rate of outgoing network traffic to the endpoint (bytes/sec)

listener_egress_bytes_max
N/A
Highest value of the rate of outgoing network traffic to the endpoint (bytes/sec)

listener_ingress_bytes
N/A
Rate of incoming network traffic to the endpoint (bytes/sec)

listener_ingress_bytes_max
N/A
Highest value of the rate of incoming network traffic to the endpoint (bytes/sec)

listener_last_req_time
N/A
Time of last command sent to the database

listener_last_res_time
N/A
Time of last response sent from the database

listener_max_connections_exceeded
`irate(endpoint_maximal_connections_exceeded[$__rate_interval])`
Number of times the number of clients connected to the database at the same time has exceeded the max limit

listener_max_connections_exceeded_max
N/A
Highest value of the number of times the number of clients connected to the database at the same time has exceeded the max limit

listener_monitor_sessions_count
N/A
Number of clients connected in monitor mode to the endpoint

listener_other_req
N/A
Rate of other (non-read/write) requests on the endpoint (ops/sec)

listener_other_req_max
N/A
Highest value of the rate of other (non-read/write) requests on the endpoint (ops/sec)

listener_other_res
N/A
Rate of other (non-read/write) responses on the endpoint (ops/sec)

listener_other_res_max
N/A
Highest value of the rate of other (non-read/write) responses on the endpoint (ops/sec)

listener_other_started_res
N/A
Number of responses sent from the database of type "other"

listener_other_started_res_max
N/A
Highest value of the number of responses sent from the database of type "other"

listener_read_req
`irate(endpoint_read_requests[$__rate_interval])`
Rate of read requests on the endpoint (ops/sec)

listener_read_req_max
N/A
Highest value of the rate of read requests on the endpoint (ops/sec)

listener_read_res
`irate(endpoint_read_responses[$__rate_interval])`
Rate of read responses on the endpoint (ops/sec)

listener_read_res_max
N/A
Highest value of the rate of read responses on the endpoint (ops/sec)

listener_read_started_res
N/A
Number of responses sent from the database of type "read"

listener_read_started_res_max
N/A
Highest value of the number of responses sent from the database of type "read"

listener_total_connections_received
`irate(endpoint_total_connections_received[$__rate_interval])`
Rate of new client connections to the endpoint (connections/sec)

listener_total_connections_received_max
N/A
Highest value of the rate of new client connections to the endpoint (connections/sec)

listener_total_req
N/A
Request rate handled by the endpoint (ops/sec)

listener_total_req_max
N/A
Highest value of the rate of all requests on the endpoint (ops/sec)

listener_total_res
N/A
Rate of all responses on the endpoint (ops/sec)

listener_total_res_max
N/A
Highest value of the rate of all responses on the endpoint (ops/sec)

listener_total_started_res
N/A
Number of responses sent from the database of all types

listener_total_started_res_max
N/A
Highest value of the number of responses sent from the database of all types

listener_write_req
`irate(endpoint_write_requests[$__rate_interval])`
Rate of write requests on the endpoint (ops/sec)

listener_write_req_max
N/A
Highest value of the rate of write requests on the endpoint (ops/sec)

listener_write_res
`irate(endpoint_write_responses[$__rate_interval])`
Rate of write responses on the endpoint (ops/sec)

listener_write_res_max
N/A
Highest value of the rate of write responses on the endpoint (ops/sec)

listener_write_started_res
N/A
Number of responses sent from the database of type "write"

listener_write_started_res_max
N/A
Highest value of the number of responses sent from the database of type "write"

## Replication metrics

V1Â metric
Equivalent V2 PromQL
Description

bdb_replicaof_syncer_ingress_bytes
`rate(replica_src_ingress_bytes[$__rate_interval])`
Rate of compressed incoming network traffic to a Replica Of database (bytes/sec)

bdb_replicaof_syncer_ingress_bytes_decompressed
`rate(replica_src_ingress_bytes_decompressed[$__rate_interval])`
Rate of decompressed incoming network traffic to a Replica Of database (bytes/sec)

bdb_replicaof_syncer_local_ingress_lag_time
`database_syncer_lag_ms{syncer_type="replicaof"}`
Lag time between the source and the destination for Replica Of traffic (ms)

bdb_replicaof_syncer_status
`database_syncer_current_status{syncer_type="replicaof"}`
Syncer status for Replica Of traffic; 0 = in-sync, 1 = syncing, 2 = out of sync

bdb_crdt_syncer_ingress_bytes
`rate(crdt_src_ingress_bytes[$__rate_interval])`
Rate of compressed incoming network traffic to CRDB (bytes/sec)

bdb_crdt_syncer_ingress_bytes_decompressed
`rate(crdt_src_ingress_bytes_decompressed[$__rate_interval])`
Rate of decompressed incoming network traffic to CRDB (bytes/sec)

bdb_crdt_syncer_local_ingress_lag_time
`database_syncer_lag_ms{syncer_type="crdt"}`
Lag time between the source and the destination (ms) for CRDB traffic

bdb_crdt_syncer_status
`database_syncer_current_status{syncer_type="crdt"}`
Syncer status for CRDB traffic; 0 = in-sync, 1 = syncing, 2 = out of sync

## Shard metrics

V1Â metric
Equivalent V2 PromQL
Description

redis_active_defrag_running
`redis_server_active_defrag_running`
Automatic memory defragmentation current aggressiveness (% cpu)

redis_allocator_active
`redis_server_allocator_active`
Total used memory, including external fragmentation

redis_allocator_allocated
`redis_server_allocator_allocated`
Total allocated memory

redis_allocator_resident
`redis_server_allocator_resident`
Total resident memory (RSS)

redis_aof_last_cow_size
`redis_server_aof_last_cow_size`
Last AOFR, CopyOnWrite memory

redis_aof_rewrite_in_progress
`redis_server_aof_rewrite_in_progress`
The number of simultaneous AOF rewrites that are in progress

redis_aof_rewrites
`redis_server_aof_rewrites`
Number of AOF rewrites this process executed

redis_aof_delayed_fsync
`redis_server_aof_delayed_fsync`
Number of times an AOF fsync caused delays in the main Redis thread (inducing latency); this can indicate that the disk is slow or overloaded

redis_blocked_clients
`redis_server_blocked_clients`
Count the clients waiting on a blocking call

redis_connected_clients
`redis_server_connected_clients`
Number of client connections to the specific shard

redis_connected_slaves
`redis_server_connected_slaves`
Number of connected replicas

redis_db0_avg_ttl
`redis_server_db0_avg_ttl`
Average TTL of all volatile keys

redis_db0_expires
`redis_server_expired_keys`
Total count of volatile keys

redis_db0_keys
`redis_server_db0_keys`
Total key count

redis_evicted_keys
`redis_server_evicted_keys`
Keys evicted so far (since restart)

redis_expire_cycle_cpu_milliseconds
`redis_server_expire_cycle_cpu_milliseconds`
The cumulative amount of time spent on active expiry cycles

redis_expired_keys
`redis_server_expired_keys`
Keys expired so far (since restart)

redis_forwarding_state
`redis_server_forwarding_state`
Shard forwarding state (on or off)

redis_keys_trimmed
`redis_server_keys_trimmed`
The number of keys that were trimmed in the current or last resharding process

redis_keyspace_read_hits
`redis_server_keyspace_read_hits`
Number of read operations accessing an existing keyspace

redis_keyspace_read_misses
`redis_server_keyspace_read_misses`
Number of read operations accessing a non-existing keyspace

redis_keyspace_write_hits
`redis_server_keyspace_write_hits`
Number of write operations accessing an existing keyspace

redis_keyspace_write_misses
`redis_server_keyspace_write_misses`
Number of write operations accessing a non-existing keyspace

redis_master_link_status
`redis_server_master_link_status`
Indicates if the replica is connected to its master

redis_master_repl_offset
`redis_server_master_repl_offset`
Number of bytes sent to replicas by the shard; calculate the throughput for a time period by comparing the value at different times

redis_master_sync_in_progress
`redis_server_master_sync_in_progress`
The master shard is synchronizing (1 true; 0 false)

redis_max_process_mem
`redis_server_max_process_mem`
Current memory limit configured by redis_mgr according to node free memory

redis_maxmemory
`redis_server_maxmemory`
Current memory limit configured by redis_mgr according to database memory limits. 

To calculate the percent memory usage:
`sum by (cluster,db)(redis_server_used_memory{role="master"}) / (avg by(cluster,db)(db_memory_limit_bytes) / max by(cluster,db)(db_replication_factor))`

redis_mem_aof_buffer
`redis_server_mem_aof_buffer`
Current size of AOF buffer

redis_mem_clients_normal
`redis_server_mem_clients_normal`
Current memory used for input and output buffers of non-replica clients

redis_mem_clients_slaves
`redis_server_mem_clients_slaves`
Current memory used for input and output buffers of replica clients

redis_mem_fragmentation_ratio
`redis_server_mem_fragmentation_ratio`
Memory fragmentation ratio (1.3 means 30% overhead)

redis_mem_not_counted_for_evict
`redis_server_mem_not_counted_for_evict`
Portion of used_memory (in bytes) that's not counted for eviction and OOM error

redis_mem_replication_backlog
`redis_server_mem_replication_backlog`
Size of replication backlog

redis_module_fork_in_progress
`redis_server_module_fork_in_progress`
A binary value that indicates if there is an active fork spawned by a module (1) or not (0)

redis_process_cpu_system_seconds_total
`namedprocess_namegroup_cpu_seconds_total{mode="system"}`
Shard process system CPU time spent in seconds

redis_process_cpu_usage_percent
`namedprocess_namegroup_cpu_seconds_total{mode=~"system|user"}`
Shard process CPU usage percentage

redis_process_cpu_user_seconds_total
`namedprocess_namegroup_cpu_seconds_total{mode="user"}`
Shard user CPU time spent in seconds

redis_process_main_thread_cpu_system_seconds_total
`namedprocess_namegroup_thread_cpu_seconds_total{mode="system",threadname="redis-server"}`
Shard main thread system CPU time spent in seconds

redis_process_main_thread_cpu_user_seconds_total
`namedprocess_namegroup_thread_cpu_seconds_total{mode="user",threadname="redis-server"}`
Shard main thread user CPU time spent in seconds

redis_process_max_fds
`max(namedprocess_namegroup_open_filedesc)`
Shard maximum number of open file descriptors

redis_process_open_fds
`namedprocess_namegroup_open_filedesc`
Shard number of open file descriptors

redis_process_resident_memory_bytes
`namedprocess_namegroup_memory_bytes{memtype="resident"}`
Shard resident memory size in bytes

redis_process_start_time_seconds
`namedprocess_namegroup_oldest_start_time_seconds`
Shard start time of the process since unix epoch in seconds

redis_process_virtual_memory_bytes
`namedprocess_namegroup_memory_bytes{memtype="virtual"}`
Shard virtual memory in bytes

redis_rdb_bgsave_in_progress
`redis_server_rdb_bgsave_in_progress`
Indication if bgsave is currently in progress

redis_rdb_last_cow_size
`redis_server_rdb_last_cow_size`
Last bgsave (or SYNC fork) used CopyOnWrite memory

redis_rdb_saves
`redis_server_rdb_saves`
Total count of bgsaves since the process was restarted (including replica fullsync and persistence)

redis_repl_touch_bytes
`redis_server_repl_touch_bytes`
Number of bytes sent to replicas as TOUCH commands by the shard as a result of a READ command that was processed; calculate the throughput for a time period by comparing the value at different times

redis_total_commands_processed
`redis_server_total_commands_processed`
Number of commands processed by the shard; calculate the number of commands for a time period by comparing the value at different times

redis_total_connections_received
`redis_server_total_connections_received`
Number of connections received by the shard; calculate the number of connections for a time period by comparing the value at different times

redis_total_net_input_bytes
`redis_server_total_net_input_bytes`
Number of bytes received by the shard; calculate the throughput for a time period by comparing the value at different times

redis_total_net_output_bytes
`redis_server_total_net_output_bytes`
Number of bytes sent by the shard; calculate the throughput for a time period by comparing the value at different times

redis_up
`redis_server_up`
Shard is up and running

redis_used_memory
`redis_server_used_memory`
Memory used by shard (in BigRedis this includes flash) (bytes)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page