# Day 4: Azure Disk, Dynamic IP, and Static IP

## Table of Contents
1. [Azure Disk](#azure-disk)
    - [Overview](#overview)
    - [Types of Azure Disks](#types-of-azure-disks)
    - [Creating an Azure Disk](#creating-an-azure-disk)
2. [Dynamic IP](#dynamic-ip)
    - [Overview](#dynamic-ip-overview)
    - [Benefits of Dynamic IP](#benefits-of-dynamic-ip)
    - [Setting Up a Dynamic IP in Azure](#setting-up-a-dynamic-ip-in-azure)
3. [Static IP](#static-ip)
    - [Overview](#static-ip-overview)
    - [Benefits of Static IP](#benefits-of-static-ip)
    - [Setting Up a Static IP in Azure](#setting-up-a-static-ip-in-azure)

## Azure Disk

### Overview
Azure Disks are a type of storage that provides high-performance, durable block storage for use with Azure Virtual Machines (VMs). They come in various sizes and performance levels to suit different workloads.

### Types of Azure Disks
1. **Standard HDD**: Cost-effective storage for applications with low I/O requirements.
2. **Standard SSD**: Better performance than Standard HDD with lower latency.
3. **Premium SSD**: High-performance, low-latency storage for mission-critical applications.
4. **Ultra Disk**: Ultra-high performance for data-intensive workloads.

### Creating an Azure Disk
1. **Go to the Azure Portal**: Open the Azure portal (https://portal.azure.com).
2. **Navigate to Disks**: Search for "Disks" in the search bar and select it.
3. **Create Disk**: Click on "Create" and fill in the necessary details such as name, resource group, region, and type of disk (Standard HDD, Standard SSD, Premium SSD, Ultra Disk).
4. **Attach to VM**: After creating the disk, navigate to your VM, go to the "Disks" section, and click "Add data disk." Select your newly created disk and save.

## Dynamic IP

### Dynamic IP Overview
A Dynamic IP is an IP address that can change every time you restart a VM. Azure assigns a new IP from its pool of available addresses.

### Benefits of Dynamic IP
- **Cost-Effective**: Generally cheaper than static IPs.
- **Automatic Allocation**: Automatically allocated and managed by Azure.

### Setting Up a Dynamic IP in Azure
1. **Go to the Azure Portal**: Open the Azure portal (https://portal.azure.com).
2. **Navigate to Virtual Machines**: Search for "Virtual Machines" in the search bar and select it.
3. **Select Your VM**: Choose the VM you want to assign a dynamic IP.
4. **Networking**: Go to the "Networking" section.
5. **IP Configurations**: Under "Network Interface," click on the IP configuration name.
6. **IP Allocation**: Ensure "Assignment" is set to "Dynamic" and save.

## Static IP

### Static IP Overview
A Static IP is an IP address that remains constant regardless of VM restarts. Azure assigns this IP address permanently to your resource until you explicitly release it.

### Benefits of Static IP
- **Consistency**: The IP address does not change, making it ideal for DNS settings and firewall rules.
- **Reliability**: Ensures stable connectivity for applications requiring a fixed IP.

### Setting Up a Static IP in Azure
1. **Go to the Azure Portal**: Open the Azure portal (https://portal.azure.com).
2. **Navigate to Virtual Machines**: Search for "Virtual Machines" in the search bar and select it.
3. **Select Your VM**: Choose the VM you want to assign a static IP.
4. **Networking**: Go to the "Networking" section.
5. **IP Configurations**: Under "Network Interface," click on the IP configuration name.
6. **IP Allocation**: Change "Assignment" to "Static" and provide the desired IP address.
7. **Save**: Click "Save" to apply the changes.

---

This readme file covers the essentials of Azure Disks, Dynamic IPs, and Static IPs, providing a clear and straightforward explanation along with steps to create and configure them in Azure.
