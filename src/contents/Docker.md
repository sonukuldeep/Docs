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

![image](https://www.ibexa.co/var/site/storage/images/_aliases/ibexa_content_full/2/2/3/0/70322-1-eng-GB/IMPORT_OlVlfvWhat-is-Docker-and-why-should-I-care.png)

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


## Docker compose file 
A simple `docker-compose.yml` file

```yaml
version: '3.8'

services:
  web:
    image: nginx
    container_name: my_nginx
    ports:
      - "8080:80"
    networks:
      - front-end
    volumes:
      - ./html:/usr/share/nginx/html

  db:
    image: postgres
    container_name: my_postgres
    environment:
      POSTGRES_USER: example
      POSTGRES_PASSWORD: example_password
    networks:
      - back-end
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:

networks:
  front-end:
  back-end:
```

### **Breaking Down the `docker-compose.yml` File:**

#### 1. **version: '3.8'**
   - This specifies the version of the Docker Compose file format you are using. Each version comes with specific features and functionality.
   - In this case, version **3.8** is used, which is commonly supported in modern Docker installations. The higher the version, the more features it provides, but you need to ensure compatibility with your Docker version.

#### 2. **services:**
   - This section defines the containers (services) that Docker Compose will create and manage.
   - Each service corresponds to a container and its associated configuration.

   ##### **web (Service 1)**
   - **image: nginx**: Specifies the Docker image to use for the container. In this case, the container will run the official `nginx` image.
   - **container_name: my_nginx**: Defines the name of the container. This is the name that Docker will use to refer to this container.
   - **ports:**
     ```yaml
     - "8080:80"
     ```
     - This maps port 80 of the container (where NGINX listens) to port 8080 on the host machine. So, you can access the web server by going to `http://localhost:8080` on the host.
   - **networks:**
     ```yaml
     - front-end
     ```
     - This service will be attached to the `front-end` network. You can specify one or more networks here. It allows containers to communicate with each other across different networks.
   - **volumes:**
     ```yaml
     - ./html:/usr/share/nginx/html
     ```
     - This mounts a directory from the host (`./html`) to a directory in the container (`/usr/share/nginx/html`). This allows you to serve custom HTML files from the host machine inside the NGINX container.

   ##### **db (Service 2)**
   - **image: postgres**: This specifies the use of the official `postgres` image for the database container.
   - **container_name: my_postgres**: Name of the database container.
   - **environment:**
     ```yaml
     POSTGRES_USER: example
     POSTGRES_PASSWORD: example_password
     ```
     - These environment variables are used to configure the PostgreSQL database. In this case, it defines the database user (`example`) and the password (`example_password`).
   - **networks:**
     ```yaml
     - back-end
     ```
     - This service is attached to the `back-end` network. This network will be isolated from the `front-end` network, meaning the web container won’t be able to directly communicate with the database unless explicitly configured.
   - **volumes:**
     ```yaml
     - db_data:/var/lib/postgresql/data
     ```
     - This creates a volume (which will persist data even if the container is stopped or deleted) and mounts it to the PostgreSQL data directory (`/var/lib/postgresql/data`), which ensures the database data is persisted.

#### 3. **volumes:**
   - This section defines named volumes used by the services.
     ```yaml
     db_data:
     ```
     - **db_data**: This is a named volume for the database service. Named volumes are managed by Docker and allow data to persist even if the container is removed. It stores the PostgreSQL data to ensure it's not lost when the container is removed.

#### 4. **networks:**
   - This section defines the networks that services can attach to. It helps with networking isolation and routing between services.
     ```yaml
     front-end:
     back-end:
     ```
   - **front-end** and **back-end**: These are custom networks. The web service is connected to the `front-end` network, while the database service is connected to the `back-end` network. This ensures that containers on one network (e.g., the front-end) cannot directly communicate with containers on another network (e.g., the back-end) unless explicitly connected.

   Docker Compose automatically creates these networks unless you specify an existing external network. By default, if no networks are specified, Docker Compose will create a default network for all services.

---

### **Additional Options in `docker-compose.yml`**

#### **1. `build`**
   Instead of pulling an image from Docker Hub, you can specify a `build` context. This is useful when you want to build an image from a `Dockerfile` located in the specified directory.

   Example:
   ```yaml
   web:
     build: ./web-directory
     ports:
       - "8080:80"
   ```

   This would build the Docker image from a `Dockerfile` in the `./web-directory`.

#### **2. `depends_on`**
   The `depends_on` directive controls the order in which services start. For example, if you want the database container to start before the web container, you can specify:

   ```yaml
   web:
     image: nginx
     depends_on:
       - db
   ```

   Note that `depends_on` **does not wait for the database to be fully ready** (just for it to be started). If you need to ensure that a service is ready, you'll need additional handling (like using health checks).

#### **3. `restart`**
   The `restart` policy controls whether a container should be automatically restarted if it crashes or is stopped.

   Example:
   ```yaml
   web:
     image: nginx
     restart: always
   ```

   - `always`: Restart the container unless explicitly stopped.
   - `on-failure`: Restart the container only when it exits with a non-zero status.
   - `unless-stopped`: Restart the container unless it is manually stopped.

#### **4. `environment` (Environment Variables)**
   - The `environment` section is used to define environment variables inside the container.
   - For example:
     ```yaml
     db:
       image: postgres
       environment:
         POSTGRES_USER: myuser
         POSTGRES_PASSWORD: mypassword
     ```

---

### **Summary**

A **`docker-compose.yml`** file allows you to define and manage multi-container Docker applications. Here's a recap of the key components:

- **`version`**: Defines the Compose file format version.
- **`services`**: Defines the containers that will run, their configurations (image, environment, ports, etc.).
- **`networks`**: Defines custom networks for isolating and routing traffic between containers.
- **`volumes`**: Defines persistent data storage that can be shared or retained across container restarts.
  
Docker Compose makes it easier to manage complex applications that involve multiple services, each with its own settings and configurations, by using a simple declarative YAML format.


A **Dockerfile** is a script containing a series of instructions on how to build a Docker image. It automates the process of creating a containerized application by defining the steps to install software, set up environment variables, configure the application, and define container behavior.

Each line in a Dockerfile represents a command or instruction to be executed in the image-building process.

### **Basic Structure of a Dockerfile**

Here’s a basic template of a Dockerfile:

```Dockerfile
# 1. Specify the base image
FROM ubuntu:20.04

# 2. Set environment variables (optional)
ENV APP_HOME /app

# 3. Install dependencies
RUN apt-get update && apt-get install -y \
    curl \
    git \
    python3

# 4. Set the working directory in the container
WORKDIR /app

# 5. Copy files from the host machine into the container
COPY . .

# 6. Expose a port for communication
EXPOSE 80

# 7. Define the command to run when the container starts
CMD ["python3", "app.py"]
```

### **Dockerfile Instructions**

#### 1. **FROM**
   - The `FROM` instruction sets the base image for the container. All Docker images are built upon a base image, such as a minimal OS or language-specific image (e.g., Python, Node.js).
   - Example: 
     ```Dockerfile
     FROM node:14
     ```

#### 2. **ENV**
   - The `ENV` instruction sets environment variables inside the container, which can be accessed by the application or any other processes running in the container.
   - Example:
     ```Dockerfile
     ENV APP_HOME /usr/src/app
     ```

#### 3. **RUN**
   - The `RUN` instruction executes commands inside the container during the build process, such as installing dependencies or setting up the environment.
   - It can be used to install software, update packages, etc.
   - Example:
     ```Dockerfile
     RUN apt-get update && apt-get install -y python3
     ```

#### 4. **WORKDIR**
   - The `WORKDIR` instruction sets the working directory for subsequent instructions (like `COPY`, `RUN`, etc.).
   - If the directory doesn't exist, Docker will create it.
   - Example:
     ```Dockerfile
     WORKDIR /app
     ```

#### 5. **COPY**
   - The `COPY` instruction copies files or directories from the host system into the container’s filesystem.
   - Syntax: `COPY <src> <dest>`
   - Example:
     ```Dockerfile
     COPY . /app
     ```

#### 6. **ADD**
   - Similar to `COPY`, but `ADD` has extra features, such as automatically extracting tar files and supporting remote URLs as the source.
   - However, it’s generally recommended to use `COPY` unless you specifically need the features of `ADD`.
   - Example:
     ```Dockerfile
     ADD my_archive.tar.gz /app
     ```

#### 7. **EXPOSE**
   - The `EXPOSE` instruction informs Docker that the container listens on specific network ports at runtime.
   - Example:
     ```Dockerfile
     EXPOSE 8080
     ```

#### 8. **CMD**
   - The `CMD` instruction defines the default command to run when a container starts.
   - There can only be one `CMD` instruction in a Dockerfile. If there are multiple `CMD` instructions, only the last one will be used.
   - Example:
     ```Dockerfile
     CMD ["node", "app.js"]
     ```

#### 9. **ENTRYPOINT**
   - The `ENTRYPOINT` instruction is similar to `CMD` but is used to define the main command that will run when the container starts. 
   - It provides a way to set up a container that behaves like an executable, and you can combine it with `CMD` to specify default arguments.
   - Example:
     ```Dockerfile
     ENTRYPOINT ["python3"]
     CMD ["app.py"]
     ```

#### 10. **VOLUME**
   - The `VOLUME` instruction creates a mount point with the specified path in the container, which can be used for persistent data storage.
   - Example:
     ```Dockerfile
     VOLUME ["/data"]
     ```

#### 11. **USER**
   - The `USER` instruction sets the user name or UID (user identifier) and optionally the group name or GID (group identifier) to use when running the container.
   - Example:
     ```Dockerfile
     USER myuser
     ```

#### 12. **ARG**
   - The `ARG` instruction defines build-time variables. They can be passed to Docker at the time of building with the `--build-arg` flag.
   - Example:
     ```Dockerfile
     ARG VERSION=1.0
     ```

#### 13. **LABEL**
   - The `LABEL` instruction adds metadata to an image in the form of key-value pairs. This can be helpful for documentation, organizing images, or adding versioning information.
   - Example:
     ```Dockerfile
     LABEL version="1.0" description="My awesome application"
     ```

---

### **Example of a Full Dockerfile**

Let’s look at a more complete Dockerfile for a Node.js application.

```Dockerfile
# 1. Use an official Node.js runtime as the base image
FROM node:20

# 2. Set the working directory inside the container
WORKDIR /app

# 3. Copy package.json and install dependencies
COPY package.json /app
RUN npm install

# 4. Copy the rest of the application code
COPY . /app

# 5. Expose the port that the app will run on
EXPOSE 3000

# 6. Define the command to start the app
CMD ["npm", "start"]
```

This Dockerfile does the following:
1. Starts with a Node.js base image (`node:14`).
2. Sets the working directory to `/app`.
3. Copies `package.json` to the container and runs `npm install` to install dependencies.
4. Copies the rest of the application code to the container.
5. Exposes port 3000, which is the port the Node.js app listens on.
6. Defines the default command to run `npm start` when the container is started.

### **Building an Image Using the Dockerfile**

Once you have your `Dockerfile` ready, you can build a Docker image from it by running the following command in the same directory as your `Dockerfile`:

```bash
docker build -t my-node-app .
```

- The `-t` flag tags the image with a name (`my-node-app` in this case).
- The `.` refers to the current directory (the build context).

### **Running the Image**

Once the image is built, you can run it with the following command:

```bash
docker run -d -p 3000:3000 --name node-container my-node-app
```

- This runs the `my-node-app` image in detached mode (`-d`) and maps port 3000 from the container to port 3000 on the host.
- It also names the container `node-container`.

### **Multi-Stage Builds (Advanced)**

For more complex applications, you may want to reduce the size of your image by using **multi-stage builds**. With multi-stage builds, you can use one image to build your application and another for the final image.

Here’s an example of a multi-stage Dockerfile:

```Dockerfile
# Stage 1: Build the application
FROM node:20 AS builder

WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app

# Stage 2: Create the production image
FROM node:20-slim

WORKDIR /app
COPY --from=builder /app /app

EXPOSE 3000
CMD ["npm", "start"]
```

- In this case, the first stage (`builder`) installs dependencies and copies the app files.
- The second stage copies only the necessary files from the build stage (`COPY --from=builder`) and creates a slimmer production image.

### **Summary**

- **Dockerfile** is a script to build Docker images.
- It consists of various instructions (`FROM`, `RUN`, `CMD`, `COPY`, etc.) that guide Docker in setting up the container environment.
- Dockerfiles allow you to automate container creation and maintain consistency across different environments.



## Docker Advanced
### **Overlay Networks in Docker**

An **overlay network** in Docker is a virtual network that allows containers to communicate with each other across multiple Docker hosts (i.e., across different physical or virtual machines) as if they were on the same network. This is especially useful in multi-host Docker setups, such as in Docker Swarm or Kubernetes, where containers on different hosts need to communicate securely and seamlessly.

#### **How Overlay Networks Work**
An overlay network abstracts the underlying physical network and enables communication between containers that are deployed across multiple Docker hosts. Docker uses a **VXLAN (Virtual Extensible LAN)** technology to create this overlay network. VXLAN encapsulates Ethernet frames inside UDP packets, which are then transmitted over the physical network.

Overlay networks make it possible for Docker to simulate a single, unified network layer that spans multiple hosts, allowing containers to talk to each other as if they are all in the same local network, even though they might be distributed across different physical machines.

### **Key Concepts of Overlay Networks**

1. **VXLAN**:
   - VXLAN is the encapsulation technology used in overlay networks. It allows the creation of Layer 2 networks over a Layer 3 infrastructure (like the internet or any IP-based network).
   - VXLAN uses a unique **VXLAN Network Identifier (VNI)** for each network, making it possible to create isolated logical networks, even if they share the same underlying physical network.

2. **Docker Swarm and Overlay Networks**:
   - Overlay networks are **crucial in Docker Swarm** because they provide communication between services across different Swarm nodes.
   - When you deploy services in a Swarm cluster, they are automatically connected to the **default overlay network** (`ingress`), but you can also create custom overlay networks for more control.

3. **Control Plane and Data Plane**:
   - **Control Plane**: Docker manages the overlay network’s configuration and coordination through a control plane (using a key-value store, such as Consul or etcd).
   - **Data Plane**: Once the overlay network is configured, containers can communicate with each other across Docker hosts via the data plane, where VXLAN packets are exchanged.

4. **Routing Between Hosts**:
   - When a container on one Docker host sends a packet to a container on a different host in an overlay network, Docker encapsulates the packet in a VXLAN header and sends it across the underlying network to the destination host.
   - The destination host decapsulates the packet, delivering it to the correct container on the target machine.

### **Types of Overlay Networks in Docker**

1. **Default Overlay Network (`ingress`)**:
   - The **`ingress`** network is created automatically when you initialize a Docker Swarm.
   - This network is used for **service discovery** and communication between containers on different nodes in the Swarm, primarily for managing service ports and load balancing.
   - It is not intended for direct container-to-container communication but rather for communication involving services exposed via ports.

2. **User-defined Overlay Networks**:
   - You can create custom overlay networks for containers to communicate securely and isolate traffic.
   - Custom networks allow for finer control, such as specifying which services can talk to each other and ensuring that traffic stays isolated within the network.

   **Example**:
   ```bash
   docker network create --driver overlay my_overlay_network
   ```

   This creates a custom overlay network named `my_overlay_network`.

### **Advantages of Overlay Networks**

1. **Multi-Host Communication**:
   - The primary benefit of overlay networks is enabling containers to communicate across multiple hosts in a Docker Swarm or Kubernetes cluster, as if they are on the same local network.

2. **Isolation**:
   - Overlay networks can provide **network isolation**. Each network is isolated, so containers connected to one network cannot directly communicate with containers on other networks unless explicitly configured.

3. **Security**:
   - Overlay networks can be secured using **TLS encryption** for traffic between Docker hosts. This is particularly important when deploying containers in a distributed system where traffic traverses public or untrusted networks.

4. **Service Discovery**:
   - Docker provides **built-in service discovery** for containers connected to the same overlay network. Containers can communicate using their **service names** instead of IP addresses, making it easier to manage container communication.

5. **Scalability**:
   - Overlay networks enable containers to scale across multiple hosts. As your application grows, you can easily scale services and containers across multiple nodes in the cluster without having to reconfigure the network.

### **Setting Up an Overlay Network**

1. **Creating an Overlay Network**:
   To create a custom overlay network, you need to be running Docker Swarm (multi-node Docker setup). If Docker Swarm is not initialized, you'll need to run:
   
   ```bash
   docker swarm init
   ```

   Then, create the overlay network:
   ```bash
   docker network create --driver overlay --attachable my_overlay_network
   ```

   - **`--driver overlay`**: Tells Docker to use the overlay driver.
   - **`--attachable`**: Allows standalone containers to attach to the overlay network (useful if you're not just using Swarm services but also regular containers).

2. **Deploying Containers on the Overlay Network**:
   Once you’ve created an overlay network, you can deploy containers to it, either by using **Docker Swarm services** or by running standalone containers.

   **With Docker Swarm (using a service)**:
   ```bash
   docker service create --name my_service --replicas 3 --network my_overlay_network my_image
   ```

   **With Standalone Containers**:
   ```bash
   docker run -d --name my_container --network my_overlay_network my_image
   ```

3. **Service Discovery on Overlay Networks**:
   Docker supports **DNS-based service discovery** within the overlay network. For example, if you have a service named `my_service` running on the overlay network, other containers can refer to it by the service name `my_service`, and Docker will automatically resolve the correct IP address.

4. **Inspecting an Overlay Network**:
   To inspect the details of the overlay network, including connected containers and its configuration, use:
   ```bash
   docker network inspect my_overlay_network
   ```

### **Example Scenario: Deploying Multiple Containers Across Hosts**

1. **Initialize Docker Swarm (on both machines)**:
   On the first node:
   ```bash
   docker swarm init
   ```
   On the second node:
   ```bash
   docker swarm join --token <join_token> <manager_ip>:2377
   ```

2. **Create an Overlay Network**:
   On the manager node:
   ```bash
   docker network create --driver overlay my_overlay_network
   ```

3. **Deploy a Service Across Multiple Hosts**:
   On the manager node:
   ```bash
   docker service create --name web --replicas 2 --network my_overlay_network nginx
   ```

4. **Verify the Deployment**:
   Use the following to check the running services:
   ```bash
   docker service ls
   ```

5. **Inspect the Overlay Network**:
   To check which containers are connected to the overlay network:
   ```bash
   docker network inspect my_overlay_network
   ```

### **Use Cases for Overlay Networks**

1. **Microservices Architecture**:
   - In a microservices setup, different services may need to communicate over a network. Using overlay networks, services running on different hosts can communicate seamlessly.

2. **Distributed Applications**:
   - For applications that span multiple Docker hosts, overlay networks allow components running on different hosts to communicate as if they were part of the same local network.

3. **Isolated Network Segments**:
   - You may want to isolate traffic between services (e.g., between front-end and back-end services) using different overlay networks for security and performance reasons.

### **Summary**

- **Overlay networks** in Docker provide a way for containers to communicate across different hosts, creating a virtual network that spans multiple Docker hosts.
- **VXLAN** encapsulation is used to create these networks, and **Docker Swarm** or Kubernetes typically manages them for service discovery and multi-host communication.
- Overlay networks are essential in modern distributed applications and microservices architectures, providing security, isolation, scalability, and ease of container communication across multiple hosts.


