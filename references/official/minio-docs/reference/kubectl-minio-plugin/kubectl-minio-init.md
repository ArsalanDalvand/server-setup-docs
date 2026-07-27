---
title: "kubectl minio init — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/kubectl-minio-plugin/kubectl-minio-init.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/kubectl-minio-plugin/kubectl-minio-init"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "kubectl minio init — SILO (MinIO Object Storage)"
---
# kubectl minio init

## Description

The [`kubectl minio init`](#command-kubectl.minio.init) command initialize the MinIO Operator.

If the Kubernetes cluster has an existing MinIO Operator installation, this command upgrades the Operator to match the MinIO plugin version.
For more information on upgrading the MinIO Operator, see [[operations/deployments/k8s-upgrade-minio-operator-kubernetes#minio-k8s-upgrade-minio-operator|Upgrade MinIO Operator]].

## Syntax

EXAMPLE

The following command initializes a new MinIO Operator deployment running 7.1.1.

```
kubectl minio init
```

SYNTAX

The command has the following syntax:

```
kubectl minio init                      \
              [--cluster-domain]        \
              [--console-image]         \
              [--console-tls]           \
              [--default-kes-image]     \
              [--default-minio-image]   \
              [--image]                 \
              [--image-pull-secret]     \
              [--namespace]             \
              [--namespace-to-watch]    \
              [--output]                \
              [--prometheus-name]       \
              [--prometheus-namespace]
```

## Flags

The command supports the following flags:

--cluster-domain[](#kubectl.minio.init.-cluster-domain)
Optional

The domain name to use when configuring the DNS hostname of the operator.
Defaults to `cluster.local`.

--console-image[](#kubectl.minio.init.-console-image)
Optional

The image to use when deploying the [Operator Console](https://github.com/minio/operator) in Operator mode, where administrators can create and manage MinIO tenants using a Graphical User Interface.
Defaults to the [version bundled in variable DefaultOperatorImage for the matching Operator release](https://github.com/minio/operator/blob/master/kubectl-minio/cmd/helpers/constants.go).

--console-tls[](#kubectl.minio.init.-console-tls)
Optional

New in version 4.5.6.

Enables TLS for the Operator Console.

Disabled by default.

--default-kes-image[](#kubectl.minio.init.-default-kes-image)
Optional

The default [kes](https://github.com/minio/kes) image to use when creating a new MinIO tenant.
Defaults to the [version bundled in variable DefaultKESImage for the matching Operator release](https://github.com/minio/operator/blob/master/kubectl-minio/cmd/helpers/constants.go).

--default-minio-image[](#kubectl.minio.init.-default-minio-image)
Optional

The default [minio](https://github.com/minio/minio) image to use when creating a new MinIO tenant.
Defaults to the [version bundled in variable DefaultTenantImage for the matching Operator release](https://github.com/minio/operator/blob/master/kubectl-minio/cmd/helpers/constants.go).

--image[](#kubectl.minio.init.-image)
Optional

The image to use for deploying the operator.
Defaults to the [latest release of the operator](https://github.com/minio/operator/releases/latest).

--image-pull-secret[](#kubectl.minio.init.-image-pull-secret)
Optional

Secret key for use with pulling the [`--image`](#kubectl.minio.init.-image).

The MinIO-hosted `minio/operator` image is not password protected.
This option is only required for non-MinIO image sources which are password protected.

--namespace[](#kubectl.minio.init.-namespace)
Optional

The namespace into which to deploy the operator.
Defaults to `minio-operator`.

--namespace-to-watch[](#kubectl.minio.init.-namespace-to-watch)
Optional

The namespace which the operator watches for MinIO tenants.
Defaults to `""` for all namespaces.

--output[](#kubectl.minio.init.-output)
Optional

Performs a dry run and outputs the generated YAML to `STDOUT`.
Use this option to customize the YAML and apply it manually using `kubectl apply -f <FILE>`.

--prometheus-name[](#kubectl.minio.init.-prometheus-name)
Optional

The name of the Prometheus service managed by the Prometheus Operator.
Defaults to `PROMETHEUS_NAME`

--prometheus-namespace[](#kubectl.minio.init.-prometheus-namespace)
Optional

The namespace into which to deploy Prometheus.
Defaults to `PROMETHEUS_NAMESPACE`

--sts[](#kubectl.minio.init.-sts)
Optional

Enable Operator sts (v1alpha1)

New in version 5.0.0.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).