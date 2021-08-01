layout: true
class: middle
background-image: url(https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/propel-background.png)
background-size: contain

---

# Docker+Kubernetes Training
## Session 6
## August 2nd 2021

---

### 6.1 Session 5 Recap - Docker Hygiene

- First, see your disk space available e.g. `df -h`
- Then, count the number of images on your disk e.g. `docker images | wc`

#### Deleting (most) Docker Artifacts

- Remove all unused containers, networks, images: `docker system prune`

#### Deleting (old) Docker Artifacts

- All running containers, and their images: `docker rm -f $(docker ps -a -q)`

#### Deleting (single) Docker Artifacts

- Deleting a specific image: `docker image rm session5a:flask` (`docker rmi` works too)

#### Deleting (almost all) Docker Artifacts

- Any image not touched in the last 2 days: `docker image prune --all --filter until=48h`

---

### 6.2 Docker Compose

- use case: simple local cluster init/debug
- uses YAML files
- walks right up to k8s - so lets learn it to help us bridge
- some shops/devs favour it, but not my cup of tea (duplication)
- just boot your images in separate terminal tab, or use a shell script
- or, use k8s (e.g. `minikube` - see you in session 7), or `tye`, etc etc

*Docker (or specifically, the docker command) is used to manage individual containers, docker-compose is used to manage multi-container applications and Kubernetes is a container orchestration tool*

- what about `docker swarm`? (in short, no. use k8s)

resources:

[](https://www.techrepublic.com/article/simplifying-the-mystery-when-to-use-docker-docker-compose-and-kubernetes/)
[](https://github.com/dotnet/tye)

---

### 6.3 Docker Compose - Installation

- You might need to install it - try `docker-compose -v`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.version.png)

- see https://docs.docker.com/compose/install/ if you don't have it

---

### 6.4 Docker Compose - Flask Image 1 of 4

- Create a file called `app.py`:

```
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)

```

---

### 6.5 Docker Compose - Flask Image 2 of 4

- create a file called `requirements.txt`
- copy in the text below:

```
flask
redis

```

---

### 6.6 Docker Compose - Flask Image 3 of 4

- create a `Dockerfile`
- copy in the text below:

```
# syntax=docker/dockerfile:1
FROM python:3.8-slim-buster

WORKDIR /app
COPY . .
RUN pip3 install -r requirements.txt

EXPOSE 5000

CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0"]


```

---

### 6.7 Docker Compose - Flask Image 4 of 4

- build the image: `docker build -t session6:flask .`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.flask.1.png)
![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.flask.2.png)

---

### 6.8 Docker Compose File

- create a file called `docker-compose.yml`
- copy in the text below:

```
---
version: '3.3'
services:
  web:
    image: "session6:flask"
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"

```

---

### 6.9 Docker Compose Up

- run `docker-compose up`:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.up.png)

---

### 6.10 Check Docker Compose

- run `docker ps` in another terminal window:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.ps.png)

---

### 6.11 Check the Docker Compose app

- use your browser to open up the app url - e.g.:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.app.0.png)

- e.g. http://172.18.0.2:5000/

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.app.1.png)

- refresh the page:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.app.2.png)


---

### 6.12 Kill the Docker Compose app

- hit ctrl-c in the `docker-compose` terminal session to bring it down:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.dead.png)


- confirm its dead in the browser:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.dead.def.png)

---

### 6.13 Restart the app

- run `docker-compose up` again:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.app.3.png)

- confirm its good in the browser:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.compose.app.4.png)


---

# Homework for Session 7

- install minikube
- see https://minikube.sigs.k8s.io/docs/start/

# Planned for Session 7

- k8s
