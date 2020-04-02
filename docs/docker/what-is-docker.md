# What is Docker

Docker is a software that helps you to create, maintain and connect different containers. Docker is pretty much the standard of using containers today. There is probably more people recognising the term docker more than containers. 

Container is a technology that package a software with all it's dependencies so that you can easily run the software in a standalone, isolated environment. 


## Why do we want to use Docker or Container

### Dependencies

When you run a container using docker, all the dependencies would be isolated inside the container. This means that you will have all the dependencies without require to install all the dependencies pacakage yourself. Things like installing java before oracle database or python for another software. 

Allowing Docker to maintain your dependencies also comes with another benefit. When you require multiple versions of the same software(phython2, phython3.6.9, python3.8), mapping the environment variable for different application that required different version of the same software can be a nightmare. Docker isolate the dependenices meaning that you don't have to take care of it.

Beside that, you can also version your docker images, this means that as you build your software and change your dependencies, you still will not have problem going back to run a older version of your software. Imagine if you update the dependencies to run a new version of the software and later realise that you need to roll back. You might have to downgrade your dependencies. Running in Docker will safe you from all these issue.

### Portability
Using docker hub, we can publish our images and reuse the images in anywhere, any system that you can run docker on. Docker is currently supported on Lunux, macOS and windows meaning that most docker images can run container. 

### Security

By Isolating the dependencies, if there are any dependencies that tries to access our local files. The Isolation will prevent any internal application from accessing the files and folders running in our machine. We typically map a port from our computer to docker container and only allow infomation to pass thru within the port. 

### Ecosystem

The popularity of Docker makes cloud providers such as AWS and GCP gives great support for docker containers. You can easily run docker containers on AWS and GCP. 

Technologies such as kUbernetes can helps or orchestrate docker containers allowing them to scale when necessary. 
