---
title: "Docker"
image:
  path: /assets/img/logo_docker.png
...

Docker automates application deployment in lightweight containers. Containers
isolate applications and their dependencies, making them easier to run consistently
across development and production environments.

Source: [Wikipedia](https://en.wikipedia.org/wiki/Docker_(software))

## Useful Commands

> Docker commands that change system resources may require administrator privileges.
{: .prompt-info }

| Description | Command |
| --- | --- |
| Build an image | `docker build -t IMAGE_NAME .` |
| List images | `docker images` |
| Run an image with an interactive shell | `docker run -it IMAGE_NAME /bin/bash` |
| Copy a file from the host to a container | `docker cp PATH_ON_HOST CONTAINER_NAME:PATH_IN_CONTAINER` |
| Copy a file from a container to the host | `docker cp CONTAINER_NAME:PATH_IN_CONTAINER PATH_ON_HOST` |
| Remove all containers | `docker rm -vf $(docker ps -aq)` |
| Create a container without starting it | `docker create --name CONTAINER_NAME IMAGE_NAME` |
| Start a container | `docker start CONTAINER_NAME` |

Docker images are immutable templates used to create containers. A container is
a running instance of an image.

- `CMD` provides the default command executed when a container starts.
- `ENTRYPOINT` defines the executable that receives arguments passed to `docker run`.

## Docker Installation

The following procedure installs Docker Engine on Ubuntu.

```bash
# Remove potentially conflicting packages
for pkg in docker.io docker-doc docker-compose docker-compose-v2 \
  podman-docker containerd runc; 
do sudo apt-get remove $pkg; done

# Install prerequisites
sudo apt-get update
sudo apt-get install -y ca-certificates curl

# Add Docker's official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install Docker Engine
sudo apt-get install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin

# Verify the installation
sudo docker run --rm hello-world
```

Source: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

## Dockerfile Example

This example installs a complete TeX Live distribution in an Ubuntu-based image:

```dockerfile
FROM ubuntu:xenial
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -q && apt-get install -qy \
    texlive-full \
    python-pygments gnuplot \
    make git \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /data
VOLUME ["/data"]
```

