layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/propel-background.png)
background-size: contain

---

# Docker+Kubernetes Training
## Session 7
## August 16th 2021

---

### 7.1 Minikube+Kubectl

- Get your Minikube version: `minikube version`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.minikube.version.png)

- Get your Kubectl version: `kubectl version -o yaml`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.kubectl.version.png)

---

### 7.2 Start Minikube

- `minikube start --driver=docker`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.minikube.start.png)

---

### 7.3 Query Minikube Status

- Run `minikube status`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.minikube.status.png)

---

### 7.4 Query Minikube Status via Kubectl

- Run `kubectl cluster-info` and `kubectl get nodes`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.kubectl.status.png)

---

### 7.5 Query Minikube via Docker

- Run `docker ps` in another terminal window

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.minikube.docker.ps.png)

---

### 7.6 Minikube Addons

- Run `minikube addons list`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.minikube.addons.png)

---

### 7.7 Dashboard

- Run `minikube dashboard`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.dashboard.png)

---

### 7.8 Ingress Addon

- Run `minikube addons enable ingress`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.ingress.png)

---

### 7.9 Multi-Tier Kubernetes Web Application (Guestbook)

- Edit a file called `redis-leader-deployment.yaml`:

```
# SOURCE: https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-leader
  labels:
    app: redis
    role: leader
    tier: backend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
        role: leader
        tier: backend
    spec:
      containers:
      - name: leader
        image: "docker.io/redis:6.0.5"
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 6379
```

---

### 7.10 Launch Redis

- Run `kubectl apply -f redis-leader-deployment.yaml`
- Run `kubectl get pods`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.redis.png)

---

### 7.11 Redis Logs

- Run `kubectl logs -f deployment/redis-leader`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.redis.logs.png)