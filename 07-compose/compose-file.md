# From Configuration to Running Application

Once the Compose file describes the application, we can start it with:
```
docker compose up
```
Compose reads:
```text
compose.yml
     │
     ▼
Docker Compose
     │
     ├── Create network
     │
     ├── Create volumes
     │
     ├── Build images
     │
     └── Create/start containers
```
To run everything in the background:
```
docker compose up -d
```
To stop the application:
```
docker compose down
```
## The Important Mental Shift

With individual Docker commands, you are telling Docker:

- Create this network.
- Create this volume.
- Build this image.
- Run this container.
- Connect it to this network.
- Mount this volume.
- Set this environment variable.

With Compose, you describe the desired infrastructure:

- Here is my application.
- It has these services,
- these networks,
- these volumes,
- these ports,
- and this configuration.

Then Compose handles the creation and management of those resources.

Docker runs containers.

Docker Compose describes and manages multi-container applications.