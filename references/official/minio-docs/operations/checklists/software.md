---
title: "Software Checklist — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/checklists/software.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/checklists/software"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Software Checklist — SILO (MinIO Object Storage)"
---
# Software Checklist

Use the following checklist when planning the software configuration for a production, distributed MinIO deployment.

## MinIO Pre-requisites

Servers running a Linux operating system with a 6.6+ kernel. Red Hat Enterprise Linux (RHEL) 10 or Ubuntu LTS 22.04.01+ ship with these Kernel’s by default.
Ensure the chosen OS uses LTS and in-support releases of a 6.6+ Linux kernel.

A method to synchronize time servers across nodes, such as with `ntp`, `timedatectl` or `timesyncd`.
The method to use varies by operating system.
Check with your operating system’s documentation for how to synchronize time with a time server.

Disable system services that index, scan, or audit the filesystem, system-level calls, or kernel-level calls.
These services can reduce performance due to resource contention or interception of MinIO operations.

MinIO strongly recommends uninstalling or disabling the following services on hosts running MinIO:

`mlocate` or `plocate`

`updatedb`

`auditd`

Crowdstrike Falcon

Antivirus software (`clamav`)

The above list represents the most common services or softwares known to cause performance or behavioral issues with high performance systems like MinIO.
Consider removing or disabling any other service or software which functions similarly to those listed above on MinIO hosts.

Alternatively, configure these services to ignore or exclude the MinIO Server process and all drives or drive paths accessed by MinIO.

System administrator access to the remote servers

A management tool for distributed systems, such as Ansible, Terraform, or Kubernetes for orchestrated environments.
Kubernetes infrastructures should use the MinIO Operator for best results.

Load balancer to handle routing of requests (for example, [NGINX](https://www.nginx.com/))

[[operations/monitoring/collect-minio-metrics-using-prometheus#minio-metrics-collect-using-prometheus|Prometheus]] or a Prometheus-compatible setup for monitoring and metrics

[[operations/monitoring/grafana#minio-grafana|Grafana configured]] for dashboards

(optional) [[reference/minio-mc#command-mc|`mc`]] installed on the local host system

## MinIO Install

Install a matching version of MinIO across all nodes in the deployment.

## Post Install Tasks

(optional) Create an [[reference/minio-mc/mc-alias#command-mc.alias|`mc alias`]] for each server with [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] from your local machine for command line access to work with the MinIO deployment from a local machine

Configure [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|Bucket replication]] to duplicate contents of a bucket to another bucket location

Configure [[operations/replication/multi-site-replication#minio-site-replication-overview|Site replication]] to synchronize contents of multiple dispersed data center locations

Configure [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|Object retention rules with lifecycle management]] to manage when objects should expire

Configure [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|Object storage level rules with tiering]] to move objects between hot, warm, and cold storage and maximize storage cost efficiencies

Exclusive access to drives

MinIO requires exclusive access to the drives or volumes provided for object storage.
No other processes, software, scripts, or persons should perform any actions directly on the drives or volumes provided to MinIO or the objects or files MinIO places on them.

Unless directed by MinIO Engineering, do not use scripts or tools to directly modify, delete, or move any of the data shards, parity shards, or metadata files on the provided drives, including from one drive or node to another.
Such operations are very likely to result in widespread corruption and data loss beyond MinIO’s ability to heal.

## 3rd Party Identity Provider Tasks

Authenticate to MinIO with [[developers/security-token-service#minio-security-token-service|Security Token Service (STS)]]
Enabling this requires MinIO support.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).