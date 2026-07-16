# GitOps Configuration

The **`argocd/`** directory contains the GitOps configuration used to bootstrap and manage the Kubernetes platform with **Argo CD**. It defines the projects, deployment boundaries, and ApplicationSets responsible for continuously synchronizing the desired state from Git to the Kubernetes cluster.

```text id="nrtt8z"
argocd/
├── projects/
└── applicationsets/
```

The GitOps configuration is organized into two logical components:

* **`projects/`** defines Argo CD AppProjects that provide logical separation and access control.
* **`applicationsets/`** defines ApplicationSets that automatically generate and manage Argo CD Applications.

Together, these components enable automated synchronization, self-healing, resource pruning, and declarative application delivery.

---

## AppProjects (`argocd/projects/`)

AppProjects define logical boundaries for GitOps deployments. They control which repositories, clusters, namespaces, and Kubernetes resources an application is allowed to access.

### Resources

| Resource                      | Purpose                                              |
| ----------------------------- | ---------------------------------------------------- |
| `apps-project.yaml`           | Defines the AppProject for application workloads.    |
| `infrastructure-project.yaml` | Defines the AppProject for infrastructure resources. |
| `platform-project.yaml`       | Defines the AppProject for shared platform services. |
| `kustomization.yaml`          | Aggregates all AppProject resources.                 |

Each AppProject helps enforce separation between platform layers while providing centralized management through Argo CD.

---

## ApplicationSets (`argocd/applicationsets/`)

ApplicationSets automatically generate and manage Argo CD Applications for each platform layer. They continuously monitor the Git repository and synchronize changes to the Kubernetes cluster.

### Resources

| Resource                             | Purpose                                  |
| ------------------------------------ | ---------------------------------------- |
| `apps-applicationset.yaml`           | Deploys application workloads.           |
| `infrastructure-applicationset.yaml` | Deploys infrastructure components.       |
| `platform-applicationset.yaml`       | Deploys shared platform services.        |
| `kustomization.yaml`                 | Aggregates all ApplicationSet resources. |

Each generated Argo CD Application is configured with:

* Automated synchronization
* Self-healing
* Resource pruning
* Namespace creation
* Declarative GitOps deployment

Any change committed to the GitOps repository is automatically detected by Argo CD and reconciled with the Kubernetes cluster, ensuring the live environment continuously matches the desired state stored in Git.

---
