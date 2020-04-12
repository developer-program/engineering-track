# Building Image

Using existing images built by other developers is great, but if we want to create our own server that
runs our own code, we will need to build our own image. Let's create our own image.

## Create a hello world image

Our software requires some environment to run in. Let's start with Ubunutu.

Do the following to create an image and run it in a container:

1. Create a file, named `Dockerfile`, with the following content

   ```dockerfile
   FROM ubuntu

   CMD ["echo", "hello world"]
   ```

2. Build the image by running the `docker build .` command, which will output

   ```sh
   $ docker build .
   Successfully built a00cc775fd4b
   ```

   Note that the alphanumeric characters generated refers to the image ID.

3. Using the newly built image, run a container, which will print the "hello world" message.
   ```
   $ docker run a00cc775fd4b
   hello world
   ```

In the `Dockerfile`, we started with `FROM ubuntu`. The `FROM` keyword allows us to specify a base image to start from.
The `CMD` keyword refers to the commands to run. As such, the `echo hello world` command is run right after the
container is spun up.

Congratulations, you have created your first image and run it successfully!

## Install software dependencies

In the previous example, we have used `echo` which is a built-in program that comes with Ubuntu. Most OS or base images
will not have everything we need to run our software. Similar to setting up a server in any machine, we will have to
specify and install the required dependencies. Docker allows us to do that using the `RUN` keyword in Dockerfile.

Ubuntu by default does not come with the `ping` command. Let's create a Dockerfile that can `ping` for us.

1. Create a Dockerfile with the following content

   ```dockerfile
   FROM ubuntu

   RUN apt-get update -y
   RUN apt-get install iputils-ping -y

   CMD ["ping", "www.google.com"]
   ```

2. Build the image

   ```sh
   $ docker build .
   ```

3. Using the newly built image, run a container
   ```sh
   $ docker run <image id>
   64 bytes from sc-in-f147.1e100.net (74.125.68.147): icmp_seq=1 ttl=37 time=12.8 ms
   64 bytes from sc-in-f147.1e100.net (74.125.68.147): icmp_seq=2 ttl=37 time=12.6 ms
   64 bytes from sc-in-f147.1e100.net (74.125.68.147): icmp_seq=3 ttl=37 time=15.2 ms
   64 bytes from sc-in-f147.1e100.net (74.125.68.147): icmp_seq=4 ttl=37 time=15.8 ms
   ```
   You should see the `ping` response.

## Copying Files into docker containers

To copy files from local dir to the base image, we can use the `COPY` command.

1. Create a new directory with a `hello.sh` that simply prints a `Hello World`

```sh
#!/bin/bash

echo "Hello World"
```

2. Create a Dockerfile that runs `ubuntu`. Copy the `hello.sh` over and run the bash file on startup.

```
FROM ubuntu

COPY . .
RUN chmod 700 hello.sh

CMD ["./hello.sh"]
```

you should see the following printed out

```sh
Hello World
```

## Creating a default working dir

The files are by default copy to the home directory. If there are files/folders that with the same as those in the home directory, this may cause a problem. Is also, in general, a good practice to put all the related files into the same folder.

To do that, one way is to create a folder when building the docker file. `RUN mkdir <folder name>` and reference the file in the folder when running the command. `./<folder name>/hello.sh`. This will be highly inefficient. Docker has us covered using the `WORKDIR` command.

```
FROM ubuntu

WORKDIR app

COPY . .
RUN chmod 700 hello.sh

CMD ["./hello.sh"]
```

you should see the following printed out

```sh
Hello World
```

The above example will add all files into the `app` folder.

## Lab

Using the personal project you have created.

1. Run MongoDB using the `docker run` command
2. Create a Dockerfile for an Express lab that connects to the MongoDB
3. Create a Dockerfile for a React app that connects to the Express app
