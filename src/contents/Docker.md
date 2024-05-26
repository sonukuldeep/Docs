---
author: kuldeep
datetime: 2022-12-21
title: Docker
slug: docker
featured: true
draft: false
tags:
  - docker
ogImage: ""
description: Docker is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers.
---

# Docker

## What is docker?
The fastest way to containerize applications

Docker Desktop is secure, out-of-the-box containerization software offering developers and teams a robust, hybrid toolkit to build, share, and run applications anywhere.

## Install (Linux)
```bash
curl -sSL https://get.docker.com/ | sh
```

## Images

Docker images are a lightweight, standalone, executable package
of software that includes everything needed to run an application:
code, runtime, system tools, system libraries and settings.
```bash
# Build an Image from a Dockerfile
docker build -t <image_name>

# Build an Image from a Dockerfile without the cache
docker build -t <image_name> . –no-cache

# List local images
docker images

# Delete an Image
docker rmi <image_name>

# Remove all unused images
docker image prune
```

## Docker Hub

Docker Hub is a service provided by Docker for finding and sharing
container images with your team. Learn more and find images
at https://hub.docker.com

```bash
# Login into Docker
docker login -u <username>
# Publish an image to Docker Hub

docker push <username>/<image_name>
# Search Hub for an image

docker search <image_name>
# Pull an image from a Docker Hub

docker pull <image_name>
```

## General Commands
```bash
# Start the docker daemon
docker -d

# Get help with Docker. Can also use –help on all subcommands
docker --help

# Display system-wide information
docker info
```

## Containers

A container is a runtime instance of a docker image. A container
will always run the same, regardless of the infrastructure.
Containers isolate software from its environment and ensure
that it works uniformly despite differences for instance between
development and staging.

```bash
# Start a container
docker start <container_name>

# Stop a container
docker stop <container_name>

# Kill a container
docker kill <container_name>

# Restart a container
docker restart <container_name>

# Pause a container
docker pause <container_name>

# Resume container
docker unpause <container_name>

# Create and run a container from an image, with a custom name:
docker run --name <container_name> <image_name>

# Run a container with and publish a container’s port(s) to the host.
docker run -p <host_port>:<container_port> <image_name>

# Run a container in the background
docker run -d <image_name>

# Start or stop an existing container:
docker start|stop <container_name>(or <container-id>)

# Remove a stopped container:
docker rm <container_name>

# Open a shell inside a running container:
docker exec -it <container_name> sh

# Create image from container
docker commit <container_name> (or <container-id>) <image_name>

# Fetch and follow the logs of a container:
docker logs -f <container_name>

# To inspect a running container:
docker inspect <container_name>(or <container_id>)

# To list currently running containers:
docker ps

# List all docker containers (running and stopped):
docker ps --all

# View resource usage stats
docker container stats
```

## Dockerfile
### Keywords to know

- FROM Sets the Base Image for subsequent instructions.
- LABEL Set the Author field of the generated images.
- RUN execute any commands in a new layer on top of the current image and commit the results.
- CMD provide defaults for an executing container.
- EXPOSE informs Docker that the container listens on the specified network ports at runtime. NOTE: does not actually make ports accessible.
- ENV sets environment variable.
- ADD copies new files, directories or remote file to container. Invalidates caches. Avoid ADD and use COPY instead.
- COPY copies new files or directories to container. By default this copies as root regardless of the USER/WORKDIR settings. Use --chown=<user>:<group> to give ownership to another user/group. (Same for ADD.)
- ENTRYPOINT configures a container that will run as an executable.
- VOLUME creates a mount point for externally mounted volumes or other containers.
- USER sets the user name for following RUN / CMD / ENTRYPOINT commands.
- WORKDIR sets the working directory.
- ARG defines a build-time variable.
- ONBUILD adds a trigger instruction when the image is used as the base for another build.
- STOPSIGNAL sets the system call signal that will be sent to the container to exit.
LABEL apply key/value metadata to your images, containers, or daemons.
SHELL override default shell is used by docker to run commands.
HEALTHCHECK tells docker how to test a container to check that it is still working.

### Dockerfile example
```bash
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Define environment variable
ENV BASEURL

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Run app.py when the container launches
CMD ["python", "app.py"]
```

### Dockerfile example
```bash
# Use the official Node.js 20 image based on Alpine Linux
FROM node:20-alpine

# Create a group named 'app' and a system user named 'app' that belongs to the 'app' group
RUN addgroup app && adduser -S -G app app

# Set the working directory to /app
WORKDIR /app

# Copy the package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install the project dependencies as root user
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Change the ownership of all files in the working directory to the 'app' user and group
RUN chown -R app:app /app

# Switch to the 'app' user
USER app

# Expose port 5173 for the application
EXPOSE 5173

# Run the application using the 'npm run dev' command
CMD ["npm", "run", "dev"]
```

### .dockerignore
```bash
node_modules/
.git/
npm-debug.log
```

## docker-compose.yaml
```yaml
version: '3'
services:
  mongodb:
    image: mongo
    ports:
      - '27017:27017'
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
  mongo-express:
    image: mongo-express
    ports:
      - '8081:8081'
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
```
