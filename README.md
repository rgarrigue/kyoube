# Kyoube

Playing with arkade, kind, argocd...

## Bootstrap

```bash
curl -sLS https://get.arkade.dev | sudo sh
arkade get helm kind kubectl
kind create cluster --name kyoube
kubectl cluster-info --context kind-kyoube

# If you create more cluster, switch like this
kubectl config use-context kind-kyoube
```

## Download chart localy

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo add external-dns https://kubernetes-sigs.github.io/external-dns/
helm repo add grafana https://grafana.github.io/helm-charts
# helm repo add istio https://istio-release.storage.googleapis.com/chartsupdate
helm repo add jetstack https://charts.jetstack.io
helm repo add nats https://nats-io.github.io/k8s/helm/charts/
helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts      

helm repo update

helm pull argo/argo-cd
helm pull external-dns/external-dns
helm pull jetstack/cert-manager
# ...
```

