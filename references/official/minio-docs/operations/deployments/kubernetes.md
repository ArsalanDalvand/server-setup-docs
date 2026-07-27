---
title: "Deploy MinIO on Kubernetes — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/kubernetes.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/kubernetes"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Deploy MinIO on Kubernetes — SILO (MinIO Object Storage)"
---
# Deploy MinIO on Kubernetes

MinIO is a Kubernetes-native high performance object store with an S3-compatible API.
The MinIO Kubernetes Operator supports deploying MinIO Tenants onto private and public cloud infrastructures (“Hybrid” Cloud).

All documentation assumes familiarity with referenced Kubernetes concepts, utilities, and procedures.
While MinIO documentation may provide guidance for configuring or deploying Kubernetes-related resources on a best-effort basis, it is not a replacement for the official [Kubernetes Documentation](https://kubernetes.io/docs/).

The MinIO Operator is a first-party Kubernetes-native operator that manages the deployment of MinIO Tenants onto Kubernetes infrastructure.

The Operator provides MinIO-centric functionality around Tenant management, including support for configuring all core MinIO features.

You can interact with the Operator through the MinIO [Custom Resource Definition (CRD)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions), or through the Operator Console UI.

The CRD provides a highly customizable entry point for using tools like Kustomize for deploying Tenants.
You can also use the MinIO Operator Console, a rich web-based UI that has complete support for deploying and configuring MinIO Tenants.

Important

The MinIO Operator Console UI is deprecated and removed in MinIO Operator 6.0.0.

You can continue to use standard Kubernetes approaches for MinIO Tenant management, such as Kustomize templates, Helm Charts, and `kubectl` commands for introspecting Tenant namespaces and resources.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).