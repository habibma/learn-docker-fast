# Wrapping Up
## The Three Layers You've Now Learned

At this point, you should be able to see the bigger picture.

You've learned how Docker works at three different levels:

---

### 1. Dockerfile — Build the Image

A **Dockerfile** describes how an image should be built.

```text
Dockerfile
    │
    │ docker build
    ▼
  Image
```
The image is the packaged environment that contains everything needed to run an application:

- Base operating system/filesystem
- Dependencies
- Application files
- Configuration
- Default command
```
Dockerfile → Image
```
### 2. Docker — Run the Container

Docker takes an image and creates a running container from it.
```text
Image
  │
  │ docker run
  ▼
Container
  │
  ▼
Process
```
The container is the isolated environment in which the application's process runs.

This is where the Linux concepts from the previous section become important:

- Processes
- Networking
- volumes
```
Image → Container → Process
```
### 3. Docker Compose — Connect the Services

Real applications usually need more than one container.

Docker Compose lets us define and manage multiple services together.
```text
                    Compose
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
        Image        Image        Image
          │            │            │
          ▼            ▼            ▼
      Container    Container    Container
          │            │            │
          └────────────┼────────────┘
                       │
                 Docker Network
                       │
                ┌──────┴──────┐
                ▼             ▼
             Service A     Service B

                Persistent Volumes
```
Compose defines how these containers work together:

- Which images to use
- How containers communicate
- Which networks they use
- Which volumes they use
- Environment variables
- How services are started

Compose → Multiple Containers → Connected Application

## The Bigger Picture

Put everything together:

                    Docker Compose
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
      Service A       Service B       Service C
          │               │               │
          ▼               ▼               ▼
      Container       Container       Container
          │               │               │
          ▼               ▼               ▼
       Process         Process         Process
          │               │               │
          └───────────────┼───────────────┘
                          │
                    Docker Network
                          │
                   Persistent Data
                       Volumes

## And the complete flow is:
```text
Dockerfile
    │
    │ docker build
    ▼
  Image
    │
    │ docker run / Compose
    ▼
Container
    │
    ▼
Process
    │
    ├── Network
    │
    └── Volumes
```
This is the mental model you should take with you into the next section.

- Dockerfile builds the image.
- Docker runs the container.
- Compose connects the services.


One thing I'd emphasize in your course: **Compose doesn't replace Docker**. Compose is an orchestration/configuration layer that uses Docker to create and run the containers. That's why the three-layer model works well here.