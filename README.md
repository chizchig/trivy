🚀 Sky Deployment CI/CD Workflow

This repository automates the deployment of the skye-app microservice to a Kubernetes cluster using GitHub Actions, Kustomize overlays, ArgoCD, and security scanning tools such as Trivy and KICS.

📁 Project Structure
.
├── argocd.yaml              # ArgoCD Application manifest
├── argocdserver.yaml        # (Optional) ArgoCD server configuration
├── base                     # Base Kubernetes manifests
│   ├── deployment.yaml
│   ├── kustomization.yaml
│   ├── namespace.yaml
│   └── service.yaml
└── overlay
    ├── dev
    │   └── kustomization.yaml
    └── prod                 # (Optional) Production overlay
