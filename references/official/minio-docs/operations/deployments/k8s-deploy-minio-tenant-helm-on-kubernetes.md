---
title: "Deploy a MinIO Tenant with Helm Charts — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/k8s-deploy-minio-tenant-helm-on-kubernetes.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/k8s-deploy-minio-tenant-helm-on-kubernetes"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Deploy a MinIO Tenant with Helm Charts — SILO (MinIO Object Storage)"
---
# Deploy a MinIO Tenant with Helm Charts

## Overview

Helm is a tool for automating the deployment of applications to Kubernetes clusters.
A [Helm chart](https://helm.sh/docs/topics/charts/) is a set of YAML files, templates, and other files that define the deployment details.
The following procedure uses a Helm Chart to deploy a Tenant managed by the MinIO Operator.

This procedure requires the Kubernetes cluster have a valid [[operations/deployments/kubernetes#deploy-operator-kubernetes|Operator]] deployment.
You cannot use the MinIO Operator Tenant chart to deploy a Tenant independent of the Operator.

Important

The MinIO Operator Tenant Chart is distinct from the community-managed [MinIO Chart](https://github.com/minio/minio/tree/master/helm/minio).

The Community Helm Chart is built, maintained, and supported by the community.
MinIO does not guarantee support for any given bug, feature request, or update referencing that chart.

The [[reference/tenant-chart-values#minio-tenant-chart-values|Operator Tenant Chart]] is officially maintained and supported by MinIO.
MinIO strongly recommends the official Helm Chart for [[reference/operator-chart-values#minio-operator-chart-values|Operator]] and [[reference/tenant-chart-values#minio-tenant-chart-values|Tenants]] for production environments.

## Prerequisites

You must meet the following requirements to install a MinIO Tenant with Helm:

An existing Kubernetes cluster

The `kubectl` CLI tool on your local host with version matching the cluster.

[Helm](https://helm.sh/docs/intro/install/) version 3.8 or greater.

[yq](https://github.com/mikefarah/yq/#install) version 4.18.1 or greater.

An existing [[operations/deployments/kubernetes#deploy-operator-kubernetes|MinIO Operator installation]].

This procedure assumes your Kubernetes cluster access grants you broad administrative permissions.

For more about Tenant installation requirements, including supported Kubernetes versions and TLS certificates, see the [[operations/checklists/hardware#minio-hardware-checklist-storage|Tenant deployment prerequisites]].

This procedure assumes familiarity the with referenced Kubernetes concepts and utilities.
While this documentation may provide guidance for configuring or deploying Kubernetes-related resources on a best-effort basis, it is not a replacement for the official [Kubernetes Documentation](https://kubernetes.io/docs/).

### Namespace

The tenant must use its own namespace and cannot share a namespace with another tenant.
In addition, MinIO strongly recommends using a dedicated namespace for the tenant with no other applications running in the namespace.

## Deploy a MinIO Tenant using Helm Charts

The following procedure deploys a MinIO Tenant using the MinIO Operator Chart Repository.
This method supports a simplified installation path compared to the [local chart installation](#deploy-tenant-helm-local).

The following procedure uses Helm to deploy a MinIO Tenant using the official MinIO Tenant Chart.

Important

If you use Helm to deploy a MinIO Tenant, you must use Helm to manage or upgrade that deployment.
Do not use `kubectl krew`, Kustomize, or similar methods to manage or upgrade the MinIO Tenant.

This procedure is not exhaustive of all possible configuration options available in the [[reference/tenant-chart-values#minio-tenant-chart-values|Tenant Chart]].
It provides a baseline from which you can modify and tailor the Tenant to your requirements.

Verify your MinIO Operator Repo Configuration

MinIO maintains a Helm-compatible repository at [https://operator.min.io](https://operator.min.io).
If the repository does not already exist in your local Helm configuration, add it before continuing:

```
helm repo add minio-operator https://operator.min.io
```

You can validate the repo contents using `helm search`:

```
helm search repo minio-operator
```

The response should resemble the following:

```
NAME                            CHART VERSION   APP VERSION     DESCRIPTION
minio-operator/minio-operator   4.3.7           v4.3.7          A Helm chart for MinIO Operator
minio-operator/operator         7.1.1           v7.1.1          A Helm chart for MinIO Operator
minio-operator/tenant           7.1.1           v7.1.1          A Helm chart for MinIO Operator
```

Create a local copy of the Helm `values.yaml` for modification

```
curl -sLo values.yaml https://raw.githubusercontent.com/minio/operator/master/helm/tenant/values.yaml
```

Open the `values.yaml` object in your preferred text editor.

Configure the Tenant topology

The following fields share the `tenant.pools[0]` prefix and control the number of servers, volumes per server, and storage class of all pods deployed in the Tenant:

Field

Description

`servers`

The number of MinIO pods to deploy in the Server Pool.

`volumesPerServer`

The number of persistent volumes to attach to each MinIO pod (`servers`).
The Operator generates `volumesPerServer x servers` Persistant Volume Claims for the Tenant.

`storageClassName`

The Kubernetes storage class to associate with the generated Persistent Volume Claims.

If no storage class exists matching the specified value or if the specified storage class cannot meet the requested number of PVCs or storage capacity, the Tenant may fail to start.

`size`

The amount of storage to request for each generated PVC.

Configure Tenant Affinity or Anti-Affinity

The Tenant Chart supports the following Kubernetes Selector, Affinity and Anti-Affinity configurations:

Node Selector (`tenant.nodeSelector`)

Node/Pod Affinity or Anti-Affinity (`spec.pools[n].affinity`)

MinIO recommends configuring Tenants with Pod Anti-Affinity to ensure that the Kubernetes schedule does not schedule multiple pods on the same worker node.

If you have specific worker nodes on which you want to deploy the tenant, pass those node labels or filters to the `nodeSelector` or `affinity` field to constrain the scheduler to place pods on those nodes.

Configure Network Encryption

The MinIO Tenant CRD provides the following fields with which you can configure tenant TLS network encryption:

Field

Description

`tenant.certificate.requestAutoCert`

Enable or disable MinIO [[operations/network-encryption#minio-tls|automatic TLS certificate generation]].

Defaults to `true` or enabled if omitted.

`tenant.certificate.certConfig`

Customize the behavior of [[operations/network-encryption#minio-tls|automatic TLS]], if enabled.

`tenant.certificate.externalCertSecret`

Enable TLS for multiple hostnames via Server Name Indication (SNI).

Specify one or more Kubernetes secrets of type `kubernetes.io/tls` or `cert-manager`.

`tenant.certificate.externalCACertSecret`

Enable validation of client TLS certificates signed by unknown, third-party, or internal Certificate Authorities (CA).

Specify one or more Kubernetes secrets of type `kubernetes.io/tls` containing the full chain of CA certificates for a given authority.

Configure MinIO Environment Variables

You can set MinIO Server environment variables using the `tenant.configuration` field.

Field

Description

`tenant.configuration`

Specify a Kubernetes opaque secret whose data payload `config.env` contains each MinIO environment variable you want to set.

The `config.env` data payload must be a base64-encoded string.
You can create a local file, set your environment variables, and then use `cat LOCALFILE | base64` to create the payload.

The YAML includes an object `kind: Secret` with `metadata.name: storage-configuration` that sets the root username, password, erasure parity settings, and enables Tenant Console.

Modify this as needed to reflect your Tenant requirements.

Deploy the Tenant

Use `helm` to install the Tenant Chart using your `values.yaml` as an override:

```
helm install \
--namespace TENANT-NAMESPACE \
--create-namespace \
--values values.yaml \
TENANT-NAME minio-operator/tenant
```

You can monitor the progress using the following command:

```
watch kubectl get all -n TENANT-NAMESPACE
```

Expose the Tenant MinIO S3 API port

To test the MinIO Client [[reference/minio-mc#command-mc|`mc`]] from your local machine, forward the MinIO port and create an alias.

Forward the Tenant’s MinIO port:

```
kubectl port-forward svc/TENANT-NAME-hl 9000 -n TENANT-NAMESPACE
```

Create an alias for the Tenant service:

```
mc alias set myminio https://localhost:9000 minio minio123 --insecure
```

You can use [[reference/minio-mc/mc-mb#command-mc.mb|`mc mb`]] to create a bucket on the Tenant:

```
mc mb myminio/mybucket --insecure
```

If you deployed your MinIO Tenant using TLS certificates minted by a trusted Certificate Authority (CA) you can omit the `--insecure` flag.

See [[operations/deployments/k8s-deploy-minio-tenant-on-kubernetes#create-tenant-connect-tenant|Connect to the Tenant]] for additional documentation on external connectivity to the Tenant.

## Deploy a Tenant using a Local Helm Chart

The following procedure deploys a Tenant using a local copy of the Helm Charts.
This method may support easier pre-configuration of the Tenant compared to the [repo-based installation](#deploy-tenant-helm-repo).

Download the Helm charts

On your local host, download the Tenant Helm charts to a convenient directory:

```
curl -O https://raw.githubusercontent.com/minio/operator/master/helm-releases/tenant-7.1.1.tgz
```

Each chart contains a `values.yaml` file you can customize to suit your needs.
For details on the options available in the MinIO Tenant `values.yaml`, see [[reference/tenant-chart-values#minio-tenant-chart-values|Tenant Helm Charts]].

Open the `values.yaml` object in your preferred text editor.

Configure the Tenant topology

The following fields share the `tenant.pools[0]` prefix and control the number of servers, volumes per server, and storage class of all pods deployed in the Tenant:

Field

Description

`servers`

The number of MinIO pods to deploy in the Server Pool.

`volumesPerServer`

The number of persistent volumes to attach to each MinIO pod (`servers`).
The Operator generates `volumesPerServer x servers` Persistant Volume Claims for the Tenant.

`storageClassName`

The Kubernetes storage class to associate with the generated Persistent Volume Claims.

If no storage class exists matching the specified value or if the specified storage class cannot meet the requested number of PVCs or storage capacity, the Tenant may fail to start.

`size`

The amount of storage to request for each generated PVC.

Configure Tenant Affinity or Anti-Affinity

The Tenant Chart supports the following Kubernetes Selector, Affinity and Anti-Affinity configurations:

Node Selector (`tenant.nodeSelector`)

Node/Pod Affinity or Anti-Affinity (`spec.pools[n].affinity`)

MinIO recommends configuring Tenants with Pod Anti-Affinity to ensure that the Kubernetes schedule does not schedule multiple pods on the same worker node.

If you have specific worker nodes on which you want to deploy the tenant, pass those node labels or filters to the `nodeSelector` or `affinity` field to constrain the scheduler to place pods on those nodes.

Configure Network Encryption

The MinIO Tenant CRD provides the following fields from which you can configure tenant TLS network encryption:

Field

Description

`tenant.certificate.requestAutoCert`

Enables or disables MinIO [[operations/network-encryption#minio-tls|automatic TLS certificate generation]]

`tenant.certificate.certConfig`

Controls the settings for [[operations/network-encryption#minio-tls|automatic TLS]].
Requires `spec.requestAutoCert: true`

`tenant.certificate.externalCertSecret`

Specify one or more Kubernetes secrets of type `kubernetes.io/tls` or `cert-manager`.
MinIO uses these certificates for performing TLS handshakes based on hostname (Server Name Indication).

`tenant.certificate.externalCACertSecret`

Specify one or more Kubernetes secrets of type `kubernetes.io/tls` with the Certificate Authority (CA) chains which the Tenant must trust for allowing client TLS connections.

Configure MinIO Environment Variables

You can set MinIO Server environment variables using the `tenant.configuration` field.

The field must specify a Kubernetes opaque secret whose data payload `config.env` contains each MinIO environment variable you want to set.

The YAML includes an object `kind: Secret` with `metadata.name: storage-configuration` that sets the root username, password, erasure parity settings, and enables Tenant Console.

Modify this as needed to reflect your Tenant requirements.

The following Helm command creates a MinIO Tenant using the standard chart:

```
helm install \
--namespace TENANT-NAMESPACE \
--create-namespace \
TENANT-NAME tenant-7.1.1.tgz
```

To deploy more than one Tenant, create a Helm chart with the details of the new Tenant and repeat the deployment steps.
Redeploying the same chart updates the previously deployed Tenant.

Expose the Tenant MinIO port

To test the MinIO Client [[reference/minio-mc#command-mc|`mc`]] from your local machine, forward the MinIO port and create an alias.

Forward the Tenant’s MinIO port:

```
kubectl port-forward svc/TENANT-NAME-hl 9000 -n TENANT-NAMESPACE
```

Create an alias for the Tenant service:

```
mc alias set myminio https://localhost:9000 minio minio123 --insecure
```

This example uses the non-TLS `myminio-hl` service, which requires the `--insecure` option..

If you have a TLS cert configured, omit `--insecure` and use `svc/minio` instead.

You can use [[reference/minio-mc/mc-mb#command-mc.mb|`mc mb`]] to create a bucket on the Tenant:

```
mc mb myminio/mybucket --insecure
```

See [[operations/deployments/k8s-deploy-minio-tenant-on-kubernetes#create-tenant-connect-tenant|Connect to the Tenant]] for additional documentation on external connectivity to the Tenant.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).