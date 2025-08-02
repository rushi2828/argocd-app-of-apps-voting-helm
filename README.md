# ArgoCD App-of-Apps Voting Demo

Welcome! This project is a demonstration of deploying a sample Voting App on a Kubernetes cluster using the **ArgoCD App-of-Apps pattern** and Helm charts.

## 🚀 Features

- **App-of-Apps**: Manage multiple Kubernetes apps declaratively via ArgoCD.
- **Helm support**: Deploy using reusable Helm charts.
- **Kubernetes-native**: Uses Kind to spin up a local dev cluster.
- **Ingress NGINX**: Exposes your app for easy local access.
- **One-stop deployment**: All instructions included for repeatable installs.


## 🛠️ Quick Start Guide

### 1. Create a Kind (Kubernetes in Docker) Cluster

```bash
kind create cluster --name voting-app --config kind-config/kind-config.yaml
```


### 2. Create the ArgoCD Namespace

```bash
kubectl create namespace argocd
```


### 3. Install ArgoCD

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```


### 4. Wait for ArgoCD Pods to be Running

Check pod status:

```bash
kubectl get pods -n argocd
```

Wait until all ArgoCD pods are in `Running` status.

### 5. Retrieve the ArgoCD Admin Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```


### 6. Port-Forward for ArgoCD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8081:443
```

Access the UI at: [https://localhost:8081](https://localhost:8081)

### 7. Install Ingress NGINX

```bash
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml
```


### 8. Wait for Ingress Controller Ready

```bash
kubectl wait --namespace ingress-nginx --for=condition=Ready pod --selector=app.kubernetes.io/component=controller --timeout=180s
```


### 9. Verify Ingress NGINX Pods

```bash
kubectl get all -n ingress-nginx
```


## 🎨 Project Structure

```
├── apps/              # ArgoCD Application manifests for each sub-app (db, redis, vote, result, worker, ingress)
├── bootstrap/         # "App of Apps" ArgoCD manifest for bootstrapping the whole system
├── charts/            # Helm charts for each service
│   ├── db/
│   ├── redis/
│   ├── result/
│   ├── vote/
│   ├── worker/
├── ingress/           # Helm chart for external access to the Voting App
├── kind-config/       # Local Kind (Kubernetes-in-Docker) cluster config
└── README.md          # This file

```

- **apps/**: Definitions for sub-applications managed by ArgoCD.
- **charts/**: Helm charts for the app and dependencies.
- **kind-config/**: Kind cluster configuration.
- **manifests/**: Kubernetes manifests for ArgoCD and app resources.


## 📚 Resources

- [ArgoCD Docs](https://argo-cd.readthedocs.io/)
- [Helm Docs](https://helm.sh/docs/)
- [Kubernetes Docs](https://kubernetes.io/docs/)


