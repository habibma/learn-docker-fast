# Container Networking

The first command we will run is:

```bash
docker network ls
```

It will show you the networks that are available on your Docker host. You will see three networks: `bridge`, `host`, and `none`. These are the default networks that Docker creates when it is installed.

HINT: Have you noticed the pattern of docker commands? They all start with `docker` and then the subcommand. In this case, the subcommand is `network`, which is used to manage networks. The `ls` subcommand is used to list the networks.


OK, now let's tell Docker that we want to make our two containers communicate with each other. We will create a new network called `my-network`:

## 1 - Create the network:
```bash
docker network create my-network
```

Cleanup images and containers that we created in the previous section. You can do this by running the following commands:

```bash
docker container prune -f
docker image prune -f
```

## 2 - Run the first container:
Run a container from the 'debian' image and connect it to the `my-network` network. We will name this container `container1`:
```bash
docker run -d -it --name container1 --network my-network debian bash
```

`docker run` means run a new container.
`-d` means run the container in detached mode (in the background).
`-it` means run the container in interactive mode and give me a terminal.
`--name container1` means name the container `container1`. (We need a name to be able to communicate with it from another container.)
`--network my-network` means connect the container to the `my-network` network. 

## 3 - Run the second container:
Start another container from the 'debian' image and connect it to the `my-network` network. We will name this container `container2`, but this time not using the `-d` flag.
```bash
docker run -it --name container2 --network my-network debian bash
```

Now when we are inside `container2`, we can ping `container1` by its name:

```bash
ping container1
```
or we can inspect the network and see the IP address of `container1`:

```bash
docker network inspect my-network
```