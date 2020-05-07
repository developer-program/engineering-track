# Docker repository

## DockerHub

### Prerequiste

- make sure you have docker install `docker --version`

### Create Repo

1. Create an account in [Dockerhub](https://hub.docker.com/)
2. Create Repo on docker hub

![create repo](_media/dockerhub_repo_create.png)

### Create Dockerfile

create a file `Dockerfile`

```Dockerfile
FROM node:12

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . ./

CMD ["npm", "start"]
```

### Create Docker ignore file

create a file `.dockerginore`

```.dockerignore
node_modules

build

coverage

.dockerignore
Dockerfile
```

### Build and Tag

`docker build -t elsonlim/pokemon-demo .`

### Push to repo

1. Login with `docker login`
2. Push image `docker push <username>/pokemon-demo:latest`

### To test

1. Remove image locally `docker rmi <username>/pokemon-demo`
2. Pull image `docker pull <username>/pokemon-demo:latest`
3. Run image `docker run -p 3000:3000 -d username>/pokemon-demo`

![docker pull and run](_media/dockerhub_pull_run.png)
