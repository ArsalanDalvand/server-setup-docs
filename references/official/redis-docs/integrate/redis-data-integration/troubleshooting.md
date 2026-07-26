---
title: "Troubleshooting"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/troubleshooting/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/troubleshooting"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Troubleshooting"
---
# Troubleshooting

Solve and report simple problems with RDI

The following sections explain how you can get extra information from
Redis Data Integration (RDI) to help you solve problems that you may encounter. Redis support may
also ask you to provide this information to help you resolve issues.

## Debug information during installation

If the installer fails with an error, then try installing again with the
log level set to `DEBUG`:

```bash
./install.sh --log-level DEBUG
```

This gives you more detail about the installation steps and can often
help you to pinpoint the source of the error.

## RDI logs

By default, RDI records the following logs in the host VM file system at
`/opt/rdi/logs` (or whichever path you specified during installation);

Filename
Phase

`rdi_collector-collector-initializer.log`
Initializing the collector.

`rdi_collector-debezium-ssl-init.log`
Establishing the connector SSL connections to the source and RDI database (if you are using SSL).

`rdi_collector-collector-source.log`
Collector [[integrate/redis-data-integration/architecture|change data capture (CDC)]] operations.

`rdi_rdi-rdi-operator.log`
Main [[integrate/redis-data-integration/architecture#how-rdi-is-deployed|RDI control plane]] component.

`rdi_processor-processor.log`
RDI stream processing.

Logs are recorded at the minimum `INFO` level in a simple format that
log analysis tools can use.

Note:
  Often during the initial sync phase, the collector source log will contain a message
saying RDI is out of
memory. This is not an error but an informative message to say that RDI
is applying backpressure to the collector. See
[[integrate/redis-data-integration/architecture#backpressure-mechanism|Backpressure mechanism]]
in the Architecture guide for more information.

## Dump support package

If you need to send a comprehensive set of forensics data to Redis support,
run the
[[integrate/redis-data-integration/reference/cli/redis-di-dump-support-package|`redis-di dump-support-package`]]
command from the CLI.

This command gathers the following data:

All the internal RDI components and their status
All internal RDI configuration
List of secret names used by RDI components (but not the secrets themselves)
RDI logs
RDI component versions
Text of the `config.yaml` file
Text of the Job configuration files
Rejected records along with the reason for their rejection (should not exist in production)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page