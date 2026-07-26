---
title: "Asn"
source: "https://stalw.art/docs/ref/object/asn"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/asn"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Asn"
---
# Asn

      

Configures ASN and geolocation data sources for IP address lookups.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › ASN & GeoIP

## Fields

[Section titled “Fields”](#fields)

Asn is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Disabled"

[Section titled “@type: "Disabled"”](#type-disabled)

Disabled

### @type: "Resource"

[Section titled “@type: "Resource"”](#type-resource)

URL Resource

##### expires

[Section titled “expires”](#expires)

Type: `Duration` · default: `"1d"`

How often to refresh the ASN/Geo data.

##### maxSize

[Section titled “maxSize”](#maxsize)

Type: `Size` · default: `"100mb"`

Maximum size of the ASN/Geo data file.

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"5m"`

Time after which the ASN/Geo resource fetch is considered failed.

##### asnUrls

[Section titled “asnUrls”](#asnurls)

Type: `String[]`

URLs to fetch CSV file containing the IP to ASN mappings.

##### geoUrls

[Section titled “geoUrls”](#geourls)

Type: `String[]`

URLs to fetch CSV file containing the IP to country code mappings.

##### httpAuth

[Section titled “httpAuth”](#httpauth)

Type: [`HttpAuth`](#httpauth) · required

The type of HTTP authentication to use

##### httpHeaders

[Section titled “httpHeaders”](#httpheaders)

Type: `Map<String, String>`

Additional headers to include in HTTP requests

### @type: "Dns"

[Section titled “@type: "Dns"”](#type-dns)

DNS Lookup

##### indexAsn

[Section titled “indexAsn”](#indexasn)

Type: `UnsignedInt` · default: `0`

The position of the ASN in the DNS TXT record.

##### indexAsnName

[Section titled “indexAsnName”](#indexasnname)

Type: `UnsignedInt?`

The position of the ASN Name in the DNS TXT record.

##### indexCountry

[Section titled “indexCountry”](#indexcountry)

Type: `UnsignedInt?`

The position of the country code in the DNS TXT record.

##### separator

[Section titled “separator”](#separator)

Type: `String` · default: `"|"`

The separator character used in the DNS TXT record.

##### zoneIpV4

[Section titled “zoneIpV4”](#zoneipv4)

Type: `String` · required

The DNS zone to query for IPv4 ASN and geolocation data.

##### zoneIpV6

[Section titled “zoneIpV6”](#zoneipv6)

Type: `String` · required

The DNS zone to query for IPv6 ASN and geolocation data.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Asn singleton is available via the `urn:stalwart:jmap` capability.

### x:Asn/get

[Section titled “x:Asn/get”](#xasnget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysAsnGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Asn/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Asn/set

[Section titled “x:Asn/set”](#xasnset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysAsnUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Asn/set",          {            "update": {              "singleton": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Asn
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Asn --field description='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### HttpAuth

[Section titled “HttpAuth”](#httpauth-1)

Defines the HTTP authentication method to use for HTTP requests.

`Unauthenticated`: Anonymous. No additional fields.
`Basic`: Basic Authentication. Carries the fields of [`HttpAuthBasic`](#httpauthbasic).
`Bearer`: Bearer Token. Carries the fields of [`HttpAuthBearer`](#httpauthbearer).

#### HttpAuthBasic

[Section titled “HttpAuthBasic”](#httpauthbasic)

HTTP Basic authentication credentials.

##### username

[Section titled “username”](#username)

Type: `String` · required

Username for HTTP Basic Authentication

##### secret

[Section titled “secret”](#secret)

Type: [`SecretKey`](#secretkey) · required

Password for HTTP Basic Authentication

##### SecretKey

[Section titled “SecretKey”](#secretkey)

A secret value provided directly, from an environment variable, or from a file.

`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

##### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret-1)

Type: `String` · required · secret

Password or secret value

##### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

##### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

#### HttpAuthBearer

[Section titled “HttpAuthBearer”](#httpauthbearer)

HTTP Bearer token authentication.

##### bearerToken

[Section titled “bearerToken”](#bearertoken)

Type: [`SecretKey`](#secretkey) · required

Bearer token for HTTP Bearer Authentication