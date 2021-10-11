layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/propel-background.png)
background-size: contain

---

# CKAD Study Hall
## Open-ended study plan + learnings

---

### Certified Kubernetes Application Developer (CKAD)

https://www.cncf.io/certification/ckad/

---

### Exam Curriculum 

| Weight  | Domain      |
| :------ | ----------: |
| 20%     | Application Design and Build |
| 20%     | Application Deployment	|
| 14% | Application Observability and Maintenance |
| 25% | Application Environment, Configuration and Security |
| 20% | Services and Networking |

---

# Application Design and Build

- Define, build and modify container images
- Understand Jobs and CronJobs
- Understand multi-container Pod design patterns (e.g. sidecar, init and others)
- Utilize persistent and ephemeral volumes 

# Application Deployment
- Use Kubernetes primitives to implement common deployment strategies (e.g. blue/
green or canary)
- Understand Deployments and how to perform rolling updates
- Use the Helm package manager to deploy existing packages

# Application observability and maintenance
- Understand API deprecations
- Implement probes and health checks
- Use provided tools to monitor Kubernetes applications
- Utilize container logs
- Debugging in Kubernetes

# Application Environment, Configuration and Security
- Discover and use resources that extend Kubernetes (CRD)
- Understand authentication, authorization and admission control
- Understanding and defining resource requirements, limits and quotas
- Understand ConfigMaps
- Create & consume Secrets
- Understand ServiceAccounts
- Understand SecurityContexts

# Services & Networking
- Demonstrate basic understanding of NetworkPolicies
- Provide and troubleshoot access to applications via services
- Use Ingress rules to expose applications

---

# CKAD Exercises

- https://github.com/dgkanatsios/CKAD-exercises
- https://github.com/kelseyhightower/kubernetes-the-hard-way