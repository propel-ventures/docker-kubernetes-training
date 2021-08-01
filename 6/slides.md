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

### 6.14 Docker Internals

- what is Docker:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.internals.1.png)

resources:

[](https://delftswa.github.io/chapters/docker/)

---

### 6.15 Docker Internals

- Docker Component Diagram:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.internals.2.png)

---

### 6.16 Docker Internals

- Docker Image Diagram:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.internals.3.png)

---

### 6.17 Docker Internals

- Docker Implementation Diagram:

![](https://raw.githubusercontent.com/propel-ventures/docker-kubernetes-training/main/img/docker.internals.4.png)

---

### 6.18 Docker Internals

#### Namespaces:

- https://man7.org/linux/man-pages/man7/namespaces.7.html

*Docker makes use of kernel namespaces to provide the isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container. These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.*

- Docker Engine uses the following namespaces on Linux:
  - PID namespace for process isolation.
  - NET namespace for managing network interfaces.
  - IPC namespace for managing access to IPC resources.
  - MNT namespace for managing filesystem mount points.
  - UTS namespace for isolating kernel and version identifiers.

source: https://medium.com/@BeNitinAgarwal/understanding-the-docker-internals-7ccb052ce9fe

---

### 6.19 Docker Internals

#### cgroups:

*A control group (abbreviated as cgroup) is a collection of processes that are bound by the same criteria and associated with a set of parameters or limits*

- https://en.wikipedia.org/wiki/Cgroups

*Docker makes use of kernel control groups for resource allocation and isolation. A cgroup limits an application to a specific set of resources. Control groups allow Docker Engine to share available hardware resources to containers and optionally enforce limits and constraints.*

#### Docker Engine uses the following cgroups:
  
- **Memory** cgroup for managing accounting, limits and notifications.
- **HugeTBL** cgroup for accounting usage of huge pages by process group.
- **CPU** group for managing user / system CPU time and usage.
- **CPUSet** cgroup for binding a group to specific CPU. Useful for real time applications and NUMA systems with localized memory per CPU.
- **BlkIO** cgroup for measuring & limiting amount of blckIO by group.
- **net_cls** and **net_prio** cgroup for tagging the traffic control.
- **Devices** cgroup for reading / writing access devices.
- **Freezer** cgroup for freezing a group. Useful for cluster batch scheduling, process migration and debugging without affecting prtrace.

---

### 6.20 Docker Internals

#### UnionFS:

*Union file systems operate by creating layers, making them very lightweight and fast. Docker Engine uses UnionFS to provide the building blocks for containers. Docker Engine can use multiple UnionFS variants, including AUFS, btrfs, vfs, and devicemapper*

- https://en.wikipedia.org/wiki/UnionFS

#### Container Format

*Docker Engine combines namespaces, control groups and UnionFS into a wrapper called a container format. The default container format is libcontainer*

---

### 6.21 Docker Internals

#### Security

- Docker Engine makes use of AppArmor, Seccomp, Capabilities kernel features for security purposes.
  - Seccomp used for filtering syscalls issued by a program.
  - AppArmor allows to restrict programs capabilities with per-program profiles.
  - Capabilties for performing permission checks

---

### 6.22 Docker Internals

#### seccomp

*seccomp (short for secure computing mode) is a computer security facility in the Linux kernel. seccomp allows a process to make a one-way transition into a "secure" state where it cannot make any system calls except exit(), sigreturn(), read() and write() to already-open file descriptors. Should it attempt any other system calls, the kernel will terminate the process with SIGKILL or SIGSYS. In this sense, it does not virtualize the system's resources but isolates the process from them entirely*

- https://en.wikipedia.org/wiki/Seccomp

---

### 6.23 Docker Internals

#### SELinux and AppArmor

- https://en.wikipedia.org/wiki/Security-Enhanced_Linux (NSA)

*SELinux is a Linux kernel security module that provides a mandatory access control (MAC) mechanism for providing stricter security enforcement. SELinux defines a set of users, roles, and domains that can be mapped to the actual system users and groups. Processes are mapped to a combination of user, role, and domain, and policies define exactly what can be done based on the combinations used*

- https://wiki.ubuntu.com/AppArmor 

*AppArmor is a similar MAC mechanism that aims to confine programs to a limited set of resources. AppArmor is more focused on binding access controls to programs rather than users. It also combines capabilities and defining access to resources by path*

---

### 6.24 Docker Internals

#### Capabilities

- https://docs.docker.com/engine/security/

*Capabilities turn the binary “root/non-root” dichotomy into a fine-grained access control system. Processes (like web servers) that just need to bind on a port below 1024 do not need to run as root: they can just be granted the net_bind_service capability instead. And there are many other capabilities, for almost all the specific areas where root privileges are usually needed*

---

### 6.25 Docker Internals

#### Breaking Out

- https://unit42.paloaltonetworks.com/breaking-docker-via-runc-explaining-cve-2019-5736/
- https://www.youtube.com/watch?v=auUgVullAWM

---

# Homework for Session 7

- install minikube
- see https://minikube.sigs.k8s.io/docs/start/

# Planned for Session 7

- k8s
