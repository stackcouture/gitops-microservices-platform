## Platform Services

The **`platform/`** directory contains shared platform services that provide networking, certificate management, secret management, monitoring, namespaces, and backup capabilities for the Kubernetes platform. These components are shared across application workloads and are continuously managed through GitOps.

```text
platform/
├── cluster-secrets/
├── clusterissuer/
├── gateway-api/
├── ingress/
├── monitoring/
├── namespaces/
└── velero/
```

Each platform component is managed independently using **Kustomize**, allowing reusable base manifests and environment-specific overlays while following GitOps principles.

```text
component/
├── base/
└── overlays/
    └── dev/
```

* **`base/`** contains reusable Kubernetes manifests shared across all environments.
* **`overlays/`** contains environment-specific configuration, such as image versions, resource limits, and deployment customizations.

All platform services are continuously synchronized to the Kubernetes cluster by **Argo CD**.

---
### Cluster Secrets (`platform/cluster-secrets/`)

The **Cluster Secrets** component centralizes secret management by configuring External Secrets to retrieve secrets from **Google Secret Manager**.

#### Resources

| Resource                  | Purpose                                                   |
| ------------------------- | --------------------------------------------------------- |
| `clustersecretstore.yaml` | Defines the ClusterSecretStore for Google Secret Manager. |
| `externalsecret.yaml`     | Synchronizes external secrets into Kubernetes.            |
| `kustomization.yaml`      | Aggregates all cluster secret resources.                  |

---
### Cluster Issuer (`platform/clusterissuer/`)

The **Cluster Issuer** component manages automated TLS certificate issuance using **cert-manager** and **Let's Encrypt**.

#### Resources

| Resource                     | Purpose                                 |
| ---------------------------- | --------------------------------------- |
| `clusterissuer-prod.yaml`    | Production Let's Encrypt ClusterIssuer. |
| `clusterissuer-staging.yaml` | Staging Let's Encrypt ClusterIssuer.    |
| `kustomization.yaml`         | Aggregates all ClusterIssuer resources. |

---
### Gateway API (`platform/gateway-api/`)

The **Gateway API** component manages external traffic routing using **NGINX Gateway Fabric** and the Kubernetes Gateway API.

#### Resources

| Resource             | Purpose                                              |
| -------------------- | ---------------------------------------------------- |
| `gateway.yaml`       | Defines the platform Gateway.                        |
| `certificate.yaml`   | Configures TLS certificates for the Gateway.         |
| `httproute-*.yaml`   | Routes traffic to platform and application services. |
| `kustomization.yaml` | Aggregates all Gateway API resources.                |

---
### Ingress (`platform/ingress/`)

The **Ingress** component contains legacy Kubernetes Ingress resources required by platform services that do not yet use the Gateway API.

#### Resources

| Resource             | Purpose                               |
| -------------------- | ------------------------------------- |
| `ingress.yaml`       | Defines Kubernetes Ingress resources. |
| `kustomization.yaml` | Aggregates all Ingress resources.     |

---
### Monitoring (`platform/monitoring/`)

The **Monitoring** component deploys monitoring resources for infrastructure services and integrates them with Prometheus.

#### Resources

| Resource                 | Purpose                                |
| ------------------------ | -------------------------------------- |
| `postgres-exporter.yaml` | Deploys PostgreSQL Exporter.           |
| `redis-exporter.yaml`    | Deploys Redis Exporter.                |
| `servicemonitor.yaml`    | Enables Prometheus metrics collection. |
| `kustomization.yaml`     | Aggregates monitoring resources.       |

---
### Namespaces (`platform/namespaces/`)

The **Namespaces** component creates and manages the Kubernetes namespaces required by the platform.

#### Resources

| Resource             | Purpose                                      |
| -------------------- | -------------------------------------------- |
| `namespace-*.yaml`   | Creates platform and application namespaces. |
| `kustomization.yaml` | Aggregates namespace resources.              |

---
### Velero (`platform/velero/`)

The **Velero** component provides backup and disaster recovery capabilities for Kubernetes resources and persistent volumes.

#### Resources

| Resource               | Purpose                          |
| ---------------------- | -------------------------------- |
| `application.yaml`     | Deploys Velero through Argo CD.  |
| `backup-schedule.yaml` | Configures scheduled backups.    |
| `restore.yaml`         | Defines restore resources.       |
| `kustomization.yaml`   | Aggregates all Velero resources. |

Together, these platform services provide the foundational capabilities required to operate the Kubernetes platform, including networking, TLS, secret management, monitoring, namespace management, and backup and recovery.

---