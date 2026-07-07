## Argo Rollouts Setup 

curl -LO https://github.com/argoproj/argo-rollouts/releases/latest/download/kubectl-argo-rollouts-linux-amd64
chmod +x ./kubectl-argo-rollouts-linux-amd64
sudo mv ./kubectl-argo-rollouts-linux-amd64 /usr/local/bin/kubectl-argo-rollouts
kubectl argo rollouts version

kubectl argo rollouts list rollouts -A

kubectl argo rollouts dashboard –n vote


---

## Kustomize setup 

curl -s https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh | bash
sudo mv kustomize /usr/local/bin/
kustomize version


kubectl kustomize overlays/dev
kubectl apply -k overlays/dev

---


