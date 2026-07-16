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

