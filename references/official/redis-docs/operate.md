---
title: "Redis products"
source: "https://redis.io/docs/latest/operate/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate"
category: "redis"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis products"
---
# Redis products

Products, services, and tools to operate a Redis database.

 Redis Cloud
 Redis Software

 [[operate/rc/rc-quickstart|Get started with Redis Cloud]]  [Create a database](/docs/latest/operate/rc/databases/create-database/)  [Connect to your database](/docs/latest/operate/rc/databases/connect/)  [[operate/rc/subscriptions|Subscriptions]] [[operate/rc/api|REST API]]
 [[operate/rs/installing-upgrading|Install Redis Software]]  [[operate/rs/clusters/new-cluster-setup|Set up a new cluster]]  [Create a database](/docs/latest/operate/rs/databases/create/)  [Connect to your database](/docs/latest/operate/rs/databases/connect/) [REST API](/docs/latest/operate/rs/references/rest-api/)

 Redis Open Source
 Redis for Kubernetes

 [[operate/oss_and_stack/install/install-stack|Install Redis 8 in Redis Open Source]]  [[operate/oss_and_stack/install/archive/install-stack|Install Redis Stack]] (â¤ 7.4)  [[operate/oss_and_stack/management|Manage Redis]] 
 [Deploy Redis for Kubernetes](/docs/latest/operate/kubernetes/deployment/)  [Architecture](/docs/latest/operate/kubernetes/architecture/)  [API Reference](/docs/latest/operate/kubernetes/reference/) 

 Redis Insight
 Redis Data Integration (RDI)

 [Install Redis Insight](/docs/latest/operate/redisinsight/install/)  [Use Redis Insight](/docs/latest/develop/tools/insight/)  [Download Redis Insight](https://redis.io/downloads/#insight) 
 [[integrate/redis-data-integration|RDI overview]]  [[integrate/redis-data-integration/installation|Install RDI]]   [RDI pipelines](/docs/latest/integrate/redis-data-integration/data-pipelines/)  

## Product features

### High availability and durability

 Redis Cloud
 Redis Software
 Redis Open Source
 Redis for Kubernetes

Clustering
[Clustering](/docs/latest/operate/rc/databases/configuration/clustering/)
[Clustering](/docs/latest/operate/rs/databases/durability-ha/clustering/)
[[operate/oss_and_stack/management/scaling|Scale with Redis Cluster]]
[Redis Enterprise clusters (REC)](/docs/latest/operate/kubernetes/re-clusters/)

Replication
[Replication](/docs/latest/operate/rc/databases/configuration/high-availability/)
[Replication](/docs/latest/operate/rs/databases/durability-ha/replication/)
[[operate/oss_and_stack/management/replication|Replication]]
[Create replica databases](/docs/latest/operate/kubernetes/re-databases/replica-redb/)

Active-Active geo-distribution
[Active-Active Redis](/docs/latest/operate/rc/databases/active-active/)
[Active-Active Redis](/docs/latest/operate/rs/databases/active-active/)

[Active-Active databases](/docs/latest/operate/kubernetes/active-active/)

Rolling upgrades
[Upgrade database version](/docs/latest/operate/rc/databases/version-management/upgrade-version/)
[[operate/rs/installing-upgrading/upgrading|Upgrade Redis Software]]

[Upgrade Redis for K8s](/docs/latest/operate/kubernetes/upgrade/)

Redis Flex/Auto tiering
[Create a Redis Flex database](/docs/latest/operate/rc/databases/create-database/create-flex-database/)
[Redis Flex](/docs/latest/operate/rs/databases/flash/)

[Redis Flex](/docs/latest/operate/kubernetes/flex/)

Persistence
[Data persistence](/docs/latest/operate/rc/databases/configuration/data-persistence/)
[Persistence](/docs/latest/operate/rs/databases/configure/database-persistence/)
[[operate/oss_and_stack/management/replication|Persistence]]
[Persistence volumes](/docs/latest/operate/kubernetes/recommendations/persistent-volumes/)

Recovery
Automatic
[[operate/rs/clusters/cluster-recovery|Recover cluster]]
[[operate/oss_and_stack/management/scaling#manual-failover|Manual failover]]
[Cluster recovery](/docs/latest/operate/kubernetes/re-clusters/cluster-recovery/)

Backups
[Back up a database](/docs/latest/operate/rc/databases/back-up-data/)
[Schedule backups](/docs/latest/operate/rs/databases/import-export/schedule-backups/)
[[operate/oss_and_stack/management/replication|Persistence]]
[REDB spec.backup](/docs/latest/operate/kubernetes/reference/api/redis_enterprise_database_api/#specbackup)

### Logging and monitoring

 Redis Cloud
 Redis Software
 Redis Open Source
 Redis for Kubernetes

Monitoring
[Monitor performance](/docs/latest/operate/rc/databases/monitor-performance/)
[[operate/rs/monitoring|Monitoring]]
[INFO](/docs/latest/commands/info/), [MONITOR](/docs/latest/commands/monitor/), and [LATENCY DOCTOR](/docs/latest/commands/latency-doctor/)
[Analysis with Redis Insight](/docs/latest/develop/tools/insight/#database-analysis)
[Export metrics to Prometheus](/docs/latest/operate/kubernetes/re-clusters/connect-prometheus-operator/)

Logging
[[operate/rc/logs-reports/system-logs|System logs]]
[[operate/rs/clusters/logging|Logging]]
`/var/log/redis/redis.log`
[SLOWLOG](/docs/latest/commands/slowlog/)
[[develop/pubsub/keyspace-notifications|Keyspace notifications]]
[Logs](/docs/latest/operate/kubernetes/logs/)

Alerts
[Alerts](/docs/latest/operate/rc/databases/monitor-performance/#configure-metric-alerts)
[[operate/rs/clusters/logging/alerts-events|Alerts and events]]
[[operate/oss_and_stack/management/sentinel#pubsub-messages|Pub/sub with Redis Sentinel]]
[REDB alertSettings](/docs/latest/operate/kubernetes/reference/api/redis_enterprise_database_api/#specalertsettings)

Support
[Contact support](https://redis.io/support/)
[[operate/rs/installing-upgrading/creating-support-package|Create support package]]

[Contact support](https://redis.io/support/)

### Security

 Redis Cloud
 Redis Software
 Redis Open Source
 Redis for Kubernetes

Transport Layer Security (TLS)
[[operate/rc/security/database-security/tls-ssl|TLS]]
[[operate/rs/security/encryption/tls|TLS]]
[[operate/oss_and_stack/management/security/encryption|TLS]]
[REDB tlsMode](/docs/latest/operate/kubernetes/reference/api/redis_enterprise_database_api/#spec)

Role-based access control (RBAC)
[[operate/rc/security/access-control/data-access-control/role-based-access-control|Role-based access control]]
[[operate/rs/security/access-control|Access control]]
[[operate/oss_and_stack/management/security/acl|Access control list]]
[REC credentials](/docs/latest/operate/kubernetes/security/authentication/manage-rec-credentials/)

Lightweight Directory Access Protocol (LDAP)

[[operate/rs/security/access-control/ldap|LDAP authentication]]

[Enable LDAP](/docs/latest/operate/kubernetes/security/authentication/ldap/)

Single sign-on (SSO)
[[operate/rc/security/access-control/saml-sso|SAML SSO]]

Self-signed certificates

[Certificates](/docs/latest/operate/rs/security/certificates/)
[[operate/oss_and_stack/management/security/encryption#certificate-configuration|Certificate configuration]]
[REC certificates](/docs/latest/operate/kubernetes/security/certificates/manage-rec-certificates/)

Internode encryption
[[operate/rc/security/encryption-at-rest|Encryption at rest]]
[[operate/rs/security/encryption/internode-encryption|Internode encryption]]

[Enable internode encryption](/docs/latest/operate/kubernetes/security/certificates/internode-encryption/)

Auditing

[[operate/rs/security/audit-events|Audit events]]
[[develop/pubsub/keyspace-notifications|Keyspace notifications]]

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page