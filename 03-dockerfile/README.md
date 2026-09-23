# how do we change the image?
Actually we can't change the image directly. We can make our custom images by creating Dockerfiles.

Later we'll write something like in a Dockerfile:
```Dockerfile
FROM debian

RUN apt update && apt install -y curl
```

Then we can build our custom image with the following command:

```bash
docker build -t my-custom-image .
```
With this command, we are telling Docker to build an image from the current directory (.) and tag it with the name `my-custom-image`.  
The `FROM debian` line in the Dockerfile specifies that our custom image will be based on the Debian image, and the `RUN` command in updating apt and installing `curl` installs `curl` in the image.

After an image is built, we can run a container from it:

```bash
docker run -it my-custom-image bash
```

Remember: `-it` takes you into the container's shell, and `bash` is the command that will be executed in the container.

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

If you feel confident enough till now, Good to know that you'll eventually create images containing the software/configuration required for:

```
NGINX
WordPress/PHP
MariaDB
```
But not yet.