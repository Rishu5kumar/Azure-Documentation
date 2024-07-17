# Dat 13: Azure Container Mounting and Fileshare

## Table of Contents
1. [Introduction](#introduction)
2. [Azure Storage Accounts](#azure-storage-accounts)
3. [Azure Blob Storage](#azure-blob-storage)
4. [Azure Files](#azure-files)
5. [Why Mount is Required?](#why-mount-is-required)
6. [How to Mount Blob and File from Multiple VMs or PCs](#how-to-mount-blob-and-file-from-multiple-vms-or-pcs)
    - [Mounting Azure Blob Storage](#mounting-azure-blob-storage)
    - [Mounting Azure Files](#mounting-azure-files)
7. [Example Scenario: Storing Profile Pictures in RDBMS](#example-scenario-storing-profile-pictures-in-rdbms)
8. [Architecture](#architecture)
9. [Connecting VM with Container or File](#connecting-vm-with-container-or-file)
10. [Using Blobfuse for Mounting Blob](#using-blobfuse-for-mounting-blob)
11. [Commands and Tools](#commands-and-tools)
12. [Limitations of Blob Container Mounting](#limitations-of-blob-container-mounting)
13. [Re-mounting Blob Container](#re-mounting-blob-container)
14. [Resource Group Locks](#resource-group-locks)
15. [Conclusion](#conclusion)

## Introduction
Azure provides robust storage solutions with Azure Blob Storage and Azure Files. These services enable scalable, secure, and efficient data storage, accessible from anywhere. Mounting these storage solutions can simplify data management and access in various applications.

## Azure Storage Accounts
A storage account provides a unique namespace in Azure for your data. Each storage account can hold blobs, files, queues, and tables.

## Azure Blob Storage
Azure Blob Storage is optimized for storing massive amounts of unstructured data, such as text or binary data. Blobs can be accessed via HTTP or HTTPS.

## Azure Files
Azure Files allows you to create file shares in the cloud that can be accessed via SMB (Server Message Block) or NFS (Network File System) protocol.

## Why Mount is Required?
Mounting Azure Blob Storage or Azure Files allows you to seamlessly integrate cloud storage with your virtual machines (VMs) or local PCs. This makes it easier to access and manage files as if they were part of your local file system.

## How to Mount Blob and File from Multiple VMs or PCs

### Mounting Azure Blob Storage

#### Example
Imagine a scenario where you need to mount a container from Azure Blob Storage to a Linux VM for processing large datasets.

#### Steps to Mount

1. **Create a Storage Account and Blob Container:**
   - Go to the Azure portal.
   - Navigate to "Storage accounts" and click on "Create."
   - Fill in the necessary details (Subscription, Resource group, Storage account name, Region, etc.) and create the storage account.
   - Once the storage account is created, go to the storage account and click on "Containers" under the "Data storage" section.
   - Click on "+ Container" to create a new blob container. Name the container and set the public access level as needed.

2. **Configure Access Keys:**
   - In the storage account, go to "Access keys" under the "Security + networking" section.
   - Copy one of the access keys and the storage account name for use in the configuration.

3. **Mount Blob Storage in the VM:**
   - Go to your VM in the Azure portal.
   - Click on "Extensions + applications" and add a new extension.
   - Select the "Custom Script Extension" and configure it with a script to install Blobfuse and mount the blob storage using the access keys and container name.

### Mounting Azure Files

#### Example
Consider a scenario where you need to share files between multiple VMs in Azure for collaborative development work.

#### Steps to Mount

1. **Create a Storage Account and File Share:**
   - Go to the Azure portal.
   - Navigate to "Storage accounts" and click on "Create."
   - Fill in the necessary details (Subscription, Resource group, Storage account name, Region, etc.) and create the storage account.
   - Once the storage account is created, go to the storage account and click on "File shares" under the "Data storage" section.
   - Click on "+ File share" to create a new file share. Name the file share and set the quota as needed.

2. **Obtain the Storage Account Key:**
   - In the storage account, go to "Access keys" under the "Security + networking" section.
   - Copy one of the access keys and the storage account name for use in the configuration.

3. **Mount the File Share in Windows VM:**
   - Go to your Windows VM in the Azure portal.
   - Open the "Command Prompt" or "PowerShell" with administrative privileges.
   - Use the `net use` command to mount the file share:
     ```cmd
     net use Z: \\<storage-account-name>.file.core.windows.net\<file-share-name> /u:<storage-account-name> <storage-account-key>
     ```

4. **Mount the File Share in Linux VM:**
   - Go to your Linux VM in the Azure portal.
   - Open the terminal.
   - Install CIFS utilities if not already installed:
     ```bash
     sudo apt-get install cifs-utils
     ```
   - Create a directory to mount the file share:
     ```bash
     sudo mkdir /mnt/myfileshare
     ```
   - Mount the file share:
     ```bash
     sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<file-share-name> /mnt/myfileshare -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,sec=ntlmssp
     ```

## Example Scenario: Storing Profile Pictures in RDBMS
When creating an application like Facebook, user profile pictures cannot be directly stored in a relational database (RDBMS). Instead, you store the images in Azure Blob Storage and save the image URL (endpoint) in the database.

1. **Store Image in Blob Storage**:
   - Upload the image to an Azure Blob container.
   - Get the URL of the uploaded image.

2. **Save URL in RDBMS**:
   - Save the image URL (a string) in a database field (e.g., `profile_pic_url`).

Example:
- **Blob Storage URL**: `https://<storage-account>.blob.core.windows.net/<container>/<image>.jpg`
- **Database Field**: `profile_pic_url` (type: VARCHAR)

## Architecture

```
Subscription
   |
Resource Group (RG)
   |
VNET
   |
subnet
   |
VM
   |
storage account
   |
Blob container and file
```

## Connecting VM with Container or File
We will learn how to connect a VM with a blob container or file share.

### Steps to Connect a VM with Blob Container

1. **Set Up Blob Container**:
   - Go to the Azure portal.
   - Navigate to your storage account.
   - Click on "Containers" under "Data storage".
   - Create a new container or use an existing one.

2. **Generate SAS Token**:
   - In the storage account, navigate to "Shared access signature" under "Security + networking".
   - Configure the settings (permissions, start and expiry date/time) and generate the SAS token.
   - Copy the SAS token URL.

3. **Use Blob Storage URL in Application**:
   - Use the SAS token URL in your application or script to access the blob container from the VM.

### Steps to Connect a VM with File Share

1. **Set Up File Share**:
   - Go to the Azure portal.
   - Navigate to your storage account.
   - Click on "File shares" under "Data storage".
   - Create a new file share or use an existing one.

2. **Access File Share in Windows VM**:
   - Go to the Windows VM in the Azure portal.
   - Open "Command Prompt" or "PowerShell" with administrative privileges.
   - Use the `net use` command to connect to the file share.

3. **Access File Share in Linux VM**:
   - Go to the Linux VM in the Azure portal.
   - Open the terminal.
   - Install CIFS utilities and mount the file share using the storage account name and key.

## Using Blobfuse for Mounting Blob
Blobfuse is not installed by default in any VM. Use the following commands to download and install it.

1. **Download Blobfuse**:
   ```bash
   sudo apt-get update
   sudo apt-get install blobfuse
   ```

2. **Mounting Commands**:
   - Create a temporary path:
     ```bash
     sudo mkdir /mnt/ramdisk
     sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
     sudo mkdir /mnt/ramdisk/blobfusetmp
     sudo chown <youruser> /mnt/ramdisk/blobfusetmp
     ```
   - Mount Blob Container:
     ```bash
     sudo blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
     ```

## Commands and Tools
- **Wget**: Used to download files from public links.
- **df -h**: Used to check if there is any mounted volume.

## Limitations of Blob Container Mounting
1. It is not persistent. The mount is removed after a reboot.
2. The volume of the container cannot be increased.

## Re-mounting Blob Container
To re-mount the Blob container after a reboot:

```bash
sudo mkdir /mnt/ramdisk
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
sudo blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

## Persistent Mounting
To make the mounting of Azure Blob Storage or Azure Files persistent across reboots, you need to automate the mounting process by configuring your virtual machine to run the mounting commands at startup. This can be achieved by adding the necessary commands to the VM's startup scripts or system service files.

### Persistent Mounting of Azure Blob Storage

For Azure Blob Storage, you can use `blobfuse` to mount the storage. To make the mount persistent, you need to ensure that the mount command runs every time the VM starts.

#### Steps to Make Blob Storage Mount Persistent

1. **Install Blobfuse**:
   Make sure `blobfuse` is installed on your VM.

   ```bash
   sudo apt-get update
   sudo apt-get install blobfuse
   ```

2. **Create Mount Script**:
   Create a script that mounts the blob storage. Save this script in a location like `/usr/local/bin/mount_blob.sh`.

   ```bash
   sudo nano /usr/local/bin/mount_blob.sh
   ```

   Add the following content to the script:

   ```bash
   #!/bin/bash
   sudo mkdir -p /mnt/ramdisk
   sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
   sudo mkdir -p /mnt/ramdisk/blobfusetmp
   sudo chown <youruser> /mnt/ramdisk/blobfusetmp
   sudo mkdir -p /mnt/mycontainer
   sudo blobfuse /mnt/mycontainer --tmp-path=/mnt/ramdisk/blobfusetmp --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
   ```

   Replace `<youruser>` with your username and `/path/to/fuse_connection.cfg` with the path to your `blobfuse` configuration file.

3. **Make the Script Executable**:
   
   ```bash
   sudo chmod +x /usr/local/bin/mount_blob.sh
   ```

4. **Configure Startup Script**:
   Add the mount script to the VM's startup process. For Ubuntu, you can use `crontab`.

   ```bash
   sudo crontab -e
   ```

   Add the following line to the crontab file to run the script at reboot:

   ```bash
   @reboot /usr/local/bin/mount_blob.sh
   ```

### Persistent Mounting of Azure Files

For Azure Files, you can add an entry to the `/etc/fstab` file to ensure the file share is mounted automatically at boot.

#### Steps to Make Azure Files Mount Persistent

1. **Obtain the Storage Account Key**:
   Get the storage account name and key from the Azure portal.

2. **Create Mount Point**:
   
   ```bash
   sudo mkdir -p /mnt/myfileshare
   ```

3. **Edit fstab**:
   Add an entry to the `/etc/fstab` file.

   ```bash
   sudo nano /etc/fstab
   ```

   Add the following line to the file:

   ```bash
   //<storage-account-name>.file.core.windows.net/<file-share-name> /mnt/myfileshare cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,sec=ntlmssp 0 0
   ```

   Replace `<storage-account-name>`, `<file-share-name>`, and `<storage-account-key>` with your storage account details.

4. **Test the fstab Entry**:
   Test the `fstab` entry to ensure it mounts correctly.

   ```bash
   sudo mount -a
   ```

## Resource Group Locks
Resource group locks prevent accidental deletion or modification of resources. There are two types of locks:
1. **Read-Only**: Prevents changes to resources but allows reading.
2. **Delete**: Prevents deletion of resources.

## Conclusion
Mounting Azure Blob Storage and Azure Files to your virtual machines simplifies data management and enhances collaboration. By following the steps outlined, you can leverage Azure’s storage capabilities efficiently in your applications.

---

This documentation provides a comprehensive guide to mounting Azure Blob Storage and Azure Files using the Azure portal, including examples and diagrams for better understanding.
