---
title: "Erasure Code Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/storage-class.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/storage-class"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Erasure Code Settings — SILO (MinIO Object Storage)"
---
# Erasure Code Settings

This page covers settings that configure the [[operations/concepts/erasure-coding#minio-erasure-coding|Erasure Code]] [[operations/concepts/erasure-coding#minio-ec-parity|parity]] to use for objects written to the MinIO cluster.
This impacts how MinIO uses the space on the drive(s) and how MinIO can recover objects stored on lost drives or similar issues.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Standard Storage Class

Note

MinIO Storage Classes are distinct from AWS Storage Classes.

AWS Storage Classes refer to the specific storage tier on which to store a given object, such as `hot` or `glacier` storage.
MinIO Storage Classes affect the erasure code parity setting used and relate to [[operations/concepts/availability-and-resiliency#minio-availability-resiliency|Availability and Resiliency]] of objects.

For tiering from one type of storage to another, such as for cost management purposes, see [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|Object Transition (“Tiering”)]].

Environment Variable

MINIO_STORAGE_CLASS_STANDARD[](#envvar.MINIO_STORAGE_CLASS_STANDARD)

Configuration Setting

storage_class standard[](#mc-conf.storage_class.standard)

The [[operations/concepts/erasure-coding#minio-ec-parity|parity level]] for the deployment.
MinIO shards objects written with the default `STANDARD` storage class using this parity value.

MinIO references the `x-amz-storage-class` header in request metadata for determining which storage class to assign an object.
The specific syntax or method for setting headers depends on your preferred method for interfacing with the MinIO server.

Specify the value using `EC:M` notation, where `M` refers to the number of parity blocks to create for the object.

The following table lists the default values based on the [[operations/concepts/erasure-coding#minio-ec-erasure-set|erasure set size]] of the initial server pool in the deployment:

Erasure Set Size

Default Parity (EC:N)

1

EC:0

2-3

EC:1

4-5

EC:2

6 - 7

EC:3

8 - 16

EC:4

The minimum supported value is `0`, which indicates no erasure coding protections.
These deployments rely entirely on the storage controller or resource for availability / resiliency.

The maximum value depends on the erasure set size of the initial server pool in the deployment, where the upper bound is  \(\frac{\text{ERASURE_SET_SIZE}}{\text{2}}\).
For example, a deployment with erasure set stripe size of 16 has a maximum standard parity of 8.

You can change this value after startup to any value between `0` and the upper bound for the erasure set size.
MinIO only applies the changed parity to newly written objects.
Existing objects retain the parity value in place at the time of their creation.

## Reduced Redundancy Storage Class

Note

MinIO Storage Classes are distinct from AWS Storage Classes.

AWS Storage Classes refer to the specific storage tier on which to store a given object, such as `hot` or `glacier` storage.
MinIO Storage Classes affect the erasure code parity setting used and relate to [[operations/concepts/availability-and-resiliency#minio-availability-resiliency|Availability and Resiliency]] of objects.

For tiering from one type of storage to another, such as for cost management purposes, see [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|Object Transition (“Tiering”)]].

Environment Variable

MINIO_STORAGE_CLASS_RRS[](#envvar.MINIO_STORAGE_CLASS_RRS)

Configuration Setting

storage_class rrs[](#mc-conf.storage_class.rrs)

The [[operations/concepts/erasure-coding#minio-ec-parity|parity level]] for objects written with the `REDUCED` storage class.

MinIO references the `x-amz-storage-class` header in request metadata for determining which storage class to assign an object.
The specific syntax or method for setting headers depends on your preferred method for interfacing with the MinIO server.

Specify the value using `EC:M` notation, where `M` refers to the number of parity blocks to create for the object.

This value must be less than or equal to [`MINIO_STORAGE_CLASS_STANDARD`](#envvar.MINIO_STORAGE_CLASS_STANDARD).

You cannot set this value for deployments with an erasure set size less than 2.
Defaults to `EC:1` for deployments with erasure set size greater than 1.
Defaults to `EC:0` for deployments of erasure set size of 1.

## Parity Retention Optimization

Environment Variable

MINIO_STORAGE_CLASS_OPTIMIZE[](#envvar.MINIO_STORAGE_CLASS_OPTIMIZE)

Configuration Setting

storage_class optimize[](#mc-conf.storage_class.optimize)

MinIO by default automatically “upgrades” parity for an object if the destination erasure set maintains write quorum but has one or more drives offline.
This behavior helps ensure that the given object maintains the same availability as objects written to the healthy erasure set.

Specify `capacity` to this setting to direct MinIO to not create any additional parity for the object.
This prioritizes the overall capacity of the cluster at the cost of potentially reduced object availability in the event more drives in that erasure set fail.

## Comment

Environment Variable

MINIO_STORAGE_CLASS_COMMENT[](#envvar.MINIO_STORAGE_CLASS_COMMENT)

Configuration Setting

This setting does not have a configuration setting option.

Adds a comment to the storage class settings.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).