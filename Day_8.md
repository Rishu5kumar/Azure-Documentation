# Day 8: Azure Site-to-Site VPN and Azure IPs

## Table of Contents

1. [Introduction](#introduction)
2. [Site-to-Site (S2S) VPN in Azure](#site-to-site-s2s-vpn-in-azure)
   - [How S2S VPN Works](#how-s2s-vpn-works)
   - [Key Components](#key-components)
   - [Example: Connecting a Company Office Network to Azure](#example-connecting-a-company-office-network-to-azure)
   - [Example: Connecting a Home Network to Azure](#example-connecting-a-home-network-to-azure)
   - [P2S vs S2S VPN Connectivity](#p2s-vs-s2s-vpn-connectivity)
3. [IP Address Types](#ip-address-types)
   - [Static IP Address](#static-ip-address)
   - [Dynamic IP Address](#dynamic-ip-address)
4. [SKU in Azure](#sku-in-azure)
   - [Definition and Usage](#definition-and-usage)
   - [Standard SKU](#standard-sku)
   - [Basic SKU](#basic-sku)
5. [IPv4 and IPv6](#ipv4-and-ipv6)
   - [IPv4](#ipv4)
   - [IPv6](#ipv6)
   - [Coexistence and Transition](#coexistence-and-transition)
6. [Azure IP](#azure-ip)
   - [Regional Public IP](#regional-public-ip)
   - [Global Public IP](#global-public-ip)
   - [Example Scenarios](#example-scenarios)

## Introduction

In this document, we will cover the details of setting up a Site-to-Site (S2S) VPN in Azure, the differences between IP address types, SKUs, and the differences between IPv4 and IPv6. We will also explain Azure IP types in detail.

## Site-to-Site (S2S) VPN in Azure

A Site-to-Site (S2S) VPN allows you to connect your on-premises network to an Azure Virtual Network (VNet) over a secure connection. This type of VPN is useful for creating a seamless and secure connection between your on-premises data center and Azure, enabling hybrid cloud scenarios.

### How S2S VPN Works

- **Persistent Connection:** Unlike Point-to-Site (P2S) VPN, S2S VPN provides a persistent connection between two networks.
- **Gateway-to-Gateway:** The connection is established between your on-premises VPN device (gateway) and the Azure VPN Gateway.
- **Secure Tunnel:** All traffic between the on-premises network and Azure VNet is encrypted, ensuring data security.

### Key Components

- **On-Premises VPN Device:** A compatible VPN device or firewall configured to connect to the Azure VPN Gateway.
- **Azure VPN Gateway:** A gateway in your Azure VNet that handles the VPN connection.

### Example: Connecting a Company Office Network to Azure

#### Scenario

Imagine you work for a company called "TechCorp". TechCorp has an office with an internal network (on-premises network) that all employees use to access company resources like file servers, printers, and databases. TechCorp also has some resources hosted in Azure, such as virtual machines (VMs) and databases, which they want to access from their office network securely.

#### Goal

The goal is to connect the TechCorp office network to the Azure Virtual Network (VNet) so that employees can access the Azure resources just as if they were part of the local office network.

#### How Site-to-Site (S2S) VPN Works

1. **On-Premises Network:** This is the local network within the TechCorp office. Let's say it uses the IP address range 192.168.1.0/24.
2. **Azure Virtual Network (VNet):** This is the network in Azure where TechCorp has deployed some of their resources. Let's say it uses the IP address range 10.1.0.0/16.
3. **VPN Gateway in Azure:** This is a gateway device in Azure that facilitates the secure connection to the on-premises network.
4. **VPN Device in TechCorp Office:** This is a compatible VPN device (like a firewall or router) that connects to the Azure VPN Gateway.

#### Step-by-Step Example

1. **Create a Virtual Network (VNet) in Azure**
   - TechCorp creates a VNet in Azure with the address range 10.1.0.0/16.
   - Inside this VNet, they might have subnets like 10.1.1.0/24 for VMs and 10.1.2.0/24 for databases.

2. **Create a Virtual Network Gateway in Azure**
   - TechCorp creates a Virtual Network Gateway in Azure and attaches it to the VNet. This gateway will handle the VPN connections.

3. **Configure the Local Network Gateway in Azure**
   - TechCorp configures a Local Network Gateway in Azure to represent their on-premises network. They enter the office network's IP range (192.168.1.0/24) and the public IP address of their on-premises VPN device (if the IP is static, but if IP is dynamic, they’ll take the FQDN).

4. **Create the VPN Connection in Azure**
   - TechCorp sets up a VPN connection between the Azure VPN Gateway and the Local Network Gateway. They use a pre-shared key (a shared secret) to secure the connection.

5. **Configure the On-Premises VPN Device**
   - TechCorp configures their office VPN device with the public IP address of the Azure VPN Gateway, the same pre-shared key, and appropriate IPsec/IKE settings to establish the VPN tunnel.

6. **Establish and Verify the Connection**
   - The VPN device in the TechCorp office establishes a secure tunnel to the Azure VPN Gateway. Now, all traffic between the on-premises network (192.168.1.0/24) and the Azure VNet (10.1.0.0/16) is encrypted and travels through this secure tunnel.
   - Employees in the TechCorp office can now access Azure resources (like VMs and databases) as if they were on the local network.


### P2S vs S2S VPN Connectivity

#### Visual Representation

```
+----------------------+                            +-----------------------+
| Home Network         |                            | Azure Virtual Network |
| (192.168.1.0/24)     |                            | (10.0.0.0/16)         |
|                      |   Secure VPN Tunnel        |                       |
| [ Devices ]---------[ Router ]-------------------[ VPN Gateway ]---------[ Azure Resources ] 
|                      |                            |                       |
+----------------------+                            +-----------------------+
```


#### Visual Representation of S2S

```
+----------------------------+                              +-----------------------------+
| TechCorp On-Premises       |                              | Azure Virtual Network (VNet)|
| Network (192.168.1.0/24)   |                              | (10.0.0.0/16)               |
|                            |                              |                             |
| [ VPN Device ]-------------|---- Secure VPN Tunnel -------|----[ VPN Gateway ]          |
|   (203.0.113.10)           |                              |                             |
+----------------------------+                              +-----------------------------+
         |                                                         |
         |                                                         |
  +----------------+                                         +-----------------+
  | Local Network  |                                         | Virtual Network |
  | Gateway        |---------------connection----------------| Gateway         |
  | (On-Premises)  |                                         | (Azure)         |
  +----------------+                                         +-----------------+
```

In this example, the Local Network Gateway ensures that Azure knows how to reach the on-premises network (192.168.1.0/24) via the public IP (203.0.113.10) of the on-premises VPN device. This setup is crucial for establishing and maintaining the secure Site-to-Site VPN connection between Azure and the on-premises network.

## IP Address Types

### Static IP Address

A static IP address is manually configured for a device and remains fixed. It does not change unless an administrator manually reconfigures it.

### Dynamic IP Address

A dynamic IP address is automatically assigned by a DHCP (Dynamic Host Configuration Protocol) server when a device connects to a network. It may change periodically based on DHCP lease duration and network policies.



## SKU in Azure

### Definition and Usage

SKU (Stock Keeping Unit) in Azure refers to the specific version or configuration of an Azure service or resource. Different SKUs offer varying levels of performance, features, and pricing.

### Standard SKU

The Standard SKU provides advanced features, higher performance, and better scalability compared to the Basic SKU. It is suitable for production workloads and enterprise scenarios.

### Basic SKU

The Basic SKU offers essential features and lower cost, making it suitable for development, testing, and small-scale applications.

## IPv4 and IPv6

### IPv4

IPv4 (Internet Protocol version 4) is the fourth version of the Internet Protocol and is the most widely used. It uses 32-bit addresses, which allows for approximately 4.3 billion unique addresses.

- **Address Format:** IPv4 addresses are written in dotted-decimal format, such as 192.168.1.1.
- **Address Exhaustion:** Due to the limited number of addresses, IPv4 address exhaustion has become a significant issue.

### IPv6

IPv6 (Internet Protocol version 6) is the successor to IPv4, designed to address the limitations of IPv4, including address exhaustion. It uses 128-bit addresses, which allows for a virtually unlimited number of unique addresses.

- **Address Format:** IPv6 addresses are written in hexadecimal format, separated by colons, such as 2001:0db8:85a3:0000:0000:8a2e:0370:7334.
- **Features:** IPv6 includes improvements in routing, security, and auto-configuration.

### Coexistence and Transition

To facilitate the transition from IPv4 to IPv6, several mechanisms exist to enable the coexistence of both protocols:

- **Dual Stack:** Devices and networks run both IPv4 and IPv6 simultaneously.
- **Tunneling:** IPv6 packets are encapsulated within IPv4 packets for transmission across IPv4 networks.
- **Translation:** Protocol translation between IPv4 and IPv6 allows for communication between devices using different protocols.

## Azure IP

Azure provides different types of public IP addresses to suit various needs. These IP addresses are used for virtual machines, load balancers, and other resources.

### Regional Public IP

- **Scope:** Regional public IP addresses are tied to a specific Azure region.
- **Usage:** They are typically used for resources that need to be accessible within a particular region.

### Global Public IP

- **Scope:** Global public IP addresses are not tied to any specific region and can be used across multiple regions.
- **Usage:** They are ideal for global applications and services that require consistent access across different regions.

### Example Scenarios

- **Web Application:** A web application deployed in multiple regions for high availability and performance might use a global public IP for consistent access.
- **Database Access:** A database server that needs to be accessed only within a specific region might use a regional public IP to limit access.

---

## Detailed Steps to Create S2S VPN and Azure IP

### Creating a Site-to-Site VPN

1. **Create a Virtual Network (VNet) in Azure**
   - Go to the Azure portal and create a new VNet.
   - Specify the address space (e.g., 10.1.0.0/16) and subnets.

2. **Create a Virtual Network Gateway**
   - Navigate to "Create a resource" > "Networking" > "Virtual network gateway".
   - Choose the VNet you created, and specify the gateway SKU (e.g., VpnGw1 for standard scenarios).
   - Wait for the gateway to be created.

3. **Create a Local Network Gateway**
   - Go to "Create a resource" > "Networking" > "Local network gateway".
   - Specify the on-premises network's address range (e.g., 192.168.1.0/24) and the public IP of the on-premises VPN device.

4. **Create the VPN Connection**
   - Navigate to your Virtual Network Gateway.
   - Go to "Connections" and add a new connection.
   - Choose "Site-to-site (IPsec)" as the connection type.
   - Select the Local Network Gateway and specify the shared key.

5. **Configure the On-Premises VPN Device**
   - Configure your on-premises VPN device with the Azure VPN Gateway's public IP, the shared key, and IPsec/IKE settings.

6. **Verify the Connection**
   - Ensure that the VPN connection is established and verify connectivity between the on-premises network and the Azure VNet.

### Creating an Azure IP

1. **Navigate to Public IP Addresses**
   - In the Azure portal, go to "Create a resource" > "Networking" > "Public IP address".

2. **Create a New Public IP**
   - Specify the name, SKU (Basic or Standard), and IP address type (Static or Dynamic).
   - Choose the region for a regional IP or leave it global for a global IP.

3. **Assign the Public IP to a Resource**
   - After creating the public IP, navigate to the resource (e.g., a VM or load balancer) and associate the public IP with it.

4. **Verify the IP Configuration**
   - Ensure that the resource is accessible using the assigned public IP address.

---

This document provides a comprehensive guide to setting up a Site-to-Site VPN, understanding different IP address types, SKUs in Azure, and the differences between IPv4 and IPv6. Additionally, it includes detailed steps to create and configure these components through the Azure portal.
