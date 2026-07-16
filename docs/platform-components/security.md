## Security

The **`security/`** directory contains the platform's security controls and policies. These components implement Kubernetes policy enforcement, runtime threat detection, and network segmentation to help secure workloads and enforce operational best practices across the cluster.

```text
security/
├── kyverno/
├── falco/
└── network-policies/
```

Each security component is managed independently using **Kustomize**, allowing reusable manifests and environment-specific configurations while following GitOps principles.

```text
component/
├── base/
└── overlays/
    └── dev/
```

* **`base/`** contains reusable Kubernetes manifests shared across all environments.
* **`overlays/`** contains environment-specific configuration and deployment customizations.

All security resources are continuously synchronized to the Kubernetes cluster by **Argo CD**.

---
### Kyverno (`security/kyverno/`)

**Kyverno** provides Kubernetes-native policy enforcement to validate, mutate, and generate resources, helping ensure workloads comply with platform security standards.

#### Resources

| Resource             | Purpose                                              |
| -------------------- | ---------------------------------------------------- |
| `cluster-policies/`  | Cluster-wide security and governance policies.       |
| `policies/`          | Namespace-specific validation and mutation policies. |
| `exceptions/`        | Policy exceptions for approved workloads.            |
| `kustomization.yaml` | Aggregates all Kyverno resources.                    |

#### Policy Categories

* Pod Security Standards
* Image registry restrictions
* Resource requests and limits
* Readiness and liveness probe validation
* Required labels and annotations
* Privileged container restrictions
* Non-root container enforcement
* Network policy generation
* Platform governance

---
### Falco (`security/falco/`)

**Falco** provides runtime security by monitoring Kubernetes events and container activity for suspicious or unexpected behavior.

#### Resources

| Resource             | Purpose                                    |
| -------------------- | ------------------------------------------ |
| `falco-rules.yaml`   | Defines custom runtime detection rules.    |
| `falcosidekick.yaml` | Forwards Falco alerts to external systems. |
| `kustomization.yaml` | Aggregates all Falco resources.            |

#### Responsibilities

* Detect suspicious container activity
* Monitor Kubernetes events
* Identify privilege escalation attempts
* Generate runtime security alerts
* Forward alerts to external notification systems

---
### Network Policies (`security/network-policies/`)

Network Policies implement namespace and workload isolation by explicitly controlling communication between Kubernetes workloads.

#### Resources

| Resource             | Purpose                                         |
| -------------------- | ----------------------------------------------- |
| `default-deny.yaml`  | Denies all traffic by default.                  |
| `allow-*.yaml`       | Allows communication between trusted workloads. |
| `kustomization.yaml` | Aggregates all NetworkPolicy resources.         |

#### Allowed Communication

* Vote → Redis
* Worker → Redis
* Worker → PostgreSQL
* Result → PostgreSQL
* Prometheus → Application Metrics

All other network traffic is denied unless explicitly allowed, following a **default deny** security model.

Together, these security components provide policy enforcement, runtime threat detection, and network isolation to help protect the Kubernetes platform and its workloads.

---