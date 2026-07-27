---
title: "Expand a MinIO Tenant — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/k8s-expand-minio-tenant-on-kubernetes.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/k8s-expand-minio-tenant-on-kubernetes"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Expand a MinIO Tenant — SILO (MinIO Object Storage)"
---
# Expand a MinIO Tenant

This procedure documents expanding the available storage capacity of an existing MinIO tenant by deploying an additional pool of MinIO pods in the Kubernetes infrastructure.

Important

The MinIO Operator Console is deprecated and removed in Operator 6.0.0.

See [[operations/deployments/k8s-modify-minio-tenant-on-kubernetes#minio-k8s-modify-minio-tenant|Modify a MinIO Tenant]] for instructions on migrating Tenants installed via the Operator Console to Kustomization.

## Prerequisites

### MinIO Kubernetes Operator

This procedure on this page requires a valid installation of the MinIO Kubernetes Operator and assumes the local host has a matching installation of the MinIO Kubernetes Operator.
This procedure assumes the latest stable Operator, version 7.1.1.

See [[operations/deployments/kubernetes#deploy-operator-kubernetes|Deploy MinIO on Kubernetes]] for complete documentation on deploying the MinIO Operator.

### Available Worker Nodes

MinIO deploys additional [[reference/minio-server/minio-server#command-minio.server|`minio server`]] pods as part of the new Tenant pool.
The Kubernetes cluster must have sufficient available worker nodes on which to schedule the new pods.

The MinIO Operator provides configurations for controlling pod affinity and anti-affinity to direct scheduling to specific workers.

### Persistent Volumes

Exclusive access to drives

MinIO requires exclusive access to the drives or volumes provided for object storage.
No other processes, software, scripts, or persons should perform any actions directly on the drives or volumes provided to MinIO or the objects or files MinIO places on them.

Unless directed by MinIO Engineering, do not use scripts or tools to directly modify, delete, or move any of the data shards, parity shards, or metadata files on the provided drives, including from one drive or node to another.
Such operations are very likely to result in widespread corruption and data loss beyond MinIO’s ability to heal.

MinIO can use any Kubernetes [Persistent Volume (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes) that supports the [ReadWriteOnce](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes) access mode.
MinIO’s consistency guarantees require the exclusive storage access that `ReadWriteOnce` provides.

For Kubernetes clusters where nodes have Direct Attached Storage, MinIO strongly recommends using the [DirectPV CSI driver](https://min.io/directpv?ref=docs).
DirectPV provides a distributed persistent volume manager that can discover, format, mount, schedule, and monitor drives across Kubernetes nodes.
DirectPV addresses the limitations of manually provisioning and monitoring [local persistent volumes](https://kubernetes.io/docs/concepts/storage/volumes/#local).

Note

MinIO Tenants on EKS must use the [EBS CSI Driver](https://github.com/kubernetes-sigs/aws-ebs-csi-driver) to provision the necessary underlying persistent volumes.
MinIO strongly recommends using SSD-backed EBS volumes for best performance.
For more information on EBS resources, see [EBS Volume Types](https://aws.amazon.com/ebs/volume-types/).

## Procedure

The MinIO Operator supports expanding a MinIO Tenant by adding additional pools.

Kustomization

Review the Kustomization object which describes the Tenant object (`tenant.yaml`).

The `spec.pools` array describes the current pool topology.

Add a new entry to the `spec.pools` array.

The new pool must reflect your intended combination of Worker nodes, volumes per server, storage class, and affinity/scheduler settings.
See [[reference/operator-crd#minio-operator-crd|MinIO Custom Resource Definition]] for more complete documentation on Pool-related configuration settings.

Apply the updated Tenant configuration

Use the `kubectl apply` command to update the Tenant:

```
kubectl apply -k ~/kustomization/TENANT-NAME
```

Modify the path to the Kustomization directory to match your local configuration.

Helm

Review the Helm `values.yaml` file.

The `tenant.pools` array describes the current pool topology.

Add a new entry to the `tenant.pools` array.

The new pool must reflect your intended combination of Worker nodes, volumes per server, storage class, and affinity/scheduler settings.
See [[reference/tenant-chart-values#minio-tenant-chart-values|Tenant Helm Charts]] for more complete documentation on Pool-related configuration settings.

Apply the updated Tenant configuration

Use the `helm upgrade` command to update the Tenant:

```
helm upgrade TENANT-NAME minio-operator/tenant -f values.yaml -n TENANT-NAMESPACE
```

The command above assumes use of the MinIO Operator Chart repository.
If you installed the Chart manually or by using a different repository name, specify that chart or name in the command.

Replace `TENANT-NAME` and `TENANT-NAMESPACE` with the name and namespace of the Tenant respectively.
You can use `helm list -n TENANT-NAMESPACE` to validate the Tenant name.

You can use the `kubectl get events -n TENANT-NAMESPACE --watch` to monitor the progress of expansion.
The MinIO Operator updates services to route connections appropriately across the new nodes.
If you use customized services, routes, ingress, or similar Kubernetes network components, you may need to update those components for the new pod hostname ranges.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).