---
title: "DnsServer"
source: "https://stalw.art/docs/ref/object/dns-server"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/dns-server"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DnsServer"
---
# DnsServer

      

Defines a DNS server for automatic record management.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › DNS › DNS Providers

## Fields

[Section titled “Fields”](#fields)

DnsServer is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Tsig"

[Section titled “@type: "Tsig"”](#type-tsig)

RFC2136 (TSIG)

##### host

[Section titled “host”](#host)

Type: `IpAddr` · required

The IP address of the DNS server

##### port

[Section titled “port”](#port)

Type: `UnsignedInt` · default: `53` · max: 65535 · min: 1

The port used to communicate with the DNS server

##### keyName

[Section titled “keyName”](#keyname)

Type: `String` · required

The key used to authenticate with the DNS server

##### key

[Section titled “key”](#key)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### protocol

[Section titled “protocol”](#protocol)

Type: [`IpProtocol`](#ipprotocol) · default: `"udp"`

The protocol used to communicate with the DNS server

##### tsigAlgorithm

[Section titled “tsigAlgorithm”](#tsigalgorithm)

Type: [`TsigAlgorithm`](#tsigalgorithm) · default: `"hmac-sha512"`

The TSIG algorithm used to authenticate with the DNS server

##### description

[Section titled “description”](#description)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Deprecated1"

[Section titled “@type: "Deprecated1"”](#type-deprecated1)

RFC2136 (SIG0 - deprecated)

### @type: "Cloudflare"

[Section titled “@type: "Cloudflare"”](#type-cloudflare)

Cloudflare

##### secret

[Section titled “secret”](#secret)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-1)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-1)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-1)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-1)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-1)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-1)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "DigitalOcean"

[Section titled “@type: "DigitalOcean"”](#type-digitalocean)

DigitalOcean

##### secret

[Section titled “secret”](#secret-1)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-2)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-2)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-2)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-2)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-2)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-2)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-2)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "DeSEC"

[Section titled “@type: "DeSEC"”](#type-desec)

DeSEC

##### secret

[Section titled “secret”](#secret-2)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-3)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-3)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-3)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-3)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-3)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-3)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-3)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Ovh"

[Section titled “@type: "Ovh"”](#type-ovh)

OVH

##### applicationKey

[Section titled “applicationKey”](#applicationkey)

Type: `String` · required

The application key used to authenticate with the OVH DNS server

##### applicationSecret

[Section titled “applicationSecret”](#applicationsecret)

Type: [`SecretKey`](#secretkey) · required

The application secret used to authenticate with the OVH DNS server

##### consumerKey

[Section titled “consumerKey”](#consumerkey)

Type: [`SecretKey`](#secretkey) · required

The consumer key used to authenticate with the OVH DNS server

##### ovhEndpoint

[Section titled “ovhEndpoint”](#ovhendpoint)

Type: [`OvhEndpoint`](#ovhendpoint) · default: `"ovh-eu"`

Which OVH endpoint to use

##### description

[Section titled “description”](#description-4)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-4)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-4)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-4)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-4)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-4)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-4)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Bunny"

[Section titled “@type: "Bunny"”](#type-bunny)

BunnyDNS

##### secret

[Section titled “secret”](#secret-3)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-5)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-5)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-5)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-5)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-5)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-5)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-5)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Porkbun"

[Section titled “@type: "Porkbun"”](#type-porkbun)

Porkbun

##### apiKey

[Section titled “apiKey”](#apikey)

Type: `String` · required

The API key used to authenticate with Porkbun

##### secretApiKey

[Section titled “secretApiKey”](#secretapikey)

Type: [`SecretKey`](#secretkey) · required

The secret API key used to authenticate with Porkbun

##### description

[Section titled “description”](#description-6)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-6)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-6)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-6)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-6)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-6)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-6)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Dnsimple"

[Section titled “@type: "Dnsimple"”](#type-dnsimple)

DNSimple

##### authToken

[Section titled “authToken”](#authtoken)

Type: [`SecretKey`](#secretkey) · required

The authentication token used to authenticate with DNSimple

##### accountIdentifier

[Section titled “accountIdentifier”](#accountidentifier)

Type: `String` · required

The account ID used to authenticate with DNSimple

##### description

[Section titled “description”](#description-7)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-7)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-7)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-7)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-7)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-7)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-7)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Spaceship"

[Section titled “@type: "Spaceship"”](#type-spaceship)

Spaceship

##### apiKey

[Section titled “apiKey”](#apikey-1)

Type: `String` · required

The API key used to authenticate with Spaceship

##### secret

[Section titled “secret”](#secret-4)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-8)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-8)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-8)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-8)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-8)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-8)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-8)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Route53"

[Section titled “@type: "Route53"”](#type-route53)

AWS Route53

##### accessKeyId

[Section titled “accessKeyId”](#accesskeyid)

Type: `String` · required

The AWS access key ID

##### secretAccessKey

[Section titled “secretAccessKey”](#secretaccesskey)

Type: [`SecretKey`](#secretkey) · required

The AWS secret access key

##### sessionToken

[Section titled “sessionToken”](#sessiontoken)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional session token for temporary AWS credentials

##### region

[Section titled “region”](#region)

Type: `String` · default: `"us-east-1"`

The AWS region

##### hostedZoneId

[Section titled “hostedZoneId”](#hostedzoneid)

Type: `String?`

Hosted zone ID to use (resolved automatically by name if not set)

##### privateZoneOnly

[Section titled “privateZoneOnly”](#privatezoneonly)

Type: `Boolean` · default: `false`

Whether to restrict zone resolution to private zones only

##### description

[Section titled “description”](#description-9)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-9)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-9)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-9)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-9)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-9)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-9)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "GoogleCloudDns"

[Section titled “@type: "GoogleCloudDns"”](#type-googleclouddns)

Google Cloud DNS

##### serviceAccountJson

[Section titled “serviceAccountJson”](#serviceaccountjson)

Type: [`SecretText`](#secrettext) · required

Service account JSON credentials used to authenticate with Google Cloud

##### projectId

[Section titled “projectId”](#projectid)

Type: `String` · required

The Google Cloud project ID that owns the managed zone

##### managedZone

[Section titled “managedZone”](#managedzone)

Type: `String?`

Managed zone name (resolved automatically by longest suffix match if not set)

##### privateZone

[Section titled “privateZone”](#privatezone)

Type: `Boolean` · default: `false`

Whether to restrict zone resolution to private zones only

##### impersonateServiceAccount

[Section titled “impersonateServiceAccount”](#impersonateserviceaccount)

Type: `String?`

Optional service account email to impersonate

##### description

[Section titled “description”](#description-10)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-10)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-10)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-10)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-10)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-10)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-10)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Alidns"

[Section titled “@type: "Alidns"”](#type-alidns)

Alibaba Cloud DNS

##### accessKey

[Section titled “accessKey”](#accesskey)

Type: `String` · required

The Alibaba Cloud access key ID

##### secretKey

[Section titled “secretKey”](#secretkey)

Type: [`SecretKey`](#secretkey) · required

The Alibaba Cloud access key secret

##### region

[Section titled “region”](#region-1)

Type: `String?`

Optional regional endpoint (defaults to the global endpoint)

##### securityToken

[Section titled “securityToken”](#securitytoken)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional STS security token for temporary credentials

##### line

[Section titled “line”](#line)

Type: `String?`

Optional ISP line identifier (used for split-resolution accounts)

##### description

[Section titled “description”](#description-11)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-11)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-11)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-11)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-11)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-11)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-11)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "ArvanCloud"

[Section titled “@type: "ArvanCloud"”](#type-arvancloud)

ArvanCloud

##### secret

[Section titled “secret”](#secret-5)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-12)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-12)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-12)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-12)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-12)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-12)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-12)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Autodns"

[Section titled “@type: "Autodns"”](#type-autodns)

InterNetX AutoDNS

##### username

[Section titled “username”](#username)

Type: `String` · required

AutoDNS account username

##### password

[Section titled “password”](#password)

Type: [`SecretKey`](#secretkey) · required

AutoDNS account password

##### context

[Section titled “context”](#context)

Type: `UnsignedInt?`

Optional account context identifier

##### description

[Section titled “description”](#description-13)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-13)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-13)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-13)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-13)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-13)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-13)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "AzureDns"

[Section titled “@type: "AzureDns"”](#type-azuredns)

Microsoft Azure DNS

##### tenantId

[Section titled “tenantId”](#tenantid)

Type: `String` · required

Azure Active Directory tenant ID

##### clientId

[Section titled “clientId”](#clientid)

Type: `String` · required

Application (client) ID

##### clientSecret

[Section titled “clientSecret”](#clientsecret)

Type: [`SecretKey`](#secretkey) · required

Application client secret

##### subscriptionId

[Section titled “subscriptionId”](#subscriptionid)

Type: `String` · required

Azure subscription ID that owns the DNS zone

##### resourceGroup

[Section titled “resourceGroup”](#resourcegroup)

Type: `String` · required

Resource group that contains the DNS zone

##### environment

[Section titled “environment”](#environment)

Type: [`AzureEnvironment`](#azureenvironment) · default: `"public"`

Azure cloud environment

##### description

[Section titled “description”](#description-14)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-14)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-14)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-14)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-14)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-14)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-14)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "BaiduCloud"

[Section titled “@type: "BaiduCloud"”](#type-baiducloud)

Baidu Cloud DNS

##### accessKey

[Section titled “accessKey”](#accesskey-1)

Type: `String` · required

Baidu Cloud access key

##### secretKey

[Section titled “secretKey”](#secretkey-1)

Type: [`SecretKey`](#secretkey) · required

Baidu Cloud secret key

##### description

[Section titled “description”](#description-15)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-15)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-15)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-15)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-15)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-15)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-15)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "BluecatV2"

[Section titled “@type: "BluecatV2"”](#type-bluecatv2)

BlueCat Address Manager

##### baseUrl

[Section titled “baseUrl”](#baseurl)

Type: `String` · required

Base URL of the BlueCat Address Manager

##### username

[Section titled “username”](#username-1)

Type: `String` · required

BlueCat account username

##### password

[Section titled “password”](#password-1)

Type: [`SecretKey`](#secretkey) · required

BlueCat account password

##### configName

[Section titled “configName”](#configname)

Type: `String` · required

BlueCat configuration name

##### viewName

[Section titled “viewName”](#viewname)

Type: `String` · required

BlueCat DNS view name

##### skipDeploy

[Section titled “skipDeploy”](#skipdeploy)

Type: `Boolean` · default: `false`

Skip deploying changes after applying them

##### description

[Section titled “description”](#description-16)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-16)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-16)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-16)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-16)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-16)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-16)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "ClouDns"

[Section titled “@type: "ClouDns"”](#type-cloudns)

ClouDNS

##### authId

[Section titled “authId”](#authid)

Type: `String?`

ClouDNS auth ID (use either auth-id or sub-auth-id)

##### subAuthId

[Section titled “subAuthId”](#subauthid)

Type: `String?`

ClouDNS sub-auth ID

##### password

[Section titled “password”](#password-2)

Type: [`SecretKey`](#secretkey) · required

ClouDNS auth password

##### description

[Section titled “description”](#description-17)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-17)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-17)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-17)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-17)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-17)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-17)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Constellix"

[Section titled “@type: "Constellix"”](#type-constellix)

Constellix

##### apiKey

[Section titled “apiKey”](#apikey-2)

Type: `String` · required

Constellix API key

##### secretKey

[Section titled “secretKey”](#secretkey-2)

Type: [`SecretKey`](#secretkey) · required

Constellix secret key

##### description

[Section titled “description”](#description-18)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-18)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-18)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-18)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-18)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-18)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-18)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Cpanel"

[Section titled “@type: "Cpanel"”](#type-cpanel)

cPanel

##### baseUrl

[Section titled “baseUrl”](#baseurl-1)

Type: `String` · required

Base URL of the cPanel server (e.g. [https://host:2083](https://host:2083))

##### username

[Section titled “username”](#username-2)

Type: `String` · required

cPanel account username

##### token

[Section titled “token”](#token)

Type: [`SecretKey`](#secretkey) · required

cPanel API token

##### description

[Section titled “description”](#description-19)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-19)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-19)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-19)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-19)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-19)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-19)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Ddnss"

[Section titled “@type: "Ddnss"”](#type-ddnss)

DDNSS.de

##### secret

[Section titled “secret”](#secret-6)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-20)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-20)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-20)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-20)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-20)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-20)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-20)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "DnsMadeEasy"

[Section titled “@type: "DnsMadeEasy"”](#type-dnsmadeeasy)

DNS Made Easy

##### apiKey

[Section titled “apiKey”](#apikey-3)

Type: `String` · required

DNS Made Easy API key

##### secret

[Section titled “secret”](#secret-7)

Type: [`SecretKey`](#secretkey) · required

DNS Made Easy API secret

##### description

[Section titled “description”](#description-21)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-21)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-21)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-21)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-21)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-21)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-21)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Domeneshop"

[Section titled “@type: "Domeneshop"”](#type-domeneshop)

Domeneshop

##### authToken

[Section titled “authToken”](#authtoken-1)

Type: `String` · required

Domeneshop API token

##### secret

[Section titled “secret”](#secret-8)

Type: [`SecretKey`](#secretkey) · required

Domeneshop API secret

##### description

[Section titled “description”](#description-22)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-22)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-22)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-22)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-22)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-22)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-22)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Dreamhost"

[Section titled “@type: "Dreamhost"”](#type-dreamhost)

Dreamhost

##### secret

[Section titled “secret”](#secret-9)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-23)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-23)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-23)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-23)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-23)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-23)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-23)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "DuckDns"

[Section titled “@type: "DuckDns"”](#type-duckdns)

DuckDNS

##### secret

[Section titled “secret”](#secret-10)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-24)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-24)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-24)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-24)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-24)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-24)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-24)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Dynu"

[Section titled “@type: "Dynu"”](#type-dynu)

Dynu

##### secret

[Section titled “secret”](#secret-11)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-25)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-25)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-25)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-25)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-25)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-25)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-25)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "EasyDns"

[Section titled “@type: "EasyDns"”](#type-easydns)

EasyDNS

##### token

[Section titled “token”](#token-1)

Type: `String` · required

EasyDNS token

##### key

[Section titled “key”](#key-1)

Type: [`SecretKey`](#secretkey) · required

EasyDNS key

##### description

[Section titled “description”](#description-26)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-26)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-26)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-26)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-26)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-26)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-26)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "EdgeDns"

[Section titled “@type: "EdgeDns"”](#type-edgedns)

Akamai EdgeDNS

##### host

[Section titled “host”](#host-1)

Type: `String` · required

Akamai API host

##### clientToken

[Section titled “clientToken”](#clienttoken)

Type: `String` · required

Akamai client token

##### clientSecret

[Section titled “clientSecret”](#clientsecret-1)

Type: [`SecretKey`](#secretkey) · required

Akamai client secret

##### accessToken

[Section titled “accessToken”](#accesstoken)

Type: [`SecretKey`](#secretkey) · required

Akamai access token

##### accountSwitchKey

[Section titled “accountSwitchKey”](#accountswitchkey)

Type: `String?`

Optional account switch key for managing multiple accounts

##### description

[Section titled “description”](#description-27)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-27)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-27)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-27)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-27)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-27)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-27)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Exoscale"

[Section titled “@type: "Exoscale"”](#type-exoscale)

Exoscale

##### apiKey

[Section titled “apiKey”](#apikey-4)

Type: `String` · required

Exoscale API key

##### secret

[Section titled “secret”](#secret-12)

Type: [`SecretKey`](#secretkey) · required

Exoscale API secret

##### description

[Section titled “description”](#description-28)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-28)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-28)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-28)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-28)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-28)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-28)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "FreeMyIp"

[Section titled “@type: "FreeMyIp"”](#type-freemyip)

freemyip.com

##### secret

[Section titled “secret”](#secret-13)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-29)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-29)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-29)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-29)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-29)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-29)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-29)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "GandiV5"

[Section titled “@type: "GandiV5"”](#type-gandiv5)

Gandi LiveDNS v5

##### secret

[Section titled “secret”](#secret-14)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-30)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-30)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-30)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-30)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-30)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-30)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-30)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Gcore"

[Section titled “@type: "Gcore"”](#type-gcore)

Gcore

##### secret

[Section titled “secret”](#secret-15)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-31)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-31)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-31)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-31)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-31)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-31)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-31)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Glesys"

[Section titled “@type: "Glesys"”](#type-glesys)

GleSYS

##### apiUser

[Section titled “apiUser”](#apiuser)

Type: `String` · required

GleSYS API user

##### apiKey

[Section titled “apiKey”](#apikey-5)

Type: [`SecretKey`](#secretkey) · required

GleSYS API key

##### description

[Section titled “description”](#description-32)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-32)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-32)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-32)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-32)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-32)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-32)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Godaddy"

[Section titled “@type: "Godaddy"”](#type-godaddy)

GoDaddy

##### apiKey

[Section titled “apiKey”](#apikey-6)

Type: `String` · required

GoDaddy API key

##### secret

[Section titled “secret”](#secret-16)

Type: [`SecretKey`](#secretkey) · required

GoDaddy API secret

##### description

[Section titled “description”](#description-33)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-33)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-33)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-33)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-33)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-33)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-33)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Hetzner"

[Section titled “@type: "Hetzner"”](#type-hetzner)

Hetzner

##### secret

[Section titled “secret”](#secret-17)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-34)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-34)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-34)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-34)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-34)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-34)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-34)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "HostingDe"

[Section titled “@type: "HostingDe"”](#type-hostingde)

hosting.de

##### secret

[Section titled “secret”](#secret-18)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-35)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-35)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-35)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-35)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-35)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-35)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-35)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Hostinger"

[Section titled “@type: "Hostinger"”](#type-hostinger)

Hostinger

##### secret

[Section titled “secret”](#secret-19)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-36)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-36)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-36)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-36)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-36)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-36)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-36)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "HuaweiCloud"

[Section titled “@type: "HuaweiCloud"”](#type-huaweicloud)

Huawei Cloud DNS

##### accessKey

[Section titled “accessKey”](#accesskey-2)

Type: `String` · required

Huawei Cloud access key

##### secretKey

[Section titled “secretKey”](#secretkey-3)

Type: [`SecretKey`](#secretkey) · required

Huawei Cloud secret key

##### region

[Section titled “region”](#region-2)

Type: `String` · default: `"ap-southeast-1"`

Huawei Cloud region

##### description

[Section titled “description”](#description-37)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-37)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-37)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-37)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-37)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-37)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-37)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Hurricane"

[Section titled “@type: "Hurricane"”](#type-hurricane)

Hurricane Electric

##### credentials

[Section titled “credentials”](#credentials)

Type: [`HurricaneCredential`](#hurricanecredential)`[]` · min items: 1

Per-zone Hurricane Electric DDNS keys

##### description

[Section titled “description”](#description-38)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-38)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-38)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-38)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-38)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-38)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-38)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "IbmCloud"

[Section titled “@type: "IbmCloud"”](#type-ibmcloud)

IBM Cloud

##### username

[Section titled “username”](#username-3)

Type: `String` · required

IBM Cloud account username

##### apiKey

[Section titled “apiKey”](#apikey-7)

Type: [`SecretKey`](#secretkey) · required

IBM Cloud API key

##### description

[Section titled “description”](#description-39)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-39)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-39)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-39)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-39)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-39)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-39)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Infoblox"

[Section titled “@type: "Infoblox"”](#type-infoblox)

Infoblox NIOS WAPI

##### host

[Section titled “host”](#host-2)

Type: `String` · required

Infoblox grid master host

##### port

[Section titled “port”](#port-1)

Type: `String?`

Optional port (defaults to 443)

##### username

[Section titled “username”](#username-4)

Type: `String` · required

Infoblox account username

##### password

[Section titled “password”](#password-3)

Type: [`SecretKey`](#secretkey) · required

Infoblox account password

##### wapiVersion

[Section titled “wapiVersion”](#wapiversion)

Type: `String?`

WAPI version to use (defaults to 2.11)

##### dnsView

[Section titled “dnsView”](#dnsview)

Type: `String?`

DNS view name (defaults to External)

##### description

[Section titled “description”](#description-40)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-40)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-40)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-40)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-40)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-40)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-40)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Infomaniak"

[Section titled “@type: "Infomaniak"”](#type-infomaniak)

Infomaniak

##### secret

[Section titled “secret”](#secret-20)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-41)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-41)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-41)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-41)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-41)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-41)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-41)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Inwx"

[Section titled “@type: "Inwx"”](#type-inwx)

INWX

##### username

[Section titled “username”](#username-5)

Type: `String` · required

INWX account username

##### password

[Section titled “password”](#password-4)

Type: [`SecretKey`](#secretkey) · required

INWX account password

##### sharedSecret

[Section titled “sharedSecret”](#sharedsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional shared secret for TOTP-based two-factor authentication

##### sandbox

[Section titled “sandbox”](#sandbox)

Type: `Boolean` · default: `false`

Use the INWX sandbox API instead of production

##### description

[Section titled “description”](#description-42)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-42)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-42)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-42)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-42)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-42)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-42)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Ionos"

[Section titled “@type: "Ionos"”](#type-ionos)

IONOS

##### secret

[Section titled “secret”](#secret-21)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-43)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-43)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-43)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-43)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-43)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-43)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-43)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Ipv64"

[Section titled “@type: "Ipv64"”](#type-ipv64)

IPv64

##### secret

[Section titled “secret”](#secret-22)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-44)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-44)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-44)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-44)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-44)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-44)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-44)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Joker"

[Section titled “@type: "Joker"”](#type-joker)

Joker

##### auth

[Section titled “auth”](#auth)

Type: [`JokerAuth`](#jokerauth) · required

Joker authentication method

##### description

[Section titled “description”](#description-45)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-45)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-45)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-45)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-45)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-45)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-45)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Lightsail"

[Section titled “@type: "Lightsail"”](#type-lightsail)

AWS Lightsail

##### accessKeyId

[Section titled “accessKeyId”](#accesskeyid-1)

Type: `String` · required

AWS access key ID

##### secretAccessKey

[Section titled “secretAccessKey”](#secretaccesskey-1)

Type: [`SecretKey`](#secretkey) · required

AWS secret access key

##### sessionToken

[Section titled “sessionToken”](#sessiontoken-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional session token for temporary AWS credentials

##### region

[Section titled “region”](#region-3)

Type: `String?`

AWS region (defaults to us-east-1)

##### domain

[Section titled “domain”](#domain)

Type: `String?`

Optional Lightsail domain name to scope record operations to

##### description

[Section titled “description”](#description-46)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-46)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-46)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-46)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-46)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-46)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-46)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Linode"

[Section titled “@type: "Linode"”](#type-linode)

Linode

##### secret

[Section titled “secret”](#secret-23)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-47)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-47)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-47)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-47)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-47)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-47)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-47)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "LuaDns"

[Section titled “@type: "LuaDns"”](#type-luadns)

LuaDNS

##### username

[Section titled “username”](#username-6)

Type: `String` · required

LuaDNS account email or username

##### authToken

[Section titled “authToken”](#authtoken-2)

Type: [`SecretKey`](#secretkey) · required

LuaDNS API token

##### description

[Section titled “description”](#description-48)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-48)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-48)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-48)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-48)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-48)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-48)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "MythicBeasts"

[Section titled “@type: "MythicBeasts"”](#type-mythicbeasts)

Mythic Beasts

##### username

[Section titled “username”](#username-7)

Type: `String` · required

Mythic Beasts API key ID

##### password

[Section titled “password”](#password-5)

Type: [`SecretKey`](#secretkey) · required

Mythic Beasts API key secret

##### description

[Section titled “description”](#description-49)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-49)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-49)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-49)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-49)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-49)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-49)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Namecheap"

[Section titled “@type: "Namecheap"”](#type-namecheap)

Namecheap

##### apiUser

[Section titled “apiUser”](#apiuser-1)

Type: `String` · required

Namecheap API user

##### apiKey

[Section titled “apiKey”](#apikey-8)

Type: [`SecretKey`](#secretkey) · required

Namecheap API key

##### clientIp

[Section titled “clientIp”](#clientip)

Type: `String` · required

Whitelisted client IP address registered with Namecheap

##### username

[Section titled “username”](#username-8)

Type: `String?`

Optional account username (defaults to the API user)

##### description

[Section titled “description”](#description-50)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-50)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-50)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-50)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-50)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-50)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-50)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "NameDotCom"

[Section titled “@type: "NameDotCom"”](#type-namedotcom)

Name.com

##### username

[Section titled “username”](#username-9)

Type: `String` · required

Name.com account username

##### authToken

[Section titled “authToken”](#authtoken-3)

Type: [`SecretKey`](#secretkey) · required

Name.com API token

##### description

[Section titled “description”](#description-51)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-51)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-51)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-51)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-51)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-51)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-51)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "NameSilo"

[Section titled “@type: "NameSilo"”](#type-namesilo)

NameSilo

##### secret

[Section titled “secret”](#secret-24)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-52)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-52)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-52)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-52)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-52)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-52)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-52)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Netcup"

[Section titled “@type: "Netcup"”](#type-netcup)

Netcup

##### customerNumber

[Section titled “customerNumber”](#customernumber)

Type: `String` · required

Netcup customer number

##### apiKey

[Section titled “apiKey”](#apikey-9)

Type: `String` · required

Netcup API key

##### password

[Section titled “password”](#password-6)

Type: [`SecretKey`](#secretkey) · required

Netcup API password

##### description

[Section titled “description”](#description-53)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-53)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-53)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-53)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-53)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-53)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-53)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Netlify"

[Section titled “@type: "Netlify"”](#type-netlify)

Netlify

##### secret

[Section titled “secret”](#secret-25)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-54)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-54)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-54)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-54)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-54)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-54)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-54)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Nifcloud"

[Section titled “@type: "Nifcloud"”](#type-nifcloud)

Nifcloud

##### accessKey

[Section titled “accessKey”](#accesskey-3)

Type: `String` · required

Nifcloud access key

##### secretKey

[Section titled “secretKey”](#secretkey-4)

Type: [`SecretKey`](#secretkey) · required

Nifcloud secret key

##### description

[Section titled “description”](#description-55)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-55)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-55)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-55)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-55)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-55)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-55)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Ns1"

[Section titled “@type: "Ns1"”](#type-ns1)

NS1

##### secret

[Section titled “secret”](#secret-26)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-56)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-56)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-56)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-56)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-56)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-56)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-56)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "OracleCloud"

[Section titled “@type: "OracleCloud"”](#type-oraclecloud)

Oracle Cloud

##### tenancyOcid

[Section titled “tenancyOcid”](#tenancyocid)

Type: `String` · required

Tenancy OCID

##### userOcid

[Section titled “userOcid”](#userocid)

Type: `String` · required

User OCID

##### fingerprint

[Section titled “fingerprint”](#fingerprint)

Type: `String` · required

API signing key fingerprint

##### privateKeyPem

[Section titled “privateKeyPem”](#privatekeypem)

Type: [`SecretText`](#secrettext) · required

API signing private key in PEM format

##### privateKeyPassword

[Section titled “privateKeyPassword”](#privatekeypassword)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional passphrase for the private key

##### region

[Section titled “region”](#region-4)

Type: `String` · required

OCI region (e.g. us-ashburn-1)

##### compartmentOcid

[Section titled “compartmentOcid”](#compartmentocid)

Type: `String` · required

Compartment OCID that owns the DNS zone

##### description

[Section titled “description”](#description-57)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-57)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-57)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-57)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-57)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-57)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-57)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Plesk"

[Section titled “@type: "Plesk"”](#type-plesk)

Plesk

##### baseUrl

[Section titled “baseUrl”](#baseurl-2)

Type: `String` · required

Base URL of the Plesk server (e.g. [https://host:8443](https://host:8443))

##### apiKey

[Section titled “apiKey”](#apikey-10)

Type: [`SecretKey`](#secretkey) · required

Plesk API key

##### description

[Section titled “description”](#description-58)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-58)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-58)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-58)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-58)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-58)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-58)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Safedns"

[Section titled “@type: "Safedns"”](#type-safedns)

ANS SafeDNS

##### secret

[Section titled “secret”](#secret-27)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-59)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-59)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-59)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-59)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-59)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-59)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-59)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Scaleway"

[Section titled “@type: "Scaleway"”](#type-scaleway)

Scaleway

##### secret

[Section titled “secret”](#secret-28)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-60)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-60)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-60)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-60)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-60)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-60)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-60)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "TencentCloud"

[Section titled “@type: "TencentCloud"”](#type-tencentcloud)

Tencent Cloud DNSPod

##### secretId

[Section titled “secretId”](#secretid)

Type: `String` · required

Tencent Cloud secret ID

##### secretKey

[Section titled “secretKey”](#secretkey-5)

Type: [`SecretKey`](#secretkey) · required

Tencent Cloud secret key

##### region

[Section titled “region”](#region-5)

Type: `String?`

Optional regional endpoint

##### sessionToken

[Section titled “sessionToken”](#sessiontoken-2)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional STS session token for temporary credentials

##### description

[Section titled “description”](#description-61)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-61)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-61)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-61)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-61)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-61)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-61)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Transip"

[Section titled “@type: "Transip"”](#type-transip)

TransIP

##### username

[Section titled “username”](#username-10)

Type: `String` · required

TransIP account login

##### privateKeyPem

[Section titled “privateKeyPem”](#privatekeypem-1)

Type: [`SecretText`](#secrettext) · required

TransIP private key in PEM format

##### description

[Section titled “description”](#description-62)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-62)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-62)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-62)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-62)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-62)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-62)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "UltraDns"

[Section titled “@type: "UltraDns"”](#type-ultradns)

UltraDNS

##### username

[Section titled “username”](#username-11)

Type: `String` · required

UltraDNS account username

##### password

[Section titled “password”](#password-7)

Type: [`SecretKey`](#secretkey) · required

UltraDNS account password

##### endpoint

[Section titled “endpoint”](#endpoint)

Type: `String?`

Optional REST API endpoint override

##### description

[Section titled “description”](#description-63)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-63)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-63)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-63)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-63)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-63)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-63)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Vercel"

[Section titled “@type: "Vercel"”](#type-vercel)

Vercel

##### authToken

[Section titled “authToken”](#authtoken-4)

Type: [`SecretKey`](#secretkey) · required

Vercel auth token

##### teamId

[Section titled “teamId”](#teamid)

Type: `String?`

Optional team ID to scope API requests to

##### description

[Section titled “description”](#description-64)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-64)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-64)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-64)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-64)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-64)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-64)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Volcengine"

[Section titled “@type: "Volcengine"”](#type-volcengine)

Volcano Engine

##### accessKey

[Section titled “accessKey”](#accesskey-4)

Type: `String` · required

Volcengine access key

##### secretKey

[Section titled “secretKey”](#secretkey-6)

Type: [`SecretKey`](#secretkey) · required

Volcengine secret key

##### region

[Section titled “region”](#region-6)

Type: `String?`

Optional regional endpoint

##### host

[Section titled “host”](#host-3)

Type: `String?`

Optional API host override

##### scheme

[Section titled “scheme”](#scheme)

Type: `String?`

Optional URL scheme (http or https)

##### description

[Section titled “description”](#description-65)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-65)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-65)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-65)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-65)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-65)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-65)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "Vultr"

[Section titled “@type: "Vultr"”](#type-vultr)

Vultr

##### secret

[Section titled “secret”](#secret-29)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-66)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-66)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-66)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-66)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-66)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-66)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-66)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "WebSupport"

[Section titled “@type: "WebSupport"”](#type-websupport)

WebSupport

##### apiKey

[Section titled “apiKey”](#apikey-11)

Type: `String` · required

WebSupport API key

##### secret

[Section titled “secret”](#secret-30)

Type: [`SecretKey`](#secretkey) · required

WebSupport API secret

##### description

[Section titled “description”](#description-67)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-67)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-67)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-67)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-67)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-67)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-67)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

### @type: "YandexCloud"

[Section titled “@type: "YandexCloud"”](#type-yandexcloud)

Yandex Cloud

##### apiKey

[Section titled “apiKey”](#apikey-12)

Type: [`SecretText`](#secrettext) · required

Base64-encoded IAM service account key JSON

##### folderId

[Section titled “folderId”](#folderid)

Type: `String` · required

Yandex Cloud folder ID that owns the DNS zone

##### description

[Section titled “description”](#description-68)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-68)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-68)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-68)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-68)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-68)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-68)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The DnsServer object is available via the `urn:stalwart:jmap` capability.

### x:DnsServer/get

[Section titled “x:DnsServer/get”](#xdnsserverget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysDnsServerGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DnsServer/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DnsServer/set

[Section titled “x:DnsServer/set”](#xdnsserverset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysDnsServerCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DnsServer/set",          {            "create": {              "new1": {                "@type": "Tsig",                "description": "Example",                "host": "192.0.2.1",                "key": {                  "@type": "Value",                  "secret": "Example"                },                "keyName": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysDnsServerUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DnsServer/set",          {            "update": {              "id1": {                "keyName": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysDnsServerDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DnsServer/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DnsServer/query

[Section titled “x:DnsServer/query”](#xdnsserverquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysDnsServerQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DnsServer/query",          {            "filter": {              "memberTenantId": "id1"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:DnsServer/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`memberTenantId`id of Tenant

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get DnsServer id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create DnsServer/Tsig \  --field host=192.0.2.1 \  --field keyName=Example \  --field 'key={"@type":"Value","secret":"Example"}' \  --field description=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query DnsServerstalwart-cli query DnsServer --where memberTenantId=id1
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update DnsServer id1 --field keyName='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete DnsServer --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SecretKey

[Section titled “SecretKey”](#secretkey-7)

A secret value provided directly, from an environment variable, or from a file.

`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret-31)

Type: `String` · required · secret

Password or secret value

#### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

#### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

### SecretText

[Section titled “SecretText”](#secrettext)

A secret text value provided directly, from an environment variable, or from a file.

`Text`: Secret value. Carries the fields of [`SecretTextValue`](#secrettextvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretTextValue

[Section titled “SecretTextValue”](#secrettextvalue)

A secret text value provided directly.

##### secret

[Section titled “secret”](#secret-32)

Type: `Text` · required · secret

Password or secret value

### HurricaneCredential

[Section titled “HurricaneCredential”](#hurricanecredential)

Hurricane Electric per-zone DDNS credential.

##### zone

[Section titled “zone”](#zone)

Type: `DomainName` · required

DNS zone (origin) the credential applies to

##### secret

[Section titled “secret”](#secret-33)

Type: [`SecretKey`](#secretkey) · required

DDNS key for the zone

### JokerAuth

[Section titled “JokerAuth”](#jokerauth)

Joker DMAPI authentication credentials.

`ApiKey`: API Key. Carries the fields of [`JokerAuthApiKey`](#jokerauthapikey).
`UsernamePassword`: Username and Password. Carries the fields of [`JokerAuthUsernamePassword`](#jokerauthusernamepassword).

#### JokerAuthApiKey

[Section titled “JokerAuthApiKey”](#jokerauthapikey)

Joker API key authentication.

##### apiKey

[Section titled “apiKey”](#apikey-13)

Type: [`SecretKey`](#secretkey) · required

Joker DMAPI API key

#### JokerAuthUsernamePassword

[Section titled “JokerAuthUsernamePassword”](#jokerauthusernamepassword)

Joker username/password authentication.

##### username

[Section titled “username”](#username-12)

Type: `String` · required

Joker DMAPI account username

##### password

[Section titled “password”](#password-8)

Type: [`SecretKey`](#secretkey) · required

Joker DMAPI account password

## Enums

[Section titled “Enums”](#enums)

### IpProtocol

[Section titled “IpProtocol”](#ipprotocol)
ValueLabel`udp`UDP`tcp`TCP

### TsigAlgorithm

[Section titled “TsigAlgorithm”](#tsigalgorithm-1)
ValueLabel`hmac-md5`HMAC-MD5`gss`GSS`hmac-sha1`HMAC-SHA1`hmac-sha224`HMAC-SHA224`hmac-sha256`HMAC-SHA256`hmac-sha256-128`HMAC-SHA256-128`hmac-sha384`HMAC-SHA384`hmac-sha384-192`HMAC-SHA384-192`hmac-sha512`HMAC-SHA512`hmac-sha512-256`HMAC-SHA512-256

### OvhEndpoint

[Section titled “OvhEndpoint”](#ovhendpoint-1)
ValueLabel`ovh-eu`OVH EU`ovh-ca`OVH CA`kimsufi-eu`Kimsufi EU`kimsufi-ca`Kimsufi CA`soyoustart-eu`Soyoustart EU`soyoustart-ca`Soyoustart CA

### AzureEnvironment

[Section titled “AzureEnvironment”](#azureenvironment)
ValueLabel`public`Public`china`China`us-government`US Government