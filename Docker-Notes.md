# Docker Container Setup for Microservices

This document explains how to set up a Docker-based environment for microservices, including creating containers for services like 
**Postgres**, **Config Server**, **Eureka Server**, and **Fund Transfer Service**. 
It covers the steps required for building Docker images, running containers, networking, and testing connections.

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

### Postgres:
#### Search & Pull The Postgres Image:
- Search on Docker Hub (just Google the image name + "Postgres docker image") and find the image tags.
- Example command to pull an image:
  ```bash
  docker pull postgres:17.5-alpine
- This command fetches the specified version of the Postgres image (e.g., 17.5-alpine).
#### Running Postgres Container
While setting up the Postgres container, we also need to provide user credentials (username, password, and database name) using environment variables.
- Use the command:
  ```bash
  docker run --name postgres-container -e POSTGRES_USER=sudheer -e POSTGRES_PASSWORD=my-pass -e POSTGRES_DB=banking-service --network my-network -d -p 5433:5432 postgres:17.5-alpine
##### Explanation of the Command
- `--name postgres-container`: Names the container.
- `-e POSTGRES_USER=sudheer`: Sets the Postgres username.
- `-e POSTGRES_PASSWORD=my-pass`: Sets the Postgres password.
- `-e POSTGRES_DB=banking-service`: Defines the database name.
- `--network my-network`: Connects the container to the `my-network` Docker network.
- `-d`: Runs the container in detached mode (in the background).
- `-p 5433:5432`: Maps the docker server/host port as `5433` to the container's port `5432`.
###### The Postgres container is up and running. What’s the next step?
Now that the Postgres container is successfully created, we can verify the connection using DBeaver, which we are already using. 
To do this, follow these steps in DBeaver:
1. Open DBeaver and, in the left panel, right-click on the blank space and select **Create** → **Connection** → **PostgreSQL**.
###### Connection Settings:
- **Port Number**: Set the port number to `5433`, which is the port we specified for the container.
- **Username and Password**: Use the same username and password that were set during the container creation process.
Once all details are entered, click **Test Connection**. DBeaver will check the connection, and it will show the connection status.

### Build the image and run the container

#### Repeat the Build Image and Run Container Steps for the Following Microservices:
- **Config Server**
- **Eureka Service Registry**
- **Fund Transfer Service**

#### Build Images:
This step is optional. If your local development environment already has the required images, you can skip this step. However, if you need to work with the latest code/images, please follow the steps below to generate the Docker images.


- To start working with the project, you first need to clone the repository from GitHub. Use the following command to clone the repository to your local machine:
    ```bash
    git clone <repository-url>
    ```
    ```bash
    git clone https://github.com/java-1078/eureka-service-registry.git
    ```
- Once the repository is cloned, navigate into the project directory:
    ```bash
    cd <project-directory>
     ```
    ```bash
    cd eureka-service-registry
    ```
- Build the Project with Gradle, build the project and generate the `.jar` file, you need to run the following command using Gradle:
    ```bash
    ./gradle clean build
    ```
- Build the docker image, To build a Docker image for the service, use the following command:
    ```bash
    docker build -t <image_name>:<version> .
    ```
    ```bash
    docker build -t eureka-service-registry:1.0.0 .
    ```
    - docker build: This command is used to build a Docker image from a Dockerfile in the specified directory.
    - The `-t` flag tags the image with a name and version. Here, the image will be named `eureka-service-registry` with the version `1.0.0`.
    - The period `.` refers to the current directory where the Dockerfile is located. It tells Docker to use the Dockerfile in the current directory to build the image.


#### Running the Container
To run the container, use the following Docker command:
```bash
docker run --network <network_name> --name <container_name> -p <docker_server_port>:<container_port> -d <image_name>:<image_version>
```

```bash
docker run --network my-network --name eureka-service-registry -p 8761:8761 -d eureka-service-registry:1.0.0
```
##### Explanation of the `docker run` Command

- **`docker run`**:  
  This command creates and starts a container from a specified Docker image.

- **`--network <network_name>`**:  
  This option connects the container to a specific Docker network, allowing it to communicate with other containers on the same network.

- **`--name <container_name>`**:  
  This assigns a name to the container. Replace `<container_name>` with a desired name for your container.

- **`-p <docker_server_port>:<container_port>`**:  
  This maps a port on your local machine (`docker_server_port`) to a port inside the container (`container_port`). It allows you to access the container's services through the specified local port.

- **`-d`**:  
  This flag runs the container in detached mode, meaning it will run in the background.

- **`<image_name>:<image_version>`**:  
  This specifies the Docker image and version tag that you want to use for the container.

### Testing the Containers

After successfully creating the containers, you can test the services using Postman.

- The URL for the **Fund Transfer Service**: 
 [http://localhost:<docker_server_port>/api/v1/transfer](http://localhost:8080/api/v1/transfer)

- You can also verify the **Eureka Service Registry**: 
[http://localhost:<docker_server_port>](http://localhost:8761)
