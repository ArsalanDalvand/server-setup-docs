---
title: "Updates"
source: "https://stalw.art/docs/management/applications/update"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/applications/update"
category: "management"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Updates"
---
# Updates

      

Hosted applications in Stalwart are kept current through a combination of scheduled background checks and on-demand triggers. Each [[ref/object/application|Application]] (found in the WebUI under  Settings ›  Web Applications) is refreshed on its own cadence, and the refresh can also be forced at any time through the [[ref/object/action|Action]] object (found in the WebUI under  Management ›  Actions).

## Scheduled updates

[Section titled “Scheduled updates”](#scheduled-updates)

Every Application carries an [[ref/object/application#autoupdatefrequency|`autoUpdateFrequency`]] field, which controls how often the server checks its [[ref/object/application#resourceurl|`resourceUrl`]] for a newer bundle. The field is a duration; the default value is `90d`, so an unmodified installation revisits each Application’s download URL roughly once per quarter. Shorter values tighten the refresh loop at the cost of more frequent downloads; longer values pin an Application to a given release for longer stretches.

The schedule is per-Application, so the built-in WebUI and any additional Applications can follow different refresh policies. When the frequency elapses, the server fetches the bundle, compares it against the currently installed version, and applies it if a newer one is available. If the remote resource has not changed since the last successful check, the server leaves the unpacked files in place.

## On-demand updates

[Section titled “On-demand updates”](#on-demand-updates)

A refresh can also be forced outside the scheduled cadence by triggering the `UpdateApps` variant of the [[ref/object/action|Action]] object. Creating an Action record with this variant instructs the server to run the same update procedure immediately, for every enabled Application, without waiting for the next scheduled tick.

The trigger is exposed through every management surface:

From the [[management/webui|WebUI]], administrators invoke the action from the management section dedicated to server actions. The interface creates the corresponding `UpdateApps` Action and reports on its outcome.
From the [[management/cli|CLI]], the same operation is issued as a standard `stalwart-cli` command against the Action object. `stalwart-cli create action/update-apps` submits the request to the server.
Over the JMAP API, administrators with the `sysActionCreate` and `actionUpdateApps` [[ref/permissions|permissions]] submit an `x:Action/set` call whose `create` entry carries `{"@type": "UpdateApps"}`:

```
{  "using": ["urn:ietf:params:jmap:core", "urn:stalwart:jmap"],  "methodCalls": [    ["x:Action/set", {      "create": {        "new1": { "@type": "UpdateApps" }      }    }, "c1"]  ]}
```

## What happens during an update

[Section titled “What happens during an update”](#what-happens-during-an-update)

The update procedure is identical whether it was triggered by the schedule or by an `UpdateApps` Action. For each enabled Application the server:

Downloads the bundle from the configured [[ref/object/application#resourceurl|`resourceUrl`]] over HTTPS.
Unpacks the archive into the working directory selected by [[ref/object/application#unpackdirectory|`unpackDirectory`]], rewriting the `<base href>` in `index.html` to match the mount path.
Swaps the live mount over to the newly unpacked files so that subsequent HTTP requests for the Application’s [[ref/object/application#urlprefix|`urlPrefix`]] entries are served from the new version.

No checksum or signature verification is performed on the downloaded bundle; see the [[management/applications#bundle-format|bundle format]] section for the security implications. A malformed archive or an unreachable download is logged and leaves the previously installed bundle in service, so a failed update does not take an Application offline.

Disabled Applications (those with [[ref/object/application#enabled|`enabled`]] set to `false`) are skipped during both scheduled and on-demand runs. An `UpdateApps` Action always refreshes every enabled Application in a single run; the variant has no fields to limit the operation to a single Application.