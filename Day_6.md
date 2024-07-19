# Day 6: VNet Peering

## Table of Contents
1. [Overview](#overview)
2. [Benefits of VNet Peering](#benefits-of-vnet-peering)
3. [Key Concepts and Terms](#key-concepts-and-terms)
4. [Steps to Create and Configure VNet Peering](#steps-to-create-and-configure-vnet-peering)
    - [Creating a Virtual Network](#creating-a-virtual-network)
    - [Configuring VNet Peering](#configuring-vnet-peering)
5. [Example Scenarios](#example-scenarios)
 
## Overview
VNet Peering in Azure allows you to connect two virtual networks (VNets) seamlessly. This connection enables resources in different VNets to communicate with each other as if they were part of the same network, providing a secure and high-speed link.

## Benefits of VNet Peering
- **Low Latency and High Bandwidth**: Provides direct, low-latency connectivity between VNets.
- **Cost-Effective**: No extra charges for the bandwidth used for peered traffic within the same region.
- **Secure Communication**: Traffic between peered VNets is private and isolated from the public internet.
- **Resource Sharing**: Allows different departments or projects to share resources securely.

## Key Concepts and Terms
- **Virtual Network (VNet)**: A private network in Azure where you can place your resources such as VMs, databases, and more.
- **VNet Peering**: A connection between two VNets that allows resources to communicate across them.
- **Remote VNet**: The VNet you are peering with from your local VNet.
- **Peering Link**: The actual connection established between two VNets.

## Steps to Create and Configure VNet Peering

### Creating a Virtual Network
1. **Go to the Azure Portal**: Open the Azure portal (https://portal.azure.com).
2. **Navigate to Virtual Networks**: Search for "Virtual Networks" in the search bar and select it.
3. **Create VNet**:
    - Click on "Create" and fill in the required details:
        - **Name**: Provide a name for your VNet.
        - **Address Space**: Define the address space using CIDR notation (e.g., 10.0.0.0/16).
        - **Resource Group**: Choose an existing resource group or create a new one.
        - **Location**: Select the Azure region.
    - **Create Subnets**:
        - Add a subnet by specifying a name and address range (e.g., 10.0.1.0/24 for a subnet).
    - Click "Create" to create the VNet.

### Configuring VNet Peering
1. **Navigate to Virtual Networks**: Go to the first VNet that you want to peer.
2. **Peering**: Under "Settings," click on "Peerings" and then click "Add."
3. **Configure Peering**:
    - **Name**: Provide a name for the peering.
    - **Peering Link**: This is the name for the peering link from your local VNet to the remote VNet.
    - **Remote Virtual Network**: Select the second VNet you want to peer with.
    - **Peering Link Name**: Provide a name for the peering link from the remote VNet back to your local VNet.
    - **Traffic Forwarding**: Configure the traffic forwarding settings as needed (e.g., allowing traffic between the two VNets).
    - **Network Access**: Choose whether to allow or disallow network access from one VNet to the other.
4. **Create Peering**: Click "OK" to establish the peering connection.

5. **Configure Peering on the Remote VNet**:
    - Navigate to the second VNet and repeat the above steps to configure peering from the second VNet to the first VNet.
    - Ensure that both VNets have peering configurations pointing to each other.

## Example Scenarios

### Example 1: Connecting Development and Production Environments
Imagine you have two VNets: one for development and one for production. By peering these VNets, developers can easily deploy applications in the development environment and later migrate them to production without network reconfiguration.

### Example 2: Resource Sharing Across Departments
Different departments within an organization may have their own VNets. VNet peering allows these departments to share resources, such as databases and storage accounts, securely without exposing them to the public internet.

---

This markdown file provides a detailed explanation of VNet Peering, including the benefits, key concepts, and steps to create and configure VNet Peering in a simple and straightforward manner. The examples illustrate practical use cases to help understand the concept better.
