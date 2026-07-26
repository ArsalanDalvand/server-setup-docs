---
title: "Overview"
source: "https://stalw.art/docs/management/applications"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/applications"
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

      

Stalwart can download and host one or more single-page applications (SPAs) directly from the server. Each hosted application is configured on the [[ref/object/application|Application]] object (found in the WebUI under  Settings ›  Web Applications) and is served alongside the regular HTTP endpoints, without the need for a separate web server or reverse proxy in front.

The built-in [[management/webui|WebUI]] is itself an Application: a single bundle mounted at both `/admin` (the administration console) and `/account` (the user self-service portal). Operators can register additional Applications on the same server, for example third-party management dashboards or self-hosted front-ends for the JMAP, CalDAV, CardDAV, and WebDAV stacks.

## What an Application is

[Section titled “What an Application is”](#what-an-application-is)

An Application is a static SPA bundle, distributed as a zip archive, that the server downloads and unpacks locally. Once unpacked, the server serves the application’s files in response to HTTP requests whose path matches one of the configured mount paths. The bundle is identified by the [[ref/object/application#resourceurl|`resourceUrl`]] field, which points to the archive that will be fetched (for example the latest GitHub release of the WebUI), and by the [[ref/object/application#description|`description`]] field, which carries a short human-readable label used in the management interfaces.

Individual Applications can be disabled without removing them from the configuration by clearing the [[ref/object/application#enabled|`enabled`]] flag. A disabled Application is not downloaded and is not served, but its configuration is retained.

## Mount paths

[Section titled “Mount paths”](#mount-paths)

Each Application is mounted at one or more URL path prefixes, listed on the [[ref/object/application#urlprefix|`urlPrefix`]] field. The server matches incoming HTTP requests against the configured prefixes and routes them to the corresponding bundle. A single Application may declare multiple prefixes, which is how the WebUI is exposed at both `/admin` and `/account` from the same deployment.

Mount paths are local to the server’s HTTP namespace and must not collide with the API endpoints used by JMAP, WebDAV, or the `.well-known` URIs. A path collision between two Applications, or between an Application and a server-provided endpoint, is rejected at configuration time.

The unpacked bundle is written to a working directory on the local filesystem. By default this directory is chosen automatically (typically `/tmp`), but it can be pinned through the [[ref/object/application#unpackdirectory|`unpackDirectory`]] field when the default temporary location is not suitable, for example on systems where `/tmp` is mounted `noexec` or cleared aggressively between restarts.

## Relationship to listeners

[Section titled “Relationship to listeners”](#relationship-to-listeners)

Applications are served over HTTP traffic, but they do not themselves open sockets. The sockets are owned by the [[ref/object/network-listener|NetworkListener]] object (found in the WebUI under  Settings ›  Network › Listeners). Any listener configured with the [[ref/object/network-listener#protocol|`protocol`]] variant set to `http` will route requests whose path matches a registered mount to the corresponding Application. In a typical deployment a single HTTPS listener carries the JMAP, WebDAV, and Application traffic together; no per-Application listener is required.

When [[http/access-control|access control]] rules are in force, requests to Application mount paths are filtered alongside the rest of the HTTP surface. Operators restricting public exposure can therefore confine an Application (for example `/admin`) to a private listener while leaving the remaining HTTP endpoints reachable over the public one.

## Installing an Application

[Section titled “Installing an Application”](#installing-an-application)

An Application is installed by creating an [[ref/object/application|Application]] record with the bundle’s download URL in [[ref/object/application#resourceurl|`resourceUrl`]], the desired mount paths in [[ref/object/application#urlprefix|`urlPrefix`]], and a short [[ref/object/application#description|`description`]]. The server then fetches the archive on the schedule described on the [[management/applications/update|Updates]] page, unpacks it into the working directory, and begins serving the files. The same workflow is exposed through the WebUI, through `stalwart-cli`, and directly over the JMAP API on the `x:Application/set` method.

## Outbound network requirement

[Section titled “Outbound network requirement”](#outbound-network-requirement)

Each Application’s bundle is fetched over HTTPS from the [[ref/object/application#resourceurl|`resourceUrl`]] configured on its record, so outbound HTTPS connectivity from the Stalwart host to that URL is a hard requirement for the Application to be served. When the very first download of an Application fails, no previously installed bundle exists to fall back to and HTTP requests against the Application’s mount paths return `404 Not Found` until the next refresh succeeds.

For deployments behind a restrictive egress firewall, the firewall must allow HTTPS from the Stalwart host to whatever hosts serve each configured `resourceUrl`. When outbound internet access is not permitted at all, the bundle can be staged on an internal HTTPS server and the [[ref/object/application#resourceurl|`resourceUrl`]] field updated to point at the internal location, after which the server refreshes from there instead.

The bundle URL of the built-in WebUI and the specific symptom that misconfiguring this requirement produces are documented under the [[management/webui#outbound-network-requirement|WebUI overview]].

## Bundle format

[Section titled “Bundle format”](#bundle-format)

An Application bundle is a `.zip` archive that must contain an `index.html` at its root. When the archive is unpacked, the server rewrites the `<base href="/">` tag in `index.html` to match the mount path the bundle is served from, so the same archive can be mounted at `/admin`, `/account`, or any other prefix without being repackaged. Assets referenced by relative URLs inside the bundle therefore resolve correctly regardless of the mount path.

No checksum or signature verification is performed on the downloaded archive. Applications are intended to be installed from trusted sources only; the transport is always HTTPS, but the contents of the bundle are not cryptographically validated against an external manifest.

## Multiple mount paths for a single Application

[Section titled “Multiple mount paths for a single Application”](#multiple-mount-paths-for-a-single-application)

The built-in WebUI is modelled as one Application with two entries in [[ref/object/application#urlprefix|`urlPrefix`]], `/admin` and `/account`; the prefixes are URL-path aliases for the same bundle rather than two separate installations. Additional Applications can follow the same pattern when several aliases need to resolve to the same files.