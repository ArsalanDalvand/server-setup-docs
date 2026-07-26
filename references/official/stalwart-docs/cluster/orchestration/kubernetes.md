---
title: "Kubernetes"
source: "https://stalw.art/docs/cluster/orchestration/kubernetes"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/orchestration/kubernetes"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Kubernetes"
---
# Kubernetes

      

Kubernetes deployments of Stalwart are driven by a Helm chart that installs the server as a StatefulSet, exposes the standard mail listeners (SMTP, submission, IMAP, POP3, ManageSieve) alongside the HTTP management listener, and provisions persistent storage for the data volume. The StatefulSet shape is used (rather than a plain Deployment) so that each replica keeps a stable hostname and its own PersistentVolumeClaim, which matters when the [[ref/object/data-store|DataStore]] (found in the WebUI under  Settings ›  Storage › Data Store) is a local backend such as RocksDB and when the cluster’s [[cluster/configuration/node-id|node-id lease]] depends on a stable hostname.

The chart below is a minimal, complete reference. It can be committed to a chart repository, packaged, and installed with `helm install`, or used as a starting point for a site-specific chart.

## Liveness and readiness endpoints

[Section titled “Liveness and readiness endpoints”](#liveness-and-readiness-endpoints)

Kubernetes probes check that containers are running and ready to serve traffic. Stalwart exposes two HTTP endpoints for this purpose on the management listener:

Liveness: `/healthz/live`. A failing liveness probe causes the container to be restarted.
Readiness: `/healthz/ready`. A failing readiness probe causes the pod to be removed from Service endpoints until it recovers.

Both endpoints are wired into the StatefulSet template below.

## Chart layout

[Section titled “Chart layout”](#chart-layout)

The chart follows the standard Helm layout. A typical tree looks like this:

```
stalwart/├── Chart.yaml├── values.yaml├── .helmignore└── templates/    ├── _helpers.tpl    ├── configmap.yaml    ├── secret.yaml    ├── statefulset.yaml    ├── service.yaml    └── ingress.yaml
```

### Chart.yaml

[Section titled “Chart.yaml”](#chartyaml)

```
apiVersion: v2name: stalwartdescription: Helm chart for the Stalwart mail and collaboration servertype: applicationversion: 0.1.0appVersion: "v0.16"home: https://stalw.artsources:  - https://github.com/stalwartlabs/stalwartmaintainers:  - name: Stalwart Labs    url: https://stalw.art
```

### templates/_helpers.tpl

[Section titled “templates/_helpers.tpl”](#templates_helperstpl)

```
{{/* Expand the name of the chart. */}}{{- define "stalwart.name" -}}{{- default .Chart.Name .Values.nameOverride | trunc 63 | trimSuffix "-" -}}{{- end -}}
{{- define "stalwart.fullname" -}}{{- if .Values.fullnameOverride -}}{{- .Values.fullnameOverride | trunc 63 | trimSuffix "-" -}}{{- else -}}{{- printf "%s-%s" .Release.Name (include "stalwart.name" .) | trunc 63 | trimSuffix "-" -}}{{- end -}}{{- end -}}
{{- define "stalwart.labels" -}}app.kubernetes.io/name: {{ include "stalwart.name" . }}app.kubernetes.io/instance: {{ .Release.Name }}app.kubernetes.io/managed-by: {{ .Release.Service }}app.kubernetes.io/version: {{ .Chart.AppVersion | quote }}helm.sh/chart: {{ printf "%s-%s" .Chart.Name .Chart.Version | replace "+" "_" | trunc 63 | trimSuffix "-" }}{{- end -}}
{{- define "stalwart.selectorLabels" -}}app.kubernetes.io/name: {{ include "stalwart.name" . }}app.kubernetes.io/instance: {{ .Release.Name }}{{- end -}}
```

### templates/configmap.yaml

[Section titled “templates/configmap.yaml”](#templatesconfigmapyaml)

The ConfigMap carries the `config.json` file that Stalwart reads at startup. It only describes the DataStore; every other setting lives in the database and is edited through the WebUI or the CLI. See the [[jozve/server-setup-docs/references/official/foundationdb-docs/configuration|configuration overview]] for the rationale.

```
apiVersion: v1kind: ConfigMapmetadata:  name: {{ include "stalwart.fullname" . }}-config  labels:    {{- include "stalwart.labels" . | nindent 4 }}data:  config.json: |{{ .Values.config | toPrettyJson | indent 4 }}
```

### templates/secret.yaml

[Section titled “templates/secret.yaml”](#templatessecretyaml)

The Secret holds environment-variable values that should not be stored in a ConfigMap: the recovery administrator credential, and any external store credentials referenced from the setup wizard.

```
apiVersion: v1kind: Secretmetadata:  name: {{ include "stalwart.fullname" . }}-env  labels:    {{- include "stalwart.labels" . | nindent 4 }}type: OpaquestringData:  {{- if .Values.recoveryAdmin.enabled }}  STALWART_RECOVERY_ADMIN: {{ printf "%s:%s" .Values.recoveryAdmin.username .Values.recoveryAdmin.password | quote }}  {{- end }}  {{- range $key, $value := .Values.extraSecretEnv }}  {{ $key }}: {{ $value | quote }}  {{- end }}
```

### templates/statefulset.yaml

[Section titled “templates/statefulset.yaml”](#templatesstatefulsetyaml)

The StatefulSet runs the Stalwart image, mounts `config.json` read-only at `/etc/stalwart/config.json`, mounts the data PVC at `/var/lib/stalwart`, and starts the binary with `--config /etc/stalwart/config.json`, matching the [[install/platform/docker|Docker install flow]].

```
apiVersion: apps/v1kind: StatefulSetmetadata:  name: {{ include "stalwart.fullname" . }}  labels:    {{- include "stalwart.labels" . | nindent 4 }}spec:  serviceName: {{ include "stalwart.fullname" . }}-headless  replicas: {{ .Values.replicaCount }}  selector:    matchLabels:      {{- include "stalwart.selectorLabels" . | nindent 6 }}  template:    metadata:      labels:        {{- include "stalwart.selectorLabels" . | nindent 8 }}      annotations:        checksum/config: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}    spec:      {{- with .Values.podSecurityContext }}      securityContext:        {{- toYaml . | nindent 8 }}      {{- end }}      containers:        - name: stalwart          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"          imagePullPolicy: {{ .Values.image.pullPolicy }}          {{- with .Values.containerSecurityContext }}          securityContext:            {{- toYaml . | nindent 12 }}          {{- end }}          args:            - "--config"            - "/etc/stalwart/config.json"          env:            {{- if .Values.role }}            - name: STALWART_ROLE              value: {{ .Values.role | quote }}            {{- end }}            {{- if .Values.pushShard }}            - name: STALWART_PUSH_SHARD              value: {{ .Values.pushShard | quote }}            {{- end }}            {{- if .Values.recoveryMode.enabled }}            - name: STALWART_RECOVERY_MODE              value: "true"            - name: STALWART_RECOVERY_MODE_PORT              value: {{ .Values.recoveryMode.port | quote }}            - name: STALWART_RECOVERY_MODE_LOG_LEVEL              value: {{ .Values.recoveryMode.logLevel | quote }}            {{- end }}            {{- range $key, $value := .Values.extraEnv }}            - name: {{ $key }}              value: {{ $value | quote }}            {{- end }}          envFrom:            - secretRef:                name: {{ include "stalwart.fullname" . }}-env          ports:            - name: smtp              containerPort: 25            - name: smtps              containerPort: 465            - name: submission              containerPort: 587            - name: imap              containerPort: 143            - name: imaps              containerPort: 993            - name: pop3              containerPort: 110            - name: pop3s              containerPort: 995            - name: sieve              containerPort: 4190            - name: http              containerPort: 80            - name: https              containerPort: 443            - name: mgmt              containerPort: 8080          livenessProbe:            httpGet:              path: /healthz/live              port: mgmt            initialDelaySeconds: 30            periodSeconds: 10          readinessProbe:            httpGet:              path: /healthz/ready              port: mgmt            initialDelaySeconds: 5            periodSeconds: 10          volumeMounts:            - name: config              mountPath: /etc/stalwart/config.json              subPath: config.json              readOnly: true            {{- if .Values.persistence.enabled }}            - name: data              mountPath: /var/lib/stalwart            {{- end }}          resources:            {{- toYaml .Values.resources | nindent 12 }}      volumes:        - name: config          configMap:            name: {{ include "stalwart.fullname" . }}-config  {{- if .Values.persistence.enabled }}  volumeClaimTemplates:    - metadata:        name: data      spec:        accessModes:          - {{ .Values.persistence.accessMode | quote }}        {{- if .Values.persistence.storageClass }}        storageClassName: {{ .Values.persistence.storageClass | quote }}        {{- end }}        resources:          requests:            storage: {{ .Values.persistence.size | quote }}  {{- end }}
```

### templates/service.yaml

[Section titled “templates/service.yaml”](#templatesserviceyaml)

Two Services are defined: a headless Service backing the StatefulSet’s stable DNS, and a regular Service that exposes the mail and management ports.

```
apiVersion: v1kind: Servicemetadata:  name: {{ include "stalwart.fullname" . }}-headless  labels:    {{- include "stalwart.labels" . | nindent 4 }}spec:  clusterIP: None  selector:    {{- include "stalwart.selectorLabels" . | nindent 4 }}  ports:    - name: mgmt      port: 8080      targetPort: mgmt---apiVersion: v1kind: Servicemetadata:  name: {{ include "stalwart.fullname" . }}  labels:    {{- include "stalwart.labels" . | nindent 4 }}spec:  type: {{ .Values.service.type }}  selector:    {{- include "stalwart.selectorLabels" . | nindent 4 }}  ports:    - name: smtp      port: {{ .Values.service.ports.smtp }}      targetPort: smtp    - name: smtps      port: {{ .Values.service.ports.smtps }}      targetPort: smtps    - name: submission      port: {{ .Values.service.ports.submission }}      targetPort: submission    - name: imap      port: {{ .Values.service.ports.imap }}      targetPort: imap    - name: imaps      port: {{ .Values.service.ports.imaps }}      targetPort: imaps    - name: pop3      port: {{ .Values.service.ports.pop3 }}      targetPort: pop3    - name: pop3s      port: {{ .Values.service.ports.pop3s }}      targetPort: pop3s    - name: sieve      port: {{ .Values.service.ports.sieve }}      targetPort: sieve    - name: http      port: {{ .Values.service.ports.http }}      targetPort: http    - name: https      port: {{ .Values.service.ports.https }}      targetPort: https    - name: mgmt      port: {{ .Values.service.ports.mgmt }}      targetPort: mgmt
```

### templates/ingress.yaml

[Section titled “templates/ingress.yaml”](#templatesingressyaml)

An Ingress is only useful for the HTTP and HTTPS listeners; SMTP, IMAP, POP3, and ManageSieve require L4 exposure and should be fronted by a `LoadBalancer` Service or an external load balancer.

```
{{- if .Values.ingress.enabled }}apiVersion: networking.k8s.io/v1kind: Ingressmetadata:  name: {{ include "stalwart.fullname" . }}  labels:    {{- include "stalwart.labels" . | nindent 4 }}  {{- with .Values.ingress.annotations }}  annotations:    {{- toYaml . | nindent 4 }}  {{- end }}spec:  {{- if .Values.ingress.className }}  ingressClassName: {{ .Values.ingress.className }}  {{- end }}  {{- if .Values.ingress.tls }}  tls:    {{- toYaml .Values.ingress.tls | nindent 4 }}  {{- end }}  rules:    {{- range .Values.ingress.hosts }}    - host: {{ .host | quote }}      http:        paths:          {{- range .paths }}          - path: {{ .path }}            pathType: {{ .pathType | default "Prefix" }}            backend:              service:                name: {{ include "stalwart.fullname" $ }}                port:                  name: {{ .portName | default "https" }}          {{- end }}    {{- end }}{{- end }}
```

## values.yaml

[Section titled “values.yaml”](#valuesyaml)

`values.yaml` is the single file an operator should need to edit to produce a working deployment. The example below exposes the full set of knobs called out in the chart templates:

```
# Container image.image:  repository: stalwartlabs/stalwart  tag: "v0.16"  pullPolicy: IfNotPresent
# Number of pods in the StatefulSet.# Single-node deployments keep this at 1.# For a clustered deployment, raise this value and set `role` (and# `pushShard` where applicable) to a value that makes sense for the# whole StatefulSet, or install multiple releases of this chart with# different role values.replicaCount: 1
# Maps to STALWART_ROLE. Names a ClusterRole defined in the database.# Leave empty on single-node installs to run every task and listener.# See /docs/cluster/configuration/roles for the role model.role: ""
# Maps to STALWART_PUSH_SHARD. Only set on nodes whose role delivers# push notifications. Leave empty on single-node installs.pushShard: ""
# Recovery / bootstrap administrator credential.# Enable this on first install so the operator can sign in without# scraping the pod logs for the generated temporary password.# Remove it once a permanent administrator account has been# provisioned through the setup wizard.recoveryAdmin:  enabled: true  username: admin  password: "change-me"
# Starts the pods in recovery mode. Suspends mail services and# exposes only the management listener on the configured port.# See /docs/configuration/recovery-mode.recoveryMode:  enabled: false  port: 8080  logLevel: info
# Extra environment variables injected into the container, for# store credentials that live in plain text.extraEnv: {}
# Extra environment variables injected from the managed Secret.# Use this for anything sensitive (database passwords, S3 keys, etc.).extraSecretEnv: {}
# config.json contents. Rendered verbatim into the ConfigMap.# Only the DataStore object belongs here; every other setting lives# in the database once the server is bootstrapped.# See /docs/configuration/overview and /docs/ref/object/data-store.config:  "@type": RocksDb  path: /var/lib/stalwart
# Service definition. Change `type` to LoadBalancer when a cloud# load balancer should expose the mail ports directly, or keep it# as ClusterIP and front the chart with an Ingress / gateway for# the HTTP listeners only.service:  type: ClusterIP  ports:    smtp: 25    smtps: 465    submission: 587    imap: 143    imaps: 993    pop3: 110    pop3s: 995    sieve: 4190    http: 80    https: 443    mgmt: 8080
# Ingress for the HTTP / HTTPS listeners only. Mail protocols# require L4 exposure and are not handled here.ingress:  enabled: false  className: nginx  annotations: {}  hosts:    - host: mail.example.org      paths:        - path: /          pathType: Prefix          portName: https  tls: []
# Persistent volume for the data directory. Required when the# DataStore is a local backend such as RocksDB. Can be disabled# for deployments that use an external store (PostgreSQL, MySQL,# FoundationDB, S3-backed blob store, etc.).persistence:  enabled: true  accessMode: ReadWriteOnce  storageClass: ""  size: 20Gi
# Resource requests and limits for the container.resources: {}
# Pod-level security context. Applied to every container in the pod.# The defaults are sufficient for the `baseline` Pod Security Standard.# See "Restricted Pod Security Standards" below for the override# required under the `restricted` profile.podSecurityContext:  fsGroup: 2000  runAsUser: 2000  runAsGroup: 2000
# Container-level security context for the stalwart container.# Empty by default. Set this to opt into the `restricted` Pod Security# Standard; see "Restricted Pod Security Standards" below.containerSecurityContext: {}
```

## config.json handling

[Section titled “config.json handling”](#configjson-handling)

The server reads a single small JSON file at startup, containing only the [[ref/object/data-store|DataStore]] variant that tells it where its database lives. The chart renders the value of `.Values.config` into a ConfigMap key called `config.json`, mounts it as a file at `/etc/stalwart/config.json`, and starts the container with `--config /etc/stalwart/config.json`. This matches the flag used by the Docker install and by the native Linux packages, so `helm upgrade` does not change the runtime contract: only the DataStore value changes between a single-node RocksDB install and an externally-backed cluster.

For an external DataStore (for example PostgreSQL), change `.Values.config` accordingly and set `persistence.enabled: false`. The [[ref/object/data-store|PostgreSql]] variant reads the connection password from an [[ref/object/data-store#authsecret|`authSecret`]] nested object whose `EnvironmentVariable` form is the natural fit for a chart-managed Secret:

```
config:  "@type": "PostgreSql"  host: postgres.db.svc.cluster.local  port: 5432  database: stalwart  authUsername: stalwart  authSecret:    "@type": "EnvironmentVariable"    variableName: STALWART_DB_PASSWORDpersistence:  enabled: falseextraSecretEnv:  STALWART_DB_PASSWORD: "s3cr3t"
```

## Environment variables

[Section titled “Environment variables”](#environment-variables)

Every `STALWART_*` variable recognised by the server is documented on the [[configuration/environment-variables|environment variables]] page. The chart surfaces them as follows:

VariableValues keyNotes`STALWART_ROLE``role`Names the [[ref/object/cluster-role|ClusterRole]] (found in the WebUI under  Settings ›  Cluster › Roles) the pod adopts.`STALWART_PUSH_SHARD``pushShard`Push notification shard for pods whose role delivers push.`STALWART_RECOVERY_MODE``recoveryMode.enabled`Starts the pod in recovery mode.`STALWART_RECOVERY_MODE_PORT``recoveryMode.port`Port the recovery listener binds to. Defaults to `8080`.`STALWART_RECOVERY_MODE_LOG_LEVEL``recoveryMode.logLevel`Log verbosity while in recovery or bootstrap mode.`STALWART_RECOVERY_ADMIN``recoveryAdmin.username` + `recoveryAdmin.password`Rendered into the managed Secret as `username:password`.

Any additional variable the runtime reads (database passwords, S3 credentials, and so on) can be injected via `extraEnv` for plain values or `extraSecretEnv` for sensitive values, both of which are surfaced through the same Secret-backed `envFrom` on the StatefulSet.

## Clustered deployment

[Section titled “Clustered deployment”](#clustered-deployment)

A single-node install leaves `role` empty, keeps `replicaCount: 1`, and runs every task and listener. Two layouts are recommended for multi-node deployments.

### Single role per StatefulSet replica

[Section titled “Single role per StatefulSet replica”](#single-role-per-statefulset-replica)

For a cluster where every pod runs the same role (for example a stateless SMTP frontend backed by an external store), set `replicaCount` to the desired number of pods and leave the other values unchanged. `STALWART_ROLE` is applied to every pod in the StatefulSet, and `STALWART_PUSH_SHARD` is applied to every pod too; if per-pod sharding is required, split the StatefulSet or use the multi-release layout below.

### Multi-release layout for distinct roles

[Section titled “Multi-release layout for distinct roles”](#multi-release-layout-for-distinct-roles)

For deployments that need different roles on different nodes, install the chart multiple times with distinct values files:

Terminal window

```
helm install stalwart-frontend ./stalwart -f values-frontend.yamlhelm install stalwart-maintenance ./stalwart -f values-maintenance.yamlhelm install stalwart-push-shard-0 ./stalwart -f values-push-0.yamlhelm install stalwart-push-shard-1 ./stalwart -f values-push-1.yaml
```

Each release produces its own StatefulSet with its own PVCs, role, and push shard, while sharing the same DataStore and the same coordinator. `STALWART_PUSH_SHARD` is set to a different integer per release so that push-delivery work is partitioned across the cluster; see the [[cluster/configuration/roles|roles documentation]] for the sharding model.

### External coordinator

[Section titled “External coordinator”](#external-coordinator)

Coordination between nodes is handled by the [[ref/object/coordinator|Coordinator]] singleton (found in the WebUI under  Settings ›  Cluster › Coordinator), which can be backed by peer-to-peer, Apache Kafka / Redpanda, NATS, or Redis. The coordinator is configured from the database rather than from `config.json`, so pointing a Kubernetes deployment at an external coordinator is a post-install operation: bring the first pod up, open the WebUI, set the Coordinator object in `Settings` > `Cluster`, and redeploy with the target `replicaCount`. Subsequent pods pick the coordinator configuration up from the shared DataStore on start. See the [[cluster/coordination|coordination overview]] for the choice of backend.

## Restricted Pod Security Standards

[Section titled “Restricted Pod Security Standards”](#restricted-pod-security-standards)

The defaults shown in `values.yaml` produce a pod compatible with the [baseline](https://kubernetes.io/docs/concepts/security/pod-security-standards/) Pod Security Standard. The stricter `restricted` profile (enforced by `pod-security.kubernetes.io/enforce: restricted` on the namespace, and by the equivalent Security Context Constraints on OpenShift) additionally requires `runAsNonRoot: true`, `allowPrivilegeEscalation: false`, `readOnlyRootFilesystem: true`, `seccompProfile.type: RuntimeDefault`, and `capabilities.drop: [ALL]`.

One detail of the Stalwart image matters under this profile. The Dockerfile runs `setcap cap_net_bind_service=+ep /usr/local/bin/stalwart` so that the non-root `stalwart` user (UID 2000) can bind to the privileged mail ports (`25`, `110`, `143`, `443`, `465`, `587`, `993`, `995`). When the kernel `execve`s a binary with effective file capabilities, those capabilities must also be present in the calling process’s bounding set; `capabilities.drop: [ALL]` empties that set, and the exec is rejected with:

```
exec /usr/local/bin/stalwart: operation not permitted
```

Restricted PSS explicitly permits `NET_BIND_SERVICE` to be re-added through `capabilities.add`, which restores the bounding-set entry the file capability needs. Set the following in `values.yaml`:

```
podSecurityContext:  runAsNonRoot: true  runAsUser: 2000  runAsGroup: 2000  fsGroup: 2000  seccompProfile:    type: RuntimeDefault
containerSecurityContext:  runAsNonRoot: true  allowPrivilegeEscalation: false  readOnlyRootFilesystem: true  capabilities:    drop: [ALL]    add: [NET_BIND_SERVICE]  seccompProfile:    type: RuntimeDefault
```

`NET_BIND_SERVICE` can be omitted from `capabilities.add` only if every configured listener binds to a port `>= 1024` and the file capability has been stripped from the binary (a custom image without the `setcap` step, or an init container that runs `setcap -r` against a copy of the binary). With the file capability still set on `/usr/local/bin/stalwart`, the bounding-set check fails regardless of which ports the configuration uses.

`readOnlyRootFilesystem: true` requires that any path Stalwart writes to outside `/etc/stalwart` and `/var/lib/stalwart` is backed by a writable volume. A stock install with RocksDB and the `Console` log destination writes only into the data PVC and stdout, so no extra volumes are needed; configurations that need scratch space should add an `emptyDir` mount on `/tmp`.

## Bootstrapping

[Section titled “Bootstrapping”](#bootstrapping)

[[configuration/bootstrap-mode|Bootstrap mode]] is triggered only when `config.json` is absent at startup. On Kubernetes the ConfigMap always renders a `config.json`, so the chart never enters bootstrap mode: an empty DataStore would instead cause the server to exit with an error before serving any listener. The chart therefore expects the initial `config.json` to be populated by the operator (the `.Values.config` default already points at a single-node RocksDB DataStore that lets the pod start cleanly on first install) and provisions the rest of the configuration through the management API once the pod is running. Setting `recoveryAdmin.enabled: true` makes that first sign-in possible without scraping the pod logs for a generated temporary password.

Install the chart:

Terminal window

```
helm install stalwart ./stalwart -f values.yaml
```

Port-forward the management listener:

Terminal window

```
kubectl port-forward svc/stalwart 8080:8080
```

Open `http://127.0.0.1:8080/admin` and sign in with the credentials from `recoveryAdmin`.

Complete the setup wizard, or run `stalwart-cli apply` against the same endpoint for a declarative bootstrap. The [[management/cli|CLI documentation]] covers the `apply` flow.

Once a permanent administrator account has been provisioned, remove `recoveryAdmin` from `values.yaml` and run `helm upgrade` to drop the pinned credential. Leaving `STALWART_RECOVERY_ADMIN` set on a production deployment is discouraged; see [[configuration/recovery-mode|recovery mode]] for the rationale.

## Upgrading

[Section titled “Upgrading”](#upgrading)

Upgrades are performed by changing `image.tag` in `values.yaml` and running:

Terminal window

```
helm upgrade --install stalwart ./stalwart -f values.yaml
```

The StatefulSet rolls pods one at a time, each pod reusing its existing PVC, so on-disk data is preserved across the upgrade. Pinning a minor-version tag such as `v0.16` (rather than `latest`) keeps the deployment on a single release line and avoids breaking changes on upgrade, matching the guidance in the [[install/platform/docker#start-the-container|Docker install page]].

## Troubleshooting

[Section titled “Troubleshooting”](#troubleshooting)

Container fails to start with `exec /usr/local/bin/stalwart: operation not permitted`: the namespace enforces the `restricted` Pod Security Standard (or an equivalent SCC) and the file capability on the binary is incompatible with `capabilities.drop: [ALL]`. Add `NET_BIND_SERVICE` back to `capabilities.add` in `containerSecurityContext`. See [Restricted Pod Security Standards](#restricted-pod-security-standards).
No administrator sign-in on first install: set `recoveryAdmin.enabled: true` in `values.yaml` and redeploy. See [[configuration/bootstrap-mode|bootstrap mode]].
Pod stuck in `CrashLoopBackOff` on a fresh install with RocksDB: check that `persistence.enabled: true` and that the cluster’s default StorageClass can provision a `ReadWriteOnce` volume of the requested size. See [[install/store|persistent storage]].
Mail ports unreachable from outside the cluster: `service.type: ClusterIP` only exposes the listeners inside the cluster. Switch to `LoadBalancer`, add a `NodePort`, or front the chart with an L4 load balancer. Review [[install/security|securing your server]] before exposing SMTP and IMAP publicly.
TLS errors on the HTTPS listener: Stalwart needs a TLS certificate before mail clients will connect. Configure ACME in the WebUI under `Settings` > `Server` > `TLS` > `ACME Providers` or upload an existing certificate under `Settings` > `Server` > `TLS` > `Certificates`. See [[server/tls/acme|ACME]].
Cluster nodes do not see each other: verify that `STALWART_ROLE` is set on every pod that participates and that the Coordinator is reachable from every pod. See [[cluster/coordination|coordination overview]].
Two nodes acquire the same node id: StatefulSet pods already have stable, unique hostnames, but custom Deployment-based installs must ensure each pod runs with a distinct hostname. See [[cluster/configuration/node-id|node id]].