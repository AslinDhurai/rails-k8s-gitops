# 🚀 Deploying Ruby on Rails with PostgreSQL on Kubernetes via ArgoCD

This repository contains Kubernetes manifests and an ArgoCD configuration for deploying a Ruby on Rails application backed by a PostgreSQL database using GitOps principles.

---

## 📁 Directory Structure

```
.
├── argocd/        # ArgoCD Application and ConfigMaps
├── k8s/           # Kubernetes manifests for Rails and PostgreSQL
```

---

## ✅ Prerequisites

- A working **Kubernetes** cluster (Minikube, k3s, EKS, etc.)
- `kubectl` installed and configured
- **ArgoCD** installed and running in your cluster
- Your Rails image available on DockerHub or another registry
- This Git repository accessible from ArgoCD

---

## 🚀 How to Deploy

### 1. Deploy Kubernetes Manifests Manually (optional)

Apply all base manifests:

```bash
kubectl apply -f k8s/
```

This will create:
- Rails Deployment and Service
- PostgreSQL StatefulSet and Service
- Ingress (optional for domain access)

---

### 2. Set Up GitOps with ArgoCD

1. **Apply the ArgoCD Application**:

```bash
kubectl apply -f argocd/application.yaml
```

2. **Sync the Application from ArgoCD Dashboard**:
   - Open ArgoCD UI.
   - Locate your Rails application.
   - Click “Sync” to deploy all resources from the repo automatically.

---

## 🌐 Accessing Your App

If you have an Ingress controller (like NGINX) and DNS set up:

```
http://your-domain.com
```

If not, port-forward the service:

```bash
kubectl port-forward svc/rails-service 3000:3000
```

Then open: [http://localhost:3000](http://localhost:3000)

---

## 🧹 Clean Up

To remove all resources:

```bash
kubectl delete -f k8s/
kubectl delete -f argocd/
```

---

## ✍️ Maintained by

**Aslin Dhurai**  
📌 [GitHub Profile](https://github.com/AslinDhurai)
