---
title: "MinIO Kubernetes Operator — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/k8s-minio-operator.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/k8s-minio-operator"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Kubernetes Operator — SILO (MinIO Object Storage)"
---
# MinIO Kubernetes Operator

MinIO is a Kubernetes-native high performance object store with an S3-compatible API.
The MinIO Kubernetes Operator supports deploying MinIO Tenants onto private and public cloud infrastructures (“Hybrid” Cloud).

The MinIO Operator installs a [Custom Resource Definition (CRD)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions) to support describing MinIO tenants as a Kubernetes [object](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/).

The MinIO Operator exists in its own namespace.
Within the Operator’s namespace, the MinIO Operator utilizes two pods:

The Operator pod for the base Operator functions to deploy, manage, modify, and maintain tenants.

Console pod for the Operator’s Graphical User Interface, the Operator Console.

See the MinIO Operator [CRD Reference](https://github.com/minio/operator/blob/master/docs/tenant_crd.adoc) for complete documentation on the MinIO CRD.

## Operator Prerequisites

### Kubernetes Version

MinIO supports [maintained Kubernetes APIs](https://kubernetes.io/releases/) for deploying the Operator.

Kubernetes infrastructure running end-of-life API versions may exhibit unexpected or undesired behavior if used for deploying the Operator.

### Kustomize and kubectl

[Kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization) is a YAML-based templating tool that allows you to define Kubernetes resources in a declarative and repeatable fashion.
Kustomize is included with the [kubectl](https://kubernetes.io/docs/reference/kubectl) command line tool.

This procedure assumes that your local host machine has both the matching version of `kubectl` for your Kubernetes cluster and the necessary access to that cluster to create new resources.

The [default MinIO Operator Kustomize template](https://github.com/minio/operator/blob/master/kustomization.yaml) provides a starting point for customizing configurations for your local environment.
You can modify the default Kustomization file or apply your own [patches](https://datatracker.ietf.org/doc/html/rfc6902) to customize the Operator deployment for your Kubernetes cluster.

### Kubernetes TLS Certificate API

The MinIO Operator manages TLS Certificate Signing Requests (CSR) using the Kubernetes `certificates.k8s.io` [TLS certificate management API](https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/) to create signed TLS certificates in the following circumstances:

When `autoCert` is enabled.

For the MinIO Console when the [[reference/operator-environment-variables#envvar.MINIO_CONSOLE_TLS_ENABLE|`MINIO_CONSOLE_TLS_ENABLE`]] environment variable is set to `on`.

For [[developers/security-token-service#minio-security-token-service|STS service]] when [[reference/operator-environment-variables#envvar.OPERATOR_STS_ENABLED|`OPERATOR_STS_ENABLED`]] environment variable is set to `on`.

For retrieving the health of the cluster.

The MinIO Operator reads certificates inside the `operator-ca-tls` secret and syncs this secret within the tenant namespace to trust private certificate authorities, such as when using cert-manager.

For any of these circumstances, the MinIO Operator requires that the Kubernetes `kube-controller-manager` configuration include the following [configuration settings](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/#options):

`--cluster-signing-key-file` - Specify the PEM-encoded RSA or ECDSA private key used to sign cluster-scoped certificates.

`--cluster-signing-cert-file` - Specify the PEM-encoded x.509 Certificate Authority certificate used to issue cluster-scoped certificates.

The Kubernetes TLS API uses the CA signature algorithm for generating new TLS certificate.
MinIO recommends ECDSA (e.g. [NIST P-256 curve](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)) or EdDSA (e.g. [Curve25519](https://datatracker.ietf.org/doc/html/rfc7748.html)) TLS private keys/certificates due to their lower computation requirements compared to RSA.
See [[operations/network-encryption#minio-tls-supported-cipher-suites|Supported TLS Cipher Suites]] for a complete list of supported TLS Cipher Suites.

If the Kubernetes cluster is not configured to respond to a generated CSR, the Operator cannot complete initialization.
Some Kubernetes providers do not specify these configuration values by default.

To check whether the `kube-controller-manager` specifies the cluster signing key and certificate files, use the following command:

```
kubectl get pod kube-controller-manager-$CLUSTERNAME-control-plane \
  -n kube-system -o yaml
```

Replace `$CLUSTERNAME` with the name of the Kubernetes cluster.

Confirm that the output contains the highlighted lines.
The output of the example command above may differ from the output in your terminal:

```
spec:
 containers:
 - command:
     - kube-controller-manager
     - --allocate-node-cidrs=true
     - --authentication-kubeconfig=/etc/kubernetes/controller-manager.conf
     - --authorization-kubeconfig=/etc/kubernetes/controller-manager.conf
     - --bind-address=127.0.0.1
     - --client-ca-file=/etc/kubernetes/pki/ca.crt
     - --cluster-cidr=10.244.0.0/16
     - --cluster-name=my-cluster-name
     - --cluster-signing-cert-file=/etc/kubernetes/pki/ca.crt
     - --cluster-signing-key-file=/etc/kubernetes/pki/ca.key
 ...
```

Important

The MinIO Operator automatically generates TLS certificates for all MinIO Tenant pods using the specified Certificate Authority (CA).
Clients external to the Kubernetes cluster must trust the Kubernetes cluster CA to connect to the MinIO Operator or MinIO Tenants.

Clients which cannot trust the Kubernetes cluster CA can disable TLS validation for connections to the MinIO Operator or a MinIO Tenant.

Alternatively, you can generate x.509 TLS certificates signed by a known and trusted CA and pass those certificates to MinIO Tenants.
See [[operations/network-encryption#minio-tls|Network Encryption (TLS)]] for more complete documentation.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).