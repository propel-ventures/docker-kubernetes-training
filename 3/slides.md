layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/propel-background.png)
background-size: contain

---

# Docker+Kubernetes Training
## Session 3
## July 5th 2021

---

### 3.1 Dockerfile

```
FROM mcr.microsoft.com/dotnet/core/sdk:3.1

MAINTAINER Jason A Turner<jason.turner@mycomplianec.ai>

RUN \
    apt-get update && apt-get install -y \
    bash groff libgdiplus gdebi-core
    # mkdir -p /aws && \
    # build-essential python3 docker zip python3-setuptools python3-dev python3-pip git sed jq  && \
    # pip3 install awscli --upgrade --user

COPY ./wkhtmltox_0.12.6-0.20180618.3.dev.e6d6f54.stretch_amd64.deb /

RUN gdebi --non-interactive /wkhtmltox_0.12.6-0.20180618.3.dev.e6d6f54.stretch_amd64.deb



WORKDIR /app
EXPOSE 3395

COPY build/ /app/

ENTRYPOINT [ "dotnet", "./CIQWatson.dll" ]
```

---

### 3.2 Dockerfile Commands

- ADD
- ARG
- CMD
- COPY
- ENTRYPOINT
- ENV
- EXPOSE
- FROM
- HEALTHCHECK
- LABEL
- MAINTAINER
- ONBUILD
- RUN
- SHELL
- STOPSIGNAL
- USER
- VOLUME
- WORKDIR

resources: 

[cheatsheet](https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index)

[reference](https://docs.docker.com/engine/reference/builder/)

---

### 3.3 Dockerfile Commands You Actually Use

- FROM
  - Must be the first non-comment instruction
- RUN
  - Shell commands
  - Prepping your FROM image for your app or CI
  - as many of these as you need
- COPY
  - Getting your code into the image
  - Merging from other images
- ENV
  - Setting/editing environment variables
  - e.g. PATH, LD_LIBRARY_PATH, PUPPETEER_EXECUTABLE_PATH
- **Note: Above commands are all you need for build/test images**
- EXPOSE
  - app port
- WORKDIR
  - app launch directory
- ENTRYPOINT
  - app launch command

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

### 3.6 Building an App

- create a new folder on your machine
- create a file in that folder called `Dockerfile`
- paste in the following contents:

```
FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine

RUN mkdir /app

WORKDIR /app

RUN dotnet new webapi

EXPOSE 5000 5001

ENTRYPOINT ["dotnet run"]
```

[Dockerfile](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/3/app/Dockerfile)

---

### 3.7 Building an App

- run `docker build -t session3 .`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.build.png)

---

### 3.8 Running the App

- listing your running containers
- `docker ps`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.run.ps.png)

---

### 2.8 Docker CLI First Steps

- connecting to background image
- `docker exec -it alpine-kevin /bin/sh`
- at the new prompt' run `ps -ef`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.exec.png)

- `exit` to return to your dev machine

---

### 2.9 Docker CLI First Steps

- running a command on a background image
- `docker exec -it alpine-kevin /bin/sh -c "ps -ef"`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.exec.c.png)

- no need for `exit` to return to your dev machine
- `-c` === run a command

---

- `docker info`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.info.png)

---

### 2.11 Docker CLI First Steps

- killing a container
- lots of ways to do this
- `docker ps` to get the container id
- `docker kill {CONTAINER ID}`
- `docker ps` should no longer return the container id

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.kill.png)

---

### 2.12 Docker CLI First Steps

- zombie containers
- try running your initial daemon command
- e.g. `docker run --name alpine-kevin -d alpine /bin/sh -c "while true; do ping 8.8.8.8; done"`
- the (named) *image* for your *container* is still in the system
- `docker ps -a` will show stopped containers

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.rm.png)

---

### 2.13 Docker CLI First Steps

- `docker rm alpine-kevin`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.rm.png)

- `docker ps -a` should no longer return that container id
- we will cover docker hygiene further in a later session

---

# 'Homework' for Session 3

- stopping your engine (and starting it again)

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.dead.png)

- help out anyone who needs it

# Planned for Session 3

- Dockerfile and building your own images
- Building a working app via Dockerfile
- Docker Hub/ECR (maybe)
- Docker hygiene (maybe)

- see you then!
