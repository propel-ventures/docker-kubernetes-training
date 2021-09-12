layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/propel-background.png)
background-size: contain

---

# Docker+Kubernetes Training
## Session 10
## September 13th 2021

---

### 10.1 Multi-Container Pod Design Patterns

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.ckad.1.png)

---

### 10.2 What is a Pod

- Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.
- A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.
- A Pod's contents are always co-located and co-scheduled, and run in a shared context.
- A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled.
- Pods are ephemeral by nature, if a pod (or the node it executes on) fails, Kubernetes can automatically create a new replica of that pod to continue operations. 
- Pods include one or more containers (such as Docker containers).
- 

Resources: 
https://kubernetes.io/docs/concepts/workloads/pods/
https://www.vmware.com/topics/glossary/content/kubernetes-pods

---

### 10.3 Pod Controllers

- Pods are mostly not created directly, even singleton Pods.
- Pods are created using *workload* resources called *controllers*.
- Controllers manage rollout, replication, and health of the pods in a cluster.
- e.g. if a node in the cluster fails, a controller detects that the pods on that node are unresponsive and creates replacement pod(s) on other nodes.
- The three most common types of controllers are:
  - **Deployments** for applications that are stateless and persistent, such as web servers (HPPT servers)
  - **Jobs** for batch-type jobs that are ephemeral, and will run a task to completion 
  - **StatefulSets** for applications that are both stateful and persistent such as databases 
    - *Note: with a StatefulSet each Pod gets its own PersistentVolumeClaim, but in a Deployment with a PVC all Pods use the same PersistentVolumeClaim*

---

### 10.4 Multi-Container Pods

- Pods in a Kubernetes cluster are used in two main ways:
  - Pods that run a *single* container: The "one-container-per-Pod" model is the most common Kubernetes use case
  - Pods that run *multiple* containers that need to work together
    - A Pod can encapsulate an application composed of multiple co-located containers that are tightly coupled and need to share resources
    - When a pod contains multiple containers, communications and data sharing between the containers is simplified.

- When a pod is created it is assigned its own unique IP address
  - If there are multiple containers within a pod, they can communicate with eachother using *localhost*

---

### 10.5 Multi-Container Pod Design Patterns

- **Sidecar**: A sidecar container adds functionality to your application that could be included in the main container. By hosting this logic in a sidecar, you can keep that functionality out of your main application and evolve that independently from the actual application
- **Ambassador**: An ambassador container proxies a local connection to certain outbound connection. The ambassadors brokers the connection the outside world. This can for instance be used to shard a service or to implement client side load balancing.
- **Adapter**: An adapter container takes data from the existing application and presents that in a standardized way. This is for instance very useful for monitoring data.

Resources: 
https://blog.nillsf.com/index.php/2019/07/28/ckad-series-part-4-multi-container-pods/

---

### 10.6 Sidecar Example

- `openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out session10.crt -keyout session10.key`

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
![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.ip.png)

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

### 9.18 Wordpress App

- Check your secret values via your pods

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.mysql.png)
![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/k8s.wordpress.secrets.wp.png)

---

# Session 10 will be September 13th

- Keep safe

