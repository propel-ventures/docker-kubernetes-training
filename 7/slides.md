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

