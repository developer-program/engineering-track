# Install Docker

## Install Docker on MacOS

System Requirements:
1. macOS must be version 10.13 or newer, i.e. High Sierra (10.13), Mojave (10.14) or Catalina (10.15)
2. Mac hardware must be a 2010 or a newer model

Steps:
1. Go to [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)
2. Select "Get Docker"
3. Open **Docker.dmg** and move to Applications
4. Open Docker
5. Create Docker account via [dockerhub](https://hub.docker.com/?&utm_medium=account_create)


##  Install Docker Desktop on Windows

System Requirements:
1. 64 bit processor with Second Level Address Translation (SLAT)
2. 4GB system RAM

Steps:
1. Go to [Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
2. Select "Get Docker"
3. Run the **Docker Desktop Installer.exe**
4. Open Docker
5. Create Docker account via [dockerhub](https://hub.docker.com/?&utm_medium=account_create)

Note that installation includes Docker Engine, Docker CLI client, Docker Compose, Notary, Kubernetes, and Credential Helper.


## Verify Docker is installed
```sh
$ docker --version
$ docker run hello-world
```
