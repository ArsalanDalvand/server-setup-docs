---
title: "AWS CLI with MinIO Server — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/integrations/aws-cli-with-minio.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrations/aws-cli-with-minio"
category: "integrations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "AWS CLI with MinIO Server — SILO (MinIO Object Storage)"
---
# AWS CLI with MinIO Server

AWS CLI is a unified tool to manage AWS services. It is frequently the tool used to transfer data in and out of AWS S3. It works with any S3 compatible cloud storage service.

In this recipe we will learn how to configure and use AWS CLI to manage data with MinIO Server.

## 1. Prerequisites

Install MinIO Server from [here](https://silo.pigsty.io/operations/deployments/installation.html).

## 2. Installation

Install AWS CLI from [https://aws.amazon.com/cli/](https://aws.amazon.com/cli/)

## 3. Configuration

To configure AWS CLI, type `aws configure` and specify the MinIO key information.

Access credentials shown in this example belong to [https://play.min.io:9000](https://play.min.io:9000).
These credentials are open to public. Feel free to use this service for testing and development. Replace with your own MinIO keys in deployment.

```
aws configure
AWS Access Key ID [None]: Q3AM3UQ867SPQQA43P2F
AWS Secret Access Key [None]: zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG
Default region name [None]: us-east-1
Default output format [None]: ENTER
```

Additionally enable AWS Signature Version ‘4’ for MinIO server.

```
aws configure set default.s3.signature_version s3v4
```

## 4. Commands

### To list your buckets

```
aws --endpoint-url https://play.min.io:9000 s3 ls
2016-03-27 02:06:30 deebucket
2016-03-28 21:53:49 guestbucket
2016-03-29 13:34:34 mbtest
2016-03-26 22:01:36 mybucket
2016-03-26 15:37:02 testbucket
```

### To list contents inside bucket

```
aws --endpoint-url https://play.min.io:9000 s3 ls s3://mybucket
2016-03-30 00:26:53      69297 argparse-1.2.1.tar.gz
2016-03-30 00:35:37      67250 simplejson-3.3.0.tar.gz
```

### To make a bucket

```
aws --endpoint-url https://play.min.io:9000 s3 mb s3://mybucket
make_bucket: s3://mybucket/
```

### To add an object to a bucket

```
aws --endpoint-url https://play.min.io:9000 s3 cp simplejson-3.3.0.tar.gz s3://mybucket
upload: ./simplejson-3.3.0.tar.gz to s3://mybucket/simplejson-3.3.0.tar.gz
```

### To delete an object from a bucket

```
aws --endpoint-url https://play.min.io:9000 s3 rm s3://mybucket/argparse-1.2.1.tar.gz
delete: s3://mybucket/argparse-1.2.1.tar.gz
```

### To remove a bucket

```
aws --endpoint-url https://play.min.io:9000 s3 rb s3://mybucket
remove_bucket: s3://mybucket/
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).