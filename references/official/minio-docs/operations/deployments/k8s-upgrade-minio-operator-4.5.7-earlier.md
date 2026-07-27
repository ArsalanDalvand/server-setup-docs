---
title: "Upgrade Legacy MinIO Operators — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/k8s-upgrade-minio-operator-4.5.7-earlier.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/k8s-upgrade-minio-operator-4.5.7-earlier"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Upgrade Legacy MinIO Operators — SILO (MinIO Object Storage)"
---
# Upgrade Legacy MinIO Operators

MinIO supports the following upgrade paths for older versions of the MinIO Operator:

Current Version

Supported Upgrade Target

5.0.15 or later

7.1.1

5.0.0 to 5.0.14

5.0.15

4.2.3 to 4.5.7

4.5.8

4.0.0 through 4.2.2

4.2.3

3.X.X

4.2.2

To upgrade from Operator to 7.1.1 from version 4.5.7 or earlier, you must first upgrade to version 4.5.8, then upgrade to 5.0.15.
Depending on your current version, you may need to do one or more intermediate upgrades to reach v4.5.8.

After upgrading to 5.0.15, see [[operations/deployments/k8s-upgrade-minio-operator-kubernetes#minio-k8s-upgrade-minio-operator|Upgrade MinIO Operator]] to upgrade to the latest version.

## Upgrade MinIO Operator 4.5.8 and Later to 5.0.15

Prerequisites

This procedure requires the following:

You have an existing MinIO Operator deployment running 4.5.8 or later

Your Kubernetes cluster runs 1.21.0 or later

Your local host has `kubectl` installed and configured with access to the Kubernetes cluster

This procedure upgrades the MinIO Operator from any 4.5.8 or later release to 5.0.15

### Tenant Custom Resource Definition Changes

The following changes apply for Operator v5.0.0 or later:

The `.spec.s3` field is replaced by the `.spec.features` field.

The `.spec.credsSecret` field is replaced by the `.spec.configuration` field.

The `.spec.credsSecret` should hold all the environment variables for the MinIO deployment that contain sensitive information and should not show in `.spec.env`.
This change impacts the Tenant CRD and only impacts users editing a tenant YAML directly, such as through Helm or Kustomize.

Both the Log Search API (`.spec.log`) and Prometheus (`.spec.prometheus`) deployments have been removed.
However, existing deployments are left running as standalone deployments / statefulsets with no connection to the Tenant CR.
Deleting the Tenant CRD does not cascade to the log or Prometheus deployments.

Important

MinIO recommends that you create a yaml file to manage these deployments going forward.

### Log Search and Prometheus

The latest releases of Operator remove Log Search and Prometheus from included Operator tools.
The following steps back up the existing yaml files, perform some clean up, and provide steps to continue using either or both of these functions.

Back up Prometheus and Log Search yaml files.

```
export TENANT_NAME=myminio
export NAMESPACE=mynamespace
kubectl -n $NAMESPACE get secret $TENANT_NAME-log-secret -o yaml > $TENANT_NAME-log-secret.yaml
kubectl -n $NAMESPACE get cm $TENANT_NAME-prometheus-config-map -o yaml > $TENANT_NAME-prometheus-config-map.yaml
kubectl -n $NAMESPACE get sts $TENANT_NAME-prometheus -o yaml > $TENANT_NAME-prometheus.yaml
kubectl -n $NAMESPACE get sts $TENANT_NAME-log -o yaml > $TENANT_NAME-log.yaml
kubectl -n $NAMESPACE get deployment $TENANT_NAME-log-search-api -o yaml > $TENANT_NAME-log-search-api.yaml
kubectl -n $NAMESPACE get svc $TENANT_NAME-log-hl-svc -o yaml > $TENANT_NAME-log-hl-svc.yaml
kubectl -n $NAMESPACE get svc $TENANT_NAME-log-search-api -o yaml > $TENANT_NAME-log-search-api-svc.yaml
kubectl -n $NAMESPACE get svc $TENANT_NAME-prometheus-hl-svc -o yaml > $TENANT_NAME-prometheus-hl-svc.yaml
```

Replace `myminio` with the name of the tenant on the operator deployment you are upgrading.

Replace `mynamespace` with the namespace for the tenant on the operator deployment you are upgrading.

Repeat for each tenant.

Remove `.metadata.ownerReferences` for all backed up files for all tenants.

(Optional) To continue using Log Search API and Prometheus, add the following variables to the tenant’s yaml specification file under `.spec.env`

Use the following command to edit a tenant:

```
kubectl edit tenants <TENANT-NAME> -n <TENANT-NAMESPACE>
```

Replace `<TENANT-NAME>` with the name of the tenant to modify.

Replace `<TENANT-NAMESPACE>` with the namespace of the tenant you are modifying.

Add the following values under `.spec.env` in the file:

```
- name: MINIO_LOG_QUERY_AUTH_TOKEN
  valueFrom:
    secretKeyRef:
      key: MINIO_LOG_QUERY_AUTH_TOKEN
      name: <TENANT_NAME>-log-secret
- name: MINIO_LOG_QUERY_URL
  value: http://<TENANT_NAME>-log-search-api:8080
- name: MINIO_PROMETHEUS_JOB_ID
  value: minio-job
- name: MINIO_PROMETHEUS_URL
  value: http://<TENANT_NAME>-prometheus-hl-svc:9001
```

Replace `<TENANT_NAME>` in the `name` or `value` lines with the name of your tenant.

### Procedure

Upgrade using Kustomize

The following procedure upgrades the MinIO Operator using Kustomize.

For Operator versions 5.0.1 to 5.0.14 installed with the MinIO Kubernetes Plugin, follow the Kustomize instructions below to upgrade to 5.0.15 or later.
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
   "image": "minio/operator:v5.0.x",
   "imagePullPolicy": "IfNotPresent",
   "name": "minio-operator"
}
```

If your local host does not have the `jq` utility installed, you can run the first part of the command and locate the `spec.containers` section of the output.

Upgrade Operator with Kustomize

The following command upgrades Operator to version 5.0.15:

```
kubectl apply -k github.com/minio/operator/?ref=v5.0.15
```

In the sample output below, `configured` at the end of the line indicates where a new change was applied from the updated CRD:

```
namespace/minio-operator configured
customresourcedefinition.apiextensions.k8s.io/miniojobs.job.min.io configured
customresourcedefinition.apiextensions.k8s.io/policybindings.sts.min.io configured
customresourcedefinition.apiextensions.k8s.io/tenants.minio.min.io configured
serviceaccount/console-sa unchanged
serviceaccount/minio-operator unchanged
clusterrole.rbac.authorization.k8s.io/console-sa-role unchanged
clusterrole.rbac.authorization.k8s.io/minio-operator-role unchanged
clusterrolebinding.rbac.authorization.k8s.io/console-sa-binding unchanged
clusterrolebinding.rbac.authorization.k8s.io/minio-operator-binding unchanged
configmap/console-env unchanged
secret/console-sa-secret configured
service/console unchanged
service/operator unchanged
service/sts unchanged
deployment.apps/console configured
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

You can also introspect the operator pods directly to determine the installed version.
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
   "image": "minio/operator:v5.0.x",
   "imagePullPolicy": "IfNotPresent",
   "name": "minio-operator"
}
```

If your local host does not have the `jq` utility installed, you can run the first part of the command and locate the `spec.containers` section of the output.

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

## Upgrade MinIO Operator 4.2.3 through 4.5.7 to 4.5.8

### Prerequisites

This procedure requires the following:

You have an existing MinIO Operator deployment running 4.2.3 through 4.5.7

Your Kubernetes cluster runs 1.19.0 or later

Your local host has `kubectl` installed and configured with access to the Kubernetes cluster

### Procedure

This procedure upgrades MinIO Operator release 4.2.3 through 4.5.7 to release 4.5.8.
You can then upgrade from release 4.5.8 to 5.0.15.

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
   "image": "minio/operator:v4.5.1",
   "imagePullPolicy": "IfNotPresent",
   "name": "minio-operator"
}
```

Download the Latest Stable Version of the MinIO Kubernetes Plugin

You can install the MinIO plugin using either the Kubernetes Krew plugin manager or manually by downloading and installing the plugin binary to your local host:

Krew Plugin Manager

Krew is a `kubectl` plugin manager developed by the [Kubernetes SIG CLI group](https://github.com/kubernetes-sigs).
See the `krew` [installation documentation](https://krew.sigs.k8s.io/docs/user-guide/setup/install/) for specific instructions.
You can use the Krew plugin for Linux, macOS, and Windows operating systems.

You can use Krew to install the MinIO `kubectl` plugin using the following commands:

```
kubectl krew update
kubectl krew install minio
```

If you want to update the MinIO plugin with Krew, use the following command:

```
kubectl krew upgrade minio
```

Manual (Linux, MacOS)

You can download the MinIO `kubectl` plugin to your local system path.
The `kubectl` CLI automatically discovers and runs compatible plugins.

The following code downloads the most recent version of the MinIO Kubernetes plugin and installs it to the system path:

```
curl https://github.com/minio/operator/releases/download/v5.0.14/kubectl-minio_5.0.14_linux_amd64 -o kubectl-minio
chmod +x kubectl-minio
mv kubectl-minio /usr/local/bin/
```

The `mv` command above may require `sudo` escalation depending on the permissions of the authenticated user.

Run the following command to verify installation of the plugin:

```
kubectl minio version
```

The output should display the Operator version as 5.0.14.

Manual (Windows)

You can download the MinIO `kubectl` plugin to your local system path.
The `kubectl` CLI automatically discovers and runs compatible plugins.

The following PowerShell command downloads the most recent version of the MinIO Kubernetes plugin and installs it to the system path:

```
Invoke-WebRequest -Uri "https://github.com/minio/operator/releases/download/v5.0.14/kubectl-minio_5.0.14_windows_amd64.exe" -OutFile "C:\kubectl-plugins\kubectl-minio.exe"
```

Ensure the path to the plugin folder is included in the Windows PATH.

Run the following command to verify installation of the plugin:

```
kubectl minio version
```

The output should display the Operator version as 5.0.14.

Run the initialization command to upgrade the Operator

Use the `kubectl minio init` command to upgrade the existing MinIO Operator installation

```
kubectl minio init
```

Validate the Operator upgrade

You can check the Operator version by reviewing the object specification for an Operator Pod using a previous step.

## Upgrade MinIO Operator 4.0.0 through 4.2.2 to 4.2.3

### Prerequisites

This procedure assumes that:

You have an existing MinIO Operator deployment running any release from 4.0.0 through 4.2.2

Your Kubernetes cluster runs 1.19.0 or later

Your local host has `kubectl` installed and configured with access to the Kubernetes cluster

### Procedure

This procedure covers the necessary steps to upgrade a MinIO Operator deployment running any release from 4.0.0 through 4.2.2 to 4.2.3.
You can then perform [[operations/deployments/k8s-upgrade-minio-operator-kubernetes#minio-k8s-upgrade-minio-operator-procedure|Upgrade MinIO Operator 5.0.15 to 7.1.1]] to complete the upgrade to 7.1.1.

There is no direct upgrade path for 4.0.0 - 4.2.2 installations to 7.1.1.

(Optional) Update each MinIO Tenant to the latest stable MinIO Version.

Upgrading MinIO regularly ensures your Tenants have the latest features and performance improvements.
Test upgrades in a lower environment such as a Dev or QA Tenant, before applying to your production Tenants.

See [[operations/deployments/k8s-upgrade-minio-tenant-on-kubernetes#minio-k8s-upgrade-minio-tenant|Upgrade a MinIO Tenant]] for a procedure on upgrading MinIO Tenants.

Check the Security Context for each Tenant Pool

Use the following command to validate the specification for each managed MinIO Tenant:

```
kubectl get tenants <TENANT-NAME> -n <TENANT-NAMESPACE> -o yaml
```

If the `spec.pools.securityContext` field does not exist for a Tenant, the tenant pods likely run as root.

As part of the 4.2.3 and later series, pods run with a limited permission set enforced as part of the Operator upgrade.
However, Tenants running pods as root may fail to start due to the security context mismatch.
You can set an explicit Security Context that allows pods to run as root for those Tenants:

```
securityContext:
  runAsUser: 0
  runAsGroup: 0
  runAsNonRoot: false
  fsGroup: 0
```

You can use the following command to edit the tenant and apply the changes:

```
kubectl edit tenants <TENANT-NAME> -n <TENANT-NAMESPACE>
# Modify the securityContext as needed
```

See [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/) for more information on Kubernetes Security Contexts.

Upgrade to Operator 4.2.3

Download the MinIO Kubernetes Plugin 4.2.3 and use it to upgrade the Operator.
Open [https://github.com/minio/operator/releases/tag/v4.2.3](https://github.com/minio/operator/releases/tag/v4.2.3) in a browser and download the binary that corresponds to your local host OS.

For example, Linux hosts running an Intel or AMD processor can run the following commands:

```
wget https://github.com/minio/operator/releases/download/v4.2.3/kubectl-minio_4.2.3_linux_amd64 -o kubectl-minio_4.2.3
chmod +x kubectl-minio_4.2.3
./kubectl-minio_4.2.3 init
```

Validate all Tenants and Operator pods

Check the Operator and MinIO Tenant namespaces to ensure all pods and services started successfully.

For example:

```
kubectl get all -n minio-operator
kubectl get pods -l "v1.min.io/tenant" --all-namespaces
```

Upgrade to 7.1.1

Follow the [[operations/deployments/k8s-upgrade-minio-operator-kubernetes#minio-k8s-upgrade-minio-operator-procedure|Upgrade MinIO Operator 5.0.15 to 7.1.1]] procedure to upgrade to the latest stable Operator version.

## Upgrade MinIO Operator 3.0.0 through 3.0.29 to 4.2.2

### Prerequisites

This procedure assumes that:

You have an existing MinIO Operator deployment running 3.X.X

Your Kubernetes cluster runs 1.19.0 or later

Your local host has `kubectl` installed and configured with access to the Kubernetes cluster

### Procedure

This procedure covers the necessary steps to upgrade a MinIO Operator deployment running any release from 3.0.0 through 3.2.9 to 4.2.2.
You can then perform [Upgrade MinIO Operator 4.0.0 through 4.2.2 to 4.2.3](#minio-k8s-upgrade-minio-operator-4-2-2-procedure), followed by [[operations/deployments/k8s-upgrade-minio-operator-kubernetes#minio-k8s-upgrade-minio-operator-procedure|Upgrade MinIO Operator 5.0.15 to 7.1.1]].

There is no direct upgrade path from a 3.X.X series installation to 7.1.1.

(Optional) Update each MinIO Tenant to the latest stable MinIO Version.

Upgrading MinIO regularly ensures your Tenants have the latest features and performance improvements.

Test upgrades in a lower environment such as a Dev or QA Tenant, before applying to your production Tenants.

See [[operations/deployments/k8s-upgrade-minio-tenant-on-kubernetes#minio-k8s-upgrade-minio-tenant|Upgrade a MinIO Tenant]] for a procedure on upgrading MinIO Tenants.

Validate the Tenant `tenant.spec.zones` values

Use the following command to validate the specification for each managed MinIO Tenant:

```
kubectl get tenants <TENANT-NAME> -n <TENANT-NAMESPACE> -o yaml
```

Ensure each `tenant.spec.zones` element has a `name` field set to the name for that zone.
Each zone must have a unique name for that Tenant, such as `zone-0` and `zone-1` for the first and second zones respectively.

Ensure each `tenant.spec.zones` has an explicit `securityContext` describing the permission set with which pods run in the cluster.

The following example tenant YAML fragment sets the specified fields:

```
image: "minio/minio:$(LATEST-VERSION)"
...
zones:
- servers: 4
  name: "zone-0"
  volumesPerServer: 4
  volumeClaimTemplate:
     metadata:
     name: data
     spec:
     accessModes:
        - ReadWriteOnce
     resources:
        requests:
           storage: 1Ti
  securityContext:
     runAsUser: 0
     runAsGroup: 0
     runAsNonRoot: false
     fsGroup: 0
- servers: 4
  name: "zone-1"
  volumesPerServer: 4
  volumeClaimTemplate:
     metadata:
     name: data
     spec:
     accessModes:
        - ReadWriteOnce
     resources:
        requests:
           storage: 1Ti
  securityContext:
     runAsUser: 0
     runAsGroup: 0
     runAsNonRoot: false
     fsGroup: 0
```

You can use the following command to edit the tenant and apply the changes:

```
kubectl edit tenants <TENANT-NAME> -n <TENANT-NAMESPACE>
```

Upgrade to Operator 4.2.2

Download the MinIO Kubernetes Plugin 4.2.2 and use it to upgrade the Operator.
Open [https://github.com/minio/operator/releases/tag/v4.2.2](https://github.com/minio/operator/releases/tag/v4.2.2) in a browser and download the binary that corresponds to your local host OS.
For example, Linux hosts running an Intel or AMD processor can run the following commands:

```
wget https://github.com/minio/operator/releases/download/v4.2.3/kubectl-minio_4.2.2_linux_amd64 -o kubectl-minio_4.2.2
chmod +x kubectl-minio_4.2.2

./kubectl-minio_4.2.2 init
```

Validate all Tenants and Operator pods

Check the Operator and MinIO Tenant namespaces to ensure all pods and services started successfully.

For example:

```
kubectl get all -n minio-operator

kubectl get pods -l "v1.min.io/tenant" --all-namespaces
```

Upgrade to 4.2.3

Follow the [Upgrade MinIO Operator 4.0.0 through 4.2.2 to 4.2.3](#minio-k8s-upgrade-minio-operator-4-2-2-procedure) procedure to upgrade to Operator 4.2.3.
You can then upgrade to 7.1.1.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).