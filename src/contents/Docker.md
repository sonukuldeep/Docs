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
Docker is an open-source platform that enables developers to build, deploy, run, update and manage containers.

Containers are standardized, executable components that combine application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.

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
docker stop -t 30 <container_name>

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
docker exec -it <container_name> bash
docker exec <container_name> ls

# Create image from container
docker commit <container_name> (or <container-id>) <image_name>
docker commit -m "Added configuration changes" -t my_new_image:v1 my_container

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

## compose.yaml
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

## compose.yaml
compose with watch
```bash
version: "3"
services:
  web:
    build: .
    ports:
      - 3000:5173
    develop:
      watch:
        - path: ./package.json
          action: rebuild
        - path: ./package-lock.json
          action: rebuild
        - path: .
          target: /app
          action: sync
```

[Docker compose example](https://github.com/adrianhajdin/docker-course/blob/main/mern-docker/compose.yml)

## Volumes
Docker **volumes** are a way to persist data generated by and used by Docker containers. Unlike data stored in a container's filesystem, which is ephemeral and lost when the container is removed, volumes allow data to persist beyond the lifecycle of a container.

### **Why Use Docker Volumes?**
1. **Data Persistence:** Volumes retain data even if the container is deleted.
2. **Isolation from Container Filesystem:** They provide a managed and isolated storage location, separate from the container's filesystem.
3. **Improved Performance:** Volumes are optimized for I/O operations, especially on Docker for Linux.
4. **Data Sharing:** Multiple containers can access the same volume simultaneously, enabling data sharing.

### **Types of Docker Volumes**
1. **Anonymous Volumes:** Created automatically when you don't specify a name for the volume. They are less commonly used because they are difficult to reference after creation.
   ```bash
   docker run -v /data busybox
   ```
2. **Named Volumes:** Created explicitly by the user with a specific name. This is the most common type because they are easier to manage.
   ```bash
   docker volume create my_volume
   docker run -v my_volume:/data busybox
   ```
3. **Bind Mounts:** Allow you to map a directory from the host machine to the container. Useful when you want the container to have access to existing files on the host.
   ```bash
   docker run -v /host/path:/container/path busybox
   ```

## Network
In Docker, **networks** are used to manage how containers communicate with each other and with the outside world. Docker networking provides several built-in network drivers that enable different types of communication for containers based on your needs.

### **Why Use Docker Networks?**
1. **Isolated Communication:** Containers can communicate with each other without exposing services to the host machine or external networks.
2. **Service Discovery:** Containers on the same network can communicate using container names instead of IP addresses, simplifying connectivity in microservices.
3. **Security:** By using isolated networks, you can control access and limit communication between different containers.

### **Types of Docker Networks**
Docker provides several types of network drivers, each with its specific use cases:

#### 1. **Bridge Network** (default)
- The most common and default network type for standalone containers.
- Containers on the same bridge network can communicate using container names.
- Suitable for simple use cases or single-host setups.

**Create and use a bridge network:**
```bash
docker network create my_bridge
docker run -d --name container1 --network my_bridge nginx
docker run -d --name container2 --network my_bridge alpine sleep infinity
docker exec container2 ping container1  # Check connectivity
```

#### 2. **Host Network**
- The container shares the host's network stack, eliminating network isolation.
- Offers better performance since there's no network translation (no NAT), but lacks isolation.
- Typically used in scenarios requiring minimal latency, like monitoring tools.

**Run a container with host networking:**
```bash
docker run --network host -d nginx
```

#### 3. **Overlay Network**
- Enables communication between containers across multiple Docker hosts.
- Commonly used in Docker Swarm or Kubernetes for multi-host networking.
- Suitable for distributed systems and microservices architectures.

**Create an overlay network in a Swarm:**
```bash
docker network create -d overlay my_overlay
```

#### 4. **Macvlan Network**
- Assigns a MAC address to each container, making it appear as a physical device on the network.
- Useful for scenarios requiring containers to appear as separate devices on the network (e.g., legacy systems).

**Create a macvlan network:**
```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 my_macvlan
```

#### 5. **None Network**
- Completely disables networking for the container.
- Useful for isolating containers or testing environments where no networking is required.

**Run a container with no network:**
```bash
docker run --network none -d nginx
```

### **Docker Network Commands**
- **List all networks:**
  ```bash
  docker network ls
  ```
- **Inspect a network:**
  ```bash
  docker network inspect my_bridge
  ```
- **Create a new network:**
  ```bash
  docker network create my_network
  ```
- **Connect a container to a network:**
  ```bash
  docker network connect my_network my_container
  ```
- **Disconnect a container from a network:**
  ```bash
  docker network disconnect my_network my_container
  ```
- **Remove a network:**
  ```bash
  docker network rm my_network
  ```

### **Example: Networking with Docker Compose**
Docker Compose makes it easy to define and manage networks between services in a single file (`docker-compose.yml`):

```yaml
version: '3'
services:
  web:
    image: nginx
    networks:
      - my_network

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
    networks:
      - my_network

networks:
  my_network:
    driver: bridge
```

In this example, both `web` and `db` services share the `my_network`, allowing them to communicate using container names (`web` and `db`).

### **Use Cases**
- **Microservices Communication:** Use overlay or bridge networks for service-to-service communication.
- **Legacy Application Integration:** Use macvlan networks when containers need to appear as physical devices.
- **Performance Optimization:** Use host networks for minimal latency in high-performance applications.


