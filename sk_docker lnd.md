**Docker Master Cheat Sheet**

```markdown
# Docker Master Cheat Sheet (No Swarm)

## 🛠️ Installation & Setup

```


# Install Docker (Linux)

curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Start Docker Daemon (Linux)

sudo systemctl start docker
sudo systemctl enable docker

# Version, Info, Help

docker version
docker info
docker help

```

---

## 🏞️ Images

```

docker images                            \# List images
docker search <name>                     \# Search Docker Hub
docker pull <image>:<tag>                \# Pull image

```
docker build -t <name>:<tag> .           # Build image from Dockerfile
```

docker tag <image_id> username/repo:tag  \# Tag image
docker push username/repo:tag            \# Push image (login first)
docker rmi <image>                       \# Remove image
docker history <image>                   \# Show image history
docker save -o image.tar <image>         \# Save image to archive
docker load -i image.tar                 \# Load image from archive

```

---

## 🚀 Containers

```

docker ps                                \# List running containers
docker ps -a                             \# All containers
docker run <image>                       \# Run container
docker run -d <image>                    \# Run in background
docker run -it <image> /bin/bash         \# Interactive shell
docker run -p 8080:80 <image>            \# Port mapping

```
docker run --name <name> <image>         # Named container
```

docker run --rm <image>                  \# Auto remove after exit

```
docker exec -it <container> <cmd>        # Run command in running container
```

docker logs -f <container>               \# View logs
docker stats                             \# Resource usage
docker stop <container>                  \# Stop container
docker start <container>                 \# Start container
docker restart <container>               \# Restart container
docker rm <container>                    \# Remove container
docker container prune                   \# Remove stopped containers
docker inspect <container or image>      \# Inspect container/image JSON
docker top <container>                   \# Show processes in container
docker export <container> -o export.tar  \# Export filesystem
docker import export.tar <new-image>     \# Import filesystem as image

```

---

## 📦 Volumes

```

docker volume ls                         \# List volumes
docker volume create <name>              \# Create volume
docker volume inspect <name>             \# Inspect volume
docker volume rm <name>                  \# Remove volume
docker run -v myvol:/data <image>        \# Use volume
docker run -v \$(pwd):/app <image>        \# Bind mount
docker volume prune                      \# Remove unused volumes

```

---

## 🌐 Networks

```

docker network ls                        \# List networks
docker network create <name>             \# Create network
docker network inspect <name>            \# Inspect network
docker network rm <name>                 \# Remove network

```
docker network connect <network> <container>    # Connect to network
```

docker network disconnect <network> <container> \# Disconnect from network

```

---

## ⚙️ System Management

```

docker system df                         \# Show disk usage
docker system prune -a                   \# Cleanup system
docker stats                             \# Realtime resource statistics
docker events                            \# Live events

```

---

## 🔑 Registry Authentication/Image Publishing

```

docker login                             \# Docker Hub or custom registry
docker login registry.example.com         \# Custom registry login
docker logout                            \# Logout
docker push username/repo:tag            \# Push image
docker pull username/repo:tag            \# Pull image

```

---

## 📝 Full Dockerfile Example (Multistage, All Fields)

```


# syntax=docker/dockerfile:1

# ===== Stage 1: Builder =====

FROM node:20 AS builder
LABEL maintainer="dev@example.com"
ARG BUILD_ENV=production
ENV APP_HOME=/src/app NODE_ENV=\${BUILD_ENV}
WORKDIR \$APP_HOME
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# ===== Stage 2: Final (production) =====

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

**Dockerfile Fields**
- `FROM`: base image (required; multistage allowed)
- `LABEL`: metadata
- `ARG`: build-time variables
- `ENV`: run-time env variables
- `WORKDIR`: set working directory for instructions
- `COPY`, `ADD`: copy files
- `RUN`: build steps
- `HEALTHCHECK`: define container health
- `EXPOSE`: document ports (does not publish)
- `USER`: set container user
- `VOLUME`: declare volume mount point
- `ENTRYPOINT`, `CMD`: default process/command
- `COPY --from=builder`: artifact copy for multistage

---

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

**Compose Highlights**
- `services`: containers and configs
- `build`: build image
- `image`: get image from registry
- `container_name`: set name
- `restart`: restart policy
- `ports`: publish container ports
- `environment`: runtime env vars
- `volumes`: persistent/host data
- `depends_on`: start order
- `expose`: internal networking only
- `volumes` (top-level): shared persistent data

---

## 🔗 Example Workflow - Push Custom Image

```

docker build -t yourusername/app:1.0 .
docker login
docker push yourusername/app:1.0
docker pull yourusername/app:1.0

```

---

> **This cheat sheet covers every fundamental and advanced Docker skill you need: install, images, containers, volumes, networks, system ops, registry, Dockerfile (all fields/multistage), Docker Compose, and image publishing.**
```

Just copy and save the above content into a `.md` file. Let me know if you want a download-ready text or further sections!

