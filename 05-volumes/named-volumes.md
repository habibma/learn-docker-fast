# What is Docker Volume?

A Docker volume is a directory on the host machine (your computer) that is mounted into a container. It allows you to persist data even when the container is removed or replaced. A volume stores data outside the container's writable layer.

## Create your first volume
To create a volume, you can use the following command:
```bash
docker volume create my-volume
```

Check that the volume has been created:
```bash
docker volume ls
```

## Mount the volume to a container
First, good to know what mounting is. Mounting is the process of making a volume accessible to a container. When you mount a volume to a container, the container can read and write data to that volume.

write this command to mount the volume to a container:
```bash
docker run -it --name my-container -v my-volume:/data alpine sh
```
The important part is:
```
-v my-volume:/data
```
This means that the volume `my-volume` is mounted to the container at the path `/data`.  
Any data written to `/data` inside the container will be stored in the volume on the host machine.

Now, you have a container running with the volume mounted at `/data`.  
Let's say you want to create a file in that directory and it will be stored in the volume.

```bash
cd /data
echo "Hello, World!" > data.txt
```
Now, exit the container and check if the file is still there:
```bash
exit
```
Then remove the container:
```bash
docker rm -f my-container
```
The nice thing is you have removed the container but not your data.

Do you want to take your access back? cool! Ler's make a new container and mount the same volume to it:
```bash
docker run -it --name my-new-container -v my-volume:/data debian:bookworm bash
```
Now, check if the file is still there:
```bash
cd /data
cat data.txt
```

There? ABSOLUTELY!