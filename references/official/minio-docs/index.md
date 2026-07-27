---
title: "MinIO High Performance Object Storage — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/index.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "index"
category: "minio"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO High Performance Object Storage — SILO (MinIO Object Storage)"
---
# MinIO High Performance Object Storage

Important

This is a community-maintained fork of [minio/docs](https://github.com/minio/docs) , maintained by [Pigsty](https://pigsty.io) at [pgsty/minio-docs](https://github.com/pgsty/minio-docs) .
This project is not affiliated with, endorsed by, or sponsored by MinIO, Inc.
“MinIO” is a trademark of MinIO, Inc., used here solely to identify the upstream project.

[Installing and Running MinIO on Docker: Overview](https://youtu.be/mg9NRR6Js1s?ref=docs)

[Installing and Running MinIO on Docker: Installation Lab](https://youtu.be/Z0FtabDUPtU?ref=docs)

[Object Storage Essentials](https://www.youtube.com/playlist?list=PLFOIsHSSYIK3WitnqhqfpeZ6fRFKHxIr7)

[How to Connect to MinIO with JavaScript](https://www.youtube.com/watch?v=yUR4Fvx0D3E&list=PLFOIsHSSYIK3Dd3Y_x7itJT1NUKT5SxDh&index=5)

MinIO is a Kubernetes-native S3-compatible object storage solution designed to deploy wherever your applications are - on premises, in the private cloud, in the public cloud, and edge infrastructure.
MinIO is designed to support modern application workload patterns where high performance distributed computing meets petabyte-scale storage requirements.

This site documents Operations, Administration, and Development of MinIO Community Object Storage deployments on supported platforms.

[English](https://silo.pigsty.io) | [中文文档](https://silo.pigsty.cc) | [GitHub Repo](https://github.com/pgsty/minio) | [Docker Image](https://hub.docker.com/r/pgsty/minio) | [Ansible Deploy](https://pigsty.io/docs/minio)

## Quickstart

Sandbox

MinIO maintains a sandbox instance of the community server at [https://play.min.io](https://play.min.io).
You can use this instance for experimenting or evaluating the MinIO product on your local system.

Follow the [[reference/minio-mc#command-mc|`mc`]] CLI [[reference/minio-mc#mc-install|installation guide]] to install the utility on your local host.

[[reference/minio-mc#command-mc|`mc`]] includes a pre-configured `play` alias for connecting to the sandbox.
For example, you can use the following commands to create a bucket and copy objects to `play`:

```
mc mb play/mynewbucket

mc cp /path/to/file play/mynewbucket/prefix/filename.extension

mc stat play/mynewbucket/prefix/filename.extension
```

Important

MinIO’s Play sandbox is an ephemeral public-facing deployment with well-known access credentials.
Any private, confidential, internal, secured, or other important data uploaded to Play is effectively made public.
Exercise caution and discretion in any data you upload to Play.

Baremetal

Download the MinIO Server Process for your Operating System

Follow the instructions on the [MinIO Download Page](https://min.io/downloads?ref=docs) for your operating system to download and install the [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process.

Create a folder for use with MinIO

For example, create a folder `~/minio` in Linux/MacOS or `C:\minio` in Windows.

Start the MinIO Server

Run the [[reference/minio-server/minio-server#command-minio.server|`minio server`]] specifying the path to the directory and the [[reference/minio-server/minio-server#minio.server.-console-address|`--console-address`]] parameter to set a static console listen path:

```
minio server ~/minio --console-address :9001
# For windows, use minio.exe server ~/minio --console-address :9001`
```

The output includes connection instructions for both [[reference/minio-mc#command-mc|`mc`]] and connecting to the Console using your browser.

Kubernetes

Download [minio-dev.yaml](https://raw.githubusercontent.com/minio/docs/master/source/extra/examples/minio-dev.yaml) to your host machine:

```
curl https://raw.githubusercontent.com/minio/docs/master/source/extra/examples/minio-dev.yaml -O
```

The file describes two Kubernetes resources:

A new namespace `minio-dev`, and

A MinIO pod using a drive or volume on the Worker Node for serving data

Use `kubectl port-forward` to access the Pod, or create a service for the pod for which you can configure Ingress, Load Balancing, or similar Kubernetes-level networking.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).