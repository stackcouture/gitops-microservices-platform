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
## Platform Capabilities

| Capability                          | Implementation                                                                                                                |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **Infrastructure Provisioning**     | Provisions and manages Google Cloud infrastructure using Terraform.                                                           |
| **Container Orchestration**         | Runs containerized workloads on Google Kubernetes Engine (GKE).                                                               |
| **GitOps Continuous Delivery**      | Synchronizes Kubernetes manifests from Git repositories using Argo CD.                                                        |
| **Continuous Integration**          | Automates testing, image building, security scanning, signing, and publishing with GitHub Actions.                            |
| **Configuration Management**        | Manages Kubernetes manifests across environments using Kustomize.                                                             |
| **Progressive Delivery**            | Performs controlled canary deployments using Argo Rollouts.                                                                   |
| **Networking & Traffic Management** | Routes external traffic using the Kubernetes Gateway API.                                                                     |
| **Security & Secrets Management**   | Enforces Kubernetes policies with Kyverno and manages secrets through External Secrets integrated with Google Secret Manager and Hashicorp Vault. |
| **Observability**                   | Provides metrics, dashboards and alerting with Prometheus and Grafana.                           |
| **Autoscaling**                     | Supports resource-based and event-driven scaling using HPA and KEDA.                                                          |
| **Stateful Services**               | Deploys PostgreSQL and Redis as persistent data services for application workloads.                                           |

---
## Technology Stack

| Category                   | Technologies                                     |
| -------------------------- | ------------------------------------------------ |
| **Cloud Platform**         | Google Cloud Platform (GCP)                      |
| **Infrastructure as Code** | Terraform                                        |
| **Container Platform**     | Docker, Google Kubernetes Engine (GKE)           |
| **Continuous Integration** | GitHub Actions                                   |
| **GitOps**                 | Argo CD, Kustomize                               |
| **Container Registry**     | Google Artifact Registry                         |
| **Networking**             | Kubernetes Gateway API                           |
| **Progressive Delivery**   | Argo Rollouts                                    |
| **Security**               | Kyverno, External Secrets, Google Secret Manager |
| **Observability**          | Prometheus, Grafana, Loki                        |
| **Autoscaling**            | Horizontal Pod Autoscaler (HPA), KEDA            |
| **Data Services**          | PostgreSQL, Redis                                |
| **Languages & Frameworks** | Python (Flask), Node.js (Express), .NET          |
| **Version Control**        | Git, GitHub                                      |

---
## Platform Architecture

### Architecture Diagram

<p align="left">
  <img src="docs/images/architecture.png" width="900" alt="Architecture">
</p>

### Architecture Overview

The platform is organized into four logical layers, each responsible for a specific part of the application lifecycle.

#### Infrastructure Layer

The infrastructure layer provisions the Google Cloud environment using Terraform, including networking, IAM, Google Kubernetes Engine (GKE), and Artifact Registry.

#### CI Layer

The CI layer uses GitHub Actions to build, test, scan, sign, and publish container images. Successful builds update the GitOps repository with the new image versions.

#### GitOps Layer

The GitOps layer serves as the single source of truth for Kubernetes manifests. Argo CD continuously monitors the GitOps repository and reconciles the desired state with the Kubernetes cluster, enabling automated deployments and self-healing.

#### Kubernetes Platform Layer

The Kubernetes platform hosts the application workloads and platform services. The voting application runs alongside operational components such as Gateway API, Argo Rollouts, Kyverno, External Secrets, Prometheus, Grafana, Loki, HPA, and KEDA, providing networking, security, observability, progressive delivery, and autoscaling capabilities.

---
## Repository Structure

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

The repository is organized into logical directories, each responsible for a specific layer of the GitOps platform.

---
## Demo 

### Blue-Green Deployment

![Blue-Green Deployment](docs/images/blue-green-deployment.gif "Blue-Green Deployment Demo")

### Canary Deployment

![Canary Deployment](docs/images/canary-deployment.gif "Canary Deployment Demo")

### Kubecost

![Kubecost](docs/images/kubecost.gif "Kubecost Demo")

---
## Documentation

| Directory           | Purpose                                                                                                    |
| :------------------ | :--------------------------------------------------------------------------------------------------------- |
| **apps/**           | Kubernetes manifests for application workloads managed with Kustomize overlays.                            |
| **infrastructure/** | Stateful services such as PostgreSQL, Redis, and supporting infrastructure components.                     |
| **platform/**       | Shared platform services including networking, certificates, monitoring, namespaces, and backup resources. |
| **security/**       | Kubernetes security policies, runtime security, and network segmentation.                                  |
| **governance/**     | Namespace governance through ResourceQuota and LimitRange policies.                                        |
| **automation/**     | Operational automation using Kubernetes CronJobs and supporting resources.                                 |
| **argocd/**         | Argo CD AppProjects and ApplicationSets for GitOps-based deployment and lifecycle management.              |

> **Note:** Detailed documentation for each directory and its components is available in the **[`docs/`](docs/platform-components)** directory.

---
## Future Enhancements

The platform is continuously evolving. Planned enhancements include:

* Multi-environment support (Development, Staging, and Production)
* Automated end-to-end integration testing
* Multi-cluster GitOps deployments with Argo CD
* Disaster recovery validation using Velero restore testing
* Cost optimization and reporting with Kubecost
* Policy-as-Code validation within the CI pipeline
* Advanced observability with distributed tracing using OpenTelemetry
* Infrastructure compliance and security scanning in CI/CD
* Automated dependency and container image updates
* Support for additional cloud providers to enable a multi-cloud deployment model

These enhancements will further improve the platform's scalability, reliability, security, and operational maturity.

---
## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.
