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

### 6.4 Docker Compose - Flask Image

- Create a file called `app/py`:

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

### 5.5 Build an `nginx` reverse proxy docker image

- create a `Dockerfile`
- copy in the text below:

```
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf

```

- build the image via `docker build -t session5:nginx .`

---

### 5.6 Build a `flask` docker image to serve web pages

- `cd` into the `flask` folder in your machine
- create a file called `requirements.txt`
- copy in the text below:

```
Flask==2.0.1

```

- create a file called `app.py`
- copy in the text below:

```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Flask Session5'

```

---

### 5.7 Build a `flask` docker image to serve web pages

- create a `Dockerfile`
- copy in the text below:

```
FROM python:3.8-slim-buster

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0"]

```

- build the image via `docker build -t session5:flask .`

---

### 5.8 Build a `dotnet` docker image to serve REST

- `cd` into the `dotnet` folder in your machine
- create a `Dockerfile`
- copy in the text below:

```
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app
RUN dotnet new webapi -o WeatherForecast -f net5.0 --no-https
RUN dotnet build WeatherForecast/WeatherForecast.csproj -c Release -o /out

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS runtime
WORKDIR /app
COPY --from=build /out ./
ENTRYPOINT ["dotnet", "WeatherForecast.dll"]

```

- build the image via `docker build -t session5:dotnet .`

---

### 5.9 Build a `session5` docker network

- run `docker network create session5`
- run `docker network ls`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.network.png)

---

### 5.10 Run all three images within the network - run the REST API

- run `docker images | grep session5` on your local cli
- run `docker run -it --rm -p 80 --name dotnet --network session5 session5:dotnet`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.network.dotnet.png)

---

### 5.11 Run all three images within the network - run the web server

- open up a new terminal window
- run `docker run -it --rm -p 5000 --name flask --network session5 session5:flask`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.network.flask.png)

---

### 5.12 Run all three images within the network - run the reverse proxy

- open up a new terminal window
- run `docker run -it --rm -p 80 --name nginx --network session5 session5:nginx`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.network.nginx.png)

---

### 5.13 Get the docker network address

- open up a new terminal window
- run `docker network inspect session5`

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.network.inspect.png)

---

### 5.14 Test that the reverse proxy navigates the local cluster as expected

- note the IP address of your `nginx` container
- run it through your browser e.g. http://172.19.0.4/

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.network.http.png)

---

### 5.15 Test that the reverse proxy navigates the local cluster as expected

- now run the REST url through your browser e.g. http://172.19.0.4/api/WeatherForecast

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.network.rest.png)

---

### 5.16 Test that the reverse proxy navigates the local cluster as expected

- note the ingress logs in your nginx window:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.network.ingresses.png)

---

# Planned for Session 6

- Docker Compose
- Docker Hygiene
- Docker Internals
- Docker vs Kubernetes

# Planned for Session 7

- k8s
