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
