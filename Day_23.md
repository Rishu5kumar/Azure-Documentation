# Day 23: Monolithic vs. Microservices Architecture & Docker Basics

## Table of Contents
1. [Monolithic Architecture](#monolithic-architecture)
2. [Microservices Architecture](#microservices-architecture)
3. [Docker Basics](#docker-basics)
   - [Docker Overview](#docker-overview)
   - [Docker Volume](#docker-volume)
   - [Azure Container Registry (ACR)](#azure-container-registry-acr)
   - [Dockerfile](#dockerfile)
4. [Docker Components](#docker-components)
5. [How It Works](#how-it-works)
6. [Examples and Usage](#examples-and-usage)
   - [Mapping a Volume with Two Containers](#mapping-a-volume-with-two-containers)
   - [Creating and Using Two Volumes with One Container](#creating-and-using-two-volumes-with-one-container)
   - [Mapping a Volume to a Container from Blob Storage](#mapping-a-volume-to-a-container-from-blob-storage)
   - [Dockerfile Example](#dockerfile-example)
7. [Additional Tips](#additional-tips)
8. [venv vs container](#venv-vs-container)

---
## Monolithic Architecture

### Definition
A monolithic architecture is a single, unified application where all components (UI, business logic, data access) are tightly integrated.

### Characteristics
- **Single Codebase:** All functionality in one codebase.
- **Tightly Coupled:** Components depend on each other.
- **Single Deployment:** Entire application is deployed at once.
- **Scaling:** Difficult to scale individual parts; scale entire app.
- **Flexibility:** Harder to adopt new technologies.

### Advantages
- Simpler to develop and deploy initially.
- Better performance due to in-process communication.

### Disadvantages
- Harder to scale and maintain.
- Changes require redeploying the entire app.

## Microservices Architecture

### Definition
A microservices architecture involves splitting an application into small, independent services, each handling a specific business function.

### Characteristics
- **Multiple Codebases:** Each service has its own codebase.
- **Loosely Coupled:** Services interact via APIs.
- **Independent Deployment:** Each service can be deployed separately.
- **Scaling:** Services can be scaled independently.
- **Flexibility:** Different technologies can be used for different services.

### Advantages
- Easier to scale and maintain.
- Faster deployment and flexibility in technology.

### Disadvantages
- Increased complexity in managing services.
- Requires robust infrastructure for communication and monitoring.
---
## Docker Basics

### Docker Overview
Docker is a platform that uses containerization to run applications in isolated environments called containers. Containers are lightweight and share the host operating system.

### Docker Volume
- **What It Is:** A Docker Volume is a way to store data for your Docker containers. It's a storage area on your computer for data accessible by containers.
- **Why Use It:** Containers are temporary; volumes ensure important data persists.
- **How It Works:** Creates a directory on your host machine or cloud storage that containers can access.

**Example:** 
- **Container (Toy Box):** A Docker container is like a toy box where you keep your toys.
- **Volume (Toy Storage Drawer):** A Docker Volume is like a drawer for storing important toys.

### Azure Container Registry (ACR)
- **Private Registry:** ACR is a private repository for Docker container images and Helm charts.
- **Integration:** Seamlessly integrates with Azure services like Azure Kubernetes Service (AKS) and Azure DevOps.
- **Security:** Includes authentication, access control, and encryption.
- **Scalability:** Automatically scales to handle increased demand.

### Dockerfile
- **What It Is:** A Dockerfile is a text file with instructions on how to build a Docker image, like a recipe card for creating a container.
- **Why Use It:** Automates the process of creating Docker images.

**Basic Structure:**
1. **Start with a Base Image:** `FROM ubuntu:20.04`
2. **Set the Maintainer (Optional):** `LABEL maintainer="your-email@example.com"`
3. **Update and Install Dependencies:** 
   ```dockerfile
   RUN apt-get update && \
       apt-get install -y \
       curl \
       vim \
       git
   ```
4. **Set Environment Variables (Optional):** `ENV APP_HOME /app`
5. **Copy Files into the Container:** `COPY . /app`
6. **Set the Working Directory:** `WORKDIR /app`
7. **Install Application Dependencies:** `RUN pip install -r requirements.txt`
8. **Expose Ports:** `EXPOSE 8080`
9. **Define the Command to Run the Application:** `CMD ["python", "app.py"]`
---
## Docker Components

### Docker Images
- **Definition:** A Docker image is a read-only template containing application code, libraries, dependencies, tools, and other files needed to run an application.
- **Purpose:** Images are used to create Docker containers. Built from a series of layers, each representing a step in the build process defined in a Dockerfile.
- **Example:** An image for a web application might include the operating system, a web server, the application code, and its dependencies.

### Docker Containers
- **Definition:** A container is a runnable instance of a Docker image. It includes everything needed to run the application, including code, runtime, system tools, libraries, and settings.
- **Purpose:** Containers run applications in isolated environments. Created from Docker images, containers can be started, stopped, moved, and deleted.
- **Example:** Running a container from a web application image provides an isolated instance of that application, with its own filesystem, process space, and network interfaces.
---
## How It Works

### Dockerfile
- **Purpose:** A Dockerfile is a text file containing a set of instructions to build a Docker image.
- **Example:**
    ```dockerfile
    FROM python:3.7.4
    COPY . /flaskapp
    WORKDIR /flaskapp
    RUN pip3 install -r requirements.txt
    EXPOSE 5000
    CMD ["python", "app.py"]
    ```

### Building an Image
- **Command:** The `docker build` command is used to create an image from a Dockerfile.
- **Example:**
    ```bash
    docker build -t my_flask_app .
    ```
- **Result:** This command reads the instructions in the Dockerfile and creates an image named `my_flask_app`.

### Running a Container
- **Command:** The `docker run` command is used to create and start a container from a Docker image.
- **Example:**
    ```bash
    docker run -d -p 5000:5000 my_flask_app
    ```
- **Result:** This command runs the `my_flask_app` image as a container, mapping port 5000 on the host to port 5000 in the container.

## Summary
- **Docker Image:** A template with the application and environment needed to run it.
- **Docker Container:** A live instance of an image that runs the application in an isolated environment.

---
## Examples and Usage

### Mapping a Volume with Two Containers
1. **Create the Volume:**
   ```bash
   docker volume create shared_volume
   ```
2. **Run the Containers and Mount the Volume:**
   ```bash
   docker run -d --name container1 -v shared_volume:/data busybox
   docker run -d --name container2 -v shared_volume:/data busybox
   ```

### Creating and Using Two Volumes with One Container
1. **Create the Volumes:**
   ```bash
   docker volume create volume1
   docker volume create volume2
   ```
2. **Run the Container with Multiple Volumes:**
   ```bash
   docker run -d --name my_container -v volume1:/data1 -v volume2:/data2 busybox
   ```

### Mapping a Volume to a Container from Blob Storage
1. **Create an Azure Blob Storage Container**
2. **Use Docker Volume Plugin for Azure Blob Storage**
   ```bash
   docker volume create --driver=azurefile --name azure_volume -o share_name=<share_name> -o storage_account_name=<storage_account_name> -o storage_account_key=<storage_account_key>
   ```
3. **Run the Container with the Azure Blob Storage Volume:**
   ```bash
   docker run -d --name my_container -v azure_volume:/data busybox
   ```

### Dockerfile Example
```dockerfile
# Use the official Python image as a base image
FROM python:3.9-slim

# Set the maintainer label
LABEL maintainer="your-email@example.com"

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```
---
## Additional Tips
- **Use .dockerignore File:** To exclude files from the Docker context.
- **Optimize Layers:** Minimize layers and commands to keep the image size small.
- **Multi-Stage Builds:** Use multi-stage builds to optimize the final image.

---
## venv vs container
### venv
- **Purpose:** Isolates Python dependencies for a specific project.
- **Scope:** Only Python packages and environment.
- **Use Case:** Manage different versions of Python libraries for different projects on the same machine.
- **Advantages:** Lightweight, easy to set up, no additional overhead besides Python libraries.

### Container
- **Purpose:** Encapsulates an entire runtime environment including application code, libraries, dependencies, and the OS kernel.
- **Scope:** Entire application stack and environment.
- **Use Case:** Ensures consistency across different environments (development, testing, production) and facilitates deployment.
- **Advantages:** Portable, consistent runtime, isolation from the host system, and can include any software stack.
