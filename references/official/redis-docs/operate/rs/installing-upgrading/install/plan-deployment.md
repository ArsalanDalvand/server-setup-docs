---
title: "Plan Redis Software deployment"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/plan-deployment/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install/plan-deployment"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Plan Redis Software deployment"
---
# Plan Redis Software deployment

Plan a deployment of Redis Software.

            Redis Software
          

Before installing Redis Software, you need to:

Set up your hardware. See [[operate/rs/installing-upgrading/install/plan-deployment/hardware-requirements|Hardware requirements]] and [[operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage|Persistent and ephemeral node storage]] for more information.

Choose your [[operate/rs/installing-upgrading/install/plan-deployment/supported-platforms|deployment platform]].

Redis Software supports a variety of platforms, including:

Multiple Linux distributions (Ubuntu, Red Hat Enterprise Linux (RHEL), IBM CentOS, Oracle Linux)
[Amazon AWS AMI](/docs/latest/operate/rs/installing-upgrading/install/plan-deployment/configuring-aws-instances/)
[[operate/rs/installing-upgrading/quickstarts/docker-quickstart|Docker container]] (for development and testing only)
[Kubernetes](/docs/latest/operate/kubernetes/)

For more details, see [[operate/rs/installing-upgrading/install/plan-deployment/supported-platforms|Supported platforms]].

Open appropriate [[operate/rs/networking/port-configurations|network ports]] in the firewall to allow connections to the nodes.

Configure [[operate/rs/networking/cluster-dns|cluster DNS]] so that cluster nodes can reach each other by DNS names.

By default, the installation process requires an internet connection to install dependencies and synchronize the operating system clock. To learn more, see [[operate/rs/installing-upgrading/install/offline-installation|Offline installation]].

[Configure different mount points for data and log directories](/docs/latest/operate/rs/installing-upgrading/install/customize-install-directories/#config-diff-data-log-dirs).

## Next steps

After you finish planning your deployment, you can:

[[operate/rs/installing-upgrading/install/prepare-install/download-install-package|Download an installation package]].

[[operate/rs/installing-upgrading/install/prepare-install|Prepare to install]] Redis Software.

[[operate/rs/installing-upgrading/install/manage-installation-questions|View installation questions]] and prepare answers before installation.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page