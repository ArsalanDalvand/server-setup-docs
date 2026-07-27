---
title: "MinIO Console — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/minio-console.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/minio-console"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Console — SILO (MinIO Object Storage)"
---
# MinIO Console

The MinIO Console is a rich graphical user interface that provides similar functionality to the [[reference/minio-mc#command-mc|`mc`]] command line tool.

[](../_images/minio-console1.png)

This page provides an overview of the MinIO Console and describes configuration options and instructions for logging in.

## Overview

You can use the MinIO Console for administration tasks like Identity and Access Management, Metrics and Log Monitoring, or Server Configuration.

The MinIO Console is embedded as part of the MinIO Server.
You can also deploy a standalone MinIO Console using the instructions in the [github repository](https://github.com/minio/console).

### Supported Browsers

MinIO Console runs on a variety of current, stable release browsers.

For the best experience in the MinIO Console, use the latest stable release of your preferred browser.
Some browsers that are supported include:

Chrome

Edge

Safari

Firefox

Opera

This list is not exhaustive and is subject to change.

For a full list of browsers and versions for running MinIO Console, see the [Browserslist](https://browsersl.ist/#q=%3E0.2%25%2Cnot+dead+and+not+op_mini+all) website.

Tip

MinIO Console does not support Opera Mini.

## Configuration

The MinIO Console inherits the majority of its configuration settings from the
MinIO Server. The following environment variables enable specific behavior in
the MinIO Console:

Environment Variable

Description

[[reference/minio-server/settings/console#envvar.MINIO_PROMETHEUS_URL|`MINIO_PROMETHEUS_URL`]]

The URL for a Prometheus server configured to scrape metrics from the
MinIO deployment. The MinIO Console uses this server for populating the
metrics dashboard.

See [[operations/monitoring/collect-minio-metrics-using-prometheus#minio-metrics-collect-using-prometheus|Monitoring and Alerting using Prometheus]] for a tutorial on
configuring Prometheus to collect metrics from MinIO.

[[reference/minio-server/settings/console#envvar.MINIO_BROWSER_REDIRECT_URL|`MINIO_BROWSER_REDIRECT_URL`]]

The externally resolvable hostname for the MinIO Console used by the
configured [[administration/identity-access-management#minio-authentication-and-identity-management|external identity manager]] for returning the
authentication response.

This variable is typically necessary when using a reverse proxy,
load balancer, or similar system to expose the MinIO Console to the
public internet. Specify an externally reachable hostname that resolves
to the MinIO Console.

### Static vs Dynamic Port Assignment

MinIO by default selects a random port for the MinIO Console on each server
startup. Browser clients accessing the MinIO Server are automatically
redirected to the MinIO Console on its dynamically selected port.
This behavior emulates the legacy web browser behavior while reducing the
the risk of a port collision on systems which were running MinIO before the
embedded Console update.

You can select an explicit static port by passing the
[[reference/minio-server/minio-server#minio.server.-console-address|`minio server --console-address`]] commandline option when starting
each MinIO Server in the deployment.

For example, the following command starts a distributed MinIO deployment using
a static port assignment of `9001` for the MinIO Console. This deployment
would respond to S3 API operations on the default MinIO server port `:9000`
and browser access on the MinIO Console port `:9001`.

```
minio server https://minio-{1...4}.example.net/mnt/drive-{1...4} \
      --console-address ":9001"
```

Deployments behind network routing components which require static ports for
routing rules may require setting a static MinIO Console port. For example,
load balancers, reverse proxies, or Kubernetes ingress may by default block
or exhibit unexpected behavior with the the dynamic redirection behavior.

You must also ensure that the host system firewall grants access to the configured Console port.

## Logging In

Changed in version RELEASE.2023-03-09T23-16-13Z.

The MinIO Console displays a login screen for unauthenticated users.
The Console defaults to providing a username and password prompt for a [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO-managed user]].

For deployments configured with multiple [[administration/identity-access-management#minio-authentication-and-identity-management|identity managers]], select the Other Authentication Methods dropdown to select one of the other configured identity providers.
You can also log in using credentials generated using a [[developers/security-token-service#minio-security-token-service|Security Token Service (STS)]] API.

Try out the Console using MinIO’s Play testing environment

You can explore the Console using [https://play.min.io:9443](https://play.min.io:9443).
Log in with the following credentials:

Username: `Q3AM3UQ867SPQQA43P2F`

Password: `zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG`

The Play Console connects to the MinIO Play deployment at [https://play.min.io](https://play.min.io).
You can also access this deployment using [[reference/minio-mc#command-mc|`mc`]] and using the `play` alias.

## Documentation

The Documentation tab opens this documentation site in a separate browser window or tab.

## Available Tasks

Once logged in to the MinIO Console, users can perform many kinds of tasks.

[[administration/console/managing-objects#minio-console-managing-objects|Manage objects]] by browsing or uploading objects, managing bucket settings, or creating tiers.

[[administration/console/security-and-access#minio-console-security-access|Review or modify identity and security]] with access keys, policies, and Identity Provider settings.

[[administration/console/managing-deployment#minio-console-managing-deployment|Monitor the health and activities]] with metrics, notifications, or site replication

[[administration/console/subnet-registration#minio-console-subscription|Manage your deployment’s license]]

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).