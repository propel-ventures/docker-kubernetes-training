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

### 7.10 Launch Redis Deployment

- Run `kubectl apply -f redis-leader-deployment.yaml`
- Run `kubectl get pods`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.redis.png)

---

### 7.11 Redis Logs

- Run `kubectl logs -f deployment/redis-leader`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.redis.logs.png)

---

### 7.12 Redis Service

- Edit a file called `redis-leader-service.yaml`:

```
# SOURCE: https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook
apiVersion: v1
kind: Service
metadata:
  name: redis-leader
  labels:
    app: redis
    role: leader
    tier: backend
spec:
  ports:
  - port: 6379
    targetPort: 6379
  selector:
    app: redis
    role: leader
    tier: backend
```

---

### 7.13 Launch Redis Deployment

- Run `kubectl apply -f redis-leader-service.yaml`
- Run `kubectl get service`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.redis.service.png)

---

### 7.14 Redis Replicas

- Edit a file called `redis-follower-deployment.yaml`:

```
# SOURCE: https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-follower
  labels:
    app: redis
    role: follower
    tier: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
        role: follower
        tier: backend
    spec:
      containers:
      - name: follower
        image: gcr.io/google_samples/gb-redis-follower:v2
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 6379
```

---

### 7.15 Launch Redis Deployment

- Run `kubectl apply -f redis-follower-deployment.yaml`
- Run `kubectl get pods`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.redis.replicas.png)

---

### 7.16 Redis Replica Service

- Edit a file called `redis-follower-service.yaml`:

```
# SOURCE: https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-follower
  labels:
    app: redis
    role: follower
    tier: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
        role: follower
        tier: backend
    spec:
      containers:
      - name: follower
        image: gcr.io/google_samples/gb-redis-follower:v2
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 6379
```

---

### 7.17 Launch Redis Replica Service

- Run `kubectl apply -f redis-follower-service.yaml`
- Run `kubectl get service`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.redis.replicas.service.png)


---

### 7.18 Guestbook Frontend

- Edit a file called `frontend-deployment.yaml`:

```
# SOURCE: https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
        app: guestbook
        tier: frontend
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v5
        env:
        - name: GET_HOSTS_FROM
          value: "dns"
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 80
```

---

### 7.19 Launch Frontend

- Run `kubectl apply -f frontend-deployment.yaml`
- Run `kubectl get pods -l app=guestbook -l tier=frontend`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.frontend.png)

---

### 7.20 Guestbook Frontend

- Edit a file called `frontend-deployment.yaml`:

```
# SOURCE: https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
        app: guestbook
        tier: frontend
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v5
        env:
        - name: GET_HOSTS_FROM
          value: "dns"
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 80
```

---

### 7.21 Launch Frontend

- Run `kubectl apply -f frontend-service.yaml`
- Run `kubectl get services`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.frontend.service.png)

---

### 7.22 Port Forward

- Run `kubectl port-forward svc/frontend 8080:80`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.port.forward.png)

---

### 7.23 Access the Guestbook

- Browse to `http://localhost:8080/`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.guestbook.1.png)


- Open up another tab to `http://localhost:8080/`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.guestbook.2.png)

---

### 7.24 Refresh your dashboard

- Refresh your local dashboard page from earlier - e.g. `http://127.0.0.1:34535/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/overview?namespace=default`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.dashboard.guestbook.png)









