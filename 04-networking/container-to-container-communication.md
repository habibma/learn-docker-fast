# Container-to-Container Communication

To have a better comiunicatin experiment, We try again with two containers. This time, we will create a new network and connect both containers to it. This way, they can communicate with each other.

First, cleanup containers.
```bash
docker container rm -f container1 container2

```
Then, run two new containers with these features:
- **container1**: name it `container1`, connect it to a new network called `my-network`, and run it in detached mode , upon the image `python:3` and run the command `python3 -m http.server 8000` to start a simple HTTP server on port 8000.

- **container2**: name it `container2`, connect it to the same network `my-network`, and run it in interactive mode, upon the image `debian:bookworm` and run the command `bash` to start a bash shell.


```
docker run -d --name container1 --network my-network python:3 python3 -m http.server 8000
docker run -it --name container2 --network my-network debian:bookworm bash
```
After you are inside `container2`, you can use the `curl` command to make a request to the HTTP server running in `container1`. You can do this by using the name of the container as the hostname:

```bash
curl http://container1:8000
```
By `curl`ing `http://container1:8000`, you are making a request to the HTTP server running in `container1` on port 8000. The name `container1` is resolved to the IP address of the container by Docker's internal DNS, allowing the two containers to communicate with each other over the network.

Conceptually, the communication looks like this:
```
container2
    │
    │ curl http://container1:8000
    ▼
Docker DNS
    │
    │ container1 → IP address
    ▼
container1
    │
    │ port 8000
    ▼
Python HTTP server
```
