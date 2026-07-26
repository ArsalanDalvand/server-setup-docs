---
title: "Cluster settings"
source: "https://redis.io/docs/latest/operate/rs/clusters/configure/cluster-settings/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/configure/cluster-settings"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Cluster settings"
---
# Cluster settings

You can view and set various cluster settings such as cluster name, email service, time zone, and license.

            Redis Software
          

You can view and set various cluster settings, such as cluster name, email service, time zone, and license, on the Cluster > Configuration page.

## General configuration tab

### Upload cluster license key

After purchasing a cluster license and if your account has the "Admin" role,
you can upload the cluster license key, either during initial
cluster creation or at any time afterward. The license key defines various
cluster settings, such as the maximum number of shards you can have in
the cluster. For more detailed information seeÂ [[operate/rs/clusters/configure/license-keys|Cluster license
keys]].

### View max number of allowed shards

The maximum number of allowed shards, which is determined by the cluster license
key, appears in the Max number of shards field in the License section.

### View cluster name

The cluster name appears in the Cluster name field in the License section. This gives a
common name that your team or Redis support can refer to. It is
especially helpful if you have multiple clusters.

### Set time zone

You can change the Time zone field to ensure the date, time fields, and log entries in the Cluster Manager UI are shown in your preferred time zone. This setting doesn't affect other system logs or services.

## Alert settings tab

The Alert Settings tab lets you configure alerts that are relevant to the entire cluster, such as alerts for cluster utilization, nodes, node utilization, security, and database utilization.

You can also configure email server settings and [[operate/rs/monitoring/v1_monitoring#send-alerts-by-email|send alerts by email]] to relevant users.

### Configure email server settings

To enable email alerts:

Enter your email
server details in the Email server settings section.

Select a connection security method:

TLS/SSL

STARTTLS

None

Send a test email to verify your email server settings.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page