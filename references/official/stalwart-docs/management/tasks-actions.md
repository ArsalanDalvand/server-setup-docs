---
title: "Overview"
source: "https://stalw.art/docs/management/tasks-actions"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/tasks-actions"
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

      

Stalwart performs two kinds of server management operation beyond the regular request path: [[management/tasks-actions/actions|Actions]], which run immediately on demand, and [[management/tasks-actions/tasks|Tasks]], which run in the background according to a schedule. Both are invoked through the same JMAP API exposed by the [[management/webui|WebUI]], the [[management/cli|CLI]], and direct HTTP calls, and each is recorded so its outcome can be inspected afterwards.

## Actions

[Section titled “Actions”](#actions)

An [[management/tasks-actions/actions|Action]] is a management operation triggered on demand and executed immediately. Reloading settings or TLS certificates, invalidating caches, pausing and resuming the outbound queue, running a DMARC troubleshooting probe, and classifying a message through the spam filter are all Actions. Each invocation is stored as an [[ref/object/action|Action]] object so that any result it produces, such as a spam score or a troubleshooting report, can be retrieved later.

## Tasks

[Section titled “Tasks”](#tasks)

A [[management/tasks-actions/tasks|Task]] is a background job scheduled for execution at a point in time. Search indexing, calendar alarm dispatch, outbound DMARC and TLS reports, account destruction, store and spam filter maintenance, ACME renewal, DKIM key rotation, and DNS record updates are all Tasks. Each scheduled job is stored as a [[ref/object/task|Task]] object whose status records whether it is pending, awaiting retry, or has failed permanently, and the [[ref/object/task-manager|TaskManager]] singleton governs how failed tasks are retried.

## How they differ

[Section titled “How they differ”](#how-they-differ)

Actions and Tasks both represent server operations driven through the management API, but they differ in when and how they run:

Timing: an Action runs immediately in response to an administrator request; a Task runs in the background at its scheduled [[ref/object/task#due|`due`]] time, whether deferred once or repeated on a recurring basis.
Execution model: an Action completes synchronously and returns its result on the same request; a Task is queued and processed asynchronously by the task manager.
Failure handling: an Action either succeeds or fails on the spot; a Task carries a retry budget and is retried according to the [[ref/object/task-manager|TaskManager]] strategy until it succeeds or is marked as permanently failed.
Typical use: Actions cover interactive operations such as reloading configuration or probing message handling; Tasks cover recurring or deferred maintenance such as indexing, reporting, and certificate renewal.