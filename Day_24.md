# Day 24: Dockerfile, Private Registry, Terraform

# Table of Contents
1. [Dockerfile](#dockerfile)
2. [Docker and Related Terms](#docker-and-related-terms)
3. [Compliance Considerations](#compliance-considerations)
4. [Private Docker Registry](#private-docer-registry)
5. [Terraform](#terraform)
6. [GitOps Workflow with Terraform, GitLab and Docker](#gitOps-workflow-with-terraform-GitLab-and-docker)
7. [ADD vs COPY](#add-vs-copy)
---
## Dockerfile
  
### What It Is
A Dockerfile is a text file containing instructions on how to build a Docker image. It acts as a recipe, detailing everything that should be included in the image.

### Why Use It
Dockerfiles automate the creation of Docker images. Instead of manually setting up your environment and application each time, you define the setup once in a Dockerfile.

### How It Works
You write instructions in a Dockerfile, specifying:
- **Base image** to start with.
- **Software** to install.
- **Files** to copy.
  
Docker reads these instructions and creates an image based on them when you run the `docker build` command.

### Imagine Baking a Cake
- **Dockerfile (Recipe Card)**: A Dockerfile is like a recipe card for baking a cake. It lists all the ingredients and steps to make the cake perfectly.
  
### Scenario
- **Baking the Cake**: Suppose you want to bake a chocolate cake. You have a recipe card that tells you:
  - The type of flour and sugar to use.
  - The exact amount of cocoa powder.
  - The steps for mixing and baking.
- **Following the Recipe**: Each time you follow the recipe card, you end up with a delicious chocolate cake, regardless of the kitchen you use.
- **Sharing the Recipe**: If you give the recipe card to a friend, they can bake the exact same chocolate cake.

### How This Relates to Dockerfile
- **Creating a Container**: A Dockerfile includes all the instructions for setting up a container:
  - Base image (like choosing the type of flour).
  - Additional software/tools (like adding cocoa powder).
  - Configuration steps (like mixing and baking).
- **Reproducibility**: Using the same Dockerfile will always create the same container environment, ensuring consistency.

### Basic Structure of a Dockerfile

1. **Start with a Base Image**
   ```Dockerfile
   FROM ubuntu:20.04
   ```

2. **Set the Maintainer (Optional)**
   ```Dockerfile
   LABEL maintainer="your-email@example.com"
   ```

3. **Update and Install Dependencies**
   ```Dockerfile
   RUN apt-get update && apt-get install -y \
       curl \
       vim \
       git
   ```

4. **Set Environment Variables (Optional)**
   ```Dockerfile
   ENV APP_HOME /app
   ```

5. **Copy Files into the Container**
   ```Dockerfile
   COPY . /app
   ```

6. **Set the Working Directory**
   ```Dockerfile
   WORKDIR /app
   ```

7. **Install Application Dependencies**
   ```Dockerfile
   RUN pip install -r requirements.txt
   ```

8. **Expose Ports**
   ```Dockerfile
   EXPOSE 8080
   ```

9. **Define the Command to Run the Application**
   ```Dockerfile
   CMD ["python", "app.py"]
   ```

### Example Dockerfile

```Dockerfile
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

### Steps to Build and Run the Docker Image

1. **Save the Dockerfile**
   Save the Dockerfile in the root of your project directory.

2. **Build the Docker Image**
   ```bash
   docker build -t my-python-app .
   ```

3. **Run the Docker Container**
   ```bash
   docker run -p 4000:5000 my-python-app
   ```

### Dockerfile Commands

1. **FROM**: Specifies the base image for the Dockerfile.  
   *Example:* `FROM ubuntu:20.04`

2. **RUN**: Executes commands to build the image, typically used for installing software.  
   *Example:* `RUN apt-get update && apt-get install -y python3`

3. **CMD**: Sets the default command to run when a container starts. Only one CMD per Dockerfile is allowed.  
   *Example:* `CMD ["python3", "app.py"]`

4. **ENTRYPOINT**: Configures the container to run as an executable. It cannot be overridden by arguments passed to `docker run`.  
   *Example:* `ENTRYPOINT ["python3", "app.py"]`

5. **COPY**: Copies files or directories from the host into the image.  
   *Example:* `COPY . /app`

6. **ADD**: Similar to COPY, but also extracts tar files and supports remote URLs.  
   *Example:* `ADD https://example.com/app.tar.gz /app`

7. **ENV**: Sets environment variables within the image.  
   *Example:* `ENV APP_ENV=production`

8. **EXPOSE**: Documents the ports the container listens on at runtime.  
   *Example:* `EXPOSE 8080`

9. **WORKDIR**: Sets the working directory for subsequent instructions.  
   *Example:* `WORKDIR /app`

10. **VOLUME**: Creates a mount point for external volumes.  
    *Example:* `VOLUME /data`

11. **USER**: Sets the user name or UID for running commands.  
    *Example:* `USER appuser`

12. **ARG**: Defines build-time variables that can be passed with `docker build`.  
    *Example:* `ARG VERSION=1.0`

13. **LABEL**: Adds metadata to the image in key-value pairs, often used for versioning and maintainer information.  
    *Example:* `LABEL maintainer="you@example.com"`

14. **ONBUILD**: Adds a trigger instruction to be executed when the image is used as a base for another build.  
    *Example:* `ONBUILD COPY . /app`

---

## Docker and Related Terms

1. **Docker Image:**

  - A snapshot of a filesystem and its dependencies at a specific point in time, used to create Docker containers.
  - Example: A complete recipe and ingredients list, ready to be used whenever needed.

2. **Docker Container:**

  - A running instance of a Docker image, representing the actual executable environment where the application runs.
  - Example: A cake baked from the recipe.

3. **Docker Hub:**

  - A public registry where Docker images are stored and shared.
  - Example: A public library where anyone can check out or contribute books.

4. **Dockerfile:**

  - A text file containing instructions for building a Docker image.
  - Example: A recipe detailing each step to make a dish.

5. **Docker Volume:**

  - A storage area that Docker containers use to store data persistently.
  - Example: A drawer in the kitchen for baking tools and ingredients.

6. **Registry:**

  - A place where Docker images are stored and managed.
  - Example: A library for storing, retrieving, and organizing books.

7. **Private Docker Registry:**

  - A secure, private place to store and manage Docker images, accessible only to authorized users.
  - Example: A private library for personal rare books.

8. **Docker Compose:**

  - A tool for defining and running multi-container Docker applications using a YAML file.
  - Example: Organizing various dishes for a dinner party.

9. **Docker Engine:**

  - The core software that runs and manages Docker containers.
  - Example: The kitchen with tools used to prepare and bake dishes.

10. **Image Registry Authentication:**

  - Ensures that only authorized users can access the private Docker registry.
  - Example: A membership card to enter a private library.
---

## Compliance Considerations

1. **SOC 1:**
  - Focus: Internal controls over financial reporting.
  - Considerations: Ensure controls impacting financial statements are effective.

2. **SOC 2:**
  - Focus: Data security and privacy.
  - Considerations: Follow Trust Service Criteria: Security, Availability, Processing Integrity, Confidentiality, and Privacy.

3. **HIPAA:**
  - Focus: Protection of health information.
  - Considerations: Ensure privacy, security, and breach notification of Protected Health Information (PHI).

4. **CCSP:**
  - Focus: Cloud security.
  - Considerations: Apply controls for data protection and encryption in the cloud.

5. **CISA:**
  - Focus: Auditing and control of information systems.
  - Considerations: Implement and evaluate controls related to data integrity, confidentiality, and availability.
---
## Private Docker Registry

### What It Is
A Private Docker Registry is a secure place to store Docker images, accessible only to you or authorized users.

### Why Use It
- **Security**: Keeps your images secure, especially if they contain sensitive information.
- **Control**: Provides control over who can access and use your images.

### How It Works
- **Setup**: Set up a private registry on your server or use a managed service like Azure Container Registry (ACR).
- **Usage**: Push (upload) and pull (download) Docker images to and from the registry.

### Imagine Your Own Library
- **Private Docker Registry (Personal Library)**: Think of a private Docker registry as your personal library where you store your favorite books.

### Scenario
- **Library Setup**: You have a collection of books you don't want to share publicly.
- **Storing and Accessing Books**: Whenever you or your friends need a book, you can go to this library and check it out.
- **Control and Privacy**: You control which books are available and who can access them.

### How This Relates to Docker Registry
- **Storing Docker Images**: A private Docker registry is like a personal library for Docker images.
- **Access Control**: You control who can upload or download images.
- **Custom and Secure**: Ensures your Docker images are kept safe and accessible only to authorized users.

### **Creating a Private Docker Registry in Azure**

#### **1. Setup**
1. **Log in to the Azure Portal.**
2. **Create a Resource Group**:
   - Navigate to “Resource Groups” and click “Add” to create a new group.

#### **2. Create a VM**
1. **Create a Virtual Machine (VM)**:
   - Go to “Virtual Machines” and click “Add” to create a VM.
   - Choose a suitable image (e.g., Ubuntu) and size.
2. **Connect to VM**:
   - Use SSH to connect to the VM.

#### **3. Install Docker**
1. **Install Docker**:
   - Update package lists: `sudo apt-get update`
   - Install Docker: `sudo apt-get install docker.io`
   - Start Docker: `sudo systemctl start docker`
   - Enable Docker to start on boot: `sudo systemctl enable docker`

#### **4. Run Docker Registry**
1. **Pull Docker Registry Image**:
   - `sudo docker pull registry`
2. **Run Docker Registry Container**:
   - `sudo docker run -itd -p 5000:5000 --name simple_registry registry`

#### **5. Manage Docker Registry**
1. **Check Images in Registry**:
   - Install `elinks`: `sudo apt-get install elinks`
   - Check images with: `elinks http://127.0.0.1:5000/v2/_catalog`
   - Or use `curl`: `curl http://127.0.0.1:5000/v2/_catalog`

2. **Push Images to Registry**:
   - Pull images: `sudo docker pull mysql`, `sudo docker pull alpine`, `sudo docker pull nginx`
   - Tag images: 
     - `sudo docker tag mysql 127.0.0.1:5000/mysql`
     - `sudo docker tag alpine 127.0.0.1:5000/alpine`
     - `sudo docker tag nginx 127.0.0.1:5000/nginx`
   - Push images:
     - `sudo docker push 127.0.0.1:5000/mysql`
     - `sudo docker push 127.0.0.1:5000/alpine`
     - `sudo docker push 127.0.0.1:5000/nginx`

3. **Verify Images in Registry**:
   - Use `curl` or `elinks` to check the catalog at `http://127.0.0.1:5000/v2/_catalog`.

#### **6. Access Registry from Other Systems**
1. **Obtain VM’s Private IP**:
   - Find the private IP address of the VM in the Azure portal.

2. **Allow Port in NSG**:
   - Open port 5000 in the Network Security Group (NSG) inbound rules for the VM.

3. **Tag and Push Images Using Private IP**:
   - Tag images: 
     - `sudo docker tag alpine <PRIVATE_IP>:5000/alpine`
   - Push images: 
     - `sudo docker push <PRIVATE_IP>:5000/alpine`

4. **Bypass SSL Certificates (Optional)**:
   - **Create `daemon.json`**:
     - `vi daemon.json`
     - Add:
       ```json
       {
         "insecure-registries": ["<PRIVATE_IP>:5000"]
       }
       ```
   - **Move to Docker Directory**:
     - `sudo cp daemon.json /etc/docker/`
   - **Restart Docker**:
     - `sudo systemctl restart docker`

5. **If Using SSL Certificates**:
   - Generate SSL certificates with `openssl`.
   - Place certificates in `/etc/docker/`.
   - Restart Docker: `sudo systemctl restart docker`.

#### **7. Manage Docker Containers**
1. **Start Docker Containers**:
   - Start containers if they stopped: `sudo docker container start <container_id>`

### **Notes**
- Replace `<PRIVATE_IP>` with the actual private IP address of the VM.
- Ensure Docker and registry ports are correctly configured and accessible.
- Using SSL certificates is recommended for secure communications.
  
---
## Terraform

**Terraform** is an open-source Infrastructure as Code (IaC) tool by HashiCorp. It allows you to define and manage your cloud infrastructure using configuration files. By using Terraform, you can handle both low-level components like compute instances and high-level components like DNS entries.

### Key Features of Terraform

1. **Infrastructure as Code (IaC):** Describes infrastructure in code for version control and reproducibility.
2. **Execution Plans:** Generates a plan showing what will change before applying configurations.
3. **Resource Graph:** Builds a graph of resources to manage dependencies and order of operations.
4. **Change Automation:** Applies only necessary changes, reducing manual errors.


### Key Concepts

- **Provider:** A plugin to interact with cloud services (e.g., Azure).
- **Resource:** The infrastructure components you manage (e.g., VMs, networks).
- **State:** Keeps track of the current state of your infrastructure.
- **Modules:** Reusable configuration blocks.

### Basic Workflow

1. **Write Configuration:** Define infrastructure in `.tf` files.
2. **Initialize:** Run `terraform init` to set up and download plugins.
3. **Plan:** Run `terraform plan` to preview changes.
4. **Apply:** Run `terraform apply` to create/update resources.
5. **Destroy:** Run `terraform destroy` to delete resources.

### Example: Creating a Resource Group and Virtual Network

**Configuration (main.tf):**

```hcl
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}

resource "azurerm_virtual_network" "example" {
  name                = "example-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}
```

**Commands:**

```sh
terraform init
terraform plan
terraform apply
terraform destroy
```

### Advantages of Terraform

1. **Consistency:** Ensures consistent infrastructure across environments.
2. **Automation:** Automates provisioning and management of resources.
3. **Scalability:** Manages infrastructure at any scale.
4. **Multi-Cloud:** Supports multiple cloud providers.

---

### Step-by-Step Guide to Deploying Azure Resources with Terraform

1. **Install Terraform:** Download and install from the official website.
2. **Set Up Azure Environment:** Ensure Azure CLI is installed and log in:
   ```sh
   az login
   ```
3. **Create Terraform Configuration File:** Create `main.tf` with your configuration.
4. **Initialize Terraform:** 
   ```sh
   terraform init
   ```
5. **Create an Execution Plan:** 
   ```sh
   terraform plan
   ```
6. **Apply the Configuration:** 
   ```sh
   terraform apply
   ```
7. **Verify the Deployment:** Check resources in the Azure Portal.
8. **Manage and Update Infrastructure:** Modify `main.tf` as needed and run `terraform plan` and `terraform apply`.
9. **Destroy the Infrastructure:** 
   ```sh
   terraform destroy
   ```

---

### Example: Deploying a Virtual Machine in Azure

**Configuration (main.tf):**

```hcl
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}

resource "azurerm_virtual_network" "example" {
  name                = "example-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}

resource "azurerm_subnet" "example" {
  name                 = "example-subnet"
  resource_group_name  = azurerm_resource_group.example.name
  virtual_network_name = azurerm_virtual_network.example.name
  address_prefixes     = ["10.0.1.0/24"]
}

resource "azurerm_public_ip" "example" {
  name                = "example-pip"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  allocation_method   = "Dynamic"
}

resource "azurerm_network_security_group" "example" {
  name                = "example-nsg"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}

resource "azurerm_network_interface" "example" {
  name                = "example-nic"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  ip_configuration {
    name                          = "example-ipconfig"
    subnet_id                     = azurerm_subnet.example.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.example.id
  }
}

resource "azurerm_virtual_machine" "example" {
  name                  = "example-vm"
  location              = azurerm_resource_group.example.location
  resource_group_name   = azurerm_resource_group.example.name
  network_interface_ids = [azurerm_network_interface.example.id]
  vm_size               = "Standard_DS1_v2"

  storage_os_disk {
    name              = "example-osdisk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "18.04-LTS"
    version   = "latest"
  }

  os_profile {
    computer_name  = "example-vm"
    admin_username = "adminuser"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = false
  }
}
```

**Commands:**

```sh
terraform init
terraform plan
terraform apply
```

### Advantages of Using Terraform with Azure

1. **Consistency and Repeatability:** Ensures consistent deployments.
2. **Version Control:** Allows tracking and collaboration.
3. **Scalability:** Manages large-scale infrastructure.
4. **Automation:** Reduces manual effort and errors.
5. **Multi-Cloud:** Unified management across different platforms.


### Terraform Vault Integration

**Vault Configuration (vault.tf):**

```hcl
provider "vault" {
  address = "https://vault.example.com"
}

data "vault_generic_secret" "example" {
  path = "secret/myapp"
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
  tags = {
    environment = data.vault_generic_secret.example.data["environment"]
  }
}
```

**Advantages:**

- **Security:** Protects sensitive data.
- **Automation:** Retrieves secrets automatically.
- **Compliance:** Centralizes secret management.
---

## GitOps Workflow with Terraform, GitLab and Docker

1. **Write Terraform Scripts:** Define infrastructure in Terraform files.
2. **Use GitOps:** Git as the single source of truth for infrastructure code.
3. **Push to GitLab:** Store scripts in a GitLab repository.
4. **CI/CD Pipeline:** Triggered by commits, automates deployment.
5. **Private Docker Registry:** Stores Docker images.
6. **Run Terraform in Docker:** Ensures consistent execution environment.
7. **Plan and Apply:** Terraform generates a plan and applies changes.
8. **Create/Update Resources:** Terraform updates infrastructure based on the plan.

**Summary:** Write Terraform scripts, store in GitLab, use GitLab CI/CD and Docker to automate and manage infrastructure, ensuring consistency, automation, and security.
---
## ADD vs COPY
The `ADD` and `COPY` commands in Dockerfiles are used to include files from the host system into the Docker image, but they have some key differences:

### **COPY**
- **Purpose**: Copies files or directories from the host system into the Docker image.
- **Capabilities**: 
  - Only copies files and directories.
  - Does not handle any special processing beyond copying.
- **Syntax**: `COPY <src> <dest>`
  - *Example*: `COPY ./localfile.txt /app/`

### **ADD**
- **Purpose**: Similar to `COPY`, but with additional functionalities.
- **Capabilities**: 
  - Copies files and directories from the host system.
  - Can also handle remote URLs. If a URL is provided, Docker will download the file and add it to the image.
  - Automatically extracts compressed tar files (e.g., `.tar`, `.tar.gz`, `.tgz`).
- **Syntax**: `ADD <src> <dest>`
  - *Example*: `ADD https://example.com/file.tar.gz /app/` (downloads and extracts the file)
  - *Example*: `ADD ./localfile.tar.gz /app/` (extracts the tar file into `/app/`)

### **Summary**
- Use **`COPY`** for simple file and directory copying.
- Use **`ADD`** if you need to fetch files from a URL or if you need to automatically extract tar archives.
---
