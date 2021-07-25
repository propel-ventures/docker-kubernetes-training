layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/propel-background.png)
background-size: contain

---

# Docker+Kubernetes Training
## Session 5
## July 26th 2021

---

### 5.1 Docker Network (local clustering)

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/evergiven.jpg)

---

### 5.2 Docker Network

#### Session outline

- Build an `nginx` reverse proxy docker image
- Build a `flask` docker image to serve web
- Build a `dotnet` docker image to serve REST
- Build a `session5` docker network
- Run all three images within the network (create a 'local cluster')
- Get the docker network address
- Test that the reverse proxy navigates the local cluster as expected

---

### 5.3 Build an `nginx` reverse proxy docker image

- Sign up/Login to https://hub.docker.com/

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.nginx.png)

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

### 4.10 Using the image on a cloud - GCP

- https://cloud.google.com/shell
- (set up a new project, billing)
- `docker pull kevinciq/session4:flask`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.gcp.pull.png)

---

### 4.11 Using the image on a cloud - GCP

- `docker tag kevinciq/session4:flask gcr.io/basic-decoder-319517/kevinciq/session4:flask`
- `docker push gcr.io/basic-decoder-319517/kevinciq/session4:flask`
- `gcloud run deploy --image=gcr.io/basic-decoder-319517/kevinciq/session4:flask --port=5000 --region=us-central1 --allow-unauthenticated --platform=managed`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.gcp.create.png)

---

### 4.12 Using the image on a cloud - GCP

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.gcp.created.png)

---

### 4.13 Using the image on a cloud - GCP

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.gcp.shutdown.png)

---

### 4.13 Using an image on a cloud - ECR

- `aws ecr get-login-password | docker login --username AWS --password-stdin XXXXXXXXXXXX.dkr.ecr.ap-southeast-2.amazonaws.com`
- `docker tag session4:flask XXXXXXXXXXXX.dkr.ecr.ap-southeast-2.amazonaws.com/session4:flask`
- `docker push XXXXXXXXXXXX.dkr.ecr.ap-southeast-2.amazonaws.com/session4:flask`
- `docker run -it --volume=/home/kevin/work/github/docker-kubernetes-training/1/docker-kubernetes-training/3/flask:/app --workdir="/app" --memory=4g --memory-swap=4g --memory-swappiness=0 --entrypoint=/bin/bash XXXXXXXXXXXX.dkr.ecr.ap-southeast-2.amazonaws.com/session4:flask`

---

# Session 5 is in Two Weeks - July 26th

- I'll post a multiple choice quiz for next week
