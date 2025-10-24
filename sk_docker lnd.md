Perfect! Here's a one-page, super-condensed Docker Master Cheat Sheet in Markdown, optimized for quick reference:
# 🐳 Docker Quick Reference (One-Page)

---

## 🛠 Installation & Setup
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo systemctl start docker
sudo systemctl enable docker
docker version
docker info
docker help
________________________________________
📦 Image Commands
Action	Command
List images	docker images
Search Hub	docker search <name>
Pull image	docker pull <image>:<tag>
Build image	docker build -t <name>:<tag> .
Tag image	docker tag <img_id> username/repo:tag
Push image	docker push username/repo:tag
Remove image	docker rmi <image>
Save/Load	docker save -o image.tar <img> / docker load -i image.tar
History	docker history <image>
________________________________________
🧱 Container Commands
Action	Command
List running/all	docker ps / docker ps -a
Run	docker run <image>
Detached	docker run -d <image>
Interactive shell	docker run -it <img> /bin/bash
Port mapping	docker run -p 8080:80 <img>
Named container	docker run --name <name> <img>
Auto remove	docker run --rm <img>
Exec in container	docker exec -it <cont> <cmd>
Logs	docker logs -f <cont>
Stop/Start/Restart/Remove	docker stop/start/restart/rm <cont>
Prune stopped	docker container prune
Inspect	docker inspect <cont>
Top processes	docker top <cont>
Export/Import FS	docker export <cont> -o export.tar / docker import export.tar <new-img>
________________________________________
🗄 Volumes & Bind Mounts
Action	Command
List	docker volume ls
Create	docker volume create <name>
Inspect	docker volume inspect <name>
Remove	docker volume rm <name>
Use in container	docker run -v myvol:/data <img>
Bind mount host	docker run -v $(pwd):/app <img>
Prune unused	docker volume prune
________________________________________
🌐 Networks
Action	Command
List	docker network ls
Create	docker network create <name>
Inspect	docker network inspect <name>
Remove	docker network rm <name>
Connect	docker network connect <net> <cont>
Disconnect	docker network disconnect <net> <cont>
________________________________________
🧰 System Ops
Action	Command
Disk usage	docker system df
Prune all	docker system prune -a
Live stats	docker stats
Events	docker events
________________________________________
🔐 Registry
Action	Command
Login	docker login / docker login <registry>
Logout	docker logout
Push	docker push username/repo:tag
Pull	docker pull username/repo:tag
________________________________________
📝 Dockerfile Essentials
Field	Notes
FROM	Base image, supports multistage
LABEL	Metadata
ARG / ENV	Build/runtime variables
WORKDIR	Set working dir
COPY / ADD	Copy files
RUN	Execute commands
EXPOSE	Document ports
VOLUME	Persistent storage
HEALTHCHECK	Container health
USER	Run as user
ENTRYPOINT / CMD	Runtime commands
________________________________________
🧩 Docker Compose
version: "3.9"
services:
  web:
    build: .
    container_name: flask_web
    ports: ["8080:5000"]
    environment:
      FLASK_ENV: production
      DATABASE_URL: postgresql://postgres:password@db:5432/appdb
    volumes: [.:/usr/src/app]
    depends_on: [db]

  db:
    image: postgres:15-alpine
    container_name: postgres_db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: appdb
    volumes: [pgdata:/var/lib/postgresql/data]
    expose: ["5432"]

volumes:
  pgdata:
    driver: local
________________________________________
🚀 Quick Publish
docker build -t yourusername/app:1.0 .
docker login
docker push yourusername/app:1.0
docker pull yourusername/app:1.0
________________________________________

---


