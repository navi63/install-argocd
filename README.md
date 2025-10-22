# ArgoCD Installation on K3s (Local Machine)

This tutorial walks you through the process of installing ArgoCD on a local K3s cluster for managing Kubernetes applications via GitOps.

## Prerequisites

Before starting, make sure you have the following installed:

- **K3s**: Lightweight Kubernetes distribution.
- **kubectl**: Kubernetes CLI to interact with your K3s cluster.
- **Helm** (optional): Package manager for Kubernetes, useful for ArgoCD installation via Helm (alternative to manifest installation).

## Step 1: Install K3s

If you don't have K3s installed yet, follow these steps:

### Install K3s on your machine:

```bash
curl -sfL https://get.k3s.io | sh -
```
### Verify K3s installation:

```bash
kubectl get nodes
```

## Step 2: Install ArgoCD via kubectl
You can install ArgoCD on your K3s cluster using the following commands:
1. Create a namespace for ArgoCD:

```bash
kubectl create namespace argocd
```

2. Install ArgoCD using the ArgoCD manifests:

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

3. Verify ArgoCD components are running:

```bash
kubectl get pods -n argocd
```

## Step 3: Expose ArgoCD API Server

By default, ArgoCD API server will not be exposed externally. You'll need to expose it either using a LoadBalancer (if you're using cloud providers), NodePort, or port-forward.

For local access, use port-forward:
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
This will expose the ArgoCD API Server on port 8080 on your local machine.

## Step 4: Access ArgoCD UI
Open your browser and go to:
```bash
http://localhost:8080
```

## Step 5: Login to ArgoCD
The default login credentials are:

Username: admin

Password: The password is automatically set to the name of the ArgoCD API server pod. To get the password, run:
```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
```
Look for the password field (it's base64 encoded), then decode it:
```bash
echo <encoded_password> | base64 --decode
```
Use this decoded password to log in to the ArgoCD UI.

# Use kubectl without sudo
```bash
sudo chmod 644 /etc/rancher/k3s/k3s.yaml
```

