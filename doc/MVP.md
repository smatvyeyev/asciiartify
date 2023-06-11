## Deploying Applications to k3d with ArgoCD automation



https://github.com/den-vasyliev/go-demo-app


![Alt text](argo-demo-sync.gif)

argocd app set <APPNAME> --sync-policy automated

argocd app set <APPNAME> --self-heal