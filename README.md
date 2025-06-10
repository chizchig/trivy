# 🚀 Sky Deployment CI/CD Workflow

This repository automates the deployment of the `skye-app` microservice to a Kubernetes cluster using **GitHub Actions**, **Kustomize overlays**, **ArgoCD**, and security scanning tools such as **Trivy** and **KICS**.

---

## 📁 Project Structure

```text
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


---

## ⚙️ GitHub Actions Workflow

The GitHub Actions workflow performs the following steps:

### 🔐 Security Scanning
- **Trivy:** Scans the container image `shadowhub/cocomaster` for vulnerabilities.
- **KICS:** Scans Kubernetes YAML files for infrastructure misconfigurations.

### ⚒️ Kubernetes Setup
- Installs `kubectl`, `minikube`, and `kustomize`.
- Boots up a Minikube cluster for test deployments.
- Validates Kustomize overlays using `kustomize build`.

### 🚀 Deployment
- Uses `kustomize build` to render manifests from overlays.
- Applies them using `kubectl apply`.
- Verifies the deployment via `kubectl get pods`.

---

## 🔍 Example Kustomize Patch (Overlay: `dev`)

```yaml
patches:
  - patch: |-
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: sky-deployment
        namespace: argocd
      spec:
        replicas: 4
    target:
      kind: Deployment
      name: sky-deployment
      namespace: argocd

This increases the number of replicas in the dev environment without changing the base file.

## 🚦 ArgoCD Application Configuration

apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: sky-dev
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/chizchig/trivy.git
    targetRevision: HEAD
    path: resource/dev
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd-namespace
  syncPolicy:
    automated:
      selfHeal: true
      prune: true

ArgoCD continuously syncs and manages the desired state declared in the Git repository.

## ✅ Prerequisites
A Kubernetes cluster (Minikube, EKS, GKE, etc.)

ArgoCD installed in the argocd namespace

GitHub Actions secrets (optional for DockerHub credentials or K8s secrets)

## 📦 Manual Deployment Command

kustomize build overlay/dev | kubectl apply -f -



## 📈 CI/CD Benefits
✔️ GitOps-enabled deployments via ArgoCD

✔️ Static and container vulnerability scans

✔️ Infrastructure validation before apply

✔️ Environment-specific configurations with overlays

## 👥 Maintainers
   chizchig – GitHub