# What is a Container?
A **container** is a running instance of an image.

Dive into it. Open a terminal and run the following command:

```bash
docker run debian
```
what happens?
Docker will download the `debian` image from Docker Hub (if you don't have it already) and create a container from it. 

# What does docker run actually do?
To understand this, let's experiment together. Open a terminal and run the following command:

```bash
docker run hello-world
```

You'll probably see a message explaining that Docker successfully ran the container. right?

First time you run this command, Docker will download the `hello-world` image from Docker Hub and create a container from it.

Check the list of images you have on your system by running the following command:

```bash
docker images
```
or 
```bash
docker image ls
```

You will get something like this:

```
IMAGE				ID            DISK USAGE   CONTENT SIZE   	EXTRA
hello-world   4ab4c602aa5e   		1.85kB       13.3kB        11.5kB
```

Remember `Docker images' means "What images do I have?"

To see the list of containers you have on your system, run the following command:

```bash
docker ps
```
or
```bash
docker container ls
```

You might see an empty list like this:
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

But if you run the following command:
```bash
docker ps -a
```
or
```bash
docker container ls -a
```

You will see a list of containers, including the one processes are finished running. It will look something like this:

```
CONTAINER ID   IMAGE         COMMAND       CREATED          STATUS                PORTS     NAMES
d1e3f5c6b7a8   hello-world   "/hello"  10 seconds ago   Exited (0) 5 seconds ago         hopeful_morse
```

## Why does the container stop?

Suppose the image tells Docker to execute `hello-world`.

The process runs:

```
hello-world -> prints message -> exit
```
Once the container's main process exits, the container stops.


## Debian Container
Now let's use Debian

Run:

```
docker run -it debian bash
```

This command has several pieces.
```
docker run
```
Create and start a container.
```
debian
```
Use the Debian image.
```
-it
```
This gives us an interactive terminal.

`-it` allows you to interact with the container through your terminal. This tells Docker to run Bash inside the container.

If you run the command, you should now be inside the container and you can run commands as if you were on a Debian system. For example, you can run:

```bash
apt update
```

Play around and try some bash commands.
``` bash
ls
ls -l
ls /etc/os-release
pwd
whoami
```
When you're done, type `exit` to leave the container.


## Containers are isolated
1- Remove all images and containers from your system by running the following commands:

```bash
docker container prune -f
docker image prune -a -f
```
 
2- Now run the following command to create a new container from the Debian image:

```bash
docker run -it debian bash
```
then

```bash
apt update
```
This command will update the package lists for the Debian system inside the container. It will download the latest package information from the Debian repositories.  
then

```bash
apt install -y curl
```
This command will install the `curl` package inside the container. The `-y` flag automatically confirms the installation, so you don't have to manually approve it.  

Then check the version of `curl` installed by running:

```bash
curl --version
```

Now exit the container by typing `exit` and pressing Enter.
```bash
exit
```

3- Now run again the following command to create a new container from the Debian image:

```bash
docker run -it debian bash
```

Then check if `curl` is installed by running:

```bash
curl --version
```

What do you see? something like this:
```
bash: curl: command not found
```
You think why?

Because:
```
             Debian IMAGE
              /       \
             /         \
            ↓           ↓
     Container A    Container B
       + curl          no curl
```