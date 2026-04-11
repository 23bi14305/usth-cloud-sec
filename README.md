Instructions:
```bash
# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Install Docker
sudo apt install docker.io -y
sudo usermod -aG docker $USER && newgrp docker

# Start Minikube 
minikube start --memory 4096 --cpus 2 --driver=docker

# Create the namespace for Argo CD
kubectl create namespace argocd

# Install Argo CD components
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Wait for Argo CD pods to be ready
kubectl wait --for=condition=available --timeout=600s deployment/argocd-server -n argocd

# Get Admin Password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

# Start the tunnel (Keep this terminal open)
kubectl port-forward --address 0.0.0.0 svc/argocd-server -n argocd 8080:443
```
