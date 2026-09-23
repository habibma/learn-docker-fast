# What is an Image?
Am image is a **read-only template** that contains the instructions for creating a container. You can think of it as a blueprint.

```
                  image
             /      |      \
            /       |       \
           ↓        ↓        ↓
      Container  Container  Container
          A          B          C
```

An image itself isn't a running thing. It's something Docker can use to create a container.

To see the images you have on your system, run the following command:

```bash
docker images
```
 or
```bash
docker image ls
```
It will show you a list of images, their IDs, and other information. If you haven't downloaded any images yet, the list will be empty.
