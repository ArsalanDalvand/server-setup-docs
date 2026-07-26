---
title: "Scheduling"
source: "https://stalw.art/docs/collaboration/scheduling"
source_format: official-html-to-markdown
language: "en"
doc_slug: "collaboration/scheduling"
category: "collaboration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Scheduling"
---
# Scheduling

      

Calendar scheduling coordinates meetings and events by automating the exchange of invitations. Rather than relying on manual email, calendar scheduling systems handle communication between organisers and attendees, track participation status, and manage updates and cancellations in real time.

Stalwart supports calendar scheduling through two primary mechanisms:

JMAP for Calendars, which includes built-in support for scheduling operations such as sending invitations, processing responses, and managing event updates using the JMAP protocol.
Scheduling Extensions to CalDAV, as defined in [RFC 6638](https://datatracker.ietf.org/doc/html/rfc6638). This specification extends the CalDAV protocol to support server-based scheduling operations. Events with attendee information trigger the necessary scheduling logic on the server, including delivery of invitations, processing of replies, and updates to calendars.

Calendar scheduling relies on two core protocols from the iCalendar ecosystem:

iCalendar Transport-Independent Interoperability Protocol (iTIP), defined in [RFC 5546](https://datatracker.ietf.org/doc/html/rfc5546), describes how scheduling messages (event invitations, updates, cancellations, and responses) are structured and processed using the iCalendar data format. It defines a set of methods (`REQUEST`, `REPLY`, `CANCEL`, and so on) used to negotiate scheduling operations between organisers and attendees. iTIP itself is transport-agnostic.
iCalendar Message-Based Interoperability Protocol (iMIP), described in [RFC 6047](https://datatracker.ietf.org/doc/html/rfc6047), specifies how iTIP messages are transported over email (typically via SMTP). It supports calendar scheduling between systems that may not implement CalDAV, for example sending a calendar invitation from a CalDAV account to an external recipient via email.

When Stalwart encounters a recipient that cannot be reached via CalDAV (such as an external attendee), it falls back to iMIP and delivers the scheduling message as an email with an attached `.ics` file.

## Enabling scheduling

[Section titled “Enabling scheduling”](#enabling-scheduling)

Scheduling configuration is carried on the [[ref/object/calendar-scheduling|CalendarScheduling]] singleton (found in the WebUI under  Settings ›  Calendar & Contacts › Scheduling). Scheduling is enabled by default so that invitations, participant management, and automated scheduling workflows interoperate with CalDAV clients that implement the Scheduling Extensions (RFC 6638) and with email-based iMIP.

Scheduling can be disabled globally by setting [[ref/object/calendar-scheduling#enable|`enable`]] to `false`. With that field disabled, Stalwart no longer processes scheduling requests or generates scheduling messages. Events that include attendee information are stored as standard calendar entries, but no invitations, updates, or replies are sent or processed. Both iTIP and iMIP paths are bypassed.

## Automatic event addition

[Section titled “Automatic event addition”](#automatic-event-addition)

RFC 6638 mandates that CalDAV servers automatically add scheduling messages (invitations received via iTIP or iMIP) to the recipient’s calendar. This removes the need for manual review before events appear on the calendar.

Stalwart takes a more deliberate approach. Automatic addition can be convenient, but it creates the potential for calendar spam and undermines user control over which events appear on personal or work schedules.

By default, Stalwart only adds invitations to calendars automatically when two conditions are met. First, the sender must be authenticated via [[mta/authentication/dmarc|DMARC]], confirming that the message originates from a domain authorised to send on behalf of the claimed sender. Second, the sender must be listed in the recipient’s [[collaboration/contact|address book]], indicating an established relationship. Together these safeguards support auto-processing of legitimate invitations without compromising account safety.

For deployments that wish to conform strictly to RFC 6638 and automatically accept all incoming scheduling messages, set [[ref/object/calendar-scheduling#autoaddinvitations|`autoAddInvitations`]] to `true`. With that setting enabled, Stalwart accepts all valid iTIP and iMIP invitations without verifying the sender’s authenticity or presence in the address book. This should be used with caution, particularly in open or federated environments.

## Scheduling limits

[Section titled “Scheduling limits”](#scheduling-limits)

To prevent misuse of the calendar scheduling system and to protect server stability, the CalendarScheduling singleton exposes limits on the size and scope of scheduling operations.

### iTIP message size

[Section titled “iTIP message size”](#itip-message-size)

[[ref/object/calendar-scheduling#itipmaxsize|`itipMaxSize`]] defines the maximum size of an incoming iTIP message that the server accepts and processes. Messages larger than this limit are rejected before parsing. The default is `"512kb"`. This guards against excessively large or malformed calendar invitations that could be used to exhaust memory or processing capacity.

### Outbound recipient limit

[Section titled “Outbound recipient limit”](#outbound-recipient-limit)

[[ref/object/calendar-scheduling#maxrecipients|`maxRecipients`]] limits the number of recipients that a single outbound iTIP scheduling message may include. The default is `100`. Requests that exceed this limit are rejected before delivery. This guards against mass-invitation abuse, where the calendar system might otherwise be used to send messages to a large number of users in a single operation.

In addition to these scheduling-specific limits, all existing [[mta/outbound/rate-limit|MTA-level throttling policies]] apply to outbound iMIP messages. Outbound scheduling emails are subject to the same rate limits, message size limits, and delivery policies as any other email traffic.

## Inbox auto-expunge

[Section titled “Inbox auto-expunge”](#inbox-auto-expunge)

As part of the CalDAV Scheduling Extensions, each account includes a Scheduling Inbox, a special collection where incoming iTIP messages (invitations, replies, cancellations, updates) are stored. These messages serve as a log of scheduling activity and are used by calendar clients to synchronise and process meeting-related actions.

Not all CalDAV clients reliably delete messages from the Scheduling Inbox after processing them, so the inbox can accumulate outdated or redundant scheduling messages over time. This consumes storage unnecessarily and may cause clients to continue synchronising stale data.

The retention period for the Scheduling Inbox is controlled by [[ref/object/data-retention#expungeschedulinginboxafter|`expungeSchedulingInboxAfter`]] on the [[ref/object/data-retention|DataRetention]] singleton (found in the WebUI under  Settings ›  Storage › Data Retention › Archiving,  Settings ›  Storage › Data Retention › Data Cleanup,  Settings ›  Storage › Data Retention › Auto-Expunge,  Settings ›  Storage › Data Retention › Telemetry). The value is a duration; the default is `"30d"`, meaning messages older than thirty days are removed during the routine auto-expunge run. Leaving the field unset keeps messages in the inbox indefinitely unless removed by the client or the account holder.

## HTTP RSVP

[Section titled “HTTP RSVP”](#http-rsvp)

Stalwart allows participants to respond to calendar invitations using a web link. When enabled, a unique RSVP URL is included in the iMIP invitation email sent to each attendee. Recipients can click the link to accept, decline, or tentatively respond to the invitation through a web interface, without needing a CalDAV-capable client.

This is particularly useful when scheduling with external participants or with clients that do not support calendaring or scheduling protocols.

### Endpoint

[Section titled “Endpoint”](#endpoint)

The RSVP endpoint is accessible at `https://<server-host>/calendar/rsvp`, where `<server-host>` is based on the configured [[server/general#server-hostname|hostname]]. When a participant accesses this URL, Stalwart verifies the request and presents a lightweight RSVP interface.

### Enabling

[Section titled “Enabling”](#enabling)

HTTP RSVP is enabled by default. The feature is disabled by setting [[ref/object/calendar-scheduling#httprsvpenable|`httpRsvpEnable`]] on the CalendarScheduling singleton to `false`. When disabled, RSVP links are no longer included in outgoing iMIP messages and the RSVP endpoint stops serving requests.

### Custom URL

[Section titled “Custom URL”](#custom-url)

Stalwart builds the RSVP URL from the configured [[server/general#server-hostname|`server.hostname`]]. The base URL used in invitation emails can be overridden by setting [[ref/object/calendar-scheduling#httprsvpurl|`httpRsvpUrl`]]. The path component `/calendar/rsvp` is fixed, but the hostname and any prefix path can be customised. This is particularly useful when deploying Stalwart behind a reverse proxy or on a different hostname.

### Expiration

[Section titled “Expiration”](#expiration)

[[ref/object/calendar-scheduling#httprsvplinkexpiry|`httpRsvpLinkExpiry`]] defines how long an RSVP link remains valid after being issued. The value is a duration; the default is `"90d"`. Once expired, the link no longer allows a response.

A representative HTTP RSVP configuration:

```
{  "httpRsvpEnable": true,  "httpRsvpUrl": "https://calendar.example.org/custom-path/calendar/rsvp",  "httpRsvpLinkExpiry": "7d"}
```

## Branding and templating

[Section titled “Branding and templating”](#branding-and-templating)

Stalwart supports custom branding and templating for calendar scheduling so that outbound invitations and RSVP responses can be aligned with organisational branding.

### Templates

[Section titled “Templates”](#templates)

Stalwart ships with built-in default templates for both email and web rendering. Two HTML templates are used in the scheduling system, both set on the CalendarScheduling singleton.

#### iMIP email template

[Section titled “iMIP email template”](#imip-email-template)

The iMIP template renders outbound iMIP scheduling messages (invitations, updates, cancellations). Each message contains a rich HTML MIME part rendered from the template, including event details, sender information, and the RSVP link (if HTTP RSVP is enabled). A custom template can be supplied through [[ref/object/calendar-scheduling#emailtemplate|`emailTemplate`]].

#### HTTP RSVP web template

[Section titled “HTTP RSVP web template”](#http-rsvp-web-template)

The second template is used by the HTTP RSVP endpoint to render the response page shown when a participant follows an RSVP link. This page presents the invitation details and allows the attendee to accept, decline, or respond tentatively. A custom template can be supplied through [[ref/object/calendar-scheduling#httprsvptemplate|`httpRsvpTemplate`]].

### Logo

[Section titled “Logo”](#logo)

Both default templates include a logo, which by default is the system-wide branding logo. The logo can be overridden on a per-tenant or per-domain basis, so that different business units or hosted domains display their own branding. For details, refer to the [[management/webui/branding|Branding]] section.