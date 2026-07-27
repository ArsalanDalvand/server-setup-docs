---
title: "Enable TLS for MinIO — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/network-encryption/enable-minio-tls.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/network-encryption/enable-minio-tls"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Enable TLS for MinIO — SILO (MinIO Object Storage)"
---
# Enable TLS for MinIO

MinIO supports Transport Layer Security (TLS) 1.2+ encryption of incoming and outgoing traffic.

Kubernetes

The MinIO Operator supports the following approaches to enabling TLS on a MinIO Tenant:

Automatic TLS provisioning using Kubernetes Cluster Signing Certificates

User-specified TLS using Kubernetes secrets

Certmanager-managed TLS certificates

Baremetal

MinIO automatically detects TLS certificates in the configured or default directory and starts with TLS enabled.

This procedure documents enabling TLS for a single domain in MinIO.
For instructions on TLS for multiple domains, see TODO

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

This procedure also assumes SSH or similar shell-level access with administrative permissions to each MinIO host server.

### TLS Certificates

Provision the necessary TLS certificates with a [[operations/network-encryption#minio-tls-supported-cipher-suites|supported cipher suite]] for use by MinIO.

Kubernetes

See [[operations/network-encryption#minio-tls-kubernetes|MinIO TLS on Kubernetes]] for more complete guidance on the supported Tenant TLS configurations.

Baremetal

Provision certificate susing your preferred path, such as through your organizations internal Certificate Authority or by using a well-known global provider such as Digicert or Verisign.

You can create self-signed certificates using `openssl` or the MinIO [certgen](https://github.com/minio/certgen) tool.

For example, the following command generates a self-signed certificate with a set of IP and DNS Subject Alternate Names (SANs) associated to the MinIO Server hosts:

```
certgen -host "localhost,minio-*.example.net"
```

See [[operations/network-encryption#minio-tls-baremetal|MinIO TLS on Baremetal]] for more complete guidance on certificate generation and placement.

## Procedure

Kubernetes

The MinIO Operator supports three methods of TLS certificate management on MinIO Tenants:

MinIO automatic TLS certificate generation

`cert-manager` managed TLS certificates

User managed TLS certificates

You can use any combination of the above methods to enable and configure TLS.
MinIO strongly recommends using `cert-manager` for user-specified certificates for a streamlined management and renewal proces.

You can also deploy MinIO Tenants without TLS enabled.

MinIO Auto-TLS

The following steps apply to both new and existing MinIO Deployments using `Kustomize`:

Review the [[reference/operator-crd#minio-operator-crd|Tenant CRD]] `TenantSpec.requestAutoCert` and `TenantSpec.certConfig` fields.

For existing MinIO Tenants, review the Kustomize resources used to create the Tenant and introspect those fields and their current configuration, if any.

Create or Modify your Tenant YAML to set the values of `requestAutoCert` and `certConfig` as necessary.
For example:

```
spec:
   requestAutoCert: true
   certConfig:
     commonName: "CN=MinioTenantCommonName"
     organizationName: "O=MyOrganizationName"
     dnsNames:
       - '*.minio-tenant.domain.tld'
```

See the [Kustomize Tenant base YAML](https://github.com/minio/operator/blob/master/examples/kustomization/base/tenant.yaml) for a baseline template for guidance in creating or modifying your Tenant resource.

Apply the new Kustomization template

Once you apply the changes, the MinIO Operator automatically redeploys the Tenant with the updated configuration.

CertManager

The following steps apply to both new and existing MinIO Deployments using `Kustomize`:

Review the [[reference/operator-crd#minio-operator-crd|Tenant CRD]] `TenantSpec.externalCertsCecret` fields

For existing MinIO Tenants, review the Kustomize resources used to create the Tenant and introspect that field’s current configuration, if any.

Create or Modify your Tenant YAML to reference the appropriate `cert-manager` resource.

For example, the following Tenant YAML fragment references a cert-manager resource `myminio-tls`:

```
apiVersion: minio.min.io/v2
kind: Tenant
metadata:
name: myminio
namespace: minio-tenant
spec:
   ## Disable default tls certificates.
   requestAutoCert: false
   ## Use certificates generated by cert-manager.
   externalCertSecret:
      - name: myminio-tls
         type: cert-manager.io/v1
```

Apply the new Kustomization Template

Once you apply the changes, the MinIO Operator automatically redeploys the Tenant with the updated configuration.

User-Managed

The following steps apply to both new and existing MinIO deployments using `Kustomize`:

Review the [[reference/operator-crd#minio-operator-crd|Tenant CRD]] `TenantSpec.externalCertSecret` field.

For existing MinIO Tenants, review the Kustomize resources used to create the Tenant and introspect that field’s current configuration, if any.

Create or modify your Tenant YAML to reference a secret of type `kubernetes.io/tls`:

For example, the following Tenant YAML fragment references a TLS secret which covers the domain on which the MinIO Tenant accepts connections.

```
apiVersion: minio.min.io/v2
kind: Tenant
metadata:
name: myminio
namespace: minio-tenant
spec:
   ## Disable default tls certificates.
   requestAutoCert: false
   ## Use certificates generated by cert-manager.
   externalCertSecret:
   - name: domain-certificate
     type: kubernetes.io/tls
```

Apply the new Kustomization Template

Once you apply the changes, the MinIO Operator automatically redeploys the Tenant with the updated configuration.

Baremetal

The MinIO Server searches for TLS keys and certificates for each node and uses those credentials for enabling TLS.
MinIO automatically enables TLS upon discovery and validation of certificates.
The search location depends on your MinIO configuration:

Default Path

By default, the MinIO server looks for the TLS keys and certificates for each node in the following directory:

```
${HOME}/.minio/certs
```

Where `${HOME}` is the home directory of the user running the MinIO Server process.
You may need to create the `${HOME}/.minio/certs` directory if it does not exist.

For `systemd` managed deployments this must correspond to the `USER` running the MinIO process.
If that user has no home directory, use the Custom Path option instead.

Custom Path

You can specify a path for the MinIO server to search for certificates using the [[reference/minio-server/minio-server#minio.server.-certs-dir|`minio server --certs-dir`]] or `-S` parameter.

For example, the following command fragment directs the MinIO process to use the `/opt/minio/certs` directory for TLS certificates.

```
minio server --certs-dir /opt/minio/certs ...
```

The user running the MinIO service must have read and write permissions to this directory.

Place the TLS certificates for the default domain (e.g. `minio.example.net`) in the `/certs` directory, with the private key as `private.key` and public certificate as `public.crt`.

For example:

```
/path/to/certs
private.key
public.crt
```

You can use the MinIO [certgen](https://github.com/minio/certgen) to mint self-signed certificates for evaluating MinIO with TLS enabled.
For example, the following command generates a self-signed certificate with a set of IP and DNS Subject Alternate Names (SANs) associated to the MinIO Server hosts:

```
certgen -host "localhost,minio-*.example.net"
```

Place the generated `public.crt` and `private.key` into the `/path/to/certs` directory to enable TLS for the MinIO deployment.
Applications can use the `public.crt` as a trusted Certificate Authority to allow connections to the MinIO deployment without disabling certificate validation.

If you are reconfiguring an existing deployment that did not previously have TLS enabled, update [[reference/minio-server/settings/core#envvar.MINIO_VOLUMES|`MINIO_VOLUMES`]] to specify `https` instead of `http`.
You may also need to update URLs used by applications or clients.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).