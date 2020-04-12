# What is Docker

[Docker](https://www.Docker.com/) is a software that helps you to create, maintain and connect different containers.
Docker is pretty much the standard of using containers today. In fact, there is probably more people recognising
the term "Docker" than "container".

Container is a technology that packages a software with all its dependencies so that you can easily run the software
in a standalone and isolated environment.


## Why use Docker or containers

### Dependencies

When you run a container using Docker, all the dependencies would be included and isolated inside the container.
This means that you will have all the dependencies readily available without the need to install all the them yourself.
For example, things like installing Java before Oracle database or installing Python for another software.

Allowing Docker to maintain your dependencies comes with another benefit. When you require multiple versions of
the same software (e.g. python2, python3.6.9, python3.8), mapping the environment variables for different applications
that require different versions of the same software can be a nightmare. Docker isolates the dependenices,
meaning that you don't have to take care of it.

Besides that, you can also version your Docker images. This means that as you build your software and
change your dependencies, you will not have any problem rolling back to an older version of your software.
Imagine if you have updated the dependencies to run a new version of the software, but later realise that
you need to roll back. You might need to downgrade your dependencies in order to roll back.
Running in Docker saves you from all these issues.

### Portability
Using [Docker hub](https://hub.Docker.com/), we can publish our Docker images and reuse the images anywhere,
i.e. any system that you can run Docker on. Docker is currently supported on Linux, macOS and Windows,
meaning that most Docker images can run on any system.

### Security

If there are any dependencies trying to access our local files, the isolation of dependencies in the Docker image will
prevent any Dockerized application from accessing the files and folders running in our machine. We typically map a port
from our machine to the Docker container and only allow information to pass through within the port.

### Ecosystem

The popularity of Docker makes cloud providers, such as [AWS](https://aws.amazon.com/) and [GCP](https://cloud.google.com/),
give great support for Docker containers. You can easily run Docker containers on AWS and GCP.

Technologies such as [Kubernetes](https://kubernetes.io/) can also help or orchestrate Docker containers,
allowing them to scale when necessary.
