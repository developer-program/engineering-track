# Docker Basics

## Workflow

```sh
docker run hello-world
```

1. When we run `docker run hello-world`
2. docker check if there is the `hello-world` image locally in cache
3. if image is not in cache,∏ docker will try to find the image in docker hub and put it in the image cache
4. docker then take the image from the cache which contains the instructions to create a container. 
5. the container will contain one application with all it's dependencies running.
6. the `hello-world` program will log out the text "hello world"

```sh
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

## Containers

Containers works with name spacing. Namespacing allows ocmputer to create a logical grouping of applications associated with a namespace. Docker make use of namespacing to create a virtual partition between different containers. Docker controls these containers thru docker engine which then send system call to the kernal to interact with different storage, network and other computer resource within the system. 

When we use Docker, we are actually create a VM with a linux kernel. Containers are cerated inside the VM.
You can dun `docker version` to look at the details. 

![what-is-container](_media/container-what-is-container.png)

## Docker instructions

### Running a container

We can create a docker container from an image using the run command.

1. `docker run <image> <command to run in the container>`

```sh
docker run ubuntu echo hello world
```

This will print out the hello world text exactly like how a ubuntu would print out the text.
Every image usually will contain a default command. We can overwrite the command with the 4 arguments `echo hello world`.

### Lifecycle of a container

1. image in
2. conntainer create
2. conntainer start
2. container running
3. container pause
4. container unpause
6. container stop
7. container restart
8. container kill

### To run a docker container
We can use docker to run a software by first knowing what is the image that contains the instuctions to start the application we need. 

1. create `docker create <image> <additional commands>`
2. start `docker start -a <id>`
- `docker run` === `docker create <image>` follow by `docker start -a <id>`
- the `-a` is running docker on attach mode which will print out the logs

### Check contain status

- To check containers that are running `docker ps`
- To check all containers that got created and still in our system `docker ps -a`


### Stop a container

- `docker run busybox ping www.google.com`
- `docker ps`
- `docker stop <id>`(SIGTERM) or `docker kill <id>`(SIGKILL)

Try to use docker stop which runs SIGTERM, this allow the container to clean up before shutting down. 
If `docker stop` doesn't work, try `docker kill` which more aggressively kill the container without letting the container clean up resouce. 

### Cleaning up

Using prune, we can remove all stopped containers, networks, images and build cache.
`docker system prune`


### Interating with a container

On a running container, we can interact with the container using the  `exec -it` command. the `-it` flag connects our terminal into the stdin of the container allow us to run commands on it. Running the `mongo` command, we can open mongo cli. 

1. run `docker run mongo`, this will install and run mongodb locally
2. `docker ps` to verify container is running and copy out the container id
3. `docker exec -it <id> mongo` this runs the command `mongo` in a interactive mode which allow you to connect to the mongodb
4. run `show dbs` to list all the database

we can clean up by first exit
```sh
exit
docker stop <id>
```

If we want to open the shell in a container, we can replace the mongo with shell/bash `sh`

### Port Mapping

Sometimes, we want to allow incoming request to the container. for example a request that wants to take data from a server running in docker. 

To map a port from the server to docker container, we need the `-p` flag

`docker run -p 8080:8080 <image id>`

in another words

`docker run -p <computer-port>:<docker-port> <image-id>`

## Exercise

Using docker, run mongo application and connect an express application to the mongo instance in docker container. 