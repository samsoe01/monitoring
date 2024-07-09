# Monitoring for k8s cluster

```
Preparing a Kubernetes Cluster

1. Install Docker Desktop Download and Install Docker Desktop:

Go to the Docker Desktop download page and download the installer for your operating system.

Enable Kubernetes in Docker Desktop: Open Docker Desktop. Go to Settings (click the gear icon). Navigate to the Kubernetes tab. Check the box Enable Kubernetes. Click Apply & Restart.
Single node cluster will be crated.

Access k8s dashboard:

first deploy dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

second, use kubectl proxy

third, now you should be able to access dashboard here: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

To get token:

kubectl -n kubernetes-dashboard get secret kubernetes-dashboard -o jsonpath="{.data.password}" | base64 -d

or create new user with admin rights:

kubectl create sa kube-ds-admin -n kube-system
kubectl create clusterrolebinding kube-ds-admin-role-binding --clusterrole=cluster-admin --user="system:serviceaccount:kube-system:kube-ds-admin"
kubectl create token kube-ds-admin -n kube-system
```
```
#Deploy Automation Stack
2. Install ArgoCD

Create a namespace for ArgoCD: kubectl create namespace argocd Install ArgoCD in the created namespace: kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl port-forward svc/argocd-server -n argocd 8080:443
Access ArgoCD UI
username: admin
view secret for password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
Open a browser and go to https://localhost:8080.

Login with username admin and the password retrieved in the previous step.
```
```
#Deploy Monitoring
3. Set up a Git Repository
https://github.com/samsoe01/monitoring
Copy chart kube-prometheus-stack from https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack and add it to git repo
kubectl apply -f monitoring1.yml
after applying this manifest Grafana, Prmetheus,Alertmanager, NodeExporter, prometheus-operator will be deployed with argocd.
Grafana dashboards and alert rules are included in this chart.

Access grafana dashboard using kubectl port-forward -n monitoring svc/grafana 3000:80 and go to http://localhost:3000 username: admin passwd: check secret
