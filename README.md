# Gitops Microservices Platform

<div align="center">

![ArgoCD](https://img.shields.io/badge/ArgoCD-GitOps-EF7B4D?style=for-the-badge&logo=argo)
![Kubernetes](https://img.shields.io/badge/Kubernetes-GKE-326CE5?style=for-the-badge&logo=kubernetes)
![Kustomize](https://img.shields.io/badge/Kustomize-Configuration-326CE5?style=for-the-badge)
![Gateway API](https://img.shields.io/badge/Gateway_API-Traffic_Management-009688?style=for-the-badge)
![Argo Rollouts](https://img.shields.io/badge/Argo_Rollouts-Progressive_Deployment-FC6D26?style=for-the-badge)
![External Secrets](https://img.shields.io/badge/External_Secrets-Secret_Management-6A1B9A?style=for-the-badge)

![Kyverno](https://img.shields.io/badge/Kyverno-Policy_as_Code-00A3E0?style=for-the-badge)
![Cert Manager](https://img.shields.io/badge/cert--manager-TLS_Automation-326CE5?style=for-the-badge)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?style=for-the-badge&logo=prometheus)
![Grafana](https://img.shields.io/badge/Grafana-Dashboards-F46800?style=for-the-badge&logo=grafana)
![KEDA](https://img.shields.io/badge/KEDA-Event_Driven_Autoscaling-5A0FC8?style=for-the-badge)
![Slack](https://img.shields.io/badge/Slack-Notifications-4A154B?style=for-the-badge&logo=slack)

**Production-grade GitOps repository managing Kubernetes infrastructure, platform services, applications, and operational automation on Google Kubernetes Engine**

</div>

The **GitOps repository** for the [gitops-platform-engineering](https://github.com/stackcouture/gitops-platform-engineering) portfolio project. This repository is the **single source of truth** for the desired state of all Kubernetes workloads running on the GKE cluster — including application deployments, infrastructure add-ons, platform tooling, and ArgoCD configuration.

ArgoCD continuously watches this repository and automatically reconciles the live cluster state to match what is defined here. No manual `kubectl apply` is ever used for deployments.

---
## Project Overview

This repository showcases a **production-inspired GitOps microservices platform** built on **Google Kubernetes Engine (GKE)**. It demonstrates modern cloud-native practices by combining **Terraform**, **GitHub Actions**, **Argo CD**, and **Kubernetes** to provision infrastructure, automate CI/CD, and manage declarative application deployments through GitOps.

The platform deploys a **polyglot voting application** composed of multiple microservices (Frontend, Vote, Result, Worker, Redis, and PostgreSQL) while integrating production-inspired capabilities such as **progressive delivery**, **policy enforcement**, **secret management**, **autoscaling**, and **observability**.

### Key Highlights

* Infrastructure provisioning with **Terraform**
* GitOps-based deployments using **Argo CD** and **Kustomize**
* CI pipeline automation with **GitHub Actions**
* Progressive delivery with **Argo Rollouts**
* Security using **Kyverno** and **External Secrets**
* Gateway API for Kubernetes networking
* Autoscaling with **HPA** and **KEDA**
* Monitoring with **Prometheus**, **Grafana**

This project serves as a portfolio demonstrating production-inspired DevOps, GitOps, and Platform Engineering practices on Kubernetes.

---
## Why This Project

Modern cloud-native applications require more than just deploying containers—they demand reliable infrastructure, automated delivery, security, observability, and scalable operations. This project was created to demonstrate how these capabilities can be integrated into a production-inspired Kubernetes platform using GitOps principles.

Rather than focusing only on the application itself, this repository showcases the end-to-end platform that provisions infrastructure, automates CI/CD, enforces security policies, manages secrets, enables progressive delivery, provides observability, and supports autoscaling on **Google Kubernetes Engine (GKE)**.

The goal is to provide a practical reference implementation for DevOps and Platform Engineering practices using modern CNCF and cloud-native technologies.

---
## Platform Architecture

<p align="left">
  <img src="assets/screenshots/architecture.png" width="550" alt="Architecture">
</p>

---
## 🎥 Demo Walkthrough

---

### 🔵 Blue-Green Deployment

![Blue-Green Deployment](docs/images/blue-green-deployment.gif "Blue-Green Deployment Demo")

---

### 🟢 Canary Deployment

![Canary Deployment](docs/images/canary-deployment.gif "Canary Deployment Demo")

---

### 💰 Kubecost

![Kubecost](docs/images/kubecost.gif "Kubecost Demo")

---
## Table of Contents

- [Overview](#overview)
- [How It Fits Into the Platform](#how-it-fits-into-the-platform)
- [Architecture](#architecture)
- [Folder Structure](#folder-structure)
- [Folders In Detail](#folders-in-detail)

---

## Overview

This repo contains **no application source code**. It contains only Kubernetes manifests, Kustomize overlays, and ArgoCD configuration — the desired state of the cluster. It is updated automatically by the CI pipeline in `voting-app` whenever a new image is built and pushed.

| What lives here                                  | What does NOT live here       |
|--------------------------------------------------|-------------------------------|
| Kubernetes Deployments, Services, ConfigMaps     | Application source code       |
| Kustomize `base/` and `overlays/`                | Dockerfiles                   |
| ArgoCD `Application` and `AppProject` manifests  | CI pipeline definitions       |
| Infrastructure add-on configs (namespaces, RBAC) | Terraform infrastructure code |
| Platform tooling manifests                       | Secrets (managed via External Secrets or Sealed Secrets)  |

---
## How It Fits Into the Platform

This repo is the middle layer of a three-repo GitOps platform:

| Repo                                            | Role                                   |
|-------------------------------------------------|----------------------------------------|
| `platform-infra`                                | Terraform — provisions GKE, VPC, IAM,  Artifact Registry                      |
| `voting-app`                                    | Application source code + CI builds + image push                           |
| **`gitops-microservices-platform`** (this repo) | Desired Kubernetes state — watched by  ArgoCD                                 |

```
voting-app CI pushes new image tag
              │
              ▼
  gitops-microservices-platform
  (kustomize edit set image → git commit)
              │
              ▼
      ArgoCD detects the change
              │
              ▼
    GKE cluster synced ✓
```
---
## Architecture

![Project Overview](docs/images/architecture.png "Project Demo")

---
## Folder Structure

```
gitops-microservices-platform/
├── README.md
│
├── apps/                               # Voting app microservice Kubernetes manifests
│   ├── vote/                           # Python voting frontend
│   │   ├── base/
│   │   │   ├── analysis-template.yaml
│   │   │   ├── hpa.yaml
|   |   |   ├── kustomization.yaml
│   │   │   ├── pdb.yaml
|   |   |   ├── postgres-alias.yaml
|   |   |   ├── rbac.yaml
│   │   │   ├── redis-alias.yaml
|   |   |   ├── rollout.yaml
|   |   |   ├── serviceaccount.yaml
│   │   │   ├── servicemonitor.yaml
|   |   |   ├── vote-alert-rules.yaml
│   │   │   ├── vote-canary-service.yaml
│   │   │   └── vote-stable-service.yaml
│   │   └── overlays/
│   │       └── dev/  
│   │           └── kustomization.yaml/ # Sets image tag, replica count, env patches
│   ├── result/                         # Node.js results frontend
│   │   ├── base/
│   │   │   ├── kustomization.yaml
│   │   │   ├── rbac.yaml
|   |   |   ├── result-active-svc.yaml
│   │   │   ├── result-preview-svc.yaml
|   |   |   ├── rollout.yaml
│   │   │   ├── serviceaccount.yaml
│   │   │   └── servicemonitor.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           ├── kustomization.yaml  # Updated automatically by CI with new image SHA
│   ├── worker/                         # .NET vote processor
│   │   ├── base/
│   │   │   ├── deployment.yaml
│   │   │   ├── postgres-secret.yaml
│   │   │   ├── scaledobject-worker.yaml
│   │   │   ├── serviceaccount.yaml
│   │   │   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml
|   |
├── infrastructure/   
│   ├── external-secrets-sa/                          
│   │   ├── base/
│   │   │   ├── serviceaccount.yaml
│   │   │   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml
|   |
│   ├── pgadmin/                          
│   │   ├── base/
│   │   │   ├── deployment.yaml
│   │   │   ├── namespace.yaml
|   |   |   ├── pgadmin-httproute.yaml
│   │   │   ├── pgadmin-secret.yaml
│   │   │   ├── pgadmin-svc.yaml
│   │   │   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml
|   |
│   ├── redis/                          # Redis in-memory queue
│   │   ├── base/
│   │   │   ├── service-headless.yaml
│   │   │   ├── service.yaml
|   |   |   ├── statefulset.yaml
│   │   │   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml
│   └── postgres/                             # PostgreSQL persistent store
│       ├── base/
│       │   ├── externalsecret.yaml
|       |   ├── service-headless.yaml
│       │   ├── service.yaml
│       │   ├── statefulset.yaml
│       │   ├── serviceaccount.yaml
│       │   └── kustomization.yaml
│       └── overlays/
│           └── dev/
│               └── kustomization.yaml
│
├── argocd/                             # ArgoCD bootstrap and Application definitions
│   ├── projects/
|   |   ├── apps-project.yaml           # AppProject — scopes apps to this repo and cluster
|   |   ├── infra-project.yaml   
│   │   └── platform-project.yaml     
│   ├── applicationsets/
│   │   ├── apps-appset.yaml                 # ArgoCD Application for apps service
│   │   ├── infra-appset.yaml                # ArgoCD Application for infra service
│   │   └── platform-appset.yaml             # ArgoCD Application for platform service
│
|
├── automation/   
│   ├── common/                          
│   │   ├── base/
│   │   │   ├── serviceaccount.yaml
│   │   │   ├── namespace.yaml
│   │   │   ├── rbac.yaml
│   │   │   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml
|   |
│   ├── daily-platform-report/                          
│   │   ├── base/
│   │   │   ├── cronjob.yaml
│   │   │   ├── service.yaml
|   |   |   ├── servicemonitor.yaml
│   │   │   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml
|
| 
├── governance/                             
│   ├── argocd/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml
|   |
│   ├── cert-manager/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml
|   |
│   ├── external-secrets/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml
|   |
│   ├── falco/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml
|   |
│   ├── kyverno/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml
|   | 
│   ├── monitoring/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml
|   |
│   ├── nginx-gateway/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml
|   |
│   ├── postgres/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml
|   |
│   ├── redis/
|   |   ├── kustomization.yaml          
|   |   ├── limitrange.yaml   
│   │   └── resourcequota.yaml  
|   |  
│   ├── vote/
│   │   ├── apps-appset.yaml                 
│   │   ├── infra-appset.yaml                
│   │   └── platform-appset.yaml  
|   |
|   ├── kustomization.yaml             
|
└── platform/                               
│   ├── cluster-secrets/                           
│   │   ├── base/
│   │   │   ├── cloudflare-externalsecret.yaml
│   │   │   ├── gcp-cluster-secret-store.yaml
│   │   │   ├── vault-secret-store.yaml
|   |   |   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/  
│   │           └── kustomization.yaml/ 
|   |
|   ├── clusterissuer/                           
│   │   ├── base/
│   │   │   ├── clusterissuer-staging.yaml
|   |   |   ├── clusterissuer.yaml
|   |   |   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/  
│   │           └── kustomization.yaml/
|   |
|   ├── gateway-api/                           
│   │   ├── base/
|   |   |   ├──httproutes
|   |   |   |    ├── argo-rollouts-route.yaml
|   |   |   |    ├── argocdhttproute.yaml
|   |   |   |    ├── grafanahttproute.yaml
|   |   |   |    ├── kube-cost-route.yaml
|   |   |   |    ├── postgres-exporter-route.yaml
|   |   |   |    ├── preview-result-httproute.yaml
|   |   |   |    ├── prometheushttproute.yaml
|   |   |   |    ├── redis-exporter-route.yaml
|   |   |   |    ├── resulthttproute.yaml
|   |   |   |    ├── vault-route.yaml
|   |   |   |    └── votehttproute.yaml
│   │   │   ├── gateway.yaml
|   |   |   ├── certificate.yaml
|   |   |   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/  
│   │           └── kustomization.yaml/ 
│   ├── ingress/                         
│   │   ├── base/
│   │   │   ├── kustomization.yaml
│   │   │   ├── argocd-ingress.yaml
|   |   |   ├── grafana-ingress.yaml
│   │   │   ├── prometheus-ingress.yaml
|   |   |   ├── result-ingress.yaml
│   │   │   └── vote-ingress.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml/  
|   |
│   ├── monitoring /      
|   |   ├── postgres-exporter /                   
│   │   |   ├── base/
|   |   |   |   ├── kustomization.yaml
│   │   |   │   ├── deployment.yaml
│   │   |   │   ├── service.yaml
|   |   |   |   └── servicemonitor.yaml
|   |   |   └── overlays/
|   |   |        └── dev/
│   │                └── kustomization.yaml/
|   |   ├── redis-exporter /                   
│   │   |   ├── base/
|   |   |   |   ├── kustomization.yaml
│   │   |   │   ├── deployment.yaml
│   │   |   │   ├── service.yaml
|   |   |   |   └── servicemonitor.yaml
|   |   |   └── overlays/
|   |   |        └── dev/
│   │                └── kustomization.yaml/
|   |
│   ├── namespaces/                         
│   │   ├── base/
│   │   │   ├── kustomization.yaml
│   │   │   ├── postgres-namespace.yaml
|   |   |   ├── redis-namespace.yaml
│   │   │   └── vote-namespace.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml/
|   |
│   ├── velero/                         
│   │   ├── base/
│   │   │   ├── kustomization.yaml
│   │   │   ├── backup-demo.yaml
|   |   |   ├── application.yaml
│   │   │   ├── platform-project.yaml
|   |   |   ├── restore-demo.yaml
│   │   │   └── schedules.yaml
│   │   └── overlays/
│   │       └── dev/
│   │           └── kustomization.yaml/
|
└── security/                               
│   ├── falco/                           
│   │   ├── base/
│   │   │   ├── custom-rules.yaml
|   |   |   └── kustomization.yaml
│   │   └── overlays/
│   │       └── dev/  
│   │           └── kustomization.yaml/ 
|   |
|   ├── kyverno/ 
|   |   ├── base/
|   |   |   └── kustomization.yaml
|   |   ├── governance
│   │   |      ├── block-default-namespace.yaml
|   |   |      └── labels.yaml
|   |   |       
|   |   ├── images
|   |   |      ├── latest-tag.yaml
|   |   |      └── registries.yaml
|   |   |       
|   |   ├── ingress
|   |   |      ├── clusterissuer.yaml
|   |   |      └── tls.yaml
|   |   |       
|   |   ├── network
|   |   |       └── default-deny-network-policy.yaml
|   |   |       
|   |   ├── platform
|   |   |       ├── protect-critical-resources.yaml
|   |   |       └── pss-restricted.yaml
|   |   |       
|   |   ├── rbac
|   |   |       ├── kyverno-cert-manager-reports.yaml
|   |   |       ├── kyverno-gateway-reports.yaml
|   |   |       └── kyverno-reports-extra.yaml
|   |   |       
|   |   ├── reliability
|   |   |       └── probes.yaml
|   |   |       
|   |   ├── resources
|   |   |       └── requests-limits.yaml
|   |   |       
|   |   ├── security
|   |   |       ├── capabilities.yaml
|   |   |       ├── privilege-escalation.yaml
|   |   |       ├── privileged.yaml
|   |   |       ├── run-as-non-root.yaml
|   |   |       ├── seccomp.yaml
|   |   |       └── verify-signed-images.yaml
|   |         
|   |   ├── overlays
|   |   |    ├── dev
|   |   |         └── kustomization.yaml
|   |   |
|   ├── network-policies/ 
|   |   ├── base/
|   |   |   └── kustomization.yaml
|   |   |
|   |   ├── monitoring
│   │   |      ├── allow-prometheus-to-result.yaml
|   |   |      └── allow-prometheus-to-vote.yaml
|   |   |       
|   |   ├── postgres
|   |   |      ├── allow-result-to-db.yaml
|   |   |      ├── allow-vote-namespace.yaml
|   |   |      └── default-deny.yaml
|   |   |       
|   |   ├── redis
|   |   |      ├── allow-dns.yaml
|   |   |      ├── allow-vote-namespace.yaml
|   |   |      └── default-deny.yaml
|   |   |       
|   |   ├── vote
|   |   |       ├── allow-dns.yaml
|   |   |       ├── allow-external-to-result.yaml
|   |   |       ├── allow-external-to-vote.yaml
|   |   |       ├── allow-gateway-result.yaml
|   |   |       ├── allow-ingress-nginx-to-result.yaml
|   |   |       ├── allow-ingress-nginx-to-vote.yaml
|   |   |       ├── allow-result-to-postgres.yaml
|   |   |       ├── allow-vote-to-redis.yaml
|   |   |       ├── allow-worker-to-postgres.yaml
|   |   |       ├── allow-worker-to-redis.yaml
|   |   |       └── default-deny.yaml
|   |   ├── overlays
|   |   |    ├── dev
                  └── kustomization.yaml




```
---
## Folders In Detail

The **gitops-microservices-platform** repository serves as the **GitOps source of truth** for the Kubernetes platform. Every application workload, infrastructure component, platform service, security policy, governance policy, and operational automation is defined declaratively in Git and continuously reconciled to the Kubernetes cluster by **ArgoCD**.

The repository is organized into seven logical layers:

```text
gitops-microservices-platform/
├── apps/
├── infrastructure/
├── platform/
├── security/
├── governance/
├── automation/
└── argocd/
```

Each directory has a clearly defined responsibility, providing a clean separation between application deployments, infrastructure resources, platform services, security controls, governance policies, operational automation, and GitOps configuration.

---
### 📁 apps/

The **apps/** directory contains the Kubernetes manifests for the voting application workloads. Each microservice is managed independently using **Kustomize**, allowing every service to follow its own deployment lifecycle while maintaining a consistent repository structure.

```text
apps/
├── vote/
├── result/
└── worker/
```

Every application follows the same Kustomize layout:

```text
service/
├── base/
└── overlays/
    └── dev/
```

### base/

The **base/** directory contains reusable Kubernetes manifests that remain independent of any deployment environment.

Typical resources include:

- Rollouts or Deployments
- Services
- Service Accounts
- RBAC
- Horizontal Pod Autoscalers (HPA)
- ServiceMonitors
- Pod Disruption Budgets (PDB)
- Prometheus Alert Rules
- AnalysisTemplates
- Kustomization files

These manifests define the desired application state without environment-specific configuration.

### overlays/

The **overlays/** directory contains environment-specific customizations.

Instead of modifying the base manifests, overlays apply patches such as:

- Container image tag
- Replica count
- Environment variables
- Labels and annotations
- Resource requests and limits
- Namespace configuration

After every successful build, the GitHub Actions CI pipeline automatically updates the image tag inside the corresponding overlay before committing the changes back to this GitOps repository.

---
### Vote Service

**Technology:** Python (Flask)

The Vote service provides the user-facing interface for casting votes.

#### Responsibilities

- Accept user requests
- Validate incoming votes
- Publish vote messages to Redis
- Expose Prometheus metrics
- Progressive delivery using **Argo Rollouts (Canary)**

---
### Result Service

**Technology:** Node.js

The Result service retrieves processed vote data from PostgreSQL and displays live voting results.

#### Responsibilities

- Query Cloud SQL for PostgreSQL
- Display live voting results
- Expose Prometheus metrics
- Progressive delivery using **Argo Rollouts (Blue-Green)**

---
### Worker Service

**Technology:** .NET

The Worker service performs asynchronous background processing.

#### Responsibilities

- Continuously consume messages from Redis
- Process vote events
- Persist processed data into PostgreSQL
- Automatically scale using **KEDA** based on Redis queue length

---
### 📁 infrastructure/

The **infrastructure/** directory contains the stateful infrastructure components that run inside the Kubernetes cluster.

```text
infrastructure/
├── postgres/
├── redis/
├── pgadmin/
└── external-secrets-sa/
```

These resources provide persistent storage, message queuing, administration tools, and supporting service accounts required by the application platform.

---
### PostgreSQL

Provides the application's persistent relational database.

#### Resources

- StatefulSet
- ClusterIP Service
- Headless Service
- ExternalSecret
- ServiceAccount
- Kustomize overlays

---
### Redis

Provides the in-memory message queue used by the Worker service.

#### Resources

- StatefulSet
- ClusterIP Service
- Headless Service
- Kustomize overlays

---
### pgAdmin

Provides a browser-based administration interface for PostgreSQL.

#### Resources

- Deployment
- Service
- HTTPRoute
- Secret
- Namespace
- Kustomization

---
### External Secrets Service Account

Provides the Kubernetes ServiceAccount used by External Secrets Operator to authenticate with external secret providers.

---
### 📁 platform/

The **platform/** directory contains shared platform services that support networking, certificates, secrets management, monitoring, namespaces, and backup operations.

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

Unlike application manifests, these resources provide shared services used across the entire Kubernetes platform.

---
### cluster-secrets/

Centralizes secret management across the platform.

#### Includes

- HashiCorp Vault Secret Store
- Google Secret Manager ClusterSecretStore
- Cloudflare ExternalSecret

---
### clusterissuer/

Manages automated TLS certificate issuance using **cert-manager**.

#### Includes

- Production ClusterIssuer
- Staging ClusterIssuer

---
### gateway-api/

Defines external traffic routing using **Gateway API** and **NGINX Gateway Fabric**.

#### Resources

- Gateway
- Certificate
- HTTPRoutes for:
  - Vote
  - Result
  - Preview Result
  - ArgoCD
  - Grafana
  - Prometheus
  - Vault
  - Kubecost
  - PostgreSQL Exporter
  - Redis Exporter

---
### ingress/

Contains legacy Kubernetes Ingress resources where required by platform components.

---
### monitoring/

Deploys platform monitoring exporters.

#### Includes

- PostgreSQL Exporter
- Redis Exporter
- ServiceMonitors

---
### namespaces/

Creates the Kubernetes namespaces required by the platform.

Examples include:

- vote
- postgres
- redis

---
### velero/

Provides backup and disaster recovery resources.

#### Includes

- Scheduled backups
- Backup policies
- Restore manifests
- Velero Application

---
### 📁 security/

The **security/** directory centralizes the platform's security controls.

```text
security/
├── kyverno/
├── falco/
└── network-policies/
```

These components implement admission control, runtime security, and network segmentation.

---
### Kyverno

Provides Kubernetes-native policy enforcement.

#### Policy Categories

- Pod Security Standards
- Image verification
- Signed image verification
- Resource requests and limits
- Namespace governance
- TLS enforcement
- RBAC
- Platform protection
- Container hardening
- Network security

---
### Falco

Provides runtime threat detection for Kubernetes workloads.

#### Responsibilities

- Detect suspicious container activity
- Monitor Kubernetes events
- Apply custom runtime detection rules
- Generate security alerts

---

### Network Policies

Implements namespace isolation using Kubernetes NetworkPolicies.

Explicit communication is allowed only between trusted workloads.

#### Allowed Traffic

- Vote → Redis
- Worker → Redis
- Worker → PostgreSQL
- Result → PostgreSQL
- Prometheus → Application Metrics

All other traffic is denied by default using a **Default Deny** policy.

---
### 📁 governance/

The **governance/** directory defines namespace-level governance and resource management policies.

Each namespace includes:

- ResourceQuota
- LimitRange

These policies ensure consistent resource allocation while preventing resource exhaustion caused by noisy-neighbor workloads.

Namespaces managed include:

- ArgoCD
- Monitoring
- PostgreSQL
- Redis
- Kyverno
- Falco
- External Secrets
- cert-manager
- NGINX Gateway

---
### 📁 automation/

The **automation/** directory contains operational automation executed inside Kubernetes using **CronJobs**.

Current automation includes:

- Daily platform health reports
- Kubernetes CronJobs
- Shared ServiceAccounts
- RBAC
- Platform monitoring endpoints
- Slack notifications

These workloads automate recurring operational tasks and reduce manual operational effort.

---
### 📁 argocd/

The **argocd/** directory bootstraps the entire GitOps platform.

```text
argocd/
├── projects/
└── applicationsets/
```

---
#### projects/

Defines ArgoCD **AppProjects**.

Each project restricts:

- Allowed Git repositories
- Destination namespaces
- Kubernetes clusters
- Allowed Kubernetes resource types

Separate projects are maintained for:

- Applications
- Infrastructure
- Platform

This separation improves governance and follows the principle of least privilege.

---

#### applicationsets/

Defines the **ApplicationSets** responsible for automatically generating ArgoCD Applications.

Each ApplicationSet deploys one logical platform layer:

- Applications
- Infrastructure
- Platform Services

Every generated Application is configured with:

- Automated synchronization
- Self-healing
- Resource pruning
- Declarative GitOps deployment

As a result, any change committed to Git is automatically detected by ArgoCD, synchronized to the Kubernetes cluster, and continuously reconciled to ensure the live environment always matches the desired state stored in Git.

---