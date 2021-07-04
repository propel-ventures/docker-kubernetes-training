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

see: 

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

### 2.4 Docker CLI First Steps

- listing your images
- `docker images`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.images.png)

---

### 2.5 Docker CLI First Steps

- running an interactive shell
- in your local terminal, run `whoami`
- now run `docker run -it --rm alpine`
- then run `whoami` again
- play around - e.g. `df -h`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.run.png)

- type `exit` to leave the container
- type `docker ps`
- the container should have been deleted, due to the `--rm` arg to `docker run`
- `-it` === interactive

---

- sustaining an image in the background 
- `docker run --name alpine-kevin -d alpine /bin/sh -c "while true; do ping 8.8.8.8; done"`
- check that your daemon container is running via `docker ps`
- check its logs via `docker logs {CONTAINER ID}`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.run.d.png)

- if you make a mistake, `docker rm` will come in handy

---

### 2.7 Docker CLI First Steps

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
