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

- *LoadBalancer is for a cloud to create an external load balancer like ALP/NLP in AWS*

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.externalip.pending.png)

---

### 9.7 Minikube IP

- `minikube ip`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.minikube.ip.png)

---

### 9.8 Edit the Service

- `kubectl get service wordpress -o yaml > wordpress-service.yaml`
- under `type: LoadBalancer` add your IP:

```
  externalIPs:
  - 192.168.49.2
```

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.externalip.png)

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

### 9.12 Secrets Json

- `kubectl get secrets -o json > secrets.yaml`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.yaml.png)

---

### 9.13 Decoding

- Run `echo "QFNlc3Npb245"|base64 -d`:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.decode.png)

---

### 9.14 Encoding a new secret

- Run `echo -n "@Session9a" |base64`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.encode.png)

---

### 9.15 Rolling over your secret(s)

- Edit your `secrets.yaml` and replace the old encoded secret with the new encoded secret
- Run `kubectl apply -f secrets.yaml`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.apply.png)

---

### 9.16 Verifying updates secrets

- Open up your minikube dashboard

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.dashboard.png)

---

### 9.17 Verifying updates secrets

- Check your secret values via your pods

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.mysql.png)
![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.wp.png)

---

# Session 10 will be September 13th

- Keep safe

