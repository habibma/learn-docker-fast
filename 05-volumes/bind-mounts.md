# Bind mounts

What is a bind mount? A bind mount is a type of volume that allows you to mount a file or directory from the host machine into a container. This means that any changes made to the file or directory on the host machine will be reflected in the container, and vice versa.  
Before going through the steps, Let's see the difference between a bind mount and a named volume.

## Named volumes vs bind mounts
 A named volume is managed by Docker and is stored in a specific location on the host machine, while a bind mount can be any file or directory on the host machine.


### Named volume

For example:

```
-v my-volume:/data
```
Docker manages where the volume lives.
```
my-volume
      │
      ▼
Container:/data
```
You refer to it by its name:
```
my-volume
```

### Bind mount

A host path is mounted directly:
```
Host path
/home/user/data
       │
       │ mounted
       ▼
Container:/data
```
For example:
```
-v /home/user/data:/data
```
Now you're explicitly saying:

> Mount this particular directory from my host into the container.