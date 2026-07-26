---
title: "Overview"
source: "https://stalw.art/docs/management/webui"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/webui"
category: "management"
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

      

The WebUI is the browser-based front end for managing a Stalwart server and for giving end users control over their own account. It is delivered as a single-page application packaged as an [[ref/object/application|Application]] (found in the WebUI under  Settings ›  Web Applications) and mounted at two URL prefixes on any HTTP listener the server exposes:

`/admin`, the administrator console used to configure the server, manage directories and policy, and investigate the running system.
`/account`, the Account Manager used by end users to manage their own credentials and settings.

Both surfaces are served by the same Application bundle; the prefix determines which entry point the browser lands on and which views are available after sign-in. Installation, download location, update cadence, unpack directory, and on-demand refreshes are not configured on the WebUI itself: the WebUI is a hosted application like any other, and those topics are covered under [[management/applications|Applications]].

## Administrator console

[Section titled “Administrator console”](#administrator-console)

The `/admin` mount is the administration surface. After signing in with an account that holds the required permissions, operators configure and operate the server from a single interface. The console covers the configuration objects that make up the running system and the operational views that report on its behaviour.

Configuration screens are organised around the same object model exposed over the JMAP API and the [[management/cli|CLI]]: every configurable object edited in the WebUI corresponds to a singleton or collection on the server, and every form submission is translated into a `set` call on that object. The breadcrumb notations used throughout the rest of the documentation (for example,  Management ›  Directory › Groups,  Management ›  Directory › Accounts) name the exact location under which an object is edited.

The console groups the available views into the following broad areas:

Directory management. Accounts, groups, roles, permissions, tenants, and domains, together with the authentication backends that resolve them. Operators create and edit principals, assign roles, and manage credentials from these screens.
Server configuration. Listeners, TLS, storage backends, transport pipelines, filtering rules, and the general server objects. The breadcrumbs on each reference page point to the corresponding location in the tree.
Operational tooling. Live sessions, queued messages, retry logic, scheduled tasks, and manually triggered [[ref/object/action|Actions]]. This is where administrators start, stop, or reload subsystems without leaving the browser.
Reports. DMARC, TLS, and ARF reports delivered to the server are browsable from the console, alongside [[telemetry|telemetry]] dashboards covering metrics, traces, and event history.
Backup and recovery. Archived items recovery, snapshots, and the maintenance workflows that read from the [[ref/object/data-retention|data retention]] schedule.

A global search at the top of the console locates settings by name; the search index is derived from the same object schema that drives the configuration screens, so results point directly at the field being looked for.

## Account Manager

[Section titled “Account Manager”](#account-manager)

The `/account` mount is the [[management/webui/account-manager|Account Manager]]. End users sign in with their mailbox credentials and manage their own password, [[auth/authentication/app-password|application passwords]], [[auth/authentication/2fa|two-factor authentication]], email aliases, and, under the Enterprise Edition, [[email/management/masked-email|masked email addresses]]. The Account Manager intentionally exposes a small subset of the object model: users see only the objects that belong to their own account and only the fields they are permitted to edit.

When an account without administrator permissions signs in at `/admin`, the console redirects to the Account Manager rather than rejecting the request outright.

## Access and routing

[Section titled “Access and routing”](#access-and-routing)

The WebUI is served over HTTP and so depends on a [[ref/object/network-listener|NetworkListener]] (found in the WebUI under  Settings ›  Network › Listeners) whose [[ref/object/network-listener#protocol|`protocol`]] is set to `http`. Because the `/admin` and `/account` prefixes are matched against the same HTTP surface that carries JMAP, WebDAV, and the `.well-known` endpoints, operators can restrict exposure of the administrator console to a private listener while keeping the rest of the HTTP traffic on a public one. The mechanics of scoping mount paths to specific listeners are covered under [[http/access-control|HTTP access control]].

Authentication at both mounts uses the server’s standard [[auth/authentication|authentication stack]], so operators can protect access with [[auth/authentication/2fa|two-factor authentication]] and app-scoped credentials just like any other client.

## Relationship to the CLI and the JMAP API

[Section titled “Relationship to the CLI and the JMAP API”](#relationship-to-the-cli-and-the-jmap-api)

Every action taken in the WebUI is backed by a JMAP method call on a documented object. The same calls are available through `stalwart-cli` (see the [[management/cli|CLI reference]]) and directly over the [[http/jmap|JMAP API]], so automated provisioning and interactive administration share the same surface. Administrators who prefer a terminal workflow, or who need to script bulk operations, can reach for the CLI without losing parity with what the WebUI offers.

## Outbound network requirement

[Section titled “Outbound network requirement”](#outbound-network-requirement)

The WebUI bundle is not bundled with the server binary. It is fetched on demand from `https://github.com/stalwartlabs/webui/releases/latest/` (the [[ref/object/application#resourceurl|`resourceUrl`]] of the built-in [[management/applications|Application]] record) the first time the server starts and refreshed on the schedule defined by [[ref/object/application#autoupdatefrequency|`autoUpdateFrequency`]]. The Stalwart host therefore needs outbound HTTPS access to GitHub’s release storage, which is served from `github.com` and `objects.githubusercontent.com`.

When the very first download fails, no bundle has been unpacked locally yet and both `/admin` and `/account` return `404 Not Found` until the next refresh succeeds. This is a frequent symptom on freshly installed and freshly migrated servers that sit behind a restrictive egress firewall: the rest of the server starts cleanly, the HTTP listener answers, but the WebUI mount paths reply with 404 because there is no bundle to serve. Once a successful download has happened, subsequent failed refreshes are non-fatal: the previously unpacked bundle stays in service and the WebUI keeps responding until the next successful refresh.

For deployments where outbound HTTPS to GitHub is not permitted, the WebUI bundle can be downloaded out-of-band, staged on an internal HTTPS server, and the [[ref/object/application#resourceurl|`resourceUrl`]] on the WebUI’s [[ref/object/application|Application]] record updated to point at the internal location. The general [[management/applications#outbound-network-requirement|outbound network requirement]] for hosted Applications applies to the WebUI in exactly the same way; it is documented separately here only because misconfiguring it on a fresh install produces the WebUI-shaped 404 most operators encounter first.