# Day 22: Docker Introduction and Basic Setup

## Table of Contents
1. [Introduction to Traditional Development Process](#introduction-to-traditional-development-process)
2. [Challenges in Traditional Development](#challenges-in-traditional-development)
3. [Traditional Solution: Virtualization](#traditional-solution-virtualization)
4. [Introduction to Docker](#introduction-to-docker)
5. [Why Docker Came About](#why-docker-came-about)
6. [Why Docker is Used](#why-docker-is-used)
7. [How Docker Works](#how-docker-works)
8. [Advantages of Docker](#advantages-of-docker)
9. [Traditional Virtualization vs Docker](#traditional-virtualization-vs-docker)
10. [Setting Up Docker on Azure](#setting-up-docker-on-azure)
11. [Basic Docker Commands](#basic-docker-commands)
12. [Example: Running a Web Server Inside a Docker Container](#example-running-a-web-server-inside-a-docker-container)
13. [Summary](#summary)
---

## Introduction to Traditional Development Process
1. **SRS (Software Requirements Specification)**
    - Document detailing the software requirements.
2. **Development**
    - Developers write the code based on the SRS.
3. **Version Control System (VCS)**
    - Code is stored and managed in a version control system like Git.
4. **Pull Request (PR)**
    - Developers submit PRs for code review and integration.
5. **Build**
    - The code is compiled and built into an executable.
6. **Testing**
    - The build undergoes various testing stages (unit, integration, system testing).

## Challenges in Traditional Development
### Dependency Issues
1. **Environment Discrepancies**
    - An application passes testing by one team (Team X) but fails when tested by another team (Team Y) due to differences in dependencies and configurations.
2. **Resource Allocation Problems**
    - Applications often have varying resource requirements. For example, one application might work optimally with 1 GB of RAM, while another might need more than 16 GB. Traditional VMs allocate fixed resources, and unused resources cannot be shared or reallocated dynamically.

## Traditional Solution: Virtualization
1. **Big Hardware and OS Setup**
    - Set up physical hardware with an operating system.
2. **Hypervisor**
    - A hypervisor is used to create and manage multiple VMs on the same physical machine.
3. **Creating VMs and Images**
    - Each VM requires its own OS and resources. Even if an application uses only a fraction of the allocated resources, the rest cannot be dynamically reassigned to other VMs.

---

## Introduction to Docker
Docker addresses many of the limitations of traditional virtualization by providing a more efficient, lightweight, and flexible approach to application deployment and management.

**What Docker is:**
Docker is an open-source platform that enables us to create, deploy, and manage applications inside lightweight and portable containers.

### Key Components:
- **Open-Source Platform**
    - Docker is an open-source platform that allows developers to create, deploy, and manage applications inside containers.
- **Docker Engine**
    - The Docker Engine is the core software that runs on the host OS, enabling the creation and management of containers.

## Why Docker Came About
1. **Consistent Environments**
    - Docker ensures that applications run the same way regardless of where they are deployed (development, testing, production).
2. **Simplify Deployment**
    - Containers can be easily deployed and scaled across different environments without compatibility issues.
3. **Isolation**
    - Containers provide process and resource isolation, making applications more secure and manageable.

## Why Docker is Used
1. **Portability**
    - Containers can run on any system that supports Docker, ensuring consistent performance across different environments.
2. **Efficiency**
    - Containers share the host system's kernel, making them lightweight and faster to start compared to traditional virtual machines.
3. **Scalability**
    - Applications can be easily scaled by adding or removing containers, supporting microservices architecture.

## How Docker Works
1. **Lightweight Containers**
    - Containers are lightweight and share the host OS kernel, unlike VMs that require separate OS instances.
2. **Portable Images**
    - Docker images are lightweight snapshots of applications, including all dependencies and configurations.
3. **Resource Management**
    - Containers can be easily started, stopped, and scaled, allowing dynamic allocation of resources.
4. **Isolation**
    - Each container runs in its own isolated environment, ensuring consistency and security across different environments.

## Advantages of Docker
1. **Consistency**
    - Ensures that applications run the same way in development, testing, and production environments.
2. **Portability**
    - Containers can run on any system with Docker installed.
3. **Isolation**
    - Containers are isolated from each other and from the host system, enhancing security and avoiding conflicts between applications.
4. **Scalability**
    - Containers can be easily scaled up or down based on demand.
5. **Resource Efficiency**
    - Containers share the host OS kernel, making them much more lightweight and faster to start.
6. **Speed**
    - Containers start much faster than virtual machines.
7. **DevOps Integration**
    - Docker integrates well with CI/CD tools, facilitating automated testing and deployment.

## Traditional Virtualization vs Docker
### Traditional Virtualization
```
+-------------------------------------+
|           Physical Hardware         |
+-------------------------------------+
|         Host Operating System       |
+-------------------------------------+
|             Hypervisor              |
+-------------------------------------+
|       VM1        |       VM2        |
| +--------------+ | +--------------+ |
| | Guest OS     | | | Guest OS     | |
| | Application  | | | Application  | |
| +--------------+ | +--------------+ |
+-------------------------------------+
```
### Docker Containers
```
+-------------------------------------+
|          Physical Hardware          |
+-------------------------------------+
|        Host Operating System        |
+-------------------------------------+
|            Docker Engine            |
+-------------------------------------+
|    Container 1   |    Container 2   |
| +--------------+ | +--------------+ |
| | Application  | | | Application  | |
| | Dependencies | | | Dependencies | |
| +--------------+ | +--------------+ |
+-------------------------------------+
```
---

## Setting Up Docker on Azure
Setting up Docker on Azure involves creating a virtual machine (VM) and then installing Docker on it. Here's a step-by-step guide to achieve this using the Azure portal:

### Step 1: Create a Virtual Machine
1. **Sign in to Azure Portal**
    - Go to [Azure Portal](https://portal.azure.com) and log in with your credentials.
2. **Create a Resource**
    - Click on **Create a resource** in the left-hand menu.
3. **Create a Virtual Machine**
    - Select **Allow selected ports** and choose SSH (22).

### Step 2: Connect to the Virtual Machine
1. **Go to the Virtual Machine**
    - Once the VM is created, go to the **Virtual Machines** section in the Azure Portal.
    - Select your newly created VM.
2. **Connect to the VM**
    - Click on **Connect** and choose **SSH**.
    - Follow the instructions to connect to your VM using SSH.

### Step 3: Install Docker on the Virtual Machine
1. **Update Packages and Install Docker**
    - Once connected to the VM via SSH, update your package index:
      ```bash
      sudo apt update && sudo apt install docker.io -y
      ```
2. **Verify Docker Installation**
    - Check if Docker is installed correctly by running:
      ```bash
      docker --version
      sudo systemctl start docker
      sudo systemctl enable docker
      sudo docker run hello-world  # optional
      ```

---

## Basic Docker Commands
1. **Check Docker Containers and Images**
    ```bash
    docker container ls
    docker image ls
    docker container ls -a  # Shows all containers, including stopped ones
    ```

2. **Run a Docker Container**
    ```bash
    docker container run ubuntu
    docker image ls -a
    docker container run ubuntu sleep 60  # Locks the terminal for 60 seconds
    ```

3. **Run Docker Container in Detached Mode**
    ```bash
    docker container run -d ubuntu sleep 60  # Runs in the background
    docker container ls -a
    docker container run -d ubuntu sleep 100  # Stops after 100 seconds
    ```

4. **Stop and Remove Docker Containers**
    ```bash
    docker container stop CONTAINER_ID
    docker container rm CONTAINER_ID
    ```

5. **Run Docker Container with Terminal Access**
    ```bash
    docker container run -it ubuntu
    ```

---

## Example: Running a Web Server Inside a Docker Container
1. **Pull Nginx Image**
    ```bash
    docker pull nginx
    ```

2. **Run Nginx Container**
    ```bash
    docker run -d -p 80:80 nginx
    ```

3. **Access the Web Server**
    - Open a web browser and go to your VM's public IP address to see the Nginx welcome page.
      
**Using apache2:**

  - Update packages and install Apache:
      ```sh
      apt-get update
      apt-get install apache2 -y
      cd /var/www/html
      echo "hello" > index.html
      ```
  - Run the container with port mapping:
      ```sh
      docker container run -it -p 3600:80 ubuntu /bin/bash
      service apache2 start
      ```
  - Add inbound port rule of port 3600 in NSG.
  - Access the web server at `VM_ip:3600`.

**Note:** 

  - Inspect Docker Container:**
    ```sh
    docker inspect <container_id>
    ```
  - The vi editor is not installed by default in Docker containers, so using vi <filename> will not work without installation.

---

## Summary
Docker is a powerful platform that has revolutionized the way we develop, deploy, and manage applications. It provides consistency, portability, and efficiency, making it an essential tool for modern DevOps practices. By following the steps outlined in this guide, you can easily set up Docker on an Azure virtual machine and start leveraging its benefits for your projects.

---
