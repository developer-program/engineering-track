# Docker Compose

Docker enable us to keep our dependcies for an application within the running container which is great but having to maintain multiple applications that are required to run together is still troublesome. With docker compose, we can specify all the applications/services that needs to talk to one another and spin the containers up in a single command. 

## Starting a container with a single command. 

Docker compose is a seperate program from docker, the installation typically comes together. To check if docker compose is install, type `docker-compose version` and you should see `docker-compose version 1.25.4` printed out. 

## Running single service with Docker Compose

Using the react app as an example, 
```Dockerfile
version: "3.7"
services:
  docker-react:
    build: .
    ports:
      - "3001:3000"
```

1. run the command `docker-compose up`, you should now be able to access the react app. 


### Version
The `version` in the docker compose file standard for the `Compose file format`

|Compose file format|Docker enginer release|
|---|-
|3.7|18.06.0+
|3.6|18.02.0+

### Services

`services` takes in a array of service. service can be thought of a key value pair. The key of the service can be anything tha represents the name of the service. It can be a arbitary value. We use `docker-react` in the example.

### Build

`build` specifies where we want to build the container from, pointing to the location of the default `Dockerfile`. If we want to speicify to use a dockerfile that have a different name than the default `Dockerfile`, we will have to use `build` as an object, and speicify the key `context` and the dockerfile name using the key `dockerfile`.

```
build: 
    conext: ./docker-react
    dockerfile: Dockerfile.dev
```

### PORTS

`ports` takes in an array of port that we want to connect our computer to container. 


## Network

In docker, we can create a [docker network](https://docs.docker.com/engine/reference/commandline/network/) that allows docker services to communicate with one another without exposing ports. Typically, creating docker network require a series of complicated commands, with docker-compose, all services created in the same compose file will already be connected to the same network and can communicate with one another without exposing ports.

In below, we give our docker-express service an environment variable that says `MONGO_URL=mongodb://mongo:27017/test`. The key here is the `mongo` which is typically where you will put the url(i.e. `mongodb://127.0.0.1:27017`). The `MONOGO_URL` will become the environment vairbale accessable using `process.env.MONGO_URL`.

```
version: "3.7"
services:
  docker-express:
    build:
      context: ./docker-express
      dockerfile: Dockerfile
    environment:
      - "MONGO_URL=mongodb://mongo:27017/test"
    depends_on:
      - mongo
  mongo: 
    image: "mongo:latest"
```

## Common docker-compose commands
- `docker-compose up`: running all docker images
- `docker-compose up -d`: running all docker images in background
- `docker-compose up --build`: build all docker images then and run all docker images
- `docker-compose down`: stop all images created by `docker-compose up`
- `docker-compose ps`: look for running container by docker-compose file in current directory

## LAB
Create Dockerfile and `docker-compose.yml` for your frontend application. 



