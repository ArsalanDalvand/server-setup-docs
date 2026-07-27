---
title: "Upgrade a MinIO Tenant — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/k8s-upgrade-minio-tenant-on-kubernetes.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/k8s-upgrade-minio-tenant-on-kubernetes"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Upgrade a MinIO Tenant — SILO (MinIO Object Storage)"
---
# Upgrade a MinIO Tenant

The following procedures upgrade a single MinIO Tenant, using either Kustomize or Helm.
MinIO recommends you test upgrades in a lower environment such as a Dev or QA Tenant, before upgrading production Tenants.

Important

For Tenants using a MinIO Image older than [RELEASE.2024-03-30T09-41-56Z](https://github.com/minio/minio/releases/tag/RELEASE.2024-03-30T09-41-56Z) running with [[reference/minio-server/settings/iam/ldap#minio-ldap-config-settings|AD/LDAP]] enabled, you must read through the release notes for [RELEASE.2024-04-18T19-09-19Z](https://github.com/minio/minio/releases/tag/RELEASE.2024-04-18T19-09-19Z) before starting this procedure.
You must take the extra steps documented in the linked release as part of the upgrade procedure.

## Upgrade a Tenant using Kustomize

The following procedure upgrades a MinIO Tenant using Kustomize and the `kubectl` CLI.
If you deployed the Tenant using [[operations/deployments/k8s-deploy-minio-tenant-helm-on-kubernetes#deploy-tenant-helm|Helm]], use the [Upgrade the Tenant using the MinIO Helm Chart](#minio-upgrade-tenant-helm) procedure instead.

To upgrade a Tenant with Kustomize:

If the tenant was deployed with Operator Console, there are additional steps to create a base configuration file before upgrading.

If the tenant was deployed with Kustomize, the base configuration is your existing `kustomization` files from the original tenant deployment.

Choose a tab below depending on how the tenant was deployed:

Operator Console-Deployed Tenant

Create the base configuration file:

In a convenient directory, save the current Tenant configuration to a file using `kubectl get`:

```
kubectl get tenant/my-tenant -n my-tenant-ns -o yaml > my-tenant-base.yaml
```

Replace `my-tenant` and `my-tenant-ns` with the name and namespace of the Tenant to upgrade.

Edit the file to remove the following lines:

`creationTimestamp:`

`resourceVersion:`

`uid:`

`selfLink:` (if present)

For example, remove the highlighted lines:

```
metadata:
  creationTimestamp: "2024-05-29T21:22:20Z"
  generation: 1
  name: my-tenant
  namespace: my-tenant-ns
  resourceVersion: "4699"
  uid: d5b8e468-3bed-4aa3-8ddb-dfe1ee0362da
```

In the same directory, create a `kustomization.yaml` file with contents resembling the following:

```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- my-tenant-base.yaml

patches:
- path: upgrade-minio-tenant.yaml
```

If you used a different filename for the `kubectl get` output in the previous step, replace `my-tenant-base.yaml` with the name of that file.

Existing Kustomized-deployed Tenant

You can upgrade the tenant using the `kustomization` files from the original deployment as the base configuration.
If you no longer have these files, follow the instructions in the Operator Console-Deployed Tenant tab.

Create a `upgrade-minio-tenant.yaml` file with contents resembling the following:

```
apiVersion: minio.min.io/v2
kind: Tenant

metadata:
  name: my-tenant
  namespace: my-tenant-ns

spec:
  image: minio/minio:MINIOLATEST
```

This file instructs Kustomize to upgrade the tenant using the specified image.
The name of this file, `upgrade-minio-tenant.yaml`, must match the `patches.path` filename specified in the `kustomization.yaml` file created in the previous step.

Replace `my-tenant` and `my-tenant-ns` with the name and namespace of the Tenant to upgrade.
Specify the MinIO version to upgrade to in `image:`.

Alternatively, you can update the base configuration directly, according to your local procedures.
Refer to the [Kustomize Documentation](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization) for more information.

From the same directory as the above files, apply the updated configuration to the Tenant with `kubectl apply`:

```
kubectl apply -k ./
```

The output resembles the following:

```
tenant.minio.min.io/my-tenant configured
```

## Upgrade the Tenant using the MinIO Helm Chart

This procedure upgrades an existing MinIO Tenant using Helm Charts.

If you deployed the Tenant using Kustomize, use the [Upgrade a Tenant using Kustomize](#minio-upgrade-tenant-kustomize) procedure instead.

Verify the existing MinIO Tenant installation.

Use `kubectl get all -n TENANT_NAMESPACE` to verify the health and status of all Tenant pods and services.

Use the `helm list` command to view the installed charts in the namespace:

```
helm list -n TENANT_NAMESPACE
```

The result should resemble the following:

```
NAME            NAMESPACE         REVISION        UPDATED                                 STATUS          CHART           APP VERSION
CHART_NAME      TENANT_NAMESPACE  1               2023-11-01 15:49:58.810412732 -0400 EDT deployed        tenant-5.0.x   v5.0.x
```

Update the Operator Repository

Use `helm repo update minio-operator` to update the MinIO Operator repo.
If you set a different alias for the MinIO Operator repository, specify that to the command.
You can use `helm repo list` to review your installed repositories.

Use `helm search` to check the latest available chart version after updating the Operator Repo:

```
helm search repo minio-operator
```

The response should resemble the following:

```
NAME                            CHART VERSION   APP VERSION     DESCRIPTION
minio-operator/minio-operator   4.3.7           v4.3.7          A Helm chart for MinIO Operator
minio-operator/operator         7.1.1          v7.1.1         A Helm chart for MinIO Operator
minio-operator/tenant           7.1.1          v7.1.1         A Helm chart for MinIO Operator
```

The `minio-operator/minio-operator` is a legacy chart and should not be installed under normal circumstances.

Run `helm upgrade`

Helm uses the latest chart to upgrade the Tenant:

```
helm upgrade -n minio-tenant \
  CHART_NAME minio-operator/tenant
```

The command results should return success with a bump in the `REVISION` value.

Validate the Tenant Upgrade

Check that all services and pods are online and functioning normally.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).