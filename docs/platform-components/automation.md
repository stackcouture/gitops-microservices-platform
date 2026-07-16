## Automation

The **`automation/`** directory contains operational automation that runs within the Kubernetes platform. These workloads automate recurring operational tasks, improve platform reliability, and reduce manual intervention by leveraging native Kubernetes resources such as **CronJobs**.

```text
automation/
├── health-reports/
├── maintenance/
├── notifications/
└── shared/
```

Each automation component is managed independently using **Kustomize**, allowing reusable manifests and environment-specific configuration while following GitOps principles.

```text
component/
├── base/
└── overlays/
    └── dev/
```

* **`base/`** contains reusable Kubernetes manifests shared across all environments.
* **`overlays/`** contains environment-specific configuration and deployment customizations.

All automation resources are continuously synchronized to the Kubernetes cluster by **Argo CD**.

---
### Health Reports (`automation/health-reports/`)

The **Health Reports** component runs scheduled platform health checks and generates operational reports.

#### Resources

| Resource              | Purpose                                                    |
| --------------------- | ---------------------------------------------------------- |
| `cronjob.yaml`        | Executes scheduled health report generation.               |
| `serviceaccount.yaml` | Creates the Kubernetes ServiceAccount used by the CronJob. |
| `rbac.yaml`           | Grants the required permissions for cluster operations.    |
| `kustomization.yaml`  | Aggregates all component resources.                        |

#### Responsibilities

* Perform scheduled platform health checks
* Collect cluster status and resource information
* Generate operational reports
* Publish health summaries

---
### Maintenance (`automation/maintenance/`)

The **Maintenance** component automates routine platform maintenance tasks.

#### Resources

| Resource              | Purpose                                                    |
| --------------------- | ---------------------------------------------------------- |
| `cronjob.yaml`        | Executes scheduled maintenance operations.                 |
| `serviceaccount.yaml` | Creates the Kubernetes ServiceAccount used by the CronJob. |
| `rbac.yaml`           | Grants the required permissions.                           |
| `kustomization.yaml`  | Aggregates all component resources.                        |

#### Responsibilities

* Execute recurring maintenance tasks
* Perform scheduled cleanup operations
* Automate routine administrative activities

---
### Notifications (`automation/notifications/`)

The **Notifications** component sends alerts and operational notifications to external systems.

#### Resources

| Resource              | Purpose                                                    |
| --------------------- | ---------------------------------------------------------- |
| `cronjob.yaml`        | Executes scheduled notification tasks.                     |
| `serviceaccount.yaml` | Creates the Kubernetes ServiceAccount used by the CronJob. |
| `rbac.yaml`           | Grants the required permissions.                           |
| `kustomization.yaml`  | Aggregates all component resources.                        |

#### Responsibilities

* Send platform notifications
* Deliver operational alerts
* Integrate with collaboration tools (for example, Slack)

---
### Shared (`automation/shared/`)

The **Shared** component contains Kubernetes resources that are reused across multiple automation workloads.

#### Resources

| Resource              | Purpose                                         |
| --------------------- | ----------------------------------------------- |
| `serviceaccount.yaml` | Shared ServiceAccount for automation workloads. |
| `rbac.yaml`           | Shared RBAC configuration.                      |
| `configmap.yaml`      | Shared configuration for automation jobs.       |
| `kustomization.yaml`  | Aggregates shared resources.                    |

Together, these automation components provide scheduled operational tasks, platform health reporting, maintenance, and notification capabilities that improve the reliability and operational efficiency of the Kubernetes platform.

---