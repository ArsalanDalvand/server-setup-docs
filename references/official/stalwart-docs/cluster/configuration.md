---
title: "Overview"
source: "https://stalw.art/docs/cluster/configuration"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/configuration"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

Running Stalwart in a clustered environment is flexible. The server was built from the start for distributed deployments, so horizontal scaling and high availability require little extra configuration.

Three elements must be configured to bring a Stalwart cluster online:

A unique [[cluster/configuration/node-id|node identifier]] per cluster member, so each instance can be individually addressed for coordination and logging.
A [[cluster/coordination|coordination mechanism]], allowing nodes to communicate, share internal state, and remain synchronised. Stalwart supports peer-to-peer, Apache Kafka, NATS, and Redis.
The [[cluster/configuration/roles|roles of each node]], defining which maintenance tasks and listeners each instance runs.

Each of these topics is covered in the following subsections, with examples and guidance, whether the deployment is a lightweight three-node setup or a large-scale distributed system.