# Container Storage

## First experiment — data inside a container
Run a contianer based on the `alpine` image and create a file inside it:

```bash
docker run -it --name my-container alpine sh
```
in tmp directory, create a file called `data.txt` and write some text in it:

```bash
cd /tmp
echo "Hello, World!" > data.txt
```

Now, exit the container and check if the file is still there:
```bash
exit
```

The file `data.txt` is not available outside the container, as it was created inside the container's filesystem.

if you start the container again, you will see that the file is still there:

```bash
docker start -ai my-container
cat /tmp/data.txt
```
but if you remove the container, the file will be lost:

```bash
docker rm -f my-container
```

If you run the container again, the file will not be there:

```bash
docker run -it --name my-container alpine sh
cat /tmp/data.txt
```	
Do you see the file?
NO!!!

You have lost your imporant data!