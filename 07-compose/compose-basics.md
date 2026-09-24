# Compose Basics

## 1. What problem does Compose solve?

Without Compose, you would have to manually do things like:
```
docker network create inception-network

docker volume create mariadb-data
docker volume create wordpress-data

docker build ...
docker run ...
docker run ...
docker run ...
```
And you'd have to remember:

- which container connects to which network?
- which volumes are mounted?
- which environment variables are needed?
- which ports are published?
- which service starts before another?
- which images should be built?

That's a lot of commands. aren't they? And if you forget one, your infrastructure won't work.

This is where **Compor** the great comes in.**Compose** lets you describe this infrastructure in one file:
```
docker-compose.yml
```
Then Docker Compose can create and manage the infrastructure for you.

## 2. The basic mental model

Think of your Compose file as a map:
```diagram
                    docker-compose.yml
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
          MariaDB       WordPress       NGINX
             │             │             │
             └─────────────┼─────────────┘
                           │
                        Network
                           │
                    ┌──────┴──────┐
                    │             │
                 Volumes      Configuration
```
Compose describes how all these pieces fit together.