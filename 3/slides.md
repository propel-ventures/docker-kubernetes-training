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
- **Note: Above commands are all you need for build/test (CI) images**
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
