# Day 16: Azure Persistent Disk, CDN, App Services, Azure DNS

## Table of Contents

1. [Introduction to Azure Persistent Disk](#introduction-to-azure-persistent-disk)
    1. [Managed Disks](#managed-disks)
    2. [Unmanaged Disks](#unmanaged-disks)
    3. [Comparing Managed and Unmanaged Disks](#comparing-managed-and-unmanaged-disks)
2. [Azure CDN (Content Delivery Network)](#azure-cdn-content-delivery-network)
3. [Azure App Services](#azure-app-services)
4. [Azure DNS](#azure-dns)
5. [Steps to Create Resources in Azure Portal](#steps-to-create-resources-in-azure-portal)
    1. [Create a Storage Account and Blob Storage](#create-a-storage-account-and-blob-storage)
    2. [Create a CDN Profile and Endpoint](#create-a-cdn-profile-and-endpoint)
    3. [Create an App Service](#create-an-app-service)
    4. [Create an Azure DNS Zone](#create-an-azure-dns-zone)

---

## Introduction to Azure Persistent Disk

Azure Persistent Disks provide block-level storage volumes that are used with Azure Virtual Machines (VMs). Disks in Azure are virtualized and can be managed as either Managed Disks or Unmanaged Disks.

### Managed Disks

Managed Disks simplify storage management by handling the storage account for you. Azure manages the storage, making scaling and management easier.

#### Types of Managed Disks:

1. **Standard HDD**:
  - **Use Case**: Backup storage, low IOPS applications.
  - **Advantages**: Lower cost per GB.
  - **Limitations**: Slower performance compared to SSDs.

2. **Standard SSD**:
  - **Use Case**: Web servers, small databases.
  - **Advantages**: Better performance than HDDs.
  - **Limitations**: Limited IOPS compared to Premium SSDs.

3. **Premium SSD**:
  - **Use Case**: Databases, enterprise applications.
  - **Advantages**: High IOPS and throughput.
  - **Limitations**: Higher cost per GB compared to standard disks.

4. **Ultra Disk**:
  - **Use Case**: Mission-critical applications, high-performance databases.
  - **Advantages**: Ultra-low latency and high IOPS.
  - **Limitations**: Highest cost per GB among all disk types.

#### Key Characteristics:

  - **Azure Managed**: Azure handles the storage account management.
  - **Scalability**: Easily scalable without worrying about storage account limits.
  - **Simplicity**: Simplifies VM management by abstracting storage account details.
  - **Availability Sets**: Automatically placed in different storage accounts when used with availability sets to ensure higher availability.
  - **Cost**: You only pay for the used space without managing storage account limits.

### Unmanaged Disks

Unmanaged Disks require you to handle the storage accounts and the VHD files used by VMs. They provide more control but require more effort to manage.

#### Types of Unmanaged Disks:

1. **Standard HDD**:
  - **Use Case**: Legacy systems, archival data.
  - **Advantages**: Lower cost per GB compared to SSDs.
  - **Limitations**: Slower performance compared to SSDs.

2. **Standard SSD**:
  - **Use Case**: General-purpose applications, databases.
  - **Advantages**: Improved performance over HDDs.
  - **Limitations**: Higher cost per GB compared to HDDs.

#### Key Characteristics:

  - **Storage Account Management**: You need to create and manage storage accounts.
  - **Scalability**: Limited by the storage account's capacity limits.
  - **Complexity**: Requires manual distribution of VHD files across storage accounts to balance the load.
  - **Cost**: You pay for the storage account capacity and the used space separately.

### Characteristics of Managed and Unmanaged Disk types:
```
| Managed Disk Type | Description                                                                                       | Use Case                                | Advantages                            | Limitations                                   |
|-------------------|---------------------------------------------------------------------------------------------------|-----------------------------------------|----------------------------------------|-----------------------------------------------|
| **Standard HDD**  | Offers cost-effective storage with low performance requirements.                                  | Backup storage, low IOPS applications.  | Lower cost per GB.                     | Slower performance compared to SSDs.          |
| **Standard SSD**  | Provides a balance between cost and performance, suitable for most workloads.                     | Web servers, small databases.           | Better performance than HDDs.          | Limited IOPS compared to Premium SSDs.        |
| **Premium SSD**   | Offers high-performance storage for I/O-intensive applications.                                   | Databases, enterprise applications.     | High IOPS and throughput.             | Higher cost per GB compared to standard disks. |
| **Ultra Disk**    | Delivers high-performance, low-latency disk storage for I/O-intensive applications.               | Mission-critical applications, high-performance databases. | Ultra-low latency and high IOPS.      | Highest cost per GB among all disk types.      |

| Unmanaged Disk Type | Description                                                                                       | Use Case                               | Advantages                            | Limitations                                   |
|---------------------|---------------------------------------------------------------------------------------------------|-----------------------------------------|----------------------------------------|-----------------------------------------------|
| **Standard HDD**    | Traditional HDDs managed directly within Azure Virtual Machines (i.e., requires manual management).| Legacy systems, archival data.           | Lower cost per GB compared to SSDs.    | Slower performance compared to SSDs.          |
| **Standard SSD**    | Traditional SSDs managed directly within Azure Virtual Machines (i.e., requires manual management).| General-purpose applications, databases.| Improved performance over HDDs.        | Higher cost per GB compared to HDDs.          |
```

```
+------------------------------------------------------------------------------------------------------------------------------------+
| Feature                       |                   Managed Disks                 |                 Unmanaged Disks                  |
|-------------------------------|-------------------------------------------------|--------------------------------------------------|
| Storage Account Management    | Managed by Azure                                | Managed by user                                  |
| Scalability                   | Highly scalable                                 | Limited by storage account capacity              |
| Simplicity                    | Simple, no need to manage storage               | Complex, requires managing storage               |
| Availability                  | Automatically distributed for high availability | Manual distribution needed                       |
| Cost                          | Pay for used space                              | Pay for storage account capacity and used space  |
+------------------------------------------------------------------------------------------------------------------------------------+
```
### Specification
```
+------------------------------------------------------------------------------------------+ 
|                    |                 Managed Disk                    | Unmanaged Disk    |
|--------------------|-------------------------------------------------|-------------------|
|                    | Std HDD  | Std SSD   | Prem SSD  | Ultra Disk   | Std HDD | Std SSD |
---------------------|----------|-----------|-----------|--------------|---------|---------|
|   Min Size         | 1 GB     | 1 GB      | 1 GB      | 4 GB         | 1 GB    | 1 GB    |
|   Max Size         | 32,767 GB| 32,767 GB | 32,767 GB | 64 TB        | 4 TB    | 4 TB    |
|   IOPS Min         | 500 MB   | 120 MB    | 120 MB    | 100 MB       | 500 MB  | 500 MB  |
|   IOPS Max         | 2,000 MB | 6,000 MB  | 20,000 MB | 1,600,000 MB | 500 MB  | 750 MB  |
|   Throughput Min   |    -     | 25 MB     | 25 MB     | 25 MB        | 60 MB   | 100 MB  |
|   Throughput Max   | 500 MB   | 750 MB    | 900 MB    | 2,000 MB     |    -    | 250 MB  |
+------------------------------------------------------------------------------------------+
```

### Persistent Disk Additional Information

- **IOPS (Input/Output Operations Per Second)**: Quantitative measure indicating how many read/write operations can be performed per second.
- **Throughput**: Quantitative measure indicating the amount of data (in MB/KB) that can be read or written per second.
- **Redundancy Support**:
  - **LRS (Locally Redundant Storage)**: Supports all managed disk types.
  - **ZRS (Zone Redundant Storage)**: Supports only Standard SSD and Premium SSD.

#### Notes

- **HDD (Hard Disk Drive)**:
  - Uses spinning magnetic disks and mechanical parts.
  - Slower data access speed due to mechanical limitations.
  - More prone to physical damage and mechanical failure.
  - Generally cheaper per GB compared to SSDs.

- **SSD (Solid State Drive)**:
  - Uses flash memory technology with no moving parts.
  - Much faster data access speed.
  - More durable, quieter, and consumes less power.
  - More expensive per GB compared to HDDs.

- **Shared Volumes**: Only Premium SSD and Ultra Disk support shared volumes (connecting one disk with multiple VMs).
- **OS Disk Limitation**: Ultra Disk cannot be used as OS disk.

---

## Azure CDN (Content Delivery Network)

### Introduction

Azure CDN helps reduce latency by creating Points of Presence (POPs) or Edge Locations closer to end-users. These store cached content, minimizing the distance data needs to travel.

### Key Characteristics:

- **Latency Reduction**: Reduces the number of hops, lowering latency.
- **POP/Edge Locations**: Store cached content, reducing load on the origin server.

### Use Cases:

- **Payment Gateways**: Improve response times for financial transactions.
- **Gaming Industries**: Enhance performance and user experience.
- **Web Applications**: Improve load times for static content like images and videos.

### Indirect Benefits:

- **Load Reduction**: Reduces the load on the origin server.
- **Increased Security**: Enhances security by distributing content.
- **Higher Uptime**: Improves the overall uptime of the service.

### Example Scenario

Suppose you have a web server hosted in Mumbai (West India) region. Clients from Dubai, US, and Hyderabad access the server. Clients closer to the server (e.g., Hyderabad) experience lower latency due to fewer hops. However, clients further away (e.g., Dubai and US) experience higher latency due to more hops. Using a CDN, you can create a POP near these clients, reducing latency and improving access times.

---

## Azure App Services

### Introduction

Azure App Services provide a PaaS environment for deploying and scaling web applications. It simplifies infrastructure management and reduces costs for small-scale deployments.

### Key Characteristics:

- **Tech-Stack Definition**: Define the technology stack, DB server, source code, and Docker image.
- **Cost-Effective**: Ideal for small businesses and portfolio websites.

### Requirements:

- Define the tech stack.
- Define the database server.
- Source code.
- Docker image.

### Use Cases:

- **Web Applications**: Quickly deploy and scale web applications.
- **APIs**: Host and manage APIs with built-in authentication and monitoring.

### Example Scenario

For web application deployment, typically, you create a web server, connect it to a database, and manually configure the infrastructure. Azure App Services provide an easier alternative, automating the setup and providing an endpoint with its own DNS.

---

## Azure DNS

### Introduction

Azure DNS allows you to map domain names to Azure resources, providing DNS resolution and management.

### Key Characteristics:

- **Domain Name Mapping**: Map IP addresses to domain names.
- **Types of Records**: NS, SOA, A, AAAA, CNAME, MX.
- **Route 53** is it's AWS equivalent.

### Domain Registries: Websites where we can purchase domain names.
- Hostinger
- NameCheap
- Dynadot
- Hover
- Bluehost
- GoDaddy
- BidRock

### Types of Records:

- **NS (Name Server)**: Changes the domain name configuration.
- **SOA (Start of Authority)**: Authorizes the domain name.
- **A (Alias)**: Maps the IPv4 with the domain name.
- **AAAA**: Maps the IPv6 with the domain name.
- **CNAME**: Provides a certification to the website.
- **MX (Mail Exchange)**: Supports email services.

### Example Scenario

If a user hits an IP address of a web server, the server responds. However, users typically hit a domain name, which is mapped to the IP address via Azure DNS. This mapping allows users to access the web server using a domain name instead of an IP address.

---

## Steps to Create Resources in Azure Portal

### Create a Storage Account and Blob Storage

1. **Create a Storage Account**:
   - Navigate to the [Azure portal](https://portal.azure.com/).
   - Click on **Create a resource** > **Storage** > **Storage account**.
   - Enter the required details and click **Review + create**, then click **Create**.

2. **Create a Container in Blob Storage**:
   - Navigate to the storage account.
   - Under **Blob service**, click **Containers**.
   - Click **+ Container**, enter a name, set the public access level, and click **OK**.

### Create a CDN Profile and Endpoint

1.Create a Resource Group.
2.Create a Storage Account:
 - Fill in the required details.
 - Enable anonymous access and accept default settings.
3. Create a Container in the Storage Account:
 - Upload the files to the container.
4. Create a CDN Profile:
 - Fill in the required details and accept default settings.
5. Create a Front Door Profile:
 - Fill in the required details and accept default settings.
6. Endpoint Hostname:
 - The generated endpoint hostname is provided to the on-premise service provider/developer.

### Create an App Service

1. Create a GitHub Repository and Upload Files.
2. Create a Resource Group.
3. Create a Static Web App:
 - Log in to GitHub.
 - Fill in the required details.
 - Select the repository.
 - Choose the build storage.
 - Accept default settings.
4. Access URL:
 - The generated URL displays the deployed content.

### Create an Azure DNS ZOne

1. Purchase a Domain Name from a Domain Registry.
2. Create a Resource Group.
3. Create a VM:
 - Configure the VM to deploy a web server.
4. Create a DNS Zone:
 - Fill in the required details and accept default settings.
5. Manage Domain:
 - Go to the domain registry and manage the domain.
 - Add the 4 name servers generated by Azure (remove the last dot in each name server).
6. Add Recordsets:
 - Paste the IP of the VM.
 - Add other details (e.g., names like www, app, etc.).
---
