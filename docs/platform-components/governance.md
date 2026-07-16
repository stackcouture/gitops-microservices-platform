## Governance

The **`governance/`** directory contains Kubernetes resource governance policies that help ensure fair resource allocation, maintain workload stability, and enforce namespace-level resource constraints across the platform.

```text
governance/
├── argocd/
├── cert-manager/
├── external-secrets/
├── falco/
├── gateway/
├── kyverno/
├── monitoring/
├── postgres/
├── redis/
└── vote/
```

Each namespace contains governance policies tailored to the workloads deployed within it. These policies help prevent resource exhaustion and promote consistent resource management across the platform.

---
### Namespace Governance

Every namespace follows a common structure:

```text
namespace/
├── limitrange.yaml
├── resourcequota.yaml
└── kustomization.yaml
```

#### Resources

| Resource             | Purpose                                                                                 |
| -------------------- | --------------------------------------------------------------------------------------- |
| `limitrange.yaml`    | Defines default CPU and memory requests and limits for containers within the namespace. |
| `resourcequota.yaml` | Restricts the total CPU, memory, storage, and object usage for the namespace.           |
| `kustomization.yaml` | Aggregates the namespace governance resources.                                          |

---
### Managed Namespaces

The governance policies are applied to the following namespaces:

| Namespace           | Purpose                                          |
| ------------------- | ------------------------------------------------ |
| `argocd/`           | GitOps platform components.                      |
| `cert-manager/`     | Certificate management services.                 |
| `external-secrets/` | External Secrets Operator resources.             |
| `falco/`            | Runtime security monitoring.                     |
| `gateway/`          | Gateway API and ingress components.              |
| `kyverno/`          | Kubernetes policy engine.                        |
| `monitoring/`       | Prometheus, Grafana, and observability services. |
| `postgres/`         | PostgreSQL database services.                    |
| `redis/`            | Redis messaging and caching services.            |
| `vote/`             | Application workloads.                           |

By applying **ResourceQuota** and **LimitRange** policies consistently, the platform prevents resource contention, improves cluster stability, and ensures that workloads consume resources within defined operational boundaries.

---
