# 🚀 Helm App – Multi-Environment Kubernetes

This repository showcases a full CI/CD pipeline using **GitHub Actions**, **Helm**, and **Minikube** to deploy a Python Flask application into **three isolated environments**:

- ✅ `test`
- ✅ `stage`
- ✅ `prod`

---

## 📦 Project Structure

```
my-helm-app/
├── .github/workflows/helm-deploy.yml   # GitHub Actions pipeline
├── app/                                # Python Flask app
├── Dockerfile                          # Dockerfile for the app
├── charts/myapp/                       # Helm chart for deployment
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│       ├── deployment.yaml
│       └── service.yaml
├── env/                                # Environment-specific Helm values
│   ├── prod-values.yaml
│   ├── stage-values.yaml
│   └── test-values.yaml
└── README.md
```

---

## 🐳 Application

A simple Flask app with a single route:

```python
@app.route('/')
def hello():
    return "Hello from Flask App running in Kubernetes!"
```

---

## ⚙️ Helm Chart Overview

The Helm chart includes:

- Deployment
- Service (NodePort)
- Environment-specific replica counts and image tags

All Kubernetes resources are **dynamically named using `{{ .Release.Name }}`** to support parallel environments.

---

## 🚀 GitHub Actions CI/CD Pipeline

The `.github/workflows/helm-deploy.yml` does the following:

1. ✅ Installs Docker
2. ✅ Installs Minikube
3. ✅ Installs Helm
4. ✅ Builds the Docker image inside Minikube
5. ✅ Deploys 3 Helm releases:
   - `myapp-test`
   - `myapp-stage`
   - `myapp-prod`
6. ✅ Validates deployment with `kubectl get all`

### 🔁 Trigger

This pipeline runs on every push to the `main` branch, or when any YAML/chart/app/dockerfile changes.

---

## 🔧 Environment-Specific Config

Each environment (`test`, `stage`, `prod`) has its own Helm values file:

### Example: `env/stage-values.yaml`

```yaml
replicaCount: 2
image:
  repository: myapp
  tag: stage
```

---

## 🧪 Run Locally with Minikube

### 1. Start Minikube

```bash
minikube start
```

### 2. Set Docker to use Minikube's Docker daemon

```bash
eval $(minikube docker-env)
```

### 3. Build Docker image

```bash
docker build -t myapp:stage .
```

### 4. Deploy with Helm

```bash
helm upgrade --install myapp-stage charts/myapp -f env/stage-values.yaml
```

### 5. Access the app

```bash
minikube service myapp-stage-service
```

---

## 🔐 Troubleshooting

- ❌ **Service already exists**  
  Ensure you are using `{{ .Release.Name }}` in resource names inside Helm templates to avoid name conflicts.

- ❌ **Minikube not found in GitHub Actions**  
  Ensure Minikube is correctly installed and the Docker driver is enabled.

- ❌ **Image not found**  
  Double-check that the image is built inside the Minikube Docker environment.

---

## ✅ Future Enhancements (Optional)

- Add Ingress + TLS support
- Integrate ArgoCD or Helmfile
- Use Secrets and ConfigMaps
- Push Docker image to ECR/GCR
- Add test cases for the Flask app

---

## 🙌 Authors

Made with ❤️ for learning DevOps, GitHub Actions, Helm, and Kubernetes.
