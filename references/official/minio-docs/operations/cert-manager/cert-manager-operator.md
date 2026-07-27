---
title: "cert-manager for Operator — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/cert-manager/cert-manager-operator.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/cert-manager/cert-manager-operator"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "cert-manager for Operator — SILO (MinIO Object Storage)"
---
# cert-manager for Operator

MinIO Operator manages TLS certificate issuing for the services hosted in the `minio-operator` namespace.

This page describes how to manage the Operator’s TLS certificates with [[operations/network-encryption/cert-manager#minio-certmanager|cert-manager]].

## Prerequisites

A [supported version of Kubernetes](https://kubernetes.io/releases/).

[kustomize](https://kustomize.io/) installed

`kubectl` access to your `k8s` cluster

Completed the steps to [[operations/network-encryption/cert-manager#minio-setup-certmanager|set up cert-manager]]

The MinIO Operator must not yet be installed.

## 1) Create a CA Issuer for the minio-operator namespace

This guide disables the automatic generation of certificates in MinIO Operator and issues certificates using cert-manager instead.

The `minio-operator` namespace must have its own certificate authority (CA), derived from the cluster’s `ClusterIssuer` certificate created during [[operations/network-encryption/cert-manager#minio-certmanager|cert-manager setup]].
Create this CA certificate using cert-manager.

Important

This CA certificate must exist before installing MinIO Operator.

If it does not exist, create the `minio-operator` namespace

```
kubectl create ns minio-operator
```

Request a new Certificate with `spec.isCA: true` specified.

This certificate serves as the CA for the minio-operator namespace.

Create a file called `operator-ca-tls-secret.yaml` with the following contents:

```
# operator-ca-tls-secret.yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: minio-operator-ca-certificate
  namespace: minio-operator
spec:
  isCA: true
  commonName: operator
  secretName: operator-ca-tls
  duration: 70128h # 8y
  privateKey:
    algorithm: ECDSA
    size: 256
  issuerRef:
    name: selfsigned-root
    kind: ClusterIssuer
    group: cert-manager.io
```

Important

The `spec.issueRef.name` must match the name of the `ClusterIssuer` created when [[operations/network-encryption/cert-manager#minio-cert-manager-create-cluster-issuer|setting up cert-manager]].
If you specified a different `ClusterIssuer` name or are using a different `Issuer` from the guide, modify the `issuerRef` to match your environment.

Apply the resource:

```
kubectl apply -f operator-ca-tls-secret.yaml
```

Kubernetes creates a new secret with the name `operator-ca-tls` in the `minio-operator` namespace.

Important

Make sure to trust this certificate in any applications that need to interact with the MinIO Operator.

## 2) Use the secret to create the Issuer

Use the `operator-ca-tls` secret to add an `Issuer` resource for the `minio-operator` namespace.

Create a file called `operator-ca-issuer.yaml` with the following contents:

```
# operator-ca-issuer.yaml
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: minio-operator-ca-issuer
  namespace: minio-operator
spec:
  ca:
    secretName: operator-ca-tls
```

Apply the resource:

```
kubectl apply -f operator-ca-issuer.yaml
```

## 3) Create TLS certificate

Now that the `Issuer` exists in the `minio-operator` namespace, cert-manager can add a certificate.

The certificate from cert-manager must be valid for the following DNS domains:

`sts`

`sts.minio-operator.svc.`

`sts.minio-operator.svc.<cluster domain>`

Important

Replace `<cluster domain>` with the actual value for your MinIO tenant.
`cluster domain` is the internal root DNS domain assigned in your Kubernetes cluster.
Typically, this is `cluster.local`, but confirm the value by checking your CoreDNS configuration for the correct value for your Kubernetes cluster.

For example:

```
kubectl get configmap coredns -n kube-system -o jsonpath="{.data}"
```

Different Kubernetes providers manage the root domain differently.
Check with your Kubernetes provider for more information.

Create a `Certificate` for the specified domains:

Create a file named `sts-tls-certificate.yaml` with the following contents:

```
# sts-tls-certificate.yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: sts-certmanager-cert
  namespace: minio-operator
spec:
  dnsNames:
    - sts
    - sts.minio-operator.svc
    - sts.minio-operator.svc.cluster.local # Replace cluster.local with the value for your domain.
  secretName: sts-tls
  issuerRef:
    name: minio-operator-ca-issuer
```

Important

The `spec.secretName` is not optional.

The secret name must be `sts-tls`.
Confirm this by setting `spec.secretName: sts-tls` as highlighted in the certificate YAML.

Apply the resource:

```
kubectl apply -f sts-tls-certificate.yaml
```

This creates a secret called `sts-tls` in the `minio-operator` namespace.

Warning

The STS service will not start if the `sts-tls` secret, containing the TLS certificate, is missing or contains an invalid `key-value` pair.

## 4) Install Operator with Auto TLS disabled

You can now [[operations/deployments/kubernetes#minio-operator-installation|install the MinIO Operator]].

When installing the Operator deployment, set the `OPERATOR_STS_AUTO_TLS_ENABLED` environment variable to `off` in the `minio-operator` container.

Disabling this environment variable prevents the MinIO Operator from issuing the certificates.
Instead, Operator relies on cert-manager to issue the TLS certificate.

There are various methods to define an environment variable depending on how you install the Operator.
The following steps define the variable with kustomize.

Create a kustomization patch file called `kustomization.yaml` with the following contents:

```
# minio-operator/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- github.com/minio/operator/resources

patches:
- patch: |-
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: minio-operator
      namespace: minio-operator
    spec:
      template:
        spec:
          containers:
            - name: minio-operator
              env:
                - name: OPERATOR_STS_AUTO_TLS_ENABLED
                  value: "off"
                - name: OPERATOR_STS_ENABLED
                  value: "on"
```

Apply the kustomization resource to the cluster:

```
kubectl apply -k minio-operator
```

## Migrate an existing MinIO Operator deployment to cert-manager

To transition an existing MinIO Operator deployment from using AutoCert to cert-manager, complete the following steps:

Complete the steps for [[operations/network-encryption/cert-manager#minio-certmanager|installing cert-manager]], including disabling auto-cert.

Complete steps 1-3 on this page to generate the certificate authority for the Operator.

When you get to the install step on this page, instead replace the existing Operator TLS certificate with the cert-manager issued certificate.

Create new cert-manager certificates for each tenant, similar to the steps described on the [[operations/cert-manager/cert-manager-tenants#minio-certmanager-tenants|cert-manager for Tenants]] page.

Replace the secrets in the MinIO Operator namespace for the tenants with secrets related to each tenant’s cert-manager issued certificate.

## Next steps

Set up [[operations/cert-manager/cert-manager-tenants#minio-certmanager-tenants|cert-manager for a MinIO Tenant]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).