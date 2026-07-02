# Docker & Docker Compose -- Nautilus DevOps Learning Notes

## Goal

This document summarizes the Docker and Docker Compose tasks completed
in the Nautilus labs, explaining **what to do**, **why it works**, and
**common mistakes**.

------------------------------------------------------------------------

# 1. Dockerfile Basics

Typical structure:

``` dockerfile
FROM <base-image>
WORKDIR /app
COPY <dependency-file> .
RUN <install-dependencies>
COPY . .
EXPOSE <port>
CMD ["executable","file"]
```

### What each instruction does

  Instruction   Purpose
  ------------- ------------------------------------
  FROM          Base image
  WORKDIR       Working directory inside container
  COPY          Copy files from host to image
  RUN           Executes during image build
  EXPOSE        Documents listening port
  CMD           Runs when container starts

------------------------------------------------------------------------

# 2. Node.js Application

Directory:

    /node_app
    ├── Dockerfile
    ├── package.json
    └── server.js

Dockerfile:

``` dockerfile
FROM node:20
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 6400
CMD ["node","server.js"]
```

Build:

``` bash
docker build -t nautilus/node-web-app .
```

Run:

``` bash
docker run -d --name nodeapp_nautilus -p 8097:6400 nautilus/node-web-app
```

Test:

``` bash
curl http://localhost:8097
```

------------------------------------------------------------------------

# 3. Python Application

Directory:

    /python_app
    └── src
        ├── requirements.txt
        └── server.py

Dockerfile:

``` dockerfile
FROM python:3.12
WORKDIR /app
COPY src/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY src/ .
EXPOSE 3003
CMD ["python","server.py"]
```

Build:

``` bash
docker build -t nautilus/python-app .
```

Run:

``` bash
docker run -d --name pythonapp_nautilus -p 8097:3003 nautilus/python-app
```

## Common mistake

Using an old base image such as `python:3.2` causes dependency
installation failures because modern Flask packages require newer Python
versions.

------------------------------------------------------------------------

# 4. Docker Compose Fundamentals

Compose manages multiple containers.

Basic structure:

``` yaml
services:
  web:
  db:
```

Common keys:

-   image
-   build
-   container_name
-   ports
-   volumes
-   environment
-   depends_on

------------------------------------------------------------------------

# 5. PHP + MariaDB Stack

Example:

``` yaml
services:
  web:
    image: php:8.2-apache
    container_name: php_web
    ports:
      - "6300:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    image: mariadb:latest
    container_name: mysql_web
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_web
      MYSQL_USER: dbuser
      MYSQL_PASSWORD: MyP@ss123
      MYSQL_ROOT_PASSWORD: RootP@ss123
```

Run:

``` bash
docker compose up -d
```

------------------------------------------------------------------------

# 6. Troubleshooting Docker Compose

## Error

    additional properties 'service' not allowed

Fix:

``` yaml
services:
```

not

``` yaml
service:
```

------------------------------------------------------------------------

## Error

    additional properties 'volume' not allowed

Use:

``` yaml
volumes:
```

------------------------------------------------------------------------

## Error

    additional properties 'depends' not allowed

Use:

``` yaml
depends_on:
```

------------------------------------------------------------------------

## Error

    additional properties 'from' not allowed

`from` belongs to a Dockerfile.

Compose uses:

``` yaml
image:
```

------------------------------------------------------------------------

## Error

    unable to prepare context: path "/app" not found

Incorrect build path.

Example:

``` yaml
build: ./app
```

or

``` yaml
build: .
```

depending on where the Dockerfile is located.

------------------------------------------------------------------------

## Error

    python: can't open file '/code/app.py'

Volume mounted the wrong directory.

Correct:

``` yaml
volumes:
  - ./app:/code
```

------------------------------------------------------------------------

## Error

    COPY /server.crt ... not found

Wrong COPY path.

Correct:

``` dockerfile
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key
COPY html/index.html /usr/local/apache2/htdocs/
```

------------------------------------------------------------------------

# 7. Useful Commands

``` bash
docker images
docker ps
docker ps -a
docker build -t image .
docker run -d --name container -p host:container image
docker compose config
docker compose up -d
docker compose down
docker logs <container>
docker exec -it <container> bash
```

------------------------------------------------------------------------

# 8. Debugging Workflow

1.  Read the error carefully.
2.  Validate Compose:

``` bash
docker compose config
```

3.  Build image:

``` bash
docker build -t test .
```

4.  Check running containers:

``` bash
docker ps
```

5.  Check logs:

``` bash
docker logs <container>
```

6.  Fix one error at a time.

------------------------------------------------------------------------

# Key Lessons

-   Prefer `COPY` over `RUN cp` for host files.
-   Build context matters.
-   Dockerfile instructions run inside the image.
-   Compose keywords are strict (`services`, `volumes`, `depends_on`).
-   Use modern base images.
-   Validate YAML before deployment.
-   Fix only the broken configuration in Nautilus labs.
