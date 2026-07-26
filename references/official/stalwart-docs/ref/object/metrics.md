---
title: "Metrics"
source: "https://stalw.art/docs/ref/object/metrics"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/metrics"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Metrics"
---
# Metrics

      

Configures metrics collection and export via OpenTelemetry and Prometheus.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Telemetry › Metrics › General Settings ›  Telemetry › Metrics › OpenTelemetry Settings ›  Telemetry › Metrics › Prometheus

## Fields

[Section titled “Fields”](#fields)

##### openTelemetry

[Section titled “openTelemetry”](#opentelemetry)

Type: [`MetricsOtel`](#metricsotel) · required

OpenTelemetry metrics configuration

##### prometheus

[Section titled “prometheus”](#prometheus)

Type: [`MetricsPrometheus`](#metricsprometheus) · required

Prometheus metrics endpoint configuration

##### metrics

[Section titled “metrics”](#metrics)

Type: [[ref/metrics|`MetricType`]]`[]`

List of metrics to include or exclude based on filter mode

##### metricsPolicy

[Section titled “metricsPolicy”](#metricspolicy)

Type: [`EventPolicy`](#eventpolicy) · default: `"exclude"`

How to interpret the metrics list

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Metrics singleton is available via the `urn:stalwart:jmap` capability.

### x:Metrics/get

[Section titled “x:Metrics/get”](#xmetricsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMetricsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Metrics/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Metrics/set

[Section titled “x:Metrics/set”](#xmetricsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMetricsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Metrics/set",          {            "update": {              "singleton": {                "openTelemetry": {                  "@type": "Disabled"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Metrics
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Metrics --field openTelemetry='{"@type":"Disabled"}'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### MetricsOtel

[Section titled “MetricsOtel”](#metricsotel)

OpenTelemetry metrics configuration.

`Disabled`: Disabled. No additional fields.
`Http`: HTTP. Carries the fields of [`MetricsOtelHttp`](#metricsotelhttp).
`Grpc`: gRPC. Carries the fields of [`MetricsOtelGrpc`](#metricsotelgrpc).

#### MetricsOtelHttp

[Section titled “MetricsOtelHttp”](#metricsotelhttp)

OpenTelemetry HTTP metrics exporter.

##### endpoint

[Section titled “endpoint”](#endpoint)

Type: `Uri` · required

The endpoint for Open Telemetry

##### interval

[Section titled “interval”](#interval)

Type: `Duration` · default: `"1m"`

The minimum amount of time that must pass between each push request to the OpenTelemetry endpoint

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"10s"`

Maximum amount of time that Stalwart will wait for a response from the OpenTelemetry endpoint

##### httpAuth

[Section titled “httpAuth”](#httpauth)

Type: [`HttpAuth`](#httpauth) · required

The type of HTTP authentication to use

##### httpHeaders

[Section titled “httpHeaders”](#httpheaders)

Type: `Map<String, String>`

Additional headers to include in HTTP requests

##### HttpAuth

[Section titled “HttpAuth”](#httpauth-1)

Defines the HTTP authentication method to use for HTTP requests.

`Unauthenticated`: Anonymous. No additional fields.
`Basic`: Basic Authentication. Carries the fields of [`HttpAuthBasic`](#httpauthbasic).
`Bearer`: Bearer Token. Carries the fields of [`HttpAuthBearer`](#httpauthbearer).

##### HttpAuthBasic

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

##### HttpAuthBearer

[Section titled “HttpAuthBearer”](#httpauthbearer)

HTTP Bearer token authentication.

##### bearerToken

[Section titled “bearerToken”](#bearertoken)

Type: [`SecretKey`](#secretkey) · required

Bearer token for HTTP Bearer Authentication

#### MetricsOtelGrpc

[Section titled “MetricsOtelGrpc”](#metricsotelgrpc)

OpenTelemetry gRPC metrics exporter.

##### endpoint

[Section titled “endpoint”](#endpoint-1)

Type: `Uri?`

The endpoint for Open Telemetry

##### interval

[Section titled “interval”](#interval-1)

Type: `Duration` · default: `"1m"`

The minimum amount of time that must pass between each push request to the OpenTelemetry endpoint

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration` · default: `"10s"`

Maximum amount of time that Stalwart will wait for a response from the OpenTelemetry endpoint

### MetricsPrometheus

[Section titled “MetricsPrometheus”](#metricsprometheus)

Prometheus metrics endpoint configuration.

`Disabled`: Disabled. No additional fields.
`Enabled`: Enabled. Carries the fields of [`MetricsPrometheusProperties`](#metricsprometheusproperties).

#### MetricsPrometheusProperties

[Section titled “MetricsPrometheusProperties”](#metricsprometheusproperties)

Prometheus metrics endpoint authentication.

##### authSecret

[Section titled “authSecret”](#authsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

The Prometheus endpoint’s secret for Basic authentication

##### authUsername

[Section titled “authUsername”](#authusername)

Type: `String?`

The Prometheus endpoint’s username for Basic authentication

##### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

## Enums

[Section titled “Enums”](#enums)

### EventPolicy

[Section titled “EventPolicy”](#eventpolicy)
ValueLabel`include`Only include the specified events`exclude`Exclude the specified events