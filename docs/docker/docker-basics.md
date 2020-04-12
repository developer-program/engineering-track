# Docker Basics

## Workflow

1. When we run:
   ```sh
   $ docker run hello-world
   ```
2. Docker checks if there is the `hello-world` image locally in cache
3. If the image is not in cache, Docker will try to find the image in [dockerhub](https://hub.docker.com/search?q=&type=image)
4. After it finds the image in dockerhub, it puts the found image in the cache
5. The image contains the instructions to create a container
6. Docker takes the image from the cache and creates the container
7. The container will then be spun up, containing the `hello-world` application with all its dependencies running
8. When the `hello-world` application runs, it will log out the following text:
   ```sh
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   ```

The whole output should look like this:
```sh
$ docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:f9dfddf63636d84ef479d645ab5885156ae030f611a56f3a7ac7f2fdd86d7e4e
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Containers

Containers work via namespacing. Namespacing allows the computer to create a logical grouping of applications
associated with a namespace. Docker makes use of namespacing to create a virtual partition between different containers.
Docker then controls these containers through the [Docker Engine](https://www.docker.com/products/container-runtime),
which sends system calls to the kernel to interact with different storages, network and other computer resources within the system.

When we use Docker, we are actually creating a VM with a Linux kernel. Containers are then created inside the VM.
To look at the details of the VM, run:
```sh
$ docker version

Client: Docker Engine - Community
 Version:           19.03.2
 API version:       1.40
 Go version:        go1.12.8
 Git commit:        6a30dfc
 Built:             Thu Aug 29 05:26:49 2019
 OS/Arch:           darwin/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.2
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.8
  Git commit:       6a30dfc
  Built:            Thu Aug 29 05:32:21 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.6
  GitCommit:        894b81a4b802e4eb2a91d1ce216b8817763c29fb
 runc:
  Version:          1.0.0-rc8
  GitCommit:        425e105d5a03fabd737a126ad93d62a9eeede87f
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

![what-is-container](_media/container-what-is-container.png)

### Lifecycle of a container

1. Container created (based on image)
2. Container started
3. Container running
4. Container paused
5. Container unpaused
6. Container stopped
7. Container restarted
8. Container killed

## Docker instructions

### To run a container

We can create and run a Docker container from an image using the `run` command in the following format:
```sh
$ docker run <image> <command to run in the container>
```

For example, to print "hello world" on an ubuntu image, we run:
```sh
$ docker run ubuntu echo hello world
```

This will print out the "hello world" text exactly like how ubuntu would print out the text.

Every image usually contains a default command to run when the container spins up. By providing the 4th argument
in the `docker run` command, i.e. `echo hello world` in the above example, we are overwriting this default command.
This means that the default command is now replaced by `echo hello world`.

### To create a container

We can also use Docker to first create a container but not start it. The `docker create` command prepares a container
using the necessary image for the application to run.
```sh
$ docker create <image> <additional commands>
```
The `docker create` command will output the container ID once the container is created.

When you are ready to run the created container,
```sh
$ docker start -a <container id>
```
Note that the `-a` option is for running Docker on attach mode which will print out the logs.

Essentially,
```
docker run === (docker create <image> + docker start -a <container id>)
```

### To check container status

To check which containers are currently running:
```sh
$ docker ps
```

To check all containers that are created and still in our system (regardless of current state):
```
$ docker ps -a
```

### To stop a container

Do the following to try stopping a container:

1. Run a container
   ```sh
   $ docker run busybox ping www.google.com
   ```
2. Verify that the container is running and note down its container ID
   ```
   $ docker ps
   ```
3. Stop the container using its contained ID
   ```
   $ docker stop <container id>
   ```
   The `docker stop` command runs `SIGTERM`, which allows the container to clean up its resources before shutting down.
   If `docker stop` doesn't work, try using `docker kill` which runs `SIGKILL`, hence killing the container more
   aggressively without letting the container clean up its resources.

### To clean up containers

Using `prune`, we can remove all unused data, i.e. the stopped containers, networks, images and build cache.
```sh
$ docker system prune
```

### To interact with a container

We can interact with a running container using the `exec -it` command. The `-it` options connect our terminal with
the STDIN of the container, allowing us to run commands on it.

Do the following to try interacting with a container:

1. Install and run mongoDB on a container using the `mongo` image
   ```sh
   $ docker run mongo
   ```
2. Verify that the container is running and note down its container ID
   ```
   $ docker ps
   ```
3. Run the `mongo` command to interact with the mongo shell _inside_ the container
   ```
   $ docker exec -it <container id> mongo
   ```
   Note that the `mongo` command allows you to connect to mongoDB.
4. Now list all the databases
   ```sh
   show dbs
   ```
5. To exit the interactive mode
   ```sh
   $ exit
   ```
6. Stop the container
   ```sh
   $ docker stop <container id>
   ```

For another example, instead of opening the mongo shell, to open a normal shell in a container, we can replace mongo
with shell/bash, i.e.
```sh
$ docker exec -it <container id> sh
```

### Port mapping

Sometimes, we want to allow incoming requests to the container. For example, we want to allow a request that takes data
from a server that is running in a Docker container. To do so, we need to map a port from the server to the
Docker container.

To map a port, we need the `-p` option:
```sh
$ docker run -p <computer-port>:<docker-port> <image-id>
```

For example:
```sh
$ docker run -p 8080:8080 <image id>
```

## Exercise

Using Docker, run a mongo application and connect an express application to the mongo instance in a Docker container.
