layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/propel-background.png)
background-size: contain

---

# Docker+Kubernetes Training
## Session 9
## September 6th 2021

---

### 9.1 Minikube Purge

- Run: `minikube delete --all --purge`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.minikube.purge.png)

---

### 9.2 Prepare Session9 App

- `curl -LO https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/9/kustomization.yaml`
- `curl -LO https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/9/mysql-deployment.yaml`
- `curl -LO https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/9/wordpress-deployment.yaml`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.curl.png)

---

### 9.3 Boot Session9 App

- `kubectl apply -k .`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.apply.png)

---

### 9.4 Verify the Persistent Volume Claims

- `kubectl get pvc`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.pvc.png)

---

### 9.5 Describe the Wordpress Service

- `kubectl get service wordpress`
- `minikube service wordpress --url`


![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.url.png)
---

### 9.6 A Note on LoadBalancer

- *LoadBalancer is for a cloud to create am external load balancer like ALP/NLP in AWS*

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.externalip.pending.png)1

---

### 9.7 Minikube IP

- `minikube ip`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.minikube.ip.png)

---

### 9.8 Edit the Service

- Edit `kubectl get service wordpress -o yaml > wordpress-service.yaml`
- under `type: LoadBalancer` add:

```
  externalIPs:
  - 192.168.49.2
```

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.externalip.png)1

---

### 9.9 Apply the LoadBalancer IP

- `kubectl apply -f wordpress-service.yaml`
- `minikube service wordpress --url`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.externalip.apply.png)

---

### 9.10 Verify App

- e.g. `http://192.168.49.2:30776`:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.open.png)

---

### 9.11 Secrets

- `kubectl get secrets`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.png)

---

### 8.12 Flask Logs

- Run `kubectl logs -f svc/flask`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.flask.logs.png)

---

### 8.13 Accessing a Pod

- Run `kubectl get pods`:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.kubectl.get.pods.png)

---

### 8.14 Accessing a Pod

- Run `kubectl exec -it flask-76c47d8bbd-cttdm -- sh`
- You should get a '#' prompt once you're shelled in
- Run `apt-get update`
- Run `apt-get install curl`
- Run `curl 127.0.0.1:5000`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.flask.curl.png)

---

### 8.15 Scaling

- Run `kubectl scale deployment flask --replicas=5`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.flask.scale.png)

---

# No 'Homework' for Session 8

- I'm on leave next week

# Session 9 will be September 6th

- Keep safe

