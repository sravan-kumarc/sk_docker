
 **Docker Master Cheat Sheet** 

```markdown
# 🐳 Docker Master Cheat Sheet (No Swarm)  

---

## 🚦 Quick Index

- [Installation & Setup](#installation--setup)
- [Image Commands](#image-commands)
- [Container Commands](#container-commands)
- [Volumes](#volumes)
- [Networks](#networks)
- [System Ops](#system-ops)
- [Registry & Publishing](#registry--publishing)
- [Dockerfile (All Fields)](#dockerfile-all-fields--multistage)
- [Docker Compose Example](#docker-compose-example)
- [Quick Workflow](#quick-publish-workflow)
- [Reference Tables](#reference-tables)

---

## 🛠 Installation & Setup

```
```
- curl -fsSL https://get.docker.com -o get-docker.sh
- sudo sh get-docker.sh

- sudo systemctl start docker
- sudo systemctl enable docker

- docker version     # Show Docker version
- docker info        # Show Docker info
- docker help        # List commands

```



## 📦 Image Commands
```

| Purpose             | Command                                           |
|---------------------|--------------------------------------------------|
| List images         | `docker images`                                  |
| Search Hub          | `docker search <name>`                           |
| Pull image          | `docker pull <image>:<tag>`                      |
| Build image         | `docker build -t <name>:<tag> .`                 |
| Tag image           | `docker tag <img_id> username/repo:tag`          |
| Push image          | `docker push username/repo:tag`                  |
| Remove image        | `docker rmi <image>`                             |
| Save image          | `docker save -o image.tar <image>`               |
| Load image          | `docker load -i image.tar`                       |
| Show history        | `docker history <image>`                         |

```

## 🧱 Container Commands
```

| Purpose                  | Command                                         |
|--------------------------|-------------------------------------------------|
| List running             | `docker ps`                                     |
| List all                 | `docker ps -a`                                  |
| Run default              | `docker run <image>`                            |
| Detached mode            | `docker run -d <image>`                         |
| Interactive shell        | `docker run -it <image> /bin/bash`              |
| Port mapping             | `docker run -p 8080:80 <image>`                 |
| Named container          | `docker run --name <name> <image>`              |
| Auto remove after exit   | `docker run --rm <image>`                       |
| Execute in container     | `docker exec -it <container> <cmd>`             |
| Get logs                 | `docker logs -f <container>`                    |
| Stop/Start/Restart/Remove| `docker stop/start/restart/rm <container>`      |
| Prune stopped            | `docker container prune`                        |
| Inspect container/image  | `docker inspect <container>`                    |
| Show container processes | `docker top <container>`                        |
| Export/Import filesystem | `docker export <container> -o export.tar` <br>`docker import export.tar <new-image>` |

```
## 🗄 Volumes

```
- **List**:  
  `docker volume ls`
- **Create**:  
  `docker volume create <name>`
- **Inspect**:  
  `docker volume inspect <name>`
- **Remove**:  
  `docker volume rm <name>`
- **Use in container**:  
  `docker run -v myvol:/data <image>`
- **Bind mount**:  
  `docker run -v $(pwd):/app <image>`
- **Prune unused**:  
  `docker volume prune`

```

## 🌐 Networks

```
- **List**:  
  `docker network ls`
- **Create**:  
  `docker network create <name>`
- **Inspect**:  
  `docker network inspect <name>`
- **Remove**:  
  `docker network rm <name>`
- **Connect/Disconnect**:  
  `docker network connect <network> <container>`  
  `docker network disconnect <network> <container>`

```

## 🧰 System Ops

```
- **Show disk usage**:  
  `docker system df`
- **Prune all unused data**:  
  `docker system prune -a`
- **Show live resource usage**:  
  `docker stats`
- **Show events**:  
  `docker events`
```


## 🔐 Registry & Publishing

```
- **Login:**  
  `docker login`  
  `docker login registry.example.com`
- **Logout:**  
  `docker logout`
- **Push:**  
  `docker push username/repo:tag`
- **Pull:**  
  `docker pull username/repo:tag`
```

## 📝 Dockerfile (All Fields & Multistage)

```
# syntax=docker/dockerfile:1

# ---- Builder Stage ----
FROM node:20 AS builder
LABEL maintainer="dev@example.com"
ARG BUILD_ENV=production
ENV APP_HOME=/src/app NODE_ENV=${BUILD_ENV}
WORKDIR $APP_HOME
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# ---- Production Stage ----
FROM nginx:alpine
LABEL version="1.0" description="Prod nginx for app"
ENV NGINX_PORT=80
WORKDIR /usr/share/nginx/html
COPY --from=builder /src/app/dist ./
HEALTHCHECK --interval=30s --timeout=5s \
  CMD curl -f http://localhost:80/ || exit 1
EXPOSE 80
VOLUME ["/usr/share/nginx/html"]
USER 1001
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
```

**Dockerfile Key Fields:**  
```
- `FROM` : base image, allows multistage
- `LABEL`,
- `ARG`, `ENV`, `WORKDIR`
- `COPY`, `ADD`, `RUN`, `HEALTHCHECK`, `EXPOSE`
- `USER`, `VOLUME`, `ENTRYPOINT`, `CMD`
- `COPY --from=builder` : for multistage builds

```

## 🧩 Docker Compose Example

```
version: "3.9"

services:
  web:
    build: .
    container_name: flask_web
    restart: always
    ports:
      - "8080:5000"
    environment:
      - FLASK_ENV=production
      - DATABASE_URL=postgresql://postgres:password@db:5432/appdb
    volumes:
      - .:/usr/src/app
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: appdb
    volumes:
      - pgdata:/var/lib/postgresql/data
    expose:
      - "5432"

volumes:
  pgdata:
    driver: local
```


---

## 🚦 Quick Publish Workflow

```
docker build -t yourusername/app:1.0 .
docker login
docker push yourusername/app:1.0
docker pull yourusername/app:1.0
```

---

## 📋 Reference Tables
```
### 🔥 Container Life Cycle
| Action         | Command                             |
|----------------|-------------------------------------|
| Build image    | `docker build ...`                  |
| Run container  | `docker run ...`                    |
| Exec in shell  | `docker exec -it ... /bin/bash`     |
| Stop/Remove    | `docker stop/rm ...`                |

---

### 📁 Dockerfile Fields Reference
| Field        | Required | Description                  |
|--------------|----------|-----------------------------|
| FROM         | ✔️       | Base image, multiple allowed |
| LABEL        |          | Image metadata               |
| COPY/ADD     |          | Copy files                   |
| RUN          |          | Execute build commands       |
| CMD          |          | Default container command    |
| ENTRYPOINT   |          | Default binary               |
| EXPOSE       |          | Document listening ports     |
| ENV/ARG      |          | Environment/build variables  |
| VOLUME       |          | Declare persistent storage   |
| HEALTHCHECK  |          | Container healthcheck script |
| WORKDIR      |          | Set working directory        |
| USER         |          | Set user/UID                 |

---

> **⭐ This cheat sheet is designed for DevOps, cloud, or daily use. Copy and personalize for your own Docker journey!**

```
