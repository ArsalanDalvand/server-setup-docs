---
title: "Installation and Management — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/installation.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/installation"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Installation and Management — SILO (MinIO Object Storage)"
---
# Installation and Management

This section documents steps for installing and managing the AGPLv3-licensed Community MinIO Object Storage on [[operations/deployments/kubernetes#minio-kubernetes|Kubernetes]] and [[operations/deployments/baremetal#minio-baremetal|Baremetal]] infrastructures.

[Installing and Running MinIO on Linux](https://www.youtube.com/watch?v=74usXkZpNt8&list=PLFOIsHSSYIK1BnzVY66pCL-iJ30Ht9t1o)

[Object Storage Essentials](https://www.youtube.com/playlist?list=PLFOIsHSSYIK3WitnqhqfpeZ6fRFKHxIr7)

[How to Connect to MinIO with JavaScript](https://www.youtube.com/watch?v=yUR4Fvx0D3E&list=PLFOIsHSSYIK3Dd3Y_x7itJT1NUKT5SxDh&index=5)

MinIO is a software-defined high performance distributed object storage server.
You can run MinIO on consumer or enterprise-grade hardware and a variety
of operating systems and architectures.

All MinIO deployments implement [[operations/concepts/erasure-coding#minio-erasure-coding|Erasure Coding]] backends.
You can deploy MinIO using one of the following topologies:

[Single-Node Single-Drive](#minio-snsd) (SNSD or “Standalone”)

Local development and evaluation with no/limited reliability

[Single-Node Multi-Drive](#minio-snmd) (SNMD or “Standalone Multi-Drive”)

Workloads with lower performance, scale, and capacity requirements

Drive-level reliability with configurable tolerance for loss of up to 1/2 all drives

Evaluation of multi-drive topologies and failover behavior.

[Multi-Node Multi-Drive](#minio-mnmd) (MNMD or “Distributed”)

Enterprise-grade high-performance object storage

Multi Node/Drive level reliability with configurable tolerance for loss of up to 1/2 all nodes/drives

Primary storage for AI/ML, Distributed Query, Analytics, and other Data Lake components

Scalable for Petabyte+ workloads - both storage capacity and performance

## Kubernetes

MinIO provides a Kubernetes-native Operator framework for managing and deploying Tenants onto your managed infrastructure.

MinIO fully supports upstream Kubernetes and most flavors which inherit from the upstream as a base.
This includes, but is not limited to, RedHat Openshift, SUSE Rancher, VMWare Tanzu.
MinIO also fully supports cloud-based Kubernetes engines such as Elastic Kubernetes Engine, Google Kubernetes Service, and Azure Kubernetes Service.

Select the link most appropriate for your Kubernetes infrastructure.
If your provider is not listed, use the Kubernetes Upstream documentation as a baseline and modify as needed based on your provider’s guidance or divergence from upstream semantics and behavior.

[[operations/deployments/kubernetes#deploy-operator-kubernetes|Deploy MinIO on Kubernetes (Upstream)]]

[[operations/deployments/kubernetes#deploy-operator-openshift|Deploy MinIO on Openshift Kubernetes]]

[[operations/deployments/kubernetes#deploy-operator-rancher|Deploy MinIO on SUSE Rancher Kubernetes]]

[[operations/deployments/kubernetes#deploy-operator-eks|Deploy MinIO on Elastic Kubernetes Service]]

[[operations/deployments/kubernetes#deploy-operator-gke|Deploy MinIO on Google Kubernetes Engine]]

[[operations/deployments/kubernetes#deploy-operator-aks|Deploy MinIO on Azure Kubernetes Service]]

## Baremetal

MinIO supports deploying onto baremetal infrastructure - physical machines or virtualized hosts - running Linux, MacOS, and Windows.
You can also deploy MinIO as a container onto supported Operating Systems.

[[operations/deployments/baremetal-deploy-minio-on-redhat-linux#deploy-minio-rhel|Deploy MinIO onto RedHat Linux]]

[[operations/deployments/baremetal-deploy-minio-on-ubuntu-linux#deploy-minio-ubuntu|Deploy MinIO onto Ubuntu Linux]]

[[operations/deployments/baremetal-deploy-minio-as-a-container#deploy-minio-container|Deploy MinIO as a Container]]

[[operations/deployments/baremetal-deploy-minio-on-macos#deploy-minio-macos|Deploy MinIO onto MacOS]]

[[operations/deployments/baremetal-deploy-minio-on-windows#deploy-minio-windows|Deploy MinIO onto Windows]]

Important

MinIO strongly recommends [Linux (RHEL, Ubuntu)](https://silo.pigsty.io/minio/linux/index.html) or [Kubernetes (Upstream, OpenShift)](https://silo.pigsty.io/minio/kubernetes/upstream/index.html) for long-term development and production environments.

MinIO provides no guarantee of support for SNMD or MNMD topologies on MacOS, Windows, or Containerized deployments.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).