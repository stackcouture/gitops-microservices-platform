## Infrastructure

The **`infrastructure/`** directory contains the stateful services and supporting infrastructure required by the platform. These components provide persistent storage, message queuing, database administration, and Kubernetes resources that support the application workloads.

```text
infrastructure/
├── postgres/
├── redis/
├── pgadmin/
└── external-secrets-sa/
```

Each infrastructure component is managed independently using **Kustomize**, allowing reusable base manifests and environment-specific overlays while following GitOps principles.

```text
component/
├── base/
└── overlays/
    └── dev/
```

* **`base/`** contains reusable Kubernetes manifests shared across all environments.
* **`overlays/`** contains environment-specific configuration, such as storage size, resource limits, image versions, and deployment customizations.

All infrastructure resources are continuously synchronized to the Kubernetes cluster by **Argo CD**.

---
## PostgreSQL (`infrastructure/postgres/`)

**Technology:** PostgreSQL 15

PostgreSQL provides the platform's primary relational database for persistent application data. It is deployed as a Kubernetes StatefulSet with persistent storage and monitoring integration.

### Base Resources

| Resource                 | Purpose                                                       |
| ------------------------ | ------------------------------------------------------------- |
| `statefulset.yaml`       | Deploys PostgreSQL as a StatefulSet with persistent storage.  |
| `service.yaml`           | Exposes PostgreSQL within the Kubernetes cluster.             |
| `headless-service.yaml`  | Provides stable network identities for the StatefulSet.       |
| `serviceaccount.yaml`    | Creates the Kubernetes ServiceAccount used by PostgreSQL.     |
| `externalsecret.yaml`    | Synchronizes database credentials from Google Secret Manager. |
| `servicemonitor.yaml`    | Enables Prometheus metrics collection.                        |
| `postgres-exporter.yaml` | Deploys the PostgreSQL Exporter for monitoring.               |
| `kustomization.yaml`     | Aggregates all PostgreSQL resources.                          |

### Overlay

`overlays/dev/kustomization.yaml`

Provides development-specific configuration such as:

* Storage class
* Persistent volume size
* Resource requests and limits
* Environment-specific patches

---
## Redis (`infrastructure/redis/`)

**Technology:** Redis

Redis provides in-memory data storage and message queuing for asynchronous communication between application services. The Worker service consumes messages from Redis for background processing.

### Base Resources

| Resource                | Purpose                                      |
| ----------------------- | -------------------------------------------- |
| `statefulset.yaml`      | Deploys Redis as a StatefulSet.              |
| `service.yaml`          | Exposes Redis within the Kubernetes cluster. |
| `headless-service.yaml` | Provides stable networking for Redis pods.   |
| `servicemonitor.yaml`   | Enables Prometheus metrics collection.       |
| `redis-exporter.yaml`   | Deploys the Redis Exporter for monitoring.   |
| `kustomization.yaml`    | Aggregates all Redis resources.              |

### Overlay

`overlays/dev/kustomization.yaml`

Provides development-specific configuration including:

* Storage configuration
* Resource requests and limits
* Replica configuration
* Environment-specific patches

---
## pgAdmin (`infrastructure/pgadmin/`)

**Technology:** pgAdmin 4

pgAdmin provides a web-based administration interface for managing PostgreSQL databases. It is used for database administration, schema management, and query execution.

### Base Resources

| Resource              | Purpose                                    |
| --------------------- | ------------------------------------------ |
| `deployment.yaml`     | Deploys the pgAdmin application.           |
| `service.yaml`        | Exposes pgAdmin within the cluster.        |
| `httproute.yaml`      | Publishes pgAdmin through the Gateway API. |
| `secret.yaml`         | Stores pgAdmin credentials.                |
| `serviceaccount.yaml` | Creates the Kubernetes ServiceAccount.     |
| `kustomization.yaml`  | Aggregates all pgAdmin resources.          |

### Overlay

`overlays/dev/kustomization.yaml`

Contains environment-specific configuration such as:

* Container image
* Resource limits
* Environment variables
* Labels and annotations

---
## External Secrets Service Account (`infrastructure/external-secrets-sa/`)

The **`external-secrets-sa/`** component creates the Kubernetes ServiceAccount used by the External Secrets Operator to authenticate with Google Cloud through Workload Identity.

### Resources

| Resource              | Purpose                                |
| --------------------- | -------------------------------------- |
| `serviceaccount.yaml` | Creates the Kubernetes ServiceAccount. |
| `kustomization.yaml`  | Aggregates the component resources.    |

This ServiceAccount enables External Secrets to securely retrieve secrets from **Google Secret Manager** without storing credentials inside the Kubernetes cluster.
