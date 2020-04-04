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

## Running an node container

### Building the image

To run react, we have a dependency on node and npm. We can find any image that have node installed.
You can find all the node images [here](https://hub.docker.com/_/node)

Below we also show a new keyword `WORKDIR`. The `WORKDIR` will create a folder from our home directory and all our actions will be done within the WORKDIR. This allow us to store all our code in one place and our files won't crash with any system files.

In the Dockerfile, using the `node:alpine` image as base, we first COPY the pacakge.json over, this will allow docker

1. In a react app, create a `Dockerfile`. If you don't have a react app you can create a new one. `npx create-react-app <app-name>`.

```Dockerfile
FROM node:alpine

WORKDIR '/app'

COPY ./package.json .
RUN npm install

COPY . .

CMD ["npm", "run", "start"]
```

2. Build the image. `docker build -t docker-ui-demo .`
   Again we build the docker image but this time we tag the image to a name "docker-ui-demo"

### Running the container

3. Run can now image using the name we give. `docker run -it -p 3001:3000 docker-ui-demo`

You can also choose to run in background using `detach` mode with the `-d` flag.

`docker run -it -d -p 3001:3000 docker-ui-demo`

### cleaning up the container

4. we can look at the images we have using the command `docker image ls` which list all tagged image.

5. To remove the tag using the command `docker image rm docker-ui-demo`.

## Lab

1. On your personal Project, create a `Dockerfile` on react app and run the recat app using docker
2. Run a database that the backend project is using.
3. On your personal Project, create a `Dockerfile` on exxpress app and run the express app using docker
4. Test if your application works.
