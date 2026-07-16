## `apps/`

The **`apps/`** directory contains the Kubernetes manifests for the voting application's microservices. Each service is managed independently using **Kustomize**, allowing reusable base manifests and environment-specific overlays while following GitOps principles.

```text
apps/
├── vote/
├── result/
└── worker/
```

Each microservice follows the same repository structure:

```text
service/
├── base/
└── overlays/
    └── dev/
```

* **`base/`** contains reusable Kubernetes manifests that are shared across all environments.
* **`overlays/`** contains environment-specific configuration, such as image tags, replica counts, labels, and other deployment customizations.

The CI pipeline automatically updates the container image reference in the appropriate overlay after a successful build, and Argo CD synchronizes the changes to the Kubernetes cluster.

---

## Vote Service (`apps/vote/`)

**Technology:** Python (Flask)

The Vote service provides the web interface where users submit votes. It receives user requests, publishes vote messages to Redis, exposes application metrics, and is deployed using **Argo Rollouts** for canary releases.

### Base Resources

| Resource                   | Purpose                                                                       |
| -------------------------- | ----------------------------------------------------------------------------- |
| `analysis-template.yaml`   | Defines Prometheus-based analysis used during canary deployments.             |
| `hpa.yaml`                 | Configures Horizontal Pod Autoscaler for CPU/Memory-based scaling.            |
| `kustomization.yaml`       | Aggregates all Kubernetes resources for the service.                          |
| `pdb.yaml`                 | Ensures minimum application availability during node maintenance or upgrades. |
| `postgres-alias.yaml`      | Creates an alias service for PostgreSQL connectivity.                         |
| `rbac.yaml`                | Defines Roles and RoleBindings required by the application.                   |
| `redis-alias.yaml`         | Creates an alias service for Redis connectivity.                              |
| `rollout.yaml`             | Defines the Argo Rollout resource for canary deployment.                      |
| `serviceaccount.yaml`      | Creates the Kubernetes ServiceAccount used by the application.                |
| `servicemonitor.yaml`      | Enables Prometheus to scrape application metrics.                             |
| `vote-alert-rules.yaml`    | Defines Prometheus alerting rules for the Vote service.                       |
| `vote-canary-service.yaml` | Routes traffic to canary pods during progressive delivery.                    |
| `vote-stable-service.yaml` | Routes production traffic to stable pods.                                     |

### Overlay

`overlays/dev/kustomization.yaml`

Provides development-specific configuration such as:

* Container image
* Image tag
* Replica count
* Environment variables
* Labels and annotations

The image tag is automatically updated by the GitHub Actions CI pipeline after every successful build.

---

## Result Service (`apps/result/`)

**Technology:** Node.js (Express)

The Result service displays live voting results by reading processed data from PostgreSQL. It uses **Argo Rollouts** to perform blue-green deployments while exposing Prometheus metrics for monitoring.

### Base Resources

| Resource                  | Purpose                                                        |
| ------------------------- | -------------------------------------------------------------- |
| `kustomization.yaml`      | Aggregates Kubernetes resources for the service.               |
| `rbac.yaml`               | Defines Roles and RoleBindings required by the application.    |
| `result-active-svc.yaml`  | Stable service receiving production traffic.                   |
| `result-preview-svc.yaml` | Preview service used during blue-green deployments.            |
| `rollout.yaml`            | Defines the Argo Rollout resource for blue-green deployment.   |
| `serviceaccount.yaml`     | Creates the Kubernetes ServiceAccount used by the application. |
| `servicemonitor.yaml`     | Enables Prometheus metrics collection.                         |

### Overlay

`overlays/dev/kustomization.yaml`

Contains development-specific configuration and is automatically updated by the CI pipeline with the latest container image after each successful build.

---

## Worker Service (`apps/worker/`)

**Technology:** .NET

The Worker service processes vote events asynchronously. It consumes messages from Redis, stores processed results in PostgreSQL, and scales automatically using **KEDA** based on Redis queue length.

### Base Resources

| Resource                   | Purpose                                                   |
| -------------------------- | --------------------------------------------------------- |
| `deployment.yaml`          | Deploys the worker application.                           |
| `postgres-secret.yaml`     | References database credentials required by the worker.   |
| `scaledobject-worker.yaml` | Configures KEDA event-driven autoscaling.                 |
| `serviceaccount.yaml`      | Creates the Kubernetes ServiceAccount used by the worker. |
| `kustomization.yaml`       | Aggregates all Kubernetes resources for the service.      |

### Overlay

`overlays/dev/kustomization.yaml`

Contains environment-specific configuration such as:

* Container image
* Image tag
* Replica count
* Environment variables

The image tag is automatically updated by the GitHub Actions CI pipeline, enabling GitOps-based deployments through Argo CD.
