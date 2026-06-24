# Docker Fundamentals & Nautilus DevOps Labs

## Overview

This document contains Docker concepts, troubleshooting techniques, and practical exercises based on Nautilus DevOps lab tasks.

---

# 1. Installing Docker and Docker Compose

## Objective

Prepare a Linux server to run containerized applications.

## Why Docker?

Docker allows applications to run inside containers, providing:

* Portability
* Faster deployments
* Consistent environments
* Isolation from host system

### Traditional Deployment

```text
Application
├── Dependencies
├── Libraries
├── Runtime
└── OS Configuration
```

Problems:

* Works on one server but not another
* Dependency conflicts
* Difficult upgrades

### Containerized Deployment

```text
Container
├── Application
├── Dependencies
└── Runtime

Host OS Kernel Shared
```

Benefits:

* Lightweight
* Fast startup
* Easy deployment

---

## Install Docker

### Add Docker Repository

```bash
sudo yum install -y yum-utils

sudo yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo
```

### Install Docker

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

### Install Docker Compose Plugin

```bash
sudo yum install -y docker-compose-plugin
```

### Start Docker

```bash
sudo systemctl enable --now docker
```

### Verify

```bash
docker --version
docker compose version
systemctl status docker
```

---

# 2. Running an Nginx Container

## Objective

Deploy a lightweight nginx container.

### Pull Image

```bash
docker pull nginx:alpine
```

### Create Container

```bash
docker run -d --name nginx_2 nginx:alpine
```

### Verify

```bash
docker ps
```

---

## Understanding the Command

```bash
docker run -d --name nginx_2 nginx:alpine
```

| Option       | Purpose               |
| ------------ | --------------------- |
| run          | Create container      |
| -d           | Detached mode         |
| --name       | Assign container name |
| nginx:alpine | Image and tag         |

---

## Why Alpine?

| Image        | Approx Size |
| ------------ | ----------- |
| nginx:latest | ~190 MB     |
| nginx:alpine | ~40 MB      |

Advantages:

* Smaller download size
* Faster startup
* Reduced attack surface

---

# 3. Removing Containers

## Objective

Delete an unwanted container.

### Remove Container

```bash
docker rm -f kke-container
```

### Verify

```bash
docker ps -a
```

---

## Understanding

```bash
docker rm -f container_name
```

The `-f` flag:

1. Stops the container
2. Removes the container

---

# 4. Copying Files Between Host and Container

## Objective

Copy an encrypted file into a running container.

### Host File

```text
/tmp/nautilus.txt.gpg
```

### Destination

```text
/opt/
```

inside container `ubuntu_latest`

### Command

```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/
```

### Verify

```bash
docker exec ubuntu_latest ls -l /opt/
```

---

## Docker Copy Operations

### Host → Container

```bash
docker cp file.txt container:/path/
```

### Container → Host

```bash
docker cp container:/path/file.txt .
```

---

# 5. Docker Volumes

## Objective

Persist data outside containers.

### Example

```bash
docker run -d \
-v /var/www/html:/usr/local/apache2/htdocs \
httpd
```

### Mapping

```text
Host
/var/www/html
        │
        ▼
Container
/usr/local/apache2/htdocs
```

Benefits:

* Persistent data
* Easy backups
* Share data between host and container

---

# 6. Docker Port Mapping

## Objective

Expose container services to users.

### Example

```bash
docker run -d -p 8085:80 httpd
```

### Flow

```text
Host Port 8085
       │
       ▼
Container Port 80
```

### Access

```bash
curl http://localhost:8085/
```

---

# 7. Docker Troubleshooting

## Check Running Containers

```bash
docker ps
```

## Check All Containers

```bash
docker ps -a
```

## Inspect Container

```bash
docker inspect container_name
```

Useful for:

* Volume mappings
* Port mappings
* Environment variables
* Container status

---

## View Logs

```bash
docker logs container_name
```

Useful when:

* Container exits immediately
* Application crashes
* Startup failures occur

---

## Start Container

```bash
docker start container_name
```

---

## Stop Container

```bash
docker stop container_name
```

---

## Restart Container

```bash
docker restart container_name
```

---

# 8. Real Nautilus Troubleshooting Scenario

### Problem

Container:

```text
nautilus
```

was not running.

### Investigation

Check:

```bash
docker ps -a
```

Result:

```text
Status: Exited
```

Inspect:

```bash
docker inspect nautilus
```

Found:

### Correct Volume Mapping

```text
/var/www/html
        │
        ▼
/usr/local/apache2/htdocs
```

### Correct Port Mapping

```text
8085 -> 80
```

### Root Cause

Container stopped unexpectedly.

### Fix

```bash
docker start nautilus
```

or recreate:

```bash
docker rm -f nautilus

docker run -d \
--name nautilus \
-p 8085:80 \
-v /var/www/html:/usr/local/apache2/htdocs \
httpd
```

---

# Common Docker Commands Cheat Sheet

## Images

```bash
docker images
docker pull nginx
docker rmi image_name
```

## Containers

```bash
docker ps
docker ps -a
docker run
docker start
docker stop
docker restart
docker rm -f
```

## Logs

```bash
docker logs container_name
```

## Exec

```bash
docker exec -it container_name bash
```

## Copy Files

```bash
docker cp hostfile container:/path/
docker cp container:/path/file .
```

## Inspect

```bash
docker inspect container_name
```

## Volumes

```bash
docker volume ls
docker volume inspect volume_name
```

---

# Key Concepts Learned

* Docker Engine installation
* Docker Compose installation
* Running containers
* Removing containers
* Copying files into containers
* Port mapping
* Volume mapping
* Container inspection
* Container troubleshooting
* Using logs for debugging
* Website accessibility testing with curl

These are foundational skills for Docker, Kubernetes, and modern DevOps workflows.
