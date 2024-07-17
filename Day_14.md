# Day 14: Static Website Hosting, Migration Strategies, Managed and Unmanaged Disks

## Table of Contents

1. [Azure Static Website Hosting](#azure-static-website-hosting)
   - [Introduction](#introduction)
   - [Steps to Host a Static Website](#steps-to-host-a-static-website)
   - [Azure Storage Explorer](#azure-storage-explorer)
2. [Migration Strategies](#migration-strategies)
   - [Migrating VM Content](#migrating-vm-content)
   - [Example: VM-to-VM Migration](#example-vm-to-vm-migration)
3. [Managed and Unmanaged Disks](#managed-and-unmanaged-disks)
   - [Overview](#overview)
   - [Managed Disks](#managed-disks)
   - [Unmanaged Disks](#unmanaged-disks)
   - [Comparison](#comparison)

## Azure Static Website Hosting

### Introduction

Hosting a static website in Azure involves using Azure Storage to serve static content like HTML, CSS, JavaScript, and images directly from a storage account.
### VM Configuration as a Web Server:

- Windows: Internet Information Services (IIS)
- Linux: Apache2
### Steps to Host a Static Website

1. **Create a Storage Account**:
   - Go to the Azure portal.
   - Navigate to "Storage accounts".
   - Click "Create" and fill in the required details.

2. **Enable Static Website Hosting**:
   - Go to your storage account.
   - Under "Settings", click on "Static website".
   - Enable the feature and specify the index document (e.g., `index.html`).

3. **Upload Website Content**:
   - Use Azure Storage Explorer or the Azure portal to upload your static files to the `$web` container.

4. **Access Your Website**:
   - Copy the primary endpoint URL provided under the static website settings and paste it in a browser to view your site.

### Advantages of Static Website Hosting:

- Ease of Use: Provides a URL, HTTPS, and certificate automatically.
- Simplified Management: Requires less maintenance and management.
- Cost-Effective

### Static Website Hosting Setup in Azure

```
  +---------------------+
  |   Azure Portal      |
  +---------+-----------+
            |
            v
  +---------------------+
  |  Create Storage     |
  |     Account         |
  +---------------------+
            |
            v
  +---------------------+
  |  Enable Static      |
  | Website Hosting     |
  +---------------------+
            |
            v
  +---------------------+
  |  Upload Website     |
  | Content (HTML, CSS, |
  | JS, Images)         |
  +---------------------+
            |
            v
  +---------------------+
  |  Azure Storage      |
  |   Account           |
  |    +--------------+ |
  |    |   $web       | |
  |    |  Container   | |
  |    +--------------+ |
  +---------------------+
            |
            v
  +---------------------+
  |  Primary Endpoint   |
  | (URL to Access Site)|
  +---------------------+
```

### Azure Storage Explorer

Azure Storage Explorer simplifies managing Azure Storage resources.

#### Use Cases:

- Replicating a VM to Another VM:
Example: Creating a backup of a production VM to a test environment.
- Replicating from One Tenant to Another Tenant:
Example: Migrating data from a development tenant to a production tenant for deployment.

#### Key Features
- User-friendly interface
- Cross-platform support
- Manage blobs, file shares, queues, and tables
- Access control with SAS and access policies

#### Example: Uploading a File

1. **Download and Install**:
   - Download Azure Storage Explorer from the official website and install it.

2. **Connect to Your Azure Account**:
   - Open Azure Storage Explorer.
   - Click "Add an Account" and sign in with your Azure credentials.

3. **Navigate and Upload**:
   - Navigate to your storage account and open the `$web` container.
   - Click "Upload" and select "Upload Files...".
   - Choose the file to upload and complete the process.

## Migration Strategies

### Migrating VM Content

To migrate contents of one VM to another:

1. **Stop the Source VM**:
   - In the Azure portal, navigate to the source VM and stop it.

2. **Copy the Disk**:
   - In Azure Storage Explorer, find the VHD file of the source VM.
   - Copy the VHD file to the desired location (within the same or different subscription).

3. **Create a New VM**:
   - Use the copied VHD file to create a new VM in the Azure portal.

### Example: VM-to-VM Migration

**Scenario**: Replicating VM-1 to VM-2 within the same subscription.

1. **Stop VM-1**.
2. **Copy VM-1 Disk**:
   - Use Azure Storage Explorer to copy the VHD file of VM-1.
3. **Create VM-2**:
   - In the Azure portal, create a new VM using the copied VHD file.
   - Ensure both VMs have unique public IP addresses.

## Managed and Unmanaged Disks

### Overview

Disks in Azure are block-level storage volumes used with Azure VMs. There are two types: managed and unmanaged disks.

### Managed Disks

Managed disks are managed by Azure, simplifying management and scaling.

**Key Characteristics**:
- Azure manages the storage account.
- Highly scalable.
- Simplifies VM management.
- Automatically distributed for high availability.
- Cost: Pay for used space.

**Use Case**: Preferred for ease of use, scalability, and management simplicity.

### Unmanaged Disks

Unmanaged disks require manual management of storage accounts and VHD files.

**Key Characteristics**:
- User manages storage accounts.
- Limited by storage account capacity.
- Requires manual distribution of VHD files.
- Cost: Pay for storage account capacity and used space.

**Use Case**: Suitable for scenarios requiring manual storage management.

### Comparison
```
| Feature                | Managed Disks             | Unmanaged Disks            |
|------------------------|---------------------------|----------------------------|
| Storage Account Mgmt   | Managed by Azure          | Managed by user            |
| Scalability            | Highly scalable           | Limited by storage account |
| Simplicity             | Simple                    | Complex                    |
| Availability           | Auto-distributed          | Manual distribution        |
| Cost                   | Pay for used space        | Pay for capacity and space |
```
---
Azure provides robust tools for hosting static websites, managing storage, and migrating VM content. Understanding managed and unmanaged disks helps in choosing the right storage solution. Using Azure Storage Explorer simplifies managing Azure Storage resources, making it a powerful tool for Azure administrators.
