---
title: "Redis Feature Form"
source: "https://redis.io/docs/latest/operate/featureform/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/featureform"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Feature Form"
---
# Redis Feature Form

Deploy and operate Feature Form on Kubernetes.

            featureform
          

Feature Form is currently in preview and subject to change. Feature Form Docker images are available on Docker Hub; contact your Redis account team for a license key to deploy.

Deploy and operate Feature Form on Kubernetes: prepare your environment, install the Helm chart, configure authentication, and verify the platform.

This section is for platform engineers and admins. Building features in Feature Form rather than deploying it? See the [develop docs](/docs/latest/develop/ai/featureform/).

Start with [[operate/featureform/deploy|Deploy]] for the install steps, and [[operate/featureform/configure-auth|Configure authentication and RBAC]] for OIDC and roles.

The default install path is cloud-agnostic. For Terraform examples, contact your Redis account team.

## Deployment model

The Helm chart deploys:

One shared API server deployment that exposes both REST and gRPC.
Separate services for REST and gRPC.
An optional dashboard.
Optional addons: bundled Postgres state, bundled provider infrastructure (Postgres, Redis, MinIO, LocalStack), and observability (Prometheus, Grafana).

Authentication is required. Standard installs use [[operate/featureform/configure-auth|OIDC]].

## Decisions you'll make during install

State backend. External PostgreSQL (the durable default), the chart's bundled state Postgres, or memory state (CI and local only â not durable). See [[operate/featureform/deploy#2-choose-auth-and-state-values|Choose auth and state values]].
Exposure. Internal-only services, per-service ingresses, unified ingress, or direct `LoadBalancer` services. See [[operate/featureform/deploy#configure-external-access|Configure external access]].
Helm profile. Base chart, or one of four bundled profiles for memory state, provider stack, observability, or both. See [[operate/featureform/deploy#3-pick-the-base-chart-or-a-profile|Pick the base chart or a profile]].

If workspaces or resources seem to be missing after a restart, memory state is usually the cause â REST and gRPC see different in-memory data.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page