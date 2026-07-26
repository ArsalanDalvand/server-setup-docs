---
title: "Alerts"
source: "https://stalw.art/docs/telemetry/alerts"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/alerts"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Alerts"
---
# Alerts

      

Alerts notify administrators when a server metric, or a combination of metrics, crosses a defined threshold. The condition is evaluated continuously against the live metrics stream, and when it holds, a notification is emitted. Notifications can be delivered as an in-server event (which a [[telemetry/webhooks|webhook]] can forward downstream) or as an email message.

The trigger is an expression that can reference one or more metrics. Expressions support logical operators, so conditions such as `store_foundationdb_error > 100 || store_s3_error > 100` can be built up directly. Metric identifiers in the expression use underscores in place of dots and hyphens (`security_brute_force_ban` rather than `security.brute-force-ban`) because the expression language restricts variable names to alphanumeric characters and underscores.

## Configuration

[Section titled “Configuration”](#configuration)

Each alert is represented by an [[ref/object/alert|Alert]] object (found in the WebUI under  Settings ›  Telemetry › Alerts). The relevant fields are:

[[ref/object/alert#enable|`enable`]]: whether the alert is active. Default `true`.
[[ref/object/alert#condition|`condition`]]: the expression evaluated against incoming metrics. The alert fires when the expression evaluates to true.
[[ref/object/alert#eventalert|`eventAlert`]]: event notification settings. A nested type with variants `Disabled` and `Enabled`.
[[ref/object/alert#emailalert|`emailAlert`]]: email notification settings. A nested type with variants `Disabled` and `Enabled`.

### Event notification

[Section titled “Event notification”](#event-notification)

When the `Enabled` variant of [[ref/object/alert#eventalert|`eventAlert`]] is selected, the alert emits a `telemetry.alert` event whenever [[ref/object/alert#condition|`condition`]] holds. The event can be captured by a [[ref/object/web-hook|WebHook]] so that downstream systems receive the notification.

The fields on the `Enabled` variant are:

[[ref/object/alert#alerteventproperties|`eventMessage`]]: message carried by the emitted event. May reference metric values using placeholders.

### Email notification

[Section titled “Email notification”](#email-notification)

When the `Enabled` variant of [[ref/object/alert#emailalert|`emailAlert`]] is selected, the server sends an email each time the condition becomes true. The fields on the `Enabled` variant are:

[[ref/object/alert#alertemailproperties|`fromName`]]: optional display name of the sender.
[[ref/object/alert#alertemailproperties|`fromAddress`]]: sender email address.
[[ref/object/alert#alertemailproperties|`to`]]: list of recipient addresses.
[[ref/object/alert#alertemailproperties|`subject`]]: subject line. May reference metric values using placeholders.
[[ref/object/alert#alertemailproperties|`body`]]: message body. May reference metric values using placeholders.

## Example

[Section titled “Example”](#example)

The following Alert fires when the count of FoundationDB or S3 errors exceeds one hundred. It raises an event notification carrying a message with the current counts and also sends an email with the same information:

```
{  "enable": true,  "condition": {    "else": "store_foundationdb_error > 100 || store_s3_error > 100"  },  "eventAlert": {    "@type": "Enabled",    "eventMessage": "Database errors: FDB %{store.foundationdb-error}%, S3 %{store.s3-error}%"  },  "emailAlert": {    "@type": "Enabled",    "fromName": "Alert Subsystem",    "fromAddress": "[email protected]",    "to": ["[email protected]"],    "subject": "Found %{store.foundationdb-error}% FDB and %{store.s3-error}% S3 errors",    "body": "We found %{store.foundationdb-error}% FDB and %{store.s3-error}% S3 errors."  }}
```