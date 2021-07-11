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
- run `docker tag session3:flask kevinciq/session4:flask` on your local cli - use your own values, you can't push to mine
- run `docker push kevinciq/session4:flask`
![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.pushing.png)

---

### 4.8 You've now authored a docker image

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.pushed.png)
![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.pushed.hub.png)

---

### 4.9 Local Image Tag

- run `docker images | grep session4` on your local cli

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.pushed.local.png)

---

### 3.4 Dockerfile Tips

- Build Context
  - The directory you run `docker build` from
  - It is copied over in its entirety (i.e. all subfolders) to the build daemon
  - where possible, don't run it from your checkout folder, tmp, or drive root

---

### 3.5 Dockerfile Tips

- Layering
  - RUN and COPY create layers
  - Try and organise your layers so that changes between image versions happen later in the `Dockerfile`
  - Where layers never change, consider moving them into a 'base' (build) image; this will speed up your build times
  - If your app builds are fairly stable, consider using a base checkout image  with library packages already in place
  - e.g. base image: `git clone`, then `nuget restore` or `npm install` or `maven dependency:resolve`
  - e.g. build image: `git pull`, then `nuget restore` or `npm install` or `maven dependency:resolve`  
  - You can also have have multiple `FROM` commands in your `Dockerfile`
- FROM layering
  - your `Dockerfile` can have can multiple `FROM` commands, and copy between image refs

resources:

[dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

[multistage builds](https://docs.docker.com/develop/develop-images/multistage-build/)

[docker layers explained](https://dzone.com/articles/docker-layers-explained)

---

### 3.6 Building an App (Python Flask)

- clone the training repo from https://github.com/propel-ventures/docker-kubernetes-training
- cd into the '3' subfolder, then the 'flask' subfolder
- run `docker build -t session3:flask .`
- run `docker run -it --rm -p 5000:5000 session3:flask`
- navigate to http://0.0.0.0:5000/ in your browser

---

### 3.7 Building an App (Python Flask)

- Dockerfile

```
FROM ubuntu:latest
RUN apt-get update -y
RUN apt-get install -y python-dev build-essential curl
RUN curl https://bootstrap.pypa.io/pip/2.7/get-pip.py -o get-pip.py
RUN python get-pip.py
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
ENTRYPOINT ["python"]
CMD ["app.py"]
```

---

### 3.8 Building an App (dotnet)

- clone the training repo from https://github.com/propel-ventures/docker-kubernetes-training
- cd into the '3' subfolder, then the 'dotnet' subfolder
- run `docker build -t session3:dotnet .`
- run `docker run -it --rm -p 5000:80 session3:dotnet`
- navigate to http://localhost:5000/ in your browser

resources:

[Dockerfile](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/3/dotnet/Dockerfile)

[Tutorial](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/building-net-docker-images?view=aspnetcore-5.0)

---

### 3.9 Building an App (dotnet)

- run `docker build -t session3 .`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.dotnet.png)

---

# 'Homework' for Session 4

- Screenshot of http://0.0.0.0:5000/ running on your machine
- Sign up to https://hub.docker.com/
- (optional) Get the dotnet app working
- help out anyone who needs it

# Planned for Session 4

- Docker Hub/ECR
- Docker Hygiene

- see you then!
