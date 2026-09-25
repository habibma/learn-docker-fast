# Docker CLI
Docker CLI (Command Line Interface) is a powerful tool that allows users to interact with Docker through commands in the terminal. It provides a way to manage Docker containers, images and so on. In this guide, we will cover the basic commands and usage of Docker CLI.

You already know these CLI commands from the previous sections, but here is a quick recap:

```bash
docker image ls
docker container ls
docker run
docker stop
```

And it's a good time to know how to clean up your Docker environment. You can remove unused images, containers, and networks with the following commands:

```bash
docker image rm <image_id>  # Remove an image
docker container rm <container_id>  # Remove a container
```

And to remove all stopped containers and unused images, you can use:

```bash
docker system prune -a  # Remove all stopped containers and dangling images
```

If you have used them once and play with them, go to the next section.