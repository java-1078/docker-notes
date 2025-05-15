# Docker Container Setup for Microservices

This document explains how to set up a Docker-based environment for microservices, including creating containers for services like 
**Postgres**, **Config Server**, **Eureka Server**, and **Fund Transfer Service**. 
It covers the steps required for building Docker images, running containers, networking, and testing connections.

### Search & Pull The Postgres Image:
- Search on Docker Hub (just Google the image name + "Postgres docker image") and find the image tags.
- Example command to pull an image:
  ```bash
  docker pull postgres:17.5-alpine
- This command fetches the specified version of the Postgres image (e.g., 17.5-alpine).

### Check Available Images:  
- Use the command:  
  ```bash
  docker images
- This will display the images available on your system.

### Creating a Docker Network
To allow communication between containers, they should be on the same Docker network.
- Use the command:
  ```bash
  docker network create --driver bridge my-network

### Running Postgres Container
While setting up the Postgres container, we also need to provide user credentials (username, password, and database name) using environment variables.
- Use the command:
  ```bash
  docker run --name postgres-container -e POSTGRES_USER=sudheer -e POSTGRES_PASSWORD=my-pass -e POSTGRES_DB=banking-service --network my-network -d -p 5433:5432 postgres:17.5-alpine

#### Explanation of the Command

- `--name postgres-container`: Names the container.
  
- `-e POSTGRES_USER=sudheer`: Sets the Postgres username.
  
- `-e POSTGRES_PASSWORD=my-pass`: Sets the Postgres password.
  
- `-e POSTGRES_DB=banking-service`: Defines the database name.
  
- `--network my-network`: Connects the container to the `my-network` Docker network.
  
- `-d`: Runs the container in detached mode (in the background).
  
- `-p 5433:5432`: Maps the docker server/host port as `5433` to the container's port `5432`.
