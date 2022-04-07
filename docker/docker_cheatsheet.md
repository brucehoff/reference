# Table of Contents
- [Table of Contents](#table-of-contents)
- [Basic Terminology](#basic-terminology)
- [Getting Started](#getting-started)
- [Working with Containers](#working-with-containers)
  - [Running Containers](#running-containers)
  - [Managing Containers](#managing-containers)
- [Writing Dockerfiles](#writing-dockerfiles)
  - [Basics](#basics)
  - [A simple DOCKERFILE](#a-simple-dockerfile)
  - [Image Management](#image-management)


# Basic Terminology

**Daemon**: background process that receives commands.
**Dockerfile**: set of layers containing instructions used to build images.
**Image**: artifact nade from a compiled Dockerfile.
**Container**: instance of an image.

# Getting Started

```
docker run hello-world:latest
```
This will explain the mechanics of launhing launching a container.

# Working with Containers

## Running Containers
 
Running a container:
```
docker run <image>
```

Running a container and giving it a name:
```
docker run --name <name> <image>
```

Running a container and dropping into the shell:
```
docker run -it image
docker run --interactive --tty
```

Running a container in the background:
```
docker run -d <image>
```

Running a container with a restart policy:
```
docker run --restart (always|no|on-failure[:maxretries]|unless-stopped)
```

Accessing a container:

Start a container:
```
docker start <container>
```

Run a command against a container:
```
docker exec <container> <command>
```

Copy a file to the container:
```
docker cp <source> <container>:<destination>
```

Copy from a container to localhost:
```
docker cp <container>:<source> <destination>
```

## Managing Containers

Start/Stop a container:
```
docker start <container>
docker stop <container>
```

Remove a container:
```
docker rm <container>
```

Remove all stopped containers:
```
docker container prune
```

Rename a container:
```
docker rename <container> <new-name>
```

Container metrics:
```
docker stats [<container>]
```

View container information:
```
docker instpect <container>
```

Create image from container:
```
docker commit <container> <name-of-new-image>
```

Map container port to host services:
```
docker run -p <host-port>:<container-port>
```

# Writing Dockerfiles

## Basics

- Best practice is to write images for containers to perform a single task
- Images are organized in layers.  These layers are cached - which improves reusability
- They often start ```FROM``` a base image that defines the file system
- It's a good idea to bundle commands you want to ```RUN``` in your containers

## A simple DOCKERFILE

```
FROM node:10-alpine
RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app
WORKDIR /home/node/app
COPY package*.json ./
RUN npm config set registry http://registry.npmjs.org/
RUN npm install
COPY --chown=node:node . .
USER node
EXPOSE 8080
CMD [ "node", "index.js" ]
```

## Image Management

Save image locally:
```
docker pull <image>
```

List available images:
```
docker image ls
```

Remove an image:
```
docker image rm <image>
```

Prune all unused images:
```
docker image prune -a
```

View image information:
```
docker image inspect <image>
```