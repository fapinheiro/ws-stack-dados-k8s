# GitOps using ArgoCD

### install
```sh
brew install helm
brew install argocd
```

### repos
```sh
helm repo add argo https://argoproj.github.io/argo-helm
```

### build
```sh
terraform init
terraform apply
```

### configure [manual]
```sh
kubectl patch svc argocd-server -n gitops -p '{"spec": {"type": "ClusterIP"}}'
kubectl get svc argocd-server -n gitops
kubectl -n gitops get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kubectl port-forward service/argocd-server 8083:80 -n gitops
argocd login "localhost:8083" --username admin --password "pwd" --insecure
argocd cluster add "docker-desktop"
kubectl delete -f git-repo-con2-tmp.yaml -n gitops
export $(cat .env | xargs) && env
sed 's,#MY_GITHUB_ACCESS_TOKEN#,'"$MY_GITHUB_ACCESS_TOKEN"',g' git-repo-con.yaml > git-repo-con-tmp.yaml
sed 's,#MY_GITHUB_USERNAME#,'"$MY_GITHUB_USERNAME"',g' git-repo-con-tmp.yaml > git-repo-con2-tmp.yaml 
kubectl apply -f git-repo-con2-tmp.yaml -n gitops
kubectl get secret private-ws-stack-dados-k8s -n gitops -o yaml
```
