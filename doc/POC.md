## POC.md
## Install the ArgoCD with Graphical Interface in Kubernetes Cluster k3d

## install k3d:
    `````
    wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | TAG=v5.0.0 bash

    k3d version
    `````

## create cluster 

    k3d cluster create argo
    kubectl cluster-info

## install Argo CD

    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

    kubectl get ns
    kubectl get all -n argocd
    kubectl get pods -n argocd -w

## forward GUI port

    kubectl port-forward svc/argocd-server -n argocd 8080:443&

    (codespace portforwarding workaround -  Ports tab -> change portmapping 8080 to  HTTPS and port visibility to PUBLIC)

## get admin password 

    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo 
    67SVU0fzQ7mQ5Q2R

## login to GUI

    Open a web browser and navigate to the ArgoCD URL.

    On the login screen, enter your username and password and click "Login".

    Once logged in, you will be directed to the ArgoCD dashboard.



