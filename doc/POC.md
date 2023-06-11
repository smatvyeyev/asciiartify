# POC.md

# install k3d:

    wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | TAG=v5.0.0 bash

    k3d version

# create cluster 

    k3d cluster create demo
    kubectl cluster-info

# install Argo CD

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
