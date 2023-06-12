# Deploying Applications to k3d with ArgoCD automation

## Install the ArgoCD with Graphical Interface in Kubernetes Cluster k3d

### install k3d:
    
    wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | TAG=v5.0.0 bash

    k3d version

### create cluster 

    k3d cluster create argo
    kubectl cluster-info

### install Argo CD

    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

    kubectl get ns
    kubectl get all -n argocd
    kubectl get pods -n argocd -w

### forward GUI port

    kubectl port-forward svc/argocd-server -n argocd 8080:443&

    (codespace portforwarding workaround -  Ports tab -> change portmapping 8080 to  HTTPS and port visibility to PUBLIC)

### get admin password 

    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo 
   
  save password i safe place. 

### login to GUI

    Open a web browser and navigate to the ArgoCD URL.

    On the login screen, enter your username and password and click "Login"

    Once logged in, you will be directed to the ArgoCD dashboard
    
## Create Application in ArgoCd

    In the GUI of ArgoCD add new app  
    Enter Application Name "demo" and Project Name (select default)
    Set SYNC POLICY as Automation, enable check-box AUTO-CREATE NAMESPACE
    In Source section enter Git Repository URL https://github.com/den-vasyliev/go-demo-app , select revision HEAD and path of helm chart 'helm" 
    In Destination section select  Cluster URL by default (https://kubernetes.default.svc) and enter namespace name 'demo'
    Press create button.
    Now you need to synchronize the application state  with the repository - press Sync
    After synchronizing the application, you can observe  status and details of the app cluster
    All cheges the repository, should be automatically applied to the demo app cluster. 

#### demo app setup
![Alt text](argo-demo-sync.gif)


## Chek how to work app demo cluster
    In  terminal run command for app port-forward

    kubectl port-forward -n demo svc/ambassador 8088:80&

    Сheck availability (resault - app version)

    $ curl localhost:8088
    k8sdiy-api:599e1af%

    Download the image of Google logo 
    wget -O /tmp/g.png https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png 

    resault - Saving to: ‘/tmp/g.png’

    Push the image to demo app
    curl -F 'image=@/tmp/g.png' localhost:8088/img/

    Chek the result like
 <img width="1385" alt="demoapp-with-argo-deployment" src="https://github.com/smatvyeyev/asciiartify/assets/30805877/664d4ece-603b-4e86-b539-fecdbf92ad2b">

## Conclusion
    Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes.
    Application definitions, configurations, and environments should be declarative and version controlled. Application deployment and lifecycle management should be automated,       auditable, and easy to understand.
