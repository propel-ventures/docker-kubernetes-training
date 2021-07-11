layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/propel-background.png)
background-size: contain

---

# Docker+Kubernetes Training
## Session 4
## July 12th 2021

---

### 4.1 Session 3 Rehash

- https://propel-ventures.github.io/docker-kubernetes-training/3/#7
---

### 4.2 Docker Registries

#### There's a bunch

- [Docker Hub](https://hub.docker.com/)
- [Amazon ECR (Elastic Container Registry)](https://ghcr.io)
- [GitHub Container Registry](https://aws.amazon.com/ecr/)
- [Google Cloud Container Registry](https://cloud.google.com/container-registry)
- [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/)

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.registry.jpg)

---

### 4.3 Docker Hub

- Sign up/Login to https://hub.docker.com/

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.hub.png)

---

### 4.4 Create a new Repository

- click the `Create Repository` button in the top right of your Docker Hub home page
- enter in a repository name
- default settings are fine - note this is a public repo

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.create.png)

---

### 4.5 Verify your Repo

- note the `docker push kevinciq/session4:tagname` command - your version of this is needed later

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.create.push.png)

---

### 4.6 Docker Login

- run `docker login` on your local cli

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.login.png)

---

### 4.7 Docker Tag, Docker Push

- run `docker images | grep session3` on your local cli
- run `docker tag session3:flask kevinciq/session4:flask` - use your own values, you can't push to mine
- run `docker push kevinciq/session4:flask`
![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.pushing.png)

---

### 4.8 You've now authored a docker image

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.pushed.png)
![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.pushed.hub.png)

---

### 4.9 Local Image Tag

- run `docker images | grep session4` on your local cli
- note the `IMAGE ID` and size is the same for both images

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.pushed.local.png)

---

# Session 5 is in Two Weeks - July 26th

- I'll post a multiple choice quiz for next week
