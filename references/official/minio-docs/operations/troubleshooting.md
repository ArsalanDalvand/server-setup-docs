---
title: "Troubleshooting — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/troubleshooting.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/troubleshooting"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Troubleshooting — SILO (MinIO Object Storage)"
---
# Troubleshooting

## Overview

MinIO users have two options for support.

Community support from the [public Slack channel](https://slack.min.io).

Community support is best-effort only and has no SLA or SLO.

Paid subscribers have access to the MinIO Subscription Network, [SUBNET](https://min.io/pricing?jmp=docs), which provides access to health checks, direct-to-engineering support, and license management.

For current licensing levels and pricing, refer to the [MinIO SUBNET](https://min.io/pricing?jmp=docs) page.

## Tools

The [[reference/minio-mc#minio-client|MinIO Client]] provides several functions to display information about your MinIO deployment or monitor its activity.

For basic information about your MinIO deployment, use [[reference/minio-mc-admin/mc-admin-info#command-mc.admin.info|`mc admin info`]].

For deeper investigation of S3 calls and responses, use [[reference/minio-mc-admin/mc-admin-trace#command-mc.admin.trace|`mc admin trace`]].

Use [[reference/minio-mc-admin/mc-admin-logs#command-mc.admin.logs|`mc admin logs`]] to display logs from the command line.
The command supports type and quantity filters for further limiting logs output.

## Upgrades and version support

MinIO regularly releases updates to introduce features, improve performance, address security concerns, or fix bugs.
These releases can occur very frequently, and vary by product.

Always test software releases in a development environment before upgrading on a production deployment.

### Recommended upgrade schedule

MinIO recommends always installing the most recent release to obtain security enhancements and improvements.
We recognize that such a frequent release schedule may make this impractical for some organizations.
In such cases, we recommend using MinIO and our related product releases that are no older than six months.

### Version Alignment

As the various MinIO products release separately on their own schedules, we recommend the following version alignment practices:

MinIO

Update to the latest release or a release no older than six months.

MinIO Client

Update to the mc release that occurs immediately after the MinIO release, within one or two weeks.

MinIO Operator

Use a MinIO version no earlier than the latest at the time of the Operator release.
The MinIO version latest at time of release can be found in the quay.io link in the example tenant kustomization yaml file for the Operator release.

4.5.5: MinIO RELEASE.2022-12-07T00-56-37Z or later

4.5.6: MinIO RELEASE.2023-01-02T09-40-09Z or later

4.5.7: MinIO RELEASE.2023-01-12T02-06-16Z or later

4.5.8: MinIO RELEASE.2023-01-12T02-06-16Z or later

When creating a new tenant, the Operator uses either the latest available MinIO release image or the image you specify when creating the tenant.

[[operations/deployments/k8s-upgrade-minio-operator-kubernetes#minio-k8s-upgrade-minio-operator|Upgrading the Operator]] does not automatically upgrade existing tenants.
[[operations/deployments/k8s-upgrade-minio-tenant-on-kubernetes#minio-k8s-upgrade-minio-tenant|Upgrade existing tenant]] MinIO versions separately.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).