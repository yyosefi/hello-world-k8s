# start minikube using docker driver
```bash
minikube start --driver=docker
```

# enable ingress controller and tunnel
```bash
minikube addons enable ingress
minikube tunnel
```

# build and push docker file to dockerhub registry
```bash
docker build -t yyosefi/my-nginx:latest .
```

# Add /etc/hosts entry so hello-world.local points to the cluster ingress IP
```bash
echo "127.0.0.1 hello-world.local" | sudo tee -a /etc/hosts
```

## Install ArgoCD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# optional expose argocd-server
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

# get ArgoCD admin password
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

# make ArgoCD watch k8s/ folder and keep Deployment/Service/Ingress in sync
```bash
kubectl apply -f argo-app.yaml
```

## Stand up Jenkins
```bash
docker run \                
  -d \                                              
  --name jenkins-docker \
  -u root \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v jenkins_home:/var/jenkins_home \
  -p 9090:8080 \
  -p 50000:50000 \
  jenkins/jenkins:lts
```
# get Jenkins initial admin password
```bash
docker exec jenkins-docker \
  cat /var/jenkins_home/secrets/initialAdminPassword
```

# Install plugins: Git, Docker Pipeline, Kubernetes CLI (kubectl), SSH Credentials, SSH Agent
# Create credentials for GitHub (github-ssh)
# Create credentials for Dockerhub (dockerhub)
# Create a Pipeline job called hello-world-ci
# Create a Pipeline job called hello-world-cd
