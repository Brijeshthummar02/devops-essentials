## Minikube Deployment Guide 🚀

### ✅ What You Need to Install for Minikube Deployment

Since Minikube is a local Kubernetes cluster, focus on these tools:

---

### 1️⃣ Install Minikube & Kubectl

```sh
# Install Kubectl (Kubernetes CLI)
sudo apt-get update -y
sudo apt-get install -y kubectl

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start Minikube
minikube start --driver=docker
```

---

### 2️⃣ Install Jenkins in Minikube

Instead of installing Jenkins on an EC2 instance, deploy it in Minikube:

```sh
# Add Helm repo & update
helm repo add jenkins https://charts.jenkins.io
helm repo update

# Install Jenkins with Helm
helm install jenkins jenkins/jenkins --set service.type=NodePort
```

Get Jenkins credentials:

```sh
kubectl get secret --namespace default jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode
```

---

### 3️⃣ Install Docker (Required for Building Images)

Since Minikube uses an internal Docker daemon, configure it properly:

```sh
minikube docker-env
eval $(minikube -p minikube docker-env)
```

Now, any Docker images you build will be available in Minikube.

---

### 4️⃣ Install ArgoCD (For GitOps Deployment)

```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Port forward to access ArgoCD UI
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Login to ArgoCD:

```sh
argocd admin initial-password -n argocd
argocd login localhost:8080
```

---

### ❌ What You Can Skip (Since You're Using Minikube)

- AWS CLI installation
- EC2-related Jenkins setup
- Docker login for DockerHub (since Minikube uses internal Docker)
- Docker Scout (unless you want extra security scanning)

---

### ✅ Next Steps

- Modify the Jenkins pipeline to deploy to Minikube instead of Docker Run.
- Use ArgoCD to automate deployment from GitHub to Minikube.
