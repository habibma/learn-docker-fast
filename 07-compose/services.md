# Services
Before we create a `docker-compose.yml` file, we need to know about the structure of a Compose file.
A Compose file starts with the `services` section. like this:

```yaml
services:
  mariadb:
    ...
  wordpress:
	...
  nginx:
	...
```
Spaces matter in YAML, so make sure to use the correct indentation. Each service is defined under the `services` section, and each service has its own configuration options. use spaces (two spaces each), not tabs, for indentation.

Why did we devide to use `mariadb`, `wordpress`, and `nginx` as our services? Because they are the core components of our mini-project we want to build in the last section of this course.

## Services vs Containers
This distinction is worth understanding.

You might write:
```yaml
services:
  mariadb:
    image: mariadb
```
Here:

`mariadb` (first appearance) is the service definition.
`mariadb` (second appearance) is the image name.

Docker Compose uses that definition to create a container.

Conceptually:
```
Compose service
      │
      │ creates/manages
      ▼
Container
      │
      ▼
Process
```
So:
`mariadb` service is not itself the `MariaDB process`.

The actual MariaDB process runs inside the container created for that service.

## Build
But if you want to build your own image for a service, you can use the `build` option instead of `image`. For example:
```yaml
services:
  my-service:
    build:
      context: ./my-service
```
For our mini-project, we will use the our own images created from the Dockerfiles we will write in the last section of this course. So we will use the `build` option for our services.
Conceptually:
```text
./my-service
        │
        └── Dockerfile
              │
              ▼
           docker build
              │
              ▼
        our custom image
              │
              ▼
        our custom container
```