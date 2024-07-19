# Day 7: VPN, Point-to-Site VPN, and Gateway Transit

## Table of Contents
1. [Overview](#overview)
2. [VPN (Virtual Private Network)](#vpn-virtual-private-network)
3. [Point-to-Site VPN](#point-to-site-vpn)
    - [How P2S VPN Works](#how-p2s-vpn-works)
    - [Advantages](#advantages)
4. [Gateway Transit](#gateway-transit)
5. [Steps to Create and Configure](#steps-to-create-and-configure)
    - [Create a Virtual Network (VNet)](#create-a-virtual-network-vnet)
    - [Create a Virtual Network Gateway](#create-a-virtual-network-gateway)
    - [Configure Point-to-Site VPN](#configure-point-to-site-vpn)
    - [Download VPN Client](#download-vpn-client)
    - [Install and Connect](#install-and-connect)
6. [Example Scenarios](#example-scenarios)
    - [Example: Point-to-Site (P2S) VPN in Azure](#example-point-to-site-p2s-vpn-in-azure)
7. [VPN Protocols](#vpn-protocols)
    - [OpenVPN](#openvpn)
    - [Secure Socket Tunneling Protocol (SSTP)](#secure-socket-tunneling-protocol-sstp)
    - [IKEv2 VPN](#ikev2-vpn)

## Overview 
In this section, we will explore the concepts of VPN (Virtual Private Network), Point-to-Site VPN, and Gateway Transit. We will also go through the steps to create and configure these features in Azure using the Azure portal.

## VPN (Virtual Private Network)
### What is a VPN?
A Virtual Private Network (VPN) is a secure connection between two or more devices over the internet. It allows data to be transmitted securely as if the devices were connected through a private network.

### Benefits of VPN
- **Security**: Encrypts data, providing a secure connection over the internet.
- **Remote Access**: Enables users to access their organization's network from remote locations.
- **Cost-Effective**: Reduces the need for expensive dedicated private networks.

## Point-to-Site VPN
### What is a Point-to-Site VPN?
A Point-to-Site (P2S) VPN allows individual clients to connect to an Azure Virtual Network from a remote location, such as a home or office, using a secure connection.

### How P2S VPN Works
1. **Client Initiated**: The connection is initiated from the client computer, unlike a Site-to-Site (S2S) VPN, which connects entire networks.
2. **Secure Connection**: It establishes a secure connection over the internet to the Azure VNet.
3. **Individual Access**: Each client machine has its own connection, providing flexibility and control over who can connect.

### Advantages
- **Easy to set up for individual users**.
- **Doesn't require a VPN device or public IP on the user’s side**.
- **Good for remote access, especially for mobile users**.

To set up a P2S VPN:
1. **Create a Virtual Network (VNet)**.
2. **Create a Virtual Network Gateway**.
3. **Configure Point-to-Site VPN**.
4. **Download VPN Client**.
5. **Install and Connect**.

### Detailed Steps
1. **Create a Virtual Network (VNet)**
    - **Step 1**: Go to the Azure portal and click on "Create a resource".
    - **Step 2**: Select "Networking" and then "Virtual Network".
    - **Step 3**: Fill in the required fields:
        - **Name**: Give your VNet a unique name.
        - **Address Space**: Define the IP address range for the VNet (e.g., 10.0.0.0/16).
        - **Subnet**: Create at least one subnet within the VNet (e.g., 10.0.0.0/24).
        - **Resource Group**: Select an existing resource group or create a new one.
    - **Step 4**: Click "Create".

2. **Create a Virtual Network Gateway**
    - **Step 1**: Navigate to "Create a resource", select "Networking", and then "Virtual Network Gateway".
    - **Step 2**: Fill in the details:
        - **Name**: Provide a unique name for the gateway.
        - **Region**: Select the region where you want to deploy the gateway.
        - **Gateway Type**: Choose "VPN".
        - **VPN Type**: Select "Route-based".
        - **SKU**: Choose a SKU based on your performance and pricing needs.
        - **Virtual Network**: Select the VNet you created earlier.
        - **Public IP Address**: Create a new public IP address for the gateway.
    - **Step 3**: Click "Create".

3. **Configure Point-to-Site VPN**
    - **Step 1**: Once the Virtual Network Gateway is created, go to its settings.
    - **Step 2**: Select "Point-to-site configuration" and then "Configure now".
    - **Step 3**: Fill in the configuration details:
        - **Address Pool**: Specify the IP address range that will be assigned to VPN clients (e.g., 172.16.0.0/24).
        - **Tunnel Type**: Choose "OpenVPN".
        - **Authentication Type**: Select the type of authentication you want to use (certificate-based or Azure Active Directory). For certificate-based authentication:
            - **Root Certificate**: Generate and upload the root certificate.
            - **Client Certificate**: Generate and install client certificates on the client machines.
    - **Step 4**: Click "Save".

4. **Download VPN Client**
    - **Step 1**: After saving the configuration, download the VPN client from the "Point-to-site configuration" page.
    - **Step 2**: Click "Download VPN client" and choose the appropriate version for your operating system (Windows, macOS, etc.).

5. **Install and Connect**
    - **Step 1**: Install the VPN client on your machine.
    - **Step 2**: Run the client and enter the necessary details (username, password, or certificate).
    - **Step 3**: Connect to the VPN to establish a secure connection to the Azure VNet.

## Gateway Transit
### What is Gateway Transit?
Gateway Transit allows a peered virtual network to utilize the VPN gateway of another virtual network. This feature is useful for centralizing VPN gateway management and reducing costs.

### Benefits of Gateway Transit
- **Centralized Management**: Simplifies management by using a single VPN gateway for multiple VNets.
- **Cost Efficiency**: Reduces the need to deploy multiple VPN gateways.
- **Simplified Network Architecture**: Streamlines the network design by reducing the number of gateways needed.

## Example Scenarios

### Example: Point-to-Site (P2S) VPN in Azure

#### Scenario
Imagine you are a freelance web developer working from various locations (home, coffee shops, etc.). You have a Virtual Machine (VM) and a database hosted in Azure that you need to access securely from your laptop, no matter where you are.

#### Goal
The goal is to connect your laptop to the Azure Virtual Network (VNet) securely, allowing you to access the VM and database as if you were connected to a local network.

#### How Point-to-Site (P2S) VPN Works
1. **Your Laptop**: This is your device that you use to work from various locations.
2. **Azure Virtual Network (VNet)**: This is the network in Azure where your VM and database are hosted, e.g., 10.0.0.0/16.
3. **VPN Gateway in Azure**: This is a gateway device in Azure that facilitates the secure connection from your laptop.


#### Simple Example
Let's say you are at a coffee shop:
1. **At the Coffee Shop**: You open your laptop and need to access your Azure VM and database.
2. **Connect to the VPN**: You open the VPN client on your laptop and click "Connect". The client uses the VPN configuration you downloaded from Azure.
3. **Secure Connection**: A secure, encrypted tunnel is established between your laptop and the Azure VPN Gateway.
4. **Access Resources**: You can now access your Azure VM and database securely, as if you were in a private office network.

By using a P2S VPN, you can securely connect to your Azure resources from anywhere, ensuring that your data and connections are protected while you work remotely.

## VPN Protocols
### OpenVPN
#### In-Depth Explanation
OpenVPN is a popular open-source VPN protocol that uses SSL/TLS for key exchange. It's highly versatile and supports a wide range of configurations, from basic point-to-point to complex site-to-site VPNs.

#### Key Characteristics
- **Open Source**: Free and customizable, allowing users to adapt it to their specific needs.
- **Secure**: Uses SSL/TLS for encryption, ensuring that data is protected during transmission.
- **Cross-Platform**: Works on various operating systems, including Windows, macOS, Linux, iOS, and Android.

#### Use Cases
- **Secure Remote Access**: Provides secure remote access to a private network.
- **Site-to-Site VPN**: Connects entire networks securely over the internet.
- **Mobile VPN**: Allows mobile users to connect securely from their devices.

### Secure Socket Tunneling Protocol (SSTP)
SSTP is a proprietary TLS-based VPN protocol developed by Microsoft. It uses TCP port 443, making it a reliable option for traversing firewalls and NATs.

#### Key Characteristics
- **Proprietary**: Developed by Microsoft and supported on Windows devices.
- **TLS-Based**: Uses TLS for encryption, providing strong security.
- **Firewall-Friendly**: Operates over TCP port 443, making it effective in restrictive network environments.

#### Use Cases
- **Windows Devices**: Best suited for environments where Windows devices are predominant.
- **Firewall/NAT Traversal**: Effective for environments with strict firewall or NAT configurations.

### IKEv2 VPN
IKEv2 (Internet Key Exchange version 2) is a standards-based VPN protocol that uses IPsec for secure key exchange and encryption. It is known for its reliability and ability to maintain a stable connection.

#### Key Characteristics
- **Standards-Based**: Developed by Microsoft and Cisco, and widely supported across various platforms.
- **Secure**: Uses IPsec for encryption, ensuring data protection.
- **Reliable**: Maintains a stable connection, even when switching networks.

#### Use Cases
- **Cross-Platform Compatibility**: Supported on various operating systems, including Windows, macOS, iOS, and Android.
- **Mobile Users**: Ideal for mobile users who require a stable and secure connection.

By following these steps and understanding these concepts, you can effectively set up and utilize Point-to-Site VPNs in Azure, ensuring secure remote access to your resources.
