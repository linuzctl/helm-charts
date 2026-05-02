# stalwart

![Chart version: 0.1.2](https://img.shields.io/badge/chart-0.1.2-blue)
![App version: v0.16.3](https://img.shields.io/badge/app-v0.16.3-blue)

Helm chart for [Stalwart](https://stalw.art) - an all-in-one Mail & Collaboration server. Secure, scalable and fluent in every protocol (IMAP, JMAP, SMTP, CalDAV, CardDAV, WebDAV).

This Helm chart is based on the official [Stalwart Kubernetes documentation](https://stalw.art/docs/cluster/orchestration/kubernetes), with additional custom configurations by me.

## Overview

This chart deploys Stalwart as a **StatefulSet** in Kubernetes. On first boot Stalwart enters bootstrap mode. All persistent server configuration (listeners, TLS, domains, accounts, etc.) lives inside the configured data store; only the data-store bootstrap configuration is held in a ConfigMap.

## Prerequisites

- A default `StorageClass` if using the built-in RocksDB persistence (default)
- An ingress controller (e.g. ingress-nginx) if `ingress.enabled: true`
- A TLS secret (e.g. from cert-manager) if `certificate.enabled: true`

---

## Quick Start

### Single-node with RocksDB (defaults - for testing)

```bash
helm install stalwart oci://ghcr.io/linuzctl/helm-charts/stalwart
```

After the pod becomes Ready, open the management UI at `http://<ip>:8080` (or the configured ingress host) and complete the setup wizard.

## Deployment Scenarios

### PostgreSQL data store

Disable local persistence and point the config at a PostgreSQL instance. Inject the database password as environment variable via `extraSecretEnv`.

```yaml
extraSecretEnv:
  DB_PASSWORD: "postgres-password"

config:
  "@type": PostgreSql
  host: stalwart-postgres
  port: 5432
  database: stalwart
  authUsername: stalwart
  authSecret:
    "@type": EnvironmentVariable
    variableName: DB_PASSWORD

persistence:
  enabled: false
```

### PostgreSQL with CloudNativePG (recommended & tested)

1. Create PostgreSQL cluster

```yaml
---
apiVersion: postgresql.cnpg.io/v1
kind: Cluster
metadata:
  name: stalwart-postgres
  namespace: stalwart
spec:
  instances: 1
  imageName: ghcr.io/cloudnative-pg/postgresql:18.3
  storage:
    storageClass: ceph-block
    size: 5Gi
  walStorage:
    storageClass: ceph-block
    size: 1Gi
  bootstrap:
    initdb:
      database: stalwart
      owner: stalwart
  enablePDB: false
```

2. Adjust values.yaml

```yaml
extraEnv:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: stalwart-postgres-app
        key: password

config:
  "@type": PostgreSql
  host: stalwart-postgres-rw.stalwart.svc.cluster.local
  port: 5432
  database: stalwart
  authUsername: stalwart
  authSecret:
    "@type": "EnvironmentVariable"
    variableName: DB_PASSWORD

persistence:
  enabled: false
```

## TLS / Certificate Setup

The chart mounts a Kubernetes TLS secret at `/etc/stalwart/tls/` inside the container. Stalwart is then configured (via the management UI or stalwart-cli) to read `tls.crt` and `tls.key` from that path.

### With cert-manager

1. Create a `Certificate` resource:

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: mail-example-com-tls
  namespace: stalwart
spec:
  secretName: mail-example-com-tls
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
  dnsNames:
    - mail.example.com
```

2. Reference the secret in values:

```yaml
certificate:
  enabled: true
  secretName: mail-example-com-tls
```

## Ingress Setup

The Ingress only covers the HTTP/HTTPS listeners. Mail protocols (SMTP, IMAP, POP3, Sieve) require L4 exposure via `service.type: LoadBalancer` or route TCP requests to Stalwart.

```yaml
ingress:
  enabled: true
  className: nginx
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: mail.example.com
      paths:
        - path: /
          pathType: Prefix
          portName: https
  tls:
    - secretName: mail-example-com-tls
      hosts:
        - mail.example.com

# you can also attach the certificate create from ingress into the pod
certificate:
  enabled: true
  secretName: mail-example-com-tls
```

## Recovery Mode

Recovery mode suspends all mail services and exposes only the management listener. Use it to recover from a misconfiguration or a locked-out admin account.

```yaml
recoveryMode:
  enabled: true
  port: 8080
  logLevel: debug
```

See the [Stalwart recovery mode docs](https://stalw.art/docs/configuration/recovery-mode) for details.

## Upgrading

```bash
helm upgrade stalwart oci://ghcr.io/linuzctl/helm-charts/stalwart -n stalwart -f values.yaml
```

The StatefulSet template includes a `checksum/config` annotation so pods are automatically rolled when `config.json` changes.


## Uninstalling

```bash
helm uninstall stalwart
```
