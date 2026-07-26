---
title: "SystemSettings"
source: "https://stalw.art/docs/ref/object/system-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/system-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SystemSettings"
---
# SystemSettings

      

Configures core server settings including hostname, thread pool, and network services.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › Services Settings ›  Network › General

## Fields

[Section titled “Fields”](#fields)

##### defaultHostname

[Section titled “defaultHostname”](#defaulthostname)

Type: `HostName` · required

The default hostname to use in SMTP greetings, MTA reports and other places where a hostname is needed but not specified.

##### defaultDomainId

[Section titled “defaultDomainId”](#defaultdomainid)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Default domain to use for authentication and reports.

##### defaultCertificateId

[Section titled “defaultCertificateId”](#defaultcertificateid)

Type: `Id<`[[ref/object/certificate|`Certificate`]]`>?`

Default TLS certificate to use when no SNI is provided by the client

##### threadPoolSize

[Section titled “threadPoolSize”](#threadpoolsize)

Type: `UnsignedInt?` · min: 1

The number of threads in the global thread pool for CPU intensive tasks. Defaults to the number of CPU cores

##### maxConnections

[Section titled “maxConnections”](#maxconnections)

Type: `UnsignedInt` · default: `8192` · min: 1

The maximum number of concurrent connections the server will accept

##### proxyTrustedNetworks

[Section titled “proxyTrustedNetworks”](#proxytrustednetworks)

Type: `IpMask[]`

Enable proxy protocol for connections from these networks

##### mailExchangers

[Section titled “mailExchangers”](#mailexchangers)

Type: [`MailExchanger`](#mailexchanger)`[]` · default: `[{"priority":10}]`

List of mail exchangers to publish in DNS MX records.

##### services

[Section titled “services”](#services)

Type: `Map<`[`ServiceProtocol`](#serviceprotocol)`,`[`Service`](#service)`>` · default: `{"caldav":{"cleartext":false},"carddav":{"cleartext":false},"imap":{"cleartext":false},"jmap":{"cleartext":false},"managesieve":{"cleartext":false},"pop3":{"cleartext":false},"smtp":{"cleartext":false},"webdav":{"cleartext":false}}`

List of services to advertise in DNS and auto configuration services

##### providerInfo

[Section titled “providerInfo”](#providerinfo)

Type: `Map<`[`ProviderInfo`](#providerinfo)`, String>`

Information about the provider to advertise in auto configuration services.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SystemSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:SystemSettings/get

[Section titled “x:SystemSettings/get”](#xsystemsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSystemSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SystemSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SystemSettings/set

[Section titled “x:SystemSettings/set”](#xsystemsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSystemSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SystemSettings/set",          {            "update": {              "singleton": {                "defaultHostname": "mail.example.com"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SystemSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SystemSettings --field defaultHostname=mail.example.com
```

## Nested types

[Section titled “Nested types”](#nested-types)

### MailExchanger

[Section titled “MailExchanger”](#mailexchanger)

Defines a mail exchanger for DNS MX records.

##### hostname

[Section titled “hostname”](#hostname)

Type: `HostName?`

The hostname of the mail exchanger, or null to use the default hostname

##### priority

[Section titled “priority”](#priority)

Type: `UnsignedInt` · default: `10` · min: 1 · max: 65535

The priority of the mail exchanger, lower values are preferred. Mail exchangers with the same priority will be selected randomly.

### Service

[Section titled “Service”](#service)

Defines a service endpoint advertised in auto-configuration.

##### hostname

[Section titled “hostname”](#hostname-1)

Type: `HostName?`

The hostname of the service, or null to use the server’s default hostname

##### cleartext

[Section titled “cleartext”](#cleartext)

Type: `Boolean` · default: `false`

Whether to advertise the service as available without TLS encryption. This does not affect whether the service actually accepts cleartext connections, which is configured separately for each network listener.

## Enums

[Section titled “Enums”](#enums)

### ServiceProtocol

[Section titled “ServiceProtocol”](#serviceprotocol)
ValueLabel`jmap`JMAP`imap`IMAP`pop3`POP3`smtp`SMTP submission`caldav`CalDAV`carddav`CardDAV`webdav`WebDAV`managesieve`ManageSieve

### ProviderInfo

[Section titled “ProviderInfo”](#providerinfo-1)
ValueLabel`providerName`Provider name`providerShortName`Short provider name`userDocumentation`URL with user-facing documentation`developerDocumentation`URL with developer-facing documentation`contactUri`Contact information URI`logoUrl`URL to a logo image for the provider`logoWidth`Logo width in pixels`logoHeight`Logo height in pixels