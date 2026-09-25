# Compose Basics

Read this page very fast without stopping and go the next page. You will come back to each section deeper later.

## 1. What Problem Does Compose Solve?

Imagine you have an application with three services:

```text
NGINX
  │
  ▼
WordPress
  │
  ▼
MariaDB
```
Without Docker Compose, you would have to create and configure each part manually:
```bash
docker network create my-network

docker volume create mariadb-data
docker volume create wordpress-data

docker build -t my-mariadb ./mariadb
docker build -t my-wordpress ./wordpress
docker build -t my-nginx ./nginx

docker run ...
docker run ...
docker run ...
```
And you would need to remember:
- Which containers belong to which network?
- Which volumes should be mounted?
- Which environment variables are required?
- Which ports should be published?
- Which images need to be built?
- How should the services communicate?
- Which services depend on other services?

As the application grows, manually managing all of this becomes difficult.

This is where Docker Compose comes in.

## What Is Docker Compose?

Docker Compose lets you describe your application's infrastructure in a configuration file.

For example:
```
compose.yml
```
Instead of telling Docker step by step how to create every component, you describe what your application should look like.

For example:
```yaml
services:

  nginx:
    build: ./nginx
    ports:
      - "8080:80"

  wordpress:
    build: ./wordpress

  mariadb:
    build: ./mariadb
```
Compose reads this configuration and creates the required containers, networks, volumes, and other resources.  
The basic idea is:
```

                    compose.yml
                         │
                         ▼
                 Docker Compose
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       NGINX          WordPress       MariaDB
     Container        Container       Container
```
## The Basic Mental Model

Think of compose.yml as a blueprint for your application infrastructure.
```text
                    compose.yml
                         │
          ┌──────────────┼──────────────┐
          │              │              │
          ▼              ▼              ▼
       Services       Networks        Volumes
          │              │              │
          │              │              │
          ▼              ▼              ▼
       Containers    Communication    Persistent
                                      Data
```
The Compose file describes how these pieces fit together.

For example:
```yaml
services:
  nginx:
    ...

  wordpress:
    ...

  mariadb:
    ...

networks:
  internal-network:
    ...

volumes:
  mariadb-data:
  wordpress-data:
```

## The Main Building Blocks

A Compose file is mainly built from a few concepts.
```
Services
```
A service describes an application component that Compose should run.

### Services
```yaml
services:

  nginx:
    ...

  wordpress:
    ...

  mariadb:
    ...
```
Each service normally results in one or more containers.

For our project:
```
nginx      → NGINX container
wordpress  → WordPress/PHP-FPM container
mariadb    → MariaDB container
```
### Networks
Networks allow services to communicate with each other.
```yaml
networks:

  internal-network:
    driver: bridge
```
Services can then join the network:
```yaml
services:

  wordpress:
    networks:
      - internal-network

  mariadb:
    networks:
      - internal-network
```
Now WordPress can reach MariaDB using:
```
mariadb:3306
```
instead of:
```
localhost:3306
```
### Volumes

Volumes provide persistent storage.
```yaml
volumes:

  mariadb-data:
  wordpress-data:
```
They can then be mounted into services:
```yaml
services:

  mariadb:
    volumes:
      - mariadb-data:/var/lib/mysql
```
The important idea is:
```text
Container
    │
    ▼
Volume
    │
    ▼
Persistent data
```
### Ports

Ports expose a container service to the host.
```yaml
services:

  nginx:
    ports:
      - "8080:80"
```
This means:
```
Host
 │
 │ 8080
 ▼
Container
 │
 │ 80
 ▼
NGINX
```
The browser can therefore access:
```
http://localhost:8080
```
Internal services such as MariaDB usually don't need their ports published to the host.

### Environment Variables

Services often need configuration such as database credentials.

Compose can provide these through environment variables:
```yaml
services:

  mariadb:
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
```
The values are then available inside the container. 
