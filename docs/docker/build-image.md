# Building Image

Using existing images built by other developers is great but if we want to create our own server that run our own code. We will need to build our own image. Let's create our own image. 

## Creating a hello world image
Our software requires some environment to run in. We can do that by running 

1. Create a file name `Dockerfile`
```dockerfile
FROM ubuntu

CMD ["echo", "hello world"]
```

2. run `docker build .` which will output 

```sh
Successfully built a00cc775fd4b
```

the alphanumeric characters generated is the image id

3. Using the newly build image. Run a container

`docker run a00cc775fd4b`

this will result in the hello world message from printed.

```sh
hello world
```

In the example above, we created our first image. 
We started with `FROM ubuntu`, the FROM keyword allow us to specify a base image to start from.
Follow by `CMD ["echo", "hello world"]` which runs the command right after the container is created. 

Congrats, we have created first docker image and run it.

## Installing software dependencies

The above example use echo, a buildin program that comes with Ubuntu. Most OS or base image will not have everything we need to run our software. Similar to setting up a server in any machine, we will have to specifiy and install the dependencies. Docker allow us to do that using the `RUN` keyword in dockerfile.

Ubuntu by default does not come with `ping`. Let's create a docker file that can do `ping` for us.

```Dockerfile
FROM ubuntu

RUN apt-get update
RUN apt-get install iputils-ping

CMD ["ping", "www.google.com"]
```

