---
title: "Preparation"
source: "https://stalw.art/docs/migration/guide/preparation"
source_format: official-html-to-markdown
language: "en"
doc_slug: "migration/guide/preparation"
category: "migration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Preparation"
---
# Preparation

      

The migration relies on four components running alongside the existing deployment: the new Stalwart server, the Stalwart command-line interface, the migration proxy and Vandelay. This stage installs all four. None of them touches the live server, so the entire stage is performed without downtime and without any change visible to existing clients.

The guide assumes an existing, running self-hosted mail server, referred to throughout as the source: either a Stalwart deployment of version 0.15 or below, or a Dovecot and Postfix stack such as Mail-in-a-Box or mailcow. The goal is to move its accounts onto a fresh installation of the latest Stalwart release, referred to as the new deployment.

## Planning the addresses

[Section titled “Planning the addresses”](#planning-the-addresses)

The proxy will eventually take over the public ports that the source currently answers on, so the source and the new deployment need to be reachable at addresses of their own that the proxy can forward to. A common arrangement gives each an internal hostname, for example `source.internal.example.org` and `new.internal.example.org`, while the public hostname that clients already use, such as `mail.example.org`, is repointed to the proxy at the cutover. Deciding on these addresses now makes the configuration that follows straightforward. No public DNS change is made yet; the public hostname continues to resolve to the source until the cutover.

## Installing the new Stalwart deployment

[Section titled “Installing the new Stalwart deployment”](#installing-the-new-stalwart-deployment)

The new deployment is installed as a normal, fresh Stalwart server following the standard [[install/platform|installation instructions]] for the target platform. It is installed on its own host or address rather than over the existing server, because both deployments must run at the same time. The initial setup creates an administrator account, which the later steps use to create domains and accounts and to configure split delivery.

The configuration model, the administration interface and the storage layout changed substantially after version 0.15. Becoming familiar with the latest release before relying on it in production is strongly recommended. The [[jozve/server-setup-docs/references/official/foundationdb-docs/configuration|configuration]] and [[management|management]] sections describe the current model, and the next step of this guide assumes a working understanding of how the new server is administered.

## Installing the command-line interface

[Section titled “Installing the command-line interface”](#installing-the-command-line-interface)

The Stalwart command-line interface administers the new deployment and applies the account-creation plans produced during the migration. It is installed by following its [[management/cli#installation|installation instructions]]. The interface is schema-driven: it reads the server’s schema on connection and derives its commands from it, so the version installed works against the new deployment without further configuration.

## Installing the migration proxy

[Section titled “Installing the migration proxy”](#installing-the-migration-proxy)

The migration proxy is installed on the host that will front the public ports, following the proxy [[migration/proxy/installation|installation instructions]]. Installing it does not start it with a working configuration yet; the proxy is configured in a later step and is only started at the cutover. Until then its presence has no effect on the running system.

## Installing Vandelay

[Section titled “Installing Vandelay”](#installing-vandelay)

Vandelay performs the per-account data transfer. It is installed on an administrative host that can reach both deployments, following its [[migration/import-export/installation|installation instructions]]. Vandelay is a single self-contained binary and stores each account it processes in a local archive file, so the host it runs on needs enough disk space to hold the largest mailbox being migrated at any one time.

## Rollback

[Section titled “Rollback”](#rollback)

Nothing installed in this stage alters the source or the path that clients take to it. Abandoning the migration here requires no rollback: the additional components can simply be left unused or removed, and the source continues to operate exactly as before.