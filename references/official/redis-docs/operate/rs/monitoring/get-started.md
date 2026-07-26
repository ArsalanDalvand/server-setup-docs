---
title: "Get started with monitoring Redis Software"
source: "https://redis.io/docs/latest/operate/rs/monitoring/get-started/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/monitoring/get-started"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Get started with monitoring Redis Software"
---
# Get started with monitoring Redis Software

Collect and visualize Redis Software metrics.

You can use Prometheus and compatible integrations to collect and visualize your Redis Software metrics.

Metrics are exposed at the cluster, node, database, shard, and proxy levels.

[Prometheus](https://prometheus.io/) is an open source systems monitoring and alerting toolkit that aggregates metrics from different sources.

You can use Prometheus integrations to:

Collect and display metrics not available in the Cluster Manager UI

Set up automatic alerts for all resources

Display Redis Software metrics alongside data from other systems

## Prometheus integrations

You can integrate Redis Software with Prometheus and one of the following tools to collect and visualize your deployment's metrics:

[Grafana](/docs/latest/integrate/prometheus-with-redis-enterprise/)

[Datadog](/docs/latest/integrate/datadog-with-redis-enterprise/)

[Dynatrace](/docs/latest/integrate/dynatrace-with-redis-enterprise/)

[New Relic](/docs/latest/integrate/new-relic-with-redis-enterprise/)

## Best practices for monitoring

Follow these best practices when monitoring your Redis Software cluster using the metrics stream engine.

### Monitor host-level metrics

For cluster health, resources, and node stability, monitor these metrics:

Group
Metric
Why monitor
Unit

CPU utilization
`node_cpu_user`,
`node_cpu_system`
Detect CPU saturation from Redis or the OS that results in higher latency and queueing.
Seconds (counter)

Memory (freeable)
`node_memory_MemTotal_bytes`,
`node_memory_MemFree_bytes`,
`node_memory_Buffers_bytes`,
`node_memory_Cached_bytes`
Detect memory pressure early. Low free memory or cache can precede swapping or out-of-memory errors.
Bytes (gauge)

Swap usage
`node_ephemeral_storage_free`
Monitor memory and disk pressure in your setup. Sustained pressure leads to latency spikes.
Bytes (gauge)

Network traffic
`node_ingress_bytes`,
`node_egress_bytes`
Ensure the network interface is not saturated. Protects replication and client responsiveness.
Bytes (counter)

Disk space
`node_filesystem_avail_bytes`,
`node_filesystem_size_bytes`
Prevent persistence and logging outages from low disk space.
Bytes (gauge)

Cluster state
`has_quorum{â¦}`
Monitor whether quorum is maintained (1) or lost (0).
Boolean

`node_metrics_up`
Monitor whether the node is connected and reporting to the cluster.
Gauge

Licensing
`license_shards_limit`
Track shard capacity limits by type (RAM or flash).
Count

Certificates
`node_cert_expires_in_seconds`
Avoid downtime from expired node certificates.
Seconds (gauge)

Services â CPU
`namedprocess_namegroup_cpu_seconds_total`
Identify abnormal CPU usage by platform services that can starve Redis, such as `alert_mgr`, `redis_mgr`, `dmc_proxy`.
Seconds (counter)

Services â memory
`namedprocess_namegroup_memory_bytes`
Detect memory leaks or outliers in platform services, such as `alert_mgr`, `redis_mgr`, `dmc_proxy`.
Bytes (gauge)

### Monitor database-level metrics

For database performance, availability, and efficiency, monitor the following metrics:

Group
Metric
Why monitor
Unit

Memory
`redis_server_used_memory`
Track actual data memory to prevent out-of-memory errors and evictions.
Bytes

Memory
`redis_server_allocator_allocated`
Monitor bytes allocated by allocator (includes internal fragmentation).
Bytes

Memory
`redis_server_allocator_active`
Monitor bytes in active pages (includes external fragmentation). Use delta/ratio versus allocated to infer defraggable memory.
Bytes

Memory
`redis_server_active_defrag_running`
Monitor if defragmentation is active and the intended CPU %. High values can affect performance.
% (gauge)

Latency
`endpoint_read_requests_latency_histogram`,
`endpoint_write_requests_latency_histogram`,
`endpoint_other_requests_latency_histogram`
Monitor server-side command latency.
Microseconds

High availability
`redis_server_master_repl_offset`
Compute replica throughput and lag using deltas over time.
Bytes (counter)

High availability
`redis_server_master_link_status`
Monitor replica link status (up or down) for early warning of high availability risk.
Status

Active-Active
`database_syncer_dst_lag`,
`database_syncer_lag_ms`
Detect cross-region synchronization delays that impact consistency and SLAs.
Milliseconds (gauge)

Active-Active
`database_syncer_state`
Monitor operational state for troubleshooting synchronization issues.
Gauge

Traffic â requests
`endpoint_read_requests`,
`endpoint_write_requests`,
`endpoint_other_requests`
Monitor workload mix and spikes that drive capacity and latency. Total equals the sum of all three.
Counter

Traffic â responses
`endpoint_read_responses`,
`endpoint_write_responses`,
`endpoint_other_responses`
Validate service responsiveness and symmetry with requests.
Counter

Traffic â bytes
`endpoint_ingress`,
`endpoint_egress`
Monitor size trends and watch for sudden growth that impacts egress costs or bandwidth.
Bytes (counter)

Egress queue
`endpoint_egress_pending`,
`endpoint_egress_pending_discarded`
Monitor back-pressure and drops that indicate network or client issues.
Bytes (counter)

Connections
`endpoint_client_connection`
Monitor accepted connections over time and match against client rollouts or spikes.
Counter

Connections
`endpoint_client_connection_expired`
Monitor connections closed due to TTL expiry, which can indicate idle policy or client issues.
Counter

Connections
`endpoint_longest_pipeline_histogram`
Monitor long pipelines that can amplify latency bursts and detect misbehaving clients.
Histogram (count)

Connections
`endpoint_client_connections`,
`endpoint_client_disconnections`,
`endpoint_proxy_disconnections`
Monitor connection churn and identify who closed the socket (client versus proxy). Current connections â connections â disconnections.
Counter

Cache efficiency
`redis_server_db_keys`,
`redis_server_db_avg_ttl`
Monitor key inventory and TTL coverage to inform eviction strategy.
Counter

Cache efficiency
`redis_server_evicted_keys`,
`redis_server_expired_keys`
Monitor eviction and expiry rates. Frequent evictions indicate memory pressure or poor sizing.
Counter

Cache efficiency
`cache_hits`,
`cache_hit_rate`
Monitor hit rate, which drives read latency and cost. Cache hit rate equals cache_hits/(cache_hits+cache_misses).
Count / Ratio (%)

Cache efficiency
`endpoint_client_tracking_on_requests`,
`endpoint_client_tracking_off_requests`,
`endpoint_disposed_commands_after_client_caching`
Track client-side caching usage and misuse.
Counter

Big / complex keys
`redis_server_<data_type>_<size_or_items>_<bucket>`
Monitor oversized keys and cardinality that cause fragmentation, slow replication, and CPU spikes. Track to prevent incidents. Examples:
`strings_sizes_over_512M`,
`zsets_items_over_8M`
Gauge

Security â clients
`endpoint_client_expiration_refresh`,
`endpoint_client_establishment_failures`
Monitor unstable clients or problems with authentication or setup.
Counter

Security â LDAP
`endpoint_successful_ldap_authentication`,
`endpoint_failed_ldap_authentication`,
`endpoint_disconnected_ldap_client`
Monitor authentication health and detect brute-force attacks or misconfigurations.
Counter

Security â cert-based
`endpoint_successful_cba_authentication`,
`endpoint_failed_cba_authentication`,
`endpoint_disconnected_cba_client`
Monitor certificate authentication status and failures.
Counter

Security â password
`endpoint_disconnected_user_password_client`
Monitor password-authentication client disconnects and correlate with policy changes.
Counter

Security â ACL
`redis_server_acl_access_denied_auth`,
`redis_server_acl_access_denied_cmd`,
`redis_server_acl_access_denied_key`,
`redis_server_acl_access_denied_channel`
Monitor unauthorized access attempts and incorrectly scoped ACLs.
Counter

Configuration
`db_config`
This is an information metric that holds database configuration within labels such as: db_name, db_version, db_port, tls_mode.
counter

## Prometheus quick start

To get started with Prometheus:

Create a directory called `prometheus` on your local machine.

Within that directory, create a configuration file called `prometheus.yml`.

Add the following contents to the configuration file and replace `<cluster_name>` with your Redis Software cluster's FQDN:

        v2 (metrics stream engine)
      

        v1
      

```yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

# Attach these labels to any time series or alerts when communicating with
# external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: "prometheus-stack-monitor"

# Load and evaluate rules in this file every 'evaluation_interval' seconds.
#rule_files:
# - "first.rules"
# - "second.rules"

scrape_configs:
# scrape Prometheus itself
  - job_name: prometheus
    scrape_interval: 10s
    scrape_timeout: 5s
    static_configs:
      - targets: ["localhost:9090"]

# scrape Redis Software
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

```yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

# Attach these labels to any time series or alerts when communicating with
# external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: "prometheus-stack-monitor"

# Load and evaluate rules in this file every 'evaluation_interval' seconds.
#rule_files:
# - "first.rules"
# - "second.rules"

scrape_configs:
# scrape Prometheus itself
  - job_name: prometheus
    scrape_interval: 10s
    scrape_timeout: 5s
    static_configs:
      - targets: ["localhost:9090"]

# scrape Redis Software
  - job_name: redis-enterprise
    scrape_interval: 30s
    scrape_timeout: 30s
    metrics_path: /
    scheme: https
    tls_config:
      insecure_skip_verify: true
    static_configs:
      - targets: ["<cluster_name>:8070"]
```

Set up your Prometheus server.

Note:
  We recommend running Prometheus in Docker only for development and testing.

To set up Prometheus on Docker:

Create a docker-compose.yml file:

```yml
version: '3'
services:
    prometheus-server:
        image: prom/prometheus
        ports:
            - 9090:9090
        volumes:
            - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
```

To start the containers, run:

```sh
$ docker compose up -d
```

To check that all of the containers are up, run:

```sh
docker ps
```

In your browser, sign in to Prometheus at `http://localhost:9090` to make sure the server is running.

Select Status and then Targets to check that Prometheus is collecting data from your Redis Software cluster.

[](/docs/latest/images/rs/prometheus-target.png)

If Prometheus is connected to the cluster, you can type node_up in the Expression field on the Prometheus home page to see the cluster metrics.

Integrate Redis Software and your Prometheus server with one of the [compatible tools](#prometheus-integrations). For help, see the integration guide and official documentation for your chosen tool.

Add dashboards for cluster, database, node, and shard metrics.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page