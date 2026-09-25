## Container management
As you already know, containers are created from images. But what if you want to change an image? You can't change an image directly. You can only create a new image based on the original one. This is where **Dockerfiles** come in.

1- Let's make our first Dockerfile together.  
Make a directory called `docker-practice` and navigate into it:

```bash
mkdir docker-practice
cd docker-practice
```

2- Create a Dockerfile using this command:

```bash
touch Dockerfile
```

3- Using nano editor to can edit the Dockerfile:

```bash
nano Dockerfile
```
4- write the following content in the Dockerfile:
```Dockerfile
FROM debian:bookworm

RUN apt update && apt install -y curl

CMD ["bash"]
```
5- Save the file using `Ctrl+O` and exit the nano editor using `Ctrl+X`.


## FROM debian:bookworm
This is the starting point of your image.

Conceptually:
```
Your Dockerfile
      │
      ▼
FROM debian:bookworm
      │
      ▼
Debian base image
      │
      ▼
Your custom image
```
You are saying:

> "Build my image on top of Debian Bookworm."

What happs behind the scenes?  
You receive a base filesystem and metadata that Docker can use as the starting point for your image.

Conceptually:
```
debian:bookworm image
│
├── /bin
├── /etc
├── /usr
├── /var
├── system libraries
├── package management tools
└── other Debian userspace files
```
Then your Dockerfile adds things to it when it arrives aat line:

## Run apt update && apt install -y curl
This command before `&&` makes sure that the package list is up to date, and it has the latest version containd curl which we are going to install after `&&`.  
The `-y` flag automatically confirms the installation of curl without prompting for user input.

Conceptually after this line you have:
```
Debian base image
        │
        │ + curl
        ▼
Your custom image
```

## CMD ["bash"]
This command specifies the default command to run when a container is started from your custom image. In this case, it will start a Bash shell.  

## Build
The next step is building. With `Dockerfile` you have the instructions to build your custom image. You can build it using the following command:

```bash
docker build -t my-custom-image .
```
You are telling Docker to build an image from the current directory (.) and tag it with the name `my-custom-image`.

Now Let's run a container from your custom image:

```bash
docker run -it my-custom-image
```

When you run this command, Docker will create a new container from your custom image and start a Bash shell inside it. In this stage the `CMD ["bash"]` instruction in the Dockerfile is executed, and you will be inside the container's shell.

Check to see if curl is installed by running:
```bash
curl --version
```

Now exit the container by typing `exit` and pressing Enter. You will return to your host machine's terminal.  
Let's remember the concept of image and container:

```bash
docker image ls
```

What do you get? 

And then try this:
```bash
docker container ls -a
```
What do you get?


## COPY
Let's practice the `COPY` instruction. Imagine you have a file in your local machine that you want to include in your custom image to be run inseide the container. You can use the `COPY` instruction in your Dockerfile to copy files from your local machine into the image.  
MAke a file called `myfile.txt` in the same directory as your Dockerfile and add some content to it. For example, you can use the following command to create the file and add some text:

```bash
echo "Hello, World!" > myfile.txt
```

Edit your Dockerfile, and add the following line before the `CMD` instruction:

```Dockerfile
COPY myfile.txt /tmp/myfile.txt
```
Change the CMD line to:
```Dockerfile
CMD ["cat", "/tmp/myfile.txt"]
```

Let's build a new image with the new Dockerfile:

```bash
docker build -t hello-image .
```
Now, run it. how? You remmeber!

What do you get? intresting, isnt it?


## Recap
In this section, you learned how to create a Dockerfile, build a custom image from it, and run a container from that image. You also learned about the `FROM`, `RUN`, `COPY`, and `CMD` instructions in a Dockerfile. You practiced building an image based on the Debian Bookworm image, installing curl, and copying a file into the image. You also learned how to run a container from your custom image and check if curl is installed. Finally, you practiced using the `COPY` instruction to include a file in your custom image and run a command to display its content when the container starts.

Docker roughly works in three steps in the Build process:
```text
Step 1
FROM debian:bookworm
        ↓
Base image

Step 2
RUN apt update && apt install -y curl
        ↓
curl added

Step 3
COPY hello.txt /tmp/hello.txt
        ↓
file added

        ↓
IMAGE CREATED
```

And in the Run process:
```text
Step 1
docker run -it hello-image
		↓
Container started

Step 2
CMD ["cat", "/tmp/hello.txt"]
		↓
Command executed inside the container

Step 3
Output displayed in the terminal

Step 4
exit
		↓
Container stopped
```

Now try using `Docker ps`, `Docker ps -a`, and `Docker images` to see the containers and images you have created. These Commans are shortcuts for `Docker container ls`, `Docker container ls -a`, and `Docker image ls` respectively.