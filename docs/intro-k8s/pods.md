# Pods

> A Pod is the basic execution unit of a Kubernetes application--the smallest and simplest unit in the Kubernetes object model that you create or deploy. A Pod represents processes running on your cluster. - Kubernetes.io

Pod is a wrapper around a container. Each pod has a internal IP address assigned to it to make it identifable and reachable within a node. Within a Pod, you can run a single container or multiple container. Is most common to see a "one-container-per-Pod".

Being the smallest unit in Kubernetes means that you can't directly create a container. Each container needs to be inside a Pod.

## Revision on Docker

- able to build and tag docker images
- able to run a docker images

### Run Docker using image

- if docker cannot find the image locally, it will fetch from Dockerhub => cache
- docker will then fetch image from catch and use it to build container

```bash
docker run hello-world
```

### Building your own Docker images

Example: In profile folder with `Dockerfile`

```bash
docker build -t johnsnow/todo-list-api .
docker run  johnsnow/todo-list-api
```

### Pushing to Dockerhub

```bash
docker login
# type in crential for johnsnow
docker push johnsnow/todo-list-api
```

### Docker Revision Lab

1. Login in Dockerhub or sign up for an account
2. Login Docker hub in bash/terminal using `docker login`
3. find a standalone project
4. create Dockerfile for standalone project
5. build the docker image
6. push to Dockerhub and see that it successfully push to Dockerhub
7. run `docker images` and view all the images
8. run `docker rmi <username>/<image>` then run `docker images` and see all the images get deleted. (Alternative use `docker rmi -f $(docker images -a -q)` to remove all images)
9. run `docker run <username>/<image>` and see that container runs properly
10. run `docker images` to see images downloaded correctly

## Running a pod

### Run hello-world in a pod

```bash
❯ kubectl run --generator=run-pod/v1 first-pod --image hello-world
pod/first-pod created
```

### Results

```bash
❯ kubectl get pods
NAME READY STATUS RESTARTS AGE
first-pod 0/1 ContainerCreating 0 3s
```

### view logs

```bash
❯ kubectl logs first-pod

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### delete pod

```bash
❯ kubectl delete pod --all
pod "first-pod" deleted
```

Verify pods deleted

```
❯ kubectl get pods
No resources found in default name-space.
```

## Pod Template

In actual use case, people rarely run containers directly using commands such as `kubectl run`. Instead, is much more common to create configuration files(`.yaml` or `.json`) to create k8s objects for you.

Is common to put all k8s configuration files in a place. You can create a `k8s` folder in the root directory.

In `k8s/todolist-api-pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
name: todos
spec:
containers:
  - name: todos-list
image: xxx/todo-list-api
```

### Creating the pod

```bash
❯ kubectl apply -f ./k8s/todolist-api-pod.yaml
pod/todos created
```

### Deleting the pod

```bash
❯ kubectl delete -f ./k8s/todolist-api-pod.yaml
pod "todos" deleted
```

### Get logs

```get logs
❯ kubectl logs todo-api

> todo-list-api@1.0.0 start /app
> nodemon src index.js

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node src index.js`
Example app listening at http://localhost:5000
```

### Port forwarding

We can connect to our port using port forwarding.

If you are using `kubectl apply`

```bash
❯ kubectl port-forward todos 5000:5000
Forwarding from 127.0.0.1:5000 -> 5000
Forwarding from [::1]:5000 -> 5000
Handling connection for 5000
Handling connection for 5000
```

### lab for Kubectl apply

you will need an React app and express app to do this lab.
If you don't have one, just create one with boilerplate code.

1. For express app, Create a YAML file to create a pod and run it in k8s at port 5000
2. Repeat React app, repeat the same at port 3000
3. connect them using `localhost:3000` and `localhost:5000`
