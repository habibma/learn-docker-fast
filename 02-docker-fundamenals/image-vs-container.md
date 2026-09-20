# Image vs Container

## Image

An image is the starting template.

```
Debian image
├── Debian filesystem
├── binaries
├── libraries
└── configuration
```  

## Container

A container is created from that image.
```
Container
├── filesystem based on image
├── changes made while running
└── running/stopped process state
```
If you install:

```
apt install curl
```
you changed the container's writable layer. You didn't magically rewrite the original image. But what if you want to change an image?

This is where **Dockerfiles** come in.