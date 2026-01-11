# 🚀 Deploying Ruby on Rails with PostgreSQL on Kubernetes via ArgoCD

This repository contains Kubernetes manifests and an ArgoCD configuration for deploying a Ruby on Rails application backed by a PostgreSQL database using GitOps principles.

---

## 🎥 Project Explanation Video

For a detailed explanation of the **contents of this repository**, including the directory structure and how each part fits together, watch the video here:

[▶️ Watch on Google Drive](https://drive.google.com/file/d/1Q2SLz1hlQlmQeueIZ0g-gHCzenOUEGSD/view?usp=sharing)

---

## 📁 Directory Structure

```
.
├── argocd/        # ArgoCD Application and ConfigMaps
├── k8s/           # Kubernetes manifests for Rails and PostgreSQL
└── rails-app/     # Rails application source code
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

## 🔧 CI/CD with Tekton

This project also supports container image builds using **Tekton Pipelines**.

### ✅ Prerequisites

- Tekton Pipelines installed on the Kubernetes cluster
- DockerHub account with username and personal access token
- A PersistentVolumeClaim for shared workspace

### 📁 Tekton Directory Structure

```
tekton/
├── tasks/
│   └── kaniko-task.yaml         # Task to build and push Docker image using Kaniko
├── git-clone-task.yaml          # Task to clone the Git repo
├── pipeline.yaml                # Defines the full pipeline
├── pipeline-run.yaml            # Triggers the pipeline
└── workspace-pvc.yaml           # PVC definition
```

### 🚀 Applying Tekton Resources

1. Apply the PVC (shared workspace):
   ```bash
   kubectl apply -f tekton/workspace-pvc.yaml
   ```

2. Apply Tasks:
   ```bash
   kubectl apply -f tekton/git-clone-task.yaml
   kubectl apply -f tekton/tasks/kaniko-task.yaml
   ```

3. Apply the Pipeline:
   ```bash
   kubectl apply -f tekton/pipeline.yaml
   ```

4. Trigger the PipelineRun:
   > ⚠️ Edit `pipeline-run.yaml` to include your DockerHub credentials as `username` and `password` params, or configure them as secrets and mount via workspaces
   ```bash
   kubectl create -f tekton/pipeline-run.yaml
   ```

5. Monitor status:
   ```bash
   tkn pipelinerun logs -f
   kubectl get taskruns
   ```
