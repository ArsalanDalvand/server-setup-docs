---
title: "Metric"
source: "https://stalw.art/docs/ref/object/metric"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/metric"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Metric"
---
# Metric

      

Stores a collected server metric data point.

## Fields

[Section titled “Fields”](#fields)

Metric is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Counter"

[Section titled “@type: "Counter"”](#type-counter)

Counter metric

##### count

[Section titled “count”](#count)

Type: `UnsignedInt` · default: `0`

Count associated with the metric

##### metric

[Section titled “metric”](#metric)

Type: [[ref/metrics|`MetricType`]] · required

Metric event type

##### timestamp

[Section titled “timestamp”](#timestamp)

Type: `UTCDateTime` · required

Timestamp of the metric entry

### @type: "Gauge"

[Section titled “@type: "Gauge"”](#type-gauge)

Gauge metric

##### count

[Section titled “count”](#count-1)

Type: `UnsignedInt` · default: `0`

Count associated with the metric

##### metric

[Section titled “metric”](#metric-1)

Type: [[ref/metrics|`MetricType`]] · required

Metric event type

##### timestamp

[Section titled “timestamp”](#timestamp-1)

Type: `UTCDateTime` · required

Timestamp of the metric entry

### @type: "Histogram"

[Section titled “@type: "Histogram"”](#type-histogram)

Histogram metric

##### count

[Section titled “count”](#count-2)

Type: `UnsignedInt` · default: `0`

Value associated with the metric

##### sum

[Section titled “sum”](#sum)

Type: `UnsignedInt` · default: `0`

Sum associated with the metric

##### metric

[Section titled “metric”](#metric-2)

Type: [[ref/metrics|`MetricType`]] · required

Metric event type

##### timestamp

[Section titled “timestamp”](#timestamp-2)

Type: `UTCDateTime` · required

Timestamp of the metric entry

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Metric object is available via the `urn:stalwart:jmap` capability.

### x:Metric/get

[Section titled “x:Metric/get”](#xmetricget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMetricGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Metric/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Metric/set

[Section titled “x:Metric/set”](#xmetricset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMetricCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Metric/set",          {            "create": {              "new1": {                "@type": "Counter"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMetricUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Metric/set",          {            "update": {              "id1": {                "count": 0              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMetricDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Metric/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Metric/query

[Section titled “x:Metric/query”](#xmetricquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMetricQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Metric/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Metric id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Metric/Counter
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Metric
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Metric id1 --field count=0
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Metric --ids id1
```