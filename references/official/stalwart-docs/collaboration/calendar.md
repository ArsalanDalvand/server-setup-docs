---
title: "Calendar"
source: "https://stalw.art/docs/collaboration/calendar"
source_format: official-html-to-markdown
language: "en"
doc_slug: "collaboration/calendar"
category: "collaboration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Calendar"
---
# Calendar

      

Stalwart includes full support for calendar management and scheduling through the JMAP and CalDAV protocols, covering creation, access, and synchronization of calendar data across devices and applications.

## JMAP for Calendars

[Section titled “JMAP for Calendars”](#jmap-for-calendars)

JMAP for Calendars is a protocol that provides a JSON-based API for managing calendar data such as events, tasks, and scheduling information. It is designed as a replacement for CalDAV and CalDAV Scheduling, offering the same functionality (creating, reading, updating, deleting, and sharing calendar data) but in a simpler and more consistent way.

Unlike CalDAV, which is built on top of WebDAV and uses XML combined with embedded iCalendar data, JMAP for Calendars is entirely JSON over HTTPS. It uses the same core request/response model as JMAP for Mail, providing a unified protocol for all personal data types.

## CalDAV

[Section titled “CalDAV”](#caldav)

CalDAV is a standardized extension of WebDAV that allows clients to interact with calendar data stored on a server. It is supported by calendar applications on desktops and mobile devices, including Apple Calendar, Thunderbird, and Outlook (with plugins). CalDAV covers creating events, setting recurring appointments, inviting participants, and viewing shared calendars.

### Accessing calendars

[Section titled “Accessing calendars”](#accessing-calendars)

CalDAV clients can access calendar data on Stalwart using standardized URLs. The recommended method for client configuration is through the autodiscovery endpoint located at `/.well-known/caldav`. When a client queries this path, the server redirects the request to the appropriate CalDAV resource associated with the authenticated account. This simplifies setup by allowing most modern clients to automatically locate and configure calendar access without manual URL input.

Alternatively, CalDAV calendars can be accessed directly via the path `/dav/cal/<account_name>`, where `<account_name>` is the username of the account. For example, the calendar for the account `alice` is available at `/dav/cal/alice`. This direct path can be used in clients that support manual CalDAV configuration or when troubleshooting.

Both methods provide access to the same calendar data and are fully compatible with the CalDAV protocol.

## Limits

[Section titled “Limits”](#limits)

Calendar-wide limits are configured on the [[ref/object/calendar|Calendar]] singleton (found in the WebUI under  Settings ›  Calendar & Contacts › Calendar). They protect server resources against clients that attempt to create excessively large or complex calendar entries.

### Recurrence expansion

[Section titled “Recurrence expansion”](#recurrence-expansion)

The iCalendar format allows clients to define repeating events using RRULEs (recurrence rules). When such an event is created, Stalwart pre-expands the recurrence pattern into individual instances and stores them, avoiding expensive RRULE calculations on every calendar query.

Recurrence expansion is computationally intensive, especially for long-term or infinitely repeating events. The [[ref/object/calendar#maxrecurrenceexpansions|`maxRecurrenceExpansions`]] field on the Calendar singleton limits the number of instances generated from a recurrence rule. The default value is `3000`, sufficient for most realistic scheduling needs (for example, daily events spanning several years). If a recurrence rule would exceed this limit, the server rejects or truncates the event depending on context. Raising the value may significantly affect server performance.

### iCalendar object size

[Section titled “iCalendar object size”](#icalendar-object-size)

[[ref/object/calendar#maxicalendarsize|`maxICalendarSize`]] defines the maximum accepted size of a single iCalendar object (VEVENT, VTODO, and so on). The default is `"512kb"`. Lowering or raising the limit protects against unreasonably large calendar entries while accommodating deployment-specific client behaviour.

### Attendee limit

[Section titled “Attendee limit”](#attendee-limit)

[[ref/object/calendar#maxattendees|`maxAttendees`]] controls the maximum number of attendees allowed in a single iCalendar instance. The default is `20`. Deployments that regularly schedule large meetings may raise this value.

### JMAP parsing limit

[Section titled “JMAP parsing limit”](#jmap-parsing-limit)

The JMAP `CalendarEvent/parse` method is bounded by [[ref/object/jmap#parselimitevent|`parseLimitEvent`]] on the [[ref/object/jmap|Jmap]] singleton (found in the WebUI under  Settings ›  Network › JMAP › Limits,  Settings ›  Network › JMAP › Push,  Settings ›  Network › JMAP › WebSocket), which sets the maximum number of iCalendar items that can be parsed in a single request. The default is `10`.

## Default calendar

[Section titled “Default calendar”](#default-calendar)

To improve client compatibility and allow calendar operations to proceed without manual setup, Stalwart automatically creates a default calendar when an account is accessed and no calendars currently exist for it. The URL path of the auto-created calendar is controlled by [[ref/object/calendar#defaulthrefname|`defaultHrefName`]] on the Calendar singleton. The default value is `"default"`, so for an account named `john` the default calendar is created at `/dav/cal/john/default`. CalDAV clients then always see at least one calendar on first connection.

### Disabling automatic creation

[Section titled “Disabling automatic creation”](#disabling-automatic-creation)

Automatic creation can be suppressed by clearing [[ref/object/calendar#defaulthrefname|`defaultHrefName`]] (leaving it unset). In that case, Stalwart does not create a default calendar and clients that connect to an account with no calendars receive an appropriate error response.

### Default display name

[Section titled “Default display name”](#default-display-name)

The display name assigned to the auto-created calendar (as shown in client applications) is controlled by [[ref/object/calendar#defaultdisplayname|`defaultDisplayName`]]. The default is `"Stalwart Calendar"`; the value can be adjusted to match organisational branding or language preferences.

## Per-account limits

[Section titled “Per-account limits”](#per-account-limits)

The Calendar singleton also sets default caps on per-account calendar resources: [[ref/object/calendar#maxcalendars|`maxCalendars`]] caps the number of calendars an account can create (default `250`), [[ref/object/calendar#maxevents|`maxEvents`]] caps the number of events, [[ref/object/calendar#maxparticipantidentities|`maxParticipantIdentities`]] caps participant identities (default `100`), and [[ref/object/calendar#maxeventnotifications|`maxEventNotifications`]] caps event notifications. These apply unless overridden at the account or tenant level.