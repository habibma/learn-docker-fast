# how do we change the image?
Actually we can't change the image directly. We can make our custom images by creating Dockerfiles.

Later we'll write something like:

```Dockerfile
FROM debian

RUN apt update && apt install -y curl
````

Then we can build our custom image with the following command:

```bash
docker build -t my-custom-image .
```

Then an image is built and we can run a container from it:

```bash
docker run -it my-custom-image bash
```

Conceptually:
```
Debian image
     │
     │ Dockerfile
     ↓
Your custom image
     │
     ↓
Container
     └── +curl

```

You'll eventually create images containing the software/configuration required for:

```
NGINX
WordPress/PHP
MariaDB
```
But not yet.