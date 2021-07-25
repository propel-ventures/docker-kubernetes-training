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

- Create a folder structure for your dockerfiles
- Build an `nginx` reverse proxy docker image
- Build a `flask` docker image to serve web pages
- Build a `dotnet` docker image to serve REST
- Build a `session5` docker network
- Run all three images within the network (create a 'local cluster')
- Get the docker network address
- Test that the reverse proxy navigates the local cluster as expected

---

### 5.3 Create a folder structure for your dockerfiles

- create a `session5` folder in your machine
- create three subfolders:
 - `nginx`
 - `flask`
 - `dotnet`

 ![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.folders.png)

---

### 5.4 Build an `nginx` reverse proxy docker image

- `cd` into the `nginx` folder in your machine
- create a file called `nginx.conf`
- copy in the text below:


```
events { }

http {
  server {
    listen 80;

    location / {
      proxy_pass http://flask:5000/;
    }

    location /api/ {
      proxy_pass http://dotnet:80/;
    }
  }
}

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
