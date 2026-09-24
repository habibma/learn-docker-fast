# Commands

Let's practice with a everything we have learned so far.

Make a `docker-compose.yml` file with the following content:
```yaml
networks:
  internal-network:
    driver: bridge
services:
  service1:
    image: python:3
    command: python -m http.server 8000
    networks:
      - internal-network
  service2:
    image: debian:bookworm
    command:
      - /bin/sh
      - -c
      - apt-get update && apt-get install -y curl && sleep infinity
    networks:
      - internal-network
```
The goal isn't to create something useful.

The goal is to understand:
```
Compose
   │
   ├── creates network
   │
   ├── starts server1
   │
   └── starts server2
```
The goal isn't to create something useful. The goal is to understand how Compose orchestrates the infrastructure.
Then verify that server2 can reach:

```
server1:8000
```


Then run the following commands:
```bash
docker compose up -d
docker compose ps
docker compose logs service1
docker compose logs service2
docker compose exec service2 curl service1
docker compose down
```