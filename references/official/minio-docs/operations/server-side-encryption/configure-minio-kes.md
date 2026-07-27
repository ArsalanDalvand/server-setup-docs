---
title: "Server-Side Object Encryption with KES — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/server-side-encryption/configure-minio-kes.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/server-side-encryption/configure-minio-kes"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Server-Side Object Encryption with KES — SILO (MinIO Object Storage)"
---
# Server-Side Object Encryption with KES

Kubernetes

This procedure assumes you have access to a Kubernetes cluster with an active MinIO Operator installation.
For instructions on running KES, see the [KES docs](https://docs.min.io/community/minio-kes/tutorials/getting-started/).

As part of this procedure, you will:

Create or modify a MinIO deployment with support for SSE using KES.
Defer to the [[operations/deployments/installation#minio-mnmd|Deploy Distributed MinIO]] tutorial for guidance on production-ready MinIO deployments.

Use the MinIO Operator Console to create or manage a MinIO Tenant.

Access the Encryption settings for that tenant and configure SSE using a [supported Key Management System](https://docs.min.io/community/minio-kes/#supported-kms-targets).

Create a new EK for use with SSE.

Configure automatic bucket-default [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]].

Baremetal

This procedure provides guidance for deploying MinIO configured to use KES and enable [[operations/server-side-encryption#minio-sse-data-encryption|Server Side Encryption]].
For instructions on running KES, see the [KES docs](https://docs.min.io/community/minio-kes/tutorials/getting-started/).

As part of this procedure, you will:

Create a new EK for use with SSE.

Create or modify a MinIO deployment with support for SSE using KES.
Defer to the [[operations/deployments/installation#minio-mnmd|Deploy Distributed MinIO]] tutorial for guidance on production-ready MinIO deployments.

Configure automatic bucket-default [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]]

Important

Enabling SSE on a MinIO deployment automatically encrypts the backend data for that deployment using the default encryption key.

MinIO requires access to KES and the external KMS to decrypt the backend and start normally.
The KMS must maintain and provide access to the [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]].
You cannot disable KES later or “undo” the SSE configuration at a later point.

## Prerequisites

### Access to MinIO Cluster

Kubernetes

You must have access to the Kubernetes cluster, with administrative permissions associated to your `kubectl` configuration.

This procedure assumes your permission sets extends sufficiently to support deployment or modification of MinIO-associated resources on the Kubernetes cluster, including but not limited to pods, statefulsets, replicasets, deployments, and secrets.

Baremetal

This procedure uses [[reference/minio-mc#command-mc|`mc`]] for performing operations on the MinIO cluster.
Install `mc` on a machine with network access to the cluster.
See the `mc` [[reference/minio-mc#mc-install|Installation Quickstart]] for instructions on downloading and installing `mc`.

This procedure assumes a configured [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] for the MinIO cluster.

### Ensure KES Access to a Supported KMS Target

Kubernetes

This procedure assumes an existing [supported KMS installation](https://docs.min.io/community/minio-kes/#supported-kms-targets) accessible from the Kubernetes cluster.

For deployments within the same Kubernetes cluster as the MinIO Tenant, you can use Kubernetes service names to allow the MinIO Tenant to establish connectivity to the target KMS service.

For deployments external to the Kubernetes cluster, you must ensure the cluster supports routing communications between Kubernetes services and pods and the external network.
This may require configuration or deployment of additional Kubernetes network components and/or enabling access to the public internet.

Defer to the documentation for your chosen KMS solution for guidance on deployment and configuration.

Baremetal

This procedure assumes an existing KES installation connected to a supported KMS installation accessible, both accessible from the local host.
Refer to the installation instructions for your [supported KMS target](https://docs.min.io/community/minio-kes/#supported-kms-targets) to deploy KES and connect it to a KMS solution.

KES Operations Require Unsealed Target

Some supported KMS targets allow you to seal or unseal the vault instance.
KES returns an error if the configured KMS service is sealed.

If you restart or otherwise seal your vault instance, KES cannot perform any cryptographic operations against the vault.
You must unseal the Vault to ensure normal operations.

See the documentation for your chosen KMS solution for more information on whether unsealing may be required.

Refer to the configuration instruction in the [KES documentation](https://docs.min.io/community/minio-kes/) for your chosen supported KMS:

[AWS Secrets Manager](https://docs.min.io/community/minio-kes/integrations/aws-secrets-manager/)

[Azure KeyVault](https://docs.min.io/community/minio-kes/integrations/azure-keyvault/)

[Entrust KeyControl](https://docs.min.io/community/minio-kes/integrations/entrust-keycontrol/)

[Fortanix SDKMS](https://docs.min.io/community/minio-kes/integrations/fortanix-sdkms/)

[Google Cloud Secret Manager](https://docs.min.io/community/minio-kes/integrations/google-cloud-secret-manager/)

[HashiCorp Vault](https://docs.min.io/community/minio-kes/integrations/hashicorp-vault-keystore/)

[Thales CipherTrust Manager (formerly Gemalto KeySecure)](https://docs.min.io/community/minio-kes/integrations/thales-ciphertrust/)

## Procedure

This procedure provides instructions for configuring and enabling Server-Side Encryption using your selected [supported KMS solution](https://docs.min.io/community/minio-kes/#supported-kms-targets) in production environments.
Specifically, this procedure assumes the following:

An existing production-grade KMS target

One or more KES servers connected to the KMS target

One or more hosts for a new or existing MinIO deployment

Kubernetes

Review the Tenant CRD

Review the [[reference/operator-crd#minio-operator-crd|Tenant CRD]] `TenantSpec.kes` object, the `TenantSpec.configuration` object, and the [KES Configuration reference](https://silo.pigsty.io//kes/tutorials/configuration).

You must prepare all necessary configurations associated to your external Key Management Service of choice before proceeding.

Create or Modify your Tenant YAML to set the values of `KesConfig` as necessary:

You must modify your Tenant YAML or `Kustomize` templates to reflect the necessary KES configuration.
The following example is taken from the [MinIO Operator Kustomize examples](https://github.com/minio//operator/blob/master/examples/kustomization/tenant-kes-encryption/tenant.yaml)

```
kes:
   image: "" # minio/kes:2024-06-17T15-47-05Z
   env: [ ]
   replicas: 2
   kesSecret:
      name: kes-configuration
   imagePullPolicy: "IfNotPresent"
```

The `kes-configuration` secret must reference a Kubernetes Opaque Secret which contains a `stringData` object with the full KES configuration as `server-config.yaml`.
The `keystore` field must contain the full configuration associated with your preferred Key Management System.

Reference [the Kustomize example](https://github.com/minio/operator/blob/master/examples/kustomization/tenant-kes-encryption/kes-configuration-secret.yaml) for additional guidance.

Create or Modify your Tenant YAML to set the values of `TenantSpec.configuration` as necessary.

TODO

Generate a New Encryption Key

Unseal Vault Before Creating Key

If required by your chosen provider, you must unseal the backing vault instance before creating new encryption keys.
See the documentation for your chosen KMS solution for more information.

MinIO requires that the EK for a given bucket or object exist on the root KMS before performing SSE operations using that key.
You can use the [[reference/minio-mc-admin/mc-admin-kms-key#mc.admin.kms.key.create|`mc admin kms key create`]] command against the MinIO Tenant.

You must ensure your local host can access the MinIO Tenant pods and services before using [[reference/minio-mc#command-mc|`mc`]] to manage the Tenant.
For hosts internal to the Kubernetes cluster, you can use the [service DNS name](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#a-aaaa-records).
For hosts external to the Kubernetes cluster, specify the hostname of the service exposed by Ingress, Load Balancer, or similar Kubernetes network control component.

Run this command in a separate Terminal or Shell:

```
# Replace '-n minio' with the namespace of the MinIO deployment
# If you deployed the Tenant without TLS you may need to change the port range

# You can validate the ports in use by running
#  kubectl get svc/minio -n minio

kubectl port forward svc/minio 443:443 -n minio
```

The following commands in a new Terminal or Shell window:

Connect a local [[reference/minio-mc#command-mc|`mc`]] client to the Tenant.

Create the encryption key.

See [[reference/minio-mc#mc-install|Quickstart]] for instructions on installing `mc` on your local host.

```
# Replace USERNAME and PASSWORD with a user on the tenant with administrative permissions
# such as the root user

mc alias add k8s https://localhost:443 ROOTUSER ROOTPASSWORD

# Replace my-new-key with the name of the key you want to use for SSE-KMS
mc admin kms key create k8s encrypted-bucket-key
```

Enable SSE-KMS for a Bucket

You can use either the MinIO Tenant Console or the MinIO [[reference/minio-mc#command-mc|`mc`]] CLI to enable bucket-default SSE-KMS with the generated key:

MinIO Tenant Console

Connect to the [[operations/deployments/k8s-deploy-minio-tenant-on-kubernetes#create-tenant-connect-tenant|MinIO Tenant Console service]] and log in.
For clients internal to the Kubernetes cluster, you can specify the [service DNS name](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#a-aaaa-records).
For clients external to the Kubernetes cluster, specify the hostname of the service exposed by Ingress, Load Balancer, or similar Kubernetes network control component.

Once logged in, create a new Bucket and name it to your preference.
Select the Gear  icon to open the management view.

Select the pencil  icon next to the Encryption field to open the modal for configuring a bucket default SSE scheme.

Select SSE-KMS, then enter the name of the key created in the previous step.

Once you save your changes, try to upload a file to the bucket.
When viewing that file in the object browser, note that in the sidebar the metadata includes the SSE encryption scheme and information on the key used to encrypt that object.
This indicates the successful encrypted state of the object.

MinIO CLI

Use the [[operations/deployments/k8s-deploy-minio-tenant-on-kubernetes#create-tenant-connect-tenant|MinIO API Service]] to create a new [[reference/minio-mc/mc-alias-set#alias|alias]] for the MinIO deployment.
You can then use the [[reference/minio-mc/mc-encrypt-set#command-mc.encrypt.set|`mc encrypt set`]] command to enable SSE-KMS encryption for a bucket:

```
mc alias set k8s https://minio.minio-tenant-1.svc.cluster-domain.example:443 ROOTUSER ROOTPASSWORD

mc mb k8s/encryptedbucket
mc encrypt set SSE-KMS encrypted-bucket-key k8s/encryptedbucket
```

For clients external to the Kubernetes cluster, specify the hostname of the service exposed by Ingress, Load Balancer, or similar Kubernetes network control component.

Write a file to the bucket using [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] or any S3-compatible SDK with a `PutObject` function.
You can then run [[reference/minio-mc/mc-stat#command-mc.stat|`mc stat`]] on the file to confirm the associated encryption metadata.

Baremetal

Generate a KES API Key for use by MinIO

Use the [kes identity new](https://docs.min.io/community/minio-kes/cli/kes-identity/new) command to generate a new API key for use by the MinIO Server:

```
kes identity new
```

The output includes both the API Key for use with MinIO and the Identity hash for use with the [KES Policy configuration](https://docs.min.io/community/minio-kes/tutorials/configuration/#policy-configuration).

Configure the MinIO Environment File

Create or modify the MinIO Server environment file for all hosts in the target deployment to include the following environment variables:

Add the following lines to the MinIO Environment file on each MinIO host.
See the tutorials for [[operations/deployments/installation#minio-snsd|Installation and Management]], [[operations/deployments/installation#minio-snmd|Installation and Management]], or [[operations/deployments/installation#minio-mnmd|Installation and Management]] for more detailed descriptions of a base MinIO environment file.

```
# Add these environment variables to the existing environment file

MINIO_KMS_KES_ENDPOINT=https://HOSTNAME:7373
MINIO_KMS_KES_API_KEY="kes:v1:ACTpAsNoaGf2Ow9o5gU8OmcaG6Af/VcZ1Mt7ysuKoBjv"

# Allows validation of the KES Server Certificate (Self-Signed or Third-Party CA)
# Change this path to the location of the KES CA Path
MINIO_KMS_KES_CAPATH=|kescertpath|/kes-server.cert

# Sets the default KMS key for the backend and SSE-KMS/SSE-S3 Operations)
MINIO_KMS_KES_KEY_NAME=minio-backend-default-key
```

Replace `HOSTNAME` with the IP address or hostname of the KES server.
If the MinIO server host machines cannot resolve or reach the specified `HOSTNAME`, the deployment may return errors or fail to start.

If using a single KES server host, specify the IP or hostname of that host

If using multiple KES server hosts, specify a comma-separated list of IPs or hostnames of each host

MinIO uses the [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]] key for the following cryptographic operations:

Encrypting the MinIO backend (IAM, configuration, etc.)

Encrypting objects using [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]] if the request does not include a specific EK.

Encrypting objects using [[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3|SSE-S3]].

MinIO defaults to expecting this file at `/etc/default/minio`.
If you modified your deployment to use a different location for the environment file, modify the file at that location.

Start MinIO

KES Operations Requires Unsealed Vault

Depending on your selected KMS solution, you may need to unseal the key instance to allow normal cryptographic operations, including key creation or retrieval.
KES requires an unsealed key target to perform its operations.

Refer to the [documentation for your chosen KMS solution](https://docs.min.io/community/minio-kes/#supported-kms-targets) for information regarding whether sealing and unsealing the instance is required for operations.

You must start KES before starting MinIO.
The MinIO deployment requires access to KES as part of its startup.

You can use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart MinIO:

```
mc admin service restart ALIAS
```

Generate a New Encryption Key

MinIO requires that the EK exist on the KMS before performing SSE operations using that key.
Use `kes key create` or [[reference/minio-mc-admin/mc-admin-kms-key#mc.admin.kms.key.create|`mc admin kms key create`]] to add a new EK for use with SSE.

The following command uses the [[reference/minio-mc-admin/mc-admin-kms-key#mc.admin.kms.key.create|`mc admin kms key create`]] command to add a new External Key (EK) stored on the KMS server for use with encrypting the MinIO backend.

```
mc admin kms key create ALIAS KEYNAME
```

Enable SSE-KMS for a Bucket

Use the MinIO [[reference/minio-mc#command-mc|`mc`]] CLI to enable bucket-default SSE-KMS with the generated key:

The following commands:

Create a new [[reference/minio-mc/mc-alias-set#alias|alias]] for the MinIO deployment

Create a new bucket for storing encrypted data

Enable SSE-KMS encryption on that bucket

```
mc alias set local http://127.0.0.1:9000 ROOTUSER ROOTPASSWORD

mc mb local/encryptedbucket
mc encrypt set SSE-KMS encrypted-bucket-key ALIAS/encryptedbucket
```

Write a file to the bucket using [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] or any S3-compatible SDK with a `PutObject` function.
You can then run [[reference/minio-mc/mc-stat#command-mc.stat|`mc stat`]] on the file to confirm the associated encryption metadata.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).