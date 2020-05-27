# Docker Compose

Docker enables us to keep our dependencies for an application within the running container, which is great.
However, having to maintain multiple applications that are required to run together is still troublesome.
With docker compose, we can specify all the applications/services that need to talk to one another and
spin the containers up in a single command.

## Install Docker Compose

Although Docker Compose is a separate program from docker, the installation typically come together.
To check if Docker Compose is installed:

```sh
$ docker-compose version
docker-compose version 1.25.4
```

You should see the Docker Compose version printed out.

## Running a single service with Docker Compose

1. Using the react app as an example, create a `docker-compose.yml` as such:

   ```yml
   version: "3.7"
   services:
     docker-react:
       build: .
       ports:
         - "3001:3000"
   ```

if this does not run try.
The github issue can be found [here](https://github.com/facebook/create-react-app/issues/8688)

```yml
version: "3.7"
services:
  docker-react:
    build: .
    ports:
      - "3001:3000"
    stdin_open: true
```

2. Run the command

   ```sh
   $ docker-compose up
   ```

3. You should now be able to access the react app

### Version

The `version` in the docker-compose file refers to the standard for the `compose file format`.

| Compose file format | Docker Engine release |
| ------------------- | --------------------- |
| 3.7                 | 18.06.0+              |
| 3.6                 | 18.02.0+              |

### Services

In the docker-compose file, `services` takes in a array of services. A service can be thought of as a key value pair.
The key of the service can be anything that represents the name of the service. It can be an arbitary value.
In the above example, we used `docker-react` as the key of the service.

### Build

The `build` option specifies where we want to build the container from, pointing to the location of the default
Dockerfile. If we want to specify using a Dockerfile that has a different name than the default `Dockerfile`,
we will have to modify `build` by specifying the key `context` and the Dockerfile name using the key `dockerfile`.

```yml
build:
  context: ./docker-react
  dockerfile: Dockerfile.dev
```

### Ports

The `ports` option takes in an array of ports that we want to connect from our computer to the container.

## Network

In Docker, we can create a [docker network](https://docs.docker.com/engine/reference/commandline/network/) that allows
Docker services to communicate with one another without exposing ports. Typically, creating docker network require a
series of complicated commands. With docker-compose, all services created in the same compose file will already be
connected to the same network and can communicate with one another without exposing ports.

In the below example, we give the `docker-express` service an environment variable, `MONGO_URL`. The key here is the `mongo` service, which is typically where you will put the `MONGO_URL`, i.e. `mongodb://127.0.0.1:27017`. Instead,
the `MONGO_URL` will become an environment variable accessible in the `docker-express` service,
using `process.env.MONGO_URL`.

```yml
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

| Command                     | Description                                                                 |
| --------------------------- | --------------------------------------------------------------------------- |
| `docker-compose up`         | Runs all docker images                                                      |
| `docker-compose up -d`      | Runs all docker images in background                                        |
| `docker-compose up --build` | Builds all docker images then runs all docker images                        |
| `docker-compose down`       | Stops all images created by `docker-compose up`                             |
| `docker-compose ps`         | Looks for running containers created by docker-compose in current directory |

## Lab

Continue from the docker image lab, you should have created a Dockerfile for the React and Express app. Create a docker-compose file and replace the implementation.

1. Create a `docker-compose.yml` that runs a MongoDB instance
2. Add configuration to run the Express app on the docker-compose file
3. Add configurations to run React on the docker-compose file
