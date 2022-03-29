# Kyoube

Playing with arkade, kind, minikube, argocd...

## Bootstrap

```bash
curl -sLS https://get.arkade.dev | sudo sh
for cli in argocd helm kind kubectl kubeseal minikube; do 
  arkade get $cli
  sudo mv ~/.arkade/bin/$cli /usr/local/bin/
done
```

KIND

```
kind create cluster --name kyoube
kubectl cluster-info --context kind-kyoube

# If you create more cluster, switch like this
kubectl config use-context kind-kyoube
```

Or minikube

```
minikube start --driver kvm2 --cpus=4 --memory=8g
minikube addons enable ingress
```

Then ArgoCD

```
arkade install argocd
kubectl port-forward svc/argocd-server -n argocd 8443:443 &
PASS=$(kubectl get secret argocd-initial-admin-secret \
  -n argocd \
  -o jsonpath="{.data.password}" | base64 -d)
echo $PASS
```

Wait until it's ready

```
kubectl apply -f argocd_applicationset_infrastructure.yaml
```

## Download chart localy

```bash
# helm repo add istio https://istio-release.storage.googleapis.com/chartsupdate
helm repo add argo https://argoproj.github.io/argo-helm
helm repo add external-dns https://kubernetes-sigs.github.io/external-dns/
helm repo add grafana https://grafana.github.io/helm-charts
helm repo add jetstack https://charts.jetstack.io
helm repo add nats https://nats-io.github.io/k8s/helm/charts/
helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts      
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets

helm repo update

cd infrastructure

for chart in argo/argo-cd external-dns/external-dns jetstack/cert-manager sealed-secrets/sealed-secrets; do
  tar xf ${chart}*.tar.gz
  rm ${chart}*.tar.gz
done
```

