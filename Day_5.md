# Day 5: Capturing Images and VNet Deep Dive
 
## Table of Contents
1. [Capturing Images](#capturing-images)
    - [Overview](#overview)
    - [Steps to Capture an Image](#steps-to-capture-an-image)
2. [VNet Deep Dive](#vnet-deep-dive)
    - [Overview](#vnet-overview)
    - [Subnets](#subnets)
    - [Network Security Groups (NSGs)](#network-security-groups-nsgs)
    - [VNet Peering](#vnet-peering)
    - [Steps to Create and Configure VNets](#steps-to-create-and-configure-vnets)

## Capturing Images

### Overview
Capturing an image in Azure allows you to create a reusable template of a virtual machine (VM). This image can be used to deploy multiple identical VMs, ensuring consistency across your deployments.

### Steps to Capture an Image
1. **Go to the Azure Portal**: Open the Azure portal (https://portal.azure.com).
2. **Navigate to Virtual Machines**: Search for "Virtual Machines" in the search bar and select it.
3. **Select the VM**: Choose the VM you want to capture an image from.
4. **Deallocate the VM**: Go to the "Overview" section and click "Stop" to deallocate the VM.
5. **Capture the Image**:
    - After the VM is deallocated, click on "Capture" at the top of the VM's page.
    - Provide the necessary details such as the name of the image and the resource group.
    - Check the option to automatically delete the VM after the image is captured if you do not need the original VM anymore.
    - Click "Create."

## VNet Deep Dive

### VNet Overview
A Virtual Network (VNet) is a fundamental building block for your private network in Azure. It enables many types of Azure resources, such as Azure Virtual Machines (VMs), to communicate with each other, the internet, and on-premises networks securely.

### Subnets
Subnets are segments of a VNet that allow you to break the network into smaller, more manageable sections. This helps organize and secure resources.

#### Example:
Imagine a company network that has different departments like HR, IT, and Finance. You can create separate subnets for each department within a VNet to isolate and manage traffic effectively.

### Network Security Groups (NSGs)
NSGs are used to control network traffic to and from Azure resources within a VNet. They contain security rules that allow or deny inbound and outbound traffic.

#### Example:
You can create an NSG that only allows HTTP and HTTPS traffic to your web servers while blocking other types of traffic.

### VNet Peering
VNet Peering connects two VNets, enabling resources in different VNets to communicate with each other. This is useful for linking different environments like development and production.

#### Example:
If you have separate VNets for different projects, peering allows them to share resources without exposing the entire network to the internet.

### Steps to Create and Configure VNets

#### Create a VNet
1. **Go to the Azure Portal**: Open the Azure portal (https://portal.azure.com).
2. **Navigate to Virtual Networks**: Search for "Virtual Networks" in the search bar and select it.
3. **Create VNet**: Click on "Create" and fill in the required details:
    - Name: Provide a name for your VNet.
    - Address Space: Define the address space using CIDR notation (e.g., 10.0.0.0/16).
    - Resource Group: Choose an existing resource group or create a new one.
    - Location: Select the Azure region.

4. **Create Subnets**: In the same VNet creation wizard:
    - Click on "Subnets."
    - Add a subnet by specifying a name and address range (e.g., 10.0.1.0/24 for a subnet).

5. **Create the VNet**: Review the configuration and click "Create."

---

This markdown file covers the essentials of capturing images and provides an in-depth look at VNets, including subnets, NSGs, and VNet peering. The explanations are designed to be straightforward and easy to understand, with step-by-step instructions for configuring each component in the Azure portal.
