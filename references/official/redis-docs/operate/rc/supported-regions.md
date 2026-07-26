---
title: "Supported Cloud providers and regions"
source: "https://redis.io/docs/latest/operate/rc/supported-regions/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/supported-regions"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Supported Cloud providers and regions"
---
# Supported Cloud providers and regions

            Redis Cloud
          

Your choice of cloud provider and region may affect latency between your application and your database, and may affect what connectivity options are available for your database.

Redis Cloud supports databases on the following cloud providers:

[Amazon Web Services (AWS)](#amazon-web-services)
[Google Cloud](#google-cloud)
[Microsoft Azure](#microsoft-azure)

## Amazon Web Services

Redis Cloud supports databases in the following Amazon Web Services (AWS) regions.

Redis Cloud Pro databases on AWS support [[operate/rc/security/vpc-peering#aws-vpc-peering|VPC Peering]], [[operate/rc/security/aws-transit-gateway|Transit Gateway]], and [[operate/rc/security/aws-privatelink|AWS PrivateLink]].

        Americas
      

        Europe
      

        Asia Pacific
      

        Middle East and Africa
      

Region ID
Location
Redis Cloud Pro
Redis Cloud Essentials

`ca-central-1`
Montreal, Canada
✅
✅

`ca-west-1`
Calgary, Canada
✅
❌

`mx-central-1`
Central Mexico
✅
❌

`sa-east-1`
Sao Paulo, Brazil
✅
✅

`us-east-1`
Northern Virginia, USA
✅
✅

`us-east-2`
Ohio, USA
✅
✅

`us-west-1`
Northern California, USA
✅
✅

`us-west-2`
Oregon, USA
✅
✅

Region ID
Location
Redis Cloud Pro
Redis Cloud Essentials

`eu-central-1`
Frankfurt, Germany
✅
✅

`eu-central-2`
Zurich, Switzerland
✅
❌

`eu-north-1`
Stockholm, Sweden
✅
✅

`eu-south-1`
Milan, Italy
✅
❌

`eu-south-2`
Spain
✅
❌

`eu-west-1`
Ireland
✅
✅

`eu-west-2`
London, UK
✅
✅

`eu-west-3`
Paris, France
✅
✅

Region ID
Location
Redis Cloud Pro
Redis Cloud Essentials

`ap-east-1`
Hong Kong, China
✅
✅

`ap-east-2`
Taipei, Taiwan
✅
❌

`ap-northeast-1`
Tokyo, Japan
✅
✅

`ap-northeast-2`
Seoul, South Korea
✅
✅

`ap-northeast-3`
Osaka, Japan
✅
❌

`ap-south-1`
Mumbai, India
✅
✅

`ap-south-2`
Hyderabad, India
✅
❌

`ap-southeast-1`
Singapore
✅
✅

`ap-southeast-2`
Sydney, Australia
✅
✅

`ap-southeast-3`
Jakarta, Indonesia
✅
❌

`ap-southeast-4`
Melbourne, Australia
✅
❌

`ap-southeast-5`
Malaysia
✅
❌

`ap-southeast-6`
New Zealand
✅
❌

`ap-southeast-7`
Thailand
✅
❌

Region ID
Location
Redis Cloud Pro
Redis Cloud Essentials

`af-south-1`
Cape Town, South Africa
✅
✅

`il-central-1`
Tel Aviv, Israel
✅
❌

`me-central-1`
UAE
✅
✅

`me-south-1`
Bahrain
✅
✅

## Google Cloud

Redis Cloud supports databases in the following Google Cloud regions.

Redis Cloud Pro databases on Google Cloud support [[operate/rc/security/vpc-peering#gcp-vpc-peering|VPC Peering]] and [[operate/rc/security/private-service-connect|Private Service Connect]].

        Americas
      

        Europe
      

        Asia Pacific
      

        Middle East and Africa
      

Region ID
Location
Redis Cloud Pro
Redis Cloud Essentials

`northamerica-northeast1`
Montreal, Canada
✅
❌

`northamerica-northeast2`
Toronto, Canada
✅
❌

`northamerica-south1`
Mexico
✅
❌

`southamerica-east1`
Sao Paulo, Brazil
✅
✅

`southamerica-west1`
Santiago, Chile
✅
❌

`us-central1`
Iowa, USA
✅
✅

`us-east1`
South Carolina, USA
✅
✅

`us-east4`
Virginia, USA
✅
✅

`us-east5`
Columbus, USA
✅
❌

`us-south1`
Dallas, USA
✅
❌

`us-west1`
Oregon, USA
✅
✅

`us-west2`
Los Angeles, USA
✅
✅

`us-west3`
Salt Lake City, USA
✅
❌

`us-west4`
Las Vegas, USA
✅
❌

Region ID
Location
Redis Cloud Pro
Redis Cloud Essentials

`europe-central2`
Warsaw, Poland
✅
❌

`europe-north1`
Finland
✅
❌

`europe-north2`
Stockholm, Sweden
✅
❌

`europe-southwest1`
Madrid, Spain
✅
❌

`europe-west1`
Belgium
✅
✅

`europe-west10`
Berlin, Germany
✅
❌

`europe-west12`
Turin, Italy
✅
❌

`europe-west2`
London, UK
✅
✅

`europe-west3`
Frankfurt, Germany
✅
✅

`europe-west4`
Netherlands
✅
✅

`europe-west6`
Zurich, Switzerland
✅
❌

`europe-west8`
Milan, Italy
✅
❌

`europe-west9`
Paris, France
✅
❌

Region ID
Location
Redis Cloud Pro
Redis Cloud Essentials

`asia-east1`
Taiwan
✅
✅

`asia-east2`
Hong Kong, China
✅
❌

`asia-northeast1`
Tokyo, Japan
✅
✅

`asia-northeast2`
Osaka, Japan
✅
❌

`asia-northeast3`
Seoul, South Korea
✅
❌

`asia-south1`
Mumbai, India
✅
✅

`asia-south2`
Delhi, India
✅
❌

`asia-southeast1`
Singapore
✅
✅

`asia-southeast2`
Jakarta, Indonesia
✅
✅

`australia-southeast1`
Sydney, Australia
✅
✅

`australia-southeast2`
Melbourne, Australia
✅
❌

Region ID
Location
Redis Cloud Pro
Redis Cloud Essentials

`africa-south1`
Johannesburg, South Africa
✅
❌

`me-central1`
Doha, Qatar
✅
❌

`me-central2`
Dammam, Saudi Arabia
✅
❌

`me-west1`
Tel Aviv, Israel
✅
❌

## Microsoft Azure

Redis Cloud Essentials is available on the following Microsoft Azure regions:

Region ID
Location

`east-us`
Virginia, USA

`west-us`
California, USA

Note:
  Redis Cloud Pro is available on Azure through [Azure Managed Redis](https://azure.microsoft.com/en-us/products/managed-redis/). See [Azure Managed Redis pricing](https://azure.microsoft.com/en-us/pricing/details/managed-redis/) to view the list of Azure regions that support Azure Managed Redis.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page