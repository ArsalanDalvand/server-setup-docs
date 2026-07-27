---
title: "Upgrade MinIO Operator — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/k8s-upgrade-minio-operator-kubernetes.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/k8s-upgrade-minio-operator-kubernetes"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Upgrade MinIO Operator — SILO (MinIO Object Storage)"
---
# Upgrade MinIO Operator

You can upgrade the MinIO Operator at any time without impacting your managed MinIO Tenants.

As part of the upgrade process, the Operator may update and restart Tenants to support changes to the MinIO Custom Resource Definition (CRD).
These changes require no action on the part of any operator or administrator, and do not impact Tenant operations.

This page describes how to upgrade from Operator 5.0.15 to 7.1.1.
See [[operations/deployments/k8s-upgrade-minio-operator-4.5.7-earlier#minio-k8s-upgrade-minio-operator-to-5-0-15|Upgrade MinIO Operator 4.5.8 and Later to 5.0.15]] for instructions on upgrading to Operator 5.0.15 before starting this procedure.

Operator 6.0.0 Deprecates the Operator Console

Starting with Operator 6.0.0, the MinIO Operator Console is deprecated and removed.

You can continue to manage and deploy MinIO Tenants using standard Kubernetes approaches such as Kustomize or Helm.

## Upgrade MinIO Operator 5.0.15 to 7.1.1

Important

Operator 6.0.0 deprecates the MinIO Operator Console and removes the related resources from the MinIO Operator CRD.
This includes removal of Operator Console resources such as services and pods.

Use either Kustomization or Helm for managing Tenants moving forward.

Upgrade using Kustomize

The following procedure upgrades the MinIO Operator using Kustomize.
For deployments using Operator 5.0.0 through 5.0.14, follow the [[operations/deployments/k8s-upgrade-minio-operator-4.5.7-earlier#minio-k8s-upgrade-minio-operator-to-5-0-15|Upgrade MinIO Operator 4.5.8 and Later to 5.0.15]] procedure before performing this upgrade.

If you installed the Operator using [[operations/deployments/k8s-deploy-operator-helm-on-kubernetes#minio-k8s-deploy-operator-helm|Helm]], use the Upgrade using Helm instructions instead.

(Optional) Update each MinIO Tenant to the latest stable MinIO Version.

Upgrading MinIO regularly ensures your Tenants have the latest features and performance improvements.
Test upgrades in a lower environment such as a Dev or QA Tenant, before applying to your production Tenants.
See [[operations/deployments/k8s-upgrade-minio-tenant-on-kubernetes#minio-k8s-upgrade-minio-tenant|Upgrade a MinIO Tenant]] for a procedure on upgrading MinIO Tenants.

Verify the existing Operator installation.
Use `kubectl get all -n minio-operator` to verify the health and status of all Operator pods and services.

If you installed the Operator to a custom namespace, specify that namespace as `-n <NAMESPACE>`.

You can verify the currently installed Operator version by retrieving the object specification for an operator pod in the namespace.
The following example uses the `jq` tool to filter the necessary information from `kubectl`:

```
kubectl get pod -l 'name=minio-operator' -n minio-operator -o json | jq '.items[0].spec.containers'
```

The output resembles the following:

```
{
   "env": [
      {
         "name": "CLUSTER_DOMAIN",
         "value": "cluster.local"
      }
   ],
   "image": "minio/operator:v5.0.15",
   "imagePullPolicy": "IfNotPresent",
   "name": "minio-operator"
}
```

If your local host does not have the `jq` utility installed, you can run the first part of the command and locate the `spec.containers` section of the output.

Upgrade Operator with Kustomize

The following command upgrades Operator to version 7.1.1:

```
kubectl apply -k github.com/minio/operator
```

In the sample output below, `configured` indicates where a new change was applied from the updated CRD:

```
namespace/minio-operator unchanged
customresourcedefinition.apiextensions.k8s.io/miniojobs.job.min.io configured
customresourcedefinition.apiextensions.k8s.io/policybindings.sts.min.io configured
customresourcedefinition.apiextensions.k8s.io/tenants.minio.min.io configured
serviceaccount/minio-operator unchanged
clusterrole.rbac.authorization.k8s.io/minio-operator-role configured
clusterrolebinding.rbac.authorization.k8s.io/minio-operator-binding unchanged
service/operator unchanged
service/sts unchanged
deployment.apps/minio-operator configured
```

Validate the Operator upgrade

You can check the new Operator version with the same `kubectl` command used previously:

```
kubectl get pod -l 'name=minio-operator' -n minio-operator -o json | jq '.items[0].spec.containers'
```

Upgrade using Helm

The following procedure upgrades an existing MinIO Operator Installation using Helm.

If you installed the Operator using Kustomize, use the Upgrade using Kustomize instructions instead.

(Optional) Update each MinIO Tenant to the latest stable MinIO Version.

Upgrading MinIO regularly ensures your Tenants have the latest features and performance improvements.
Test upgrades in a lower environment such as a Dev or QA Tenant, before applying to your production Tenants.
See [[operations/deployments/k8s-upgrade-minio-tenant-on-kubernetes#minio-k8s-upgrade-minio-tenant|Upgrade a MinIO Tenant]] for a procedure on upgrading MinIO Tenants.

Verify the existing Operator installation.

Use `kubectl get all -n minio-operator` to verify the health and status of all Operator pods and services.

If you installed the Operator to a custom namespace, specify that namespace as `-n <NAMESPACE>`.

Use the `helm list` command to view the installed charts in the namespace:

```
helm list -n minio-operator
```

The result should resemble the following:

```
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
operator        minio-operator  1               2023-11-01 15:49:54.539724775 -0400 EDT deployed        operator-5.0.x v5.0.x
```

Update the Operator Repository

Use `helm repo update minio-operator` to update the MinIO Operator repo.
If you set a different alias for the MinIO Operator repository, specify that in the command instead of `minio-operator`.
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

Helm uses the latest chart to upgrade the MinIO Operator:

```
helm upgrade -n minio-operator \
operator minio-operator/operator
```

If you installed the MinIO Operator to a different namespace, specify that in the `-n` argument.

If you used a different installation name from `operator`, replace the value above with the installation name.

The command results should return success with a bump in the `REVISION` value.

Validate the Operator upgrade

You can check the new Operator version with the same `kubectl` command used previously:

```
kubectl get pod -l 'name=minio-operator' -n minio-operator -o json | jq '.items[0].spec.containers'
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).