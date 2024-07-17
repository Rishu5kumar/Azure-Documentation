# Day 10: Understanding Azure Load Balancer and Basic Load Balancer

## Table of Contents
1. [Introduction](#introduction)
2. [Scaling Concepts](#scaling-concepts)
3. [Types of Load Balancers](#types-of-load-balancers)
4. [Key Differences](#key-differences)
5. [Working with Load Balancers](#working-with-load-balancers)
6. [Creating a Basic Load Balancer](#creating-a-basic-load-balancer)
7. [Availability Sets and Availability Zones](#availability-sets-and-availability-zones)
8. [Example Scenario](#example-scenario)
9. [Conclusion](#conclusion)

---

## Introduction
Azure Load Balancer is a networking device used to distribute incoming network traffic across multiple servers, ensuring high availability and reliability of applications. This distribution helps in achieving high availability, reliability, and scalability of applications hosted in Azure.

## Scaling Concepts
When demand increases on a server, two scaling options are available:
- **Vertical Scaling**: Increasing the resources (like CPU, RAM) of a single server.
- **Horizontal Scaling**: Creating multiple replicas of servers and distributing the load among them using a load balancer.

### Scaling in and Scaling out
- **Scaling In**: Reducing the number of servers or decreasing their configuration.
- **Scaling Out**: Increasing the number of servers or their configuration to handle more load.

## Types of Load Balancers
Azure offers two main types of Load Balancers:
1. **Basic Load Balancer**: Designed for small-scale applications without high availability requirements.
2. **Standard Load Balancer**: Ideal for applications needing high performance, ultra-low latency, and high availability.

## Key Differences
### Basic Load Balancer
- Suitable for small-scale applications.
- Supports inbound traffic only.
- Uses Basic SKU public IPs.
- SKU will be basic and dynamic.
- Does not support availability zones.

### Standard Load Balancer
- Supports both inbound and outbound traffic.
- Uses Standard SKU public IPs.
- SKU will be standard and static.
- Supports availability zones for high resiliency.
- Ideal for applications requiring scalability and high availability.

## Working with Load Balancers
Load Balancers manage traffic using:
- **Frontend Pool**: Public or private IP address that clients connect to. The frontend pool in an Azure Load Balancer serves as the entry point for incoming traffic, defining the public or private IP addresses clients use to connect. It directs this traffic to the backend pool of virtual machines based on load balancing rules, ensuring efficient distribution and high availability of services.
- 
- **Backend Pool**: Pool of virtual machines (VMs) that receive the traffic.
- **Health Probe**: Checks the health of VMs in the backend pool to ensure they can handle traffic.Load balancers use health probes to check the availability of VMs. If a VM is down, the load balancer stops sending traffic to it.
Example: HTTP health probe checks if the web server on a VM returns a 200 OK response.

- **Session Persistence**: Ensures that a user's session is consistently routed to the same VM.
Example: A user shopping on an e-commerce site continues to interact with the same VM during their session.

## When to Use Basic Load Balancer
Use Basic Load Balancer for applications where high availability and redundancy are not critical, and inbound traffic distribution is sufficient for the workload.

## Load Balancer Categorization
Load balancers can be categorized based on the traffic they handle:

- **Public Load Balancer**: Handles incoming traffic from the internet.
- **Private Load Balancer**: Manages internal traffic within Azure Virtual Networks (VNets), often used for horizontal scaling of databases where traffic flows from VMs to databases and back.

## Creating a Basic Load Balancer
To create a Basic Load Balancer in Azure, follow these steps:

1. **Navigate to the Azure Portal:**
   - Log in to the [Azure Portal](https://portal.azure.com).

2. **Create a Load Balancer Resource:**
   - Click on **Create a resource**.
   - Search for **Load Balancer** and select **Basic Load Balancer**.

3. **Configure Basic Load Balancer:**
   - Provide a name and select the appropriate subscription, resource group, and region.
   - Choose **Basic** SKU for the Load Balancer.
   - Configure the **Frontend IP configuration** with a public or private IP address.

4. **Define Backend Pool:**
   - Create or select an existing backend pool consisting of VM instances.
   - Ensure VMs are in the same availability set or virtual network.

5. **Set Up Health Probes:**
   - Define health probes to monitor the availability of backend VMs.
   - Specify the protocol, port, and interval for health checks.

6. **Configure Load Balancing Rules:**
   - Define load balancing rules to distribute traffic to backend VMs.
   - Specify the frontend port, backend port, protocol, and other settings.

7. **Review and Create:**
   - Review the configuration settings.
   - Click **Create** to deploy the Basic Load Balancer.

**Diagram:**

                     +---------------------------+
                     User/Client | Hit Public IP |
                     +-------------+-------------+
                                   |
                                   |
                     +-------------+-------------+
                     |     Frontend Pool          |
                     +----------------------------+
                                   |
                                   |
                            +------+------+
                            | Load Balancer|
                            +------+------+
                                   |
                      +------------+------------+
                      |                         |
             +--------+--------+       +--------+--------+
             |      Backend     |       |      Backend     |
             |       Pool       |       |       Pool       |
             |   (App Subnet)   |       |   (DB Subnet)    |
             +------------------+       +------------------+
                      |                         |
                      |                         |
     +----------------+----------------+  +----------------+----------------+
     |   Availability Set              |  |   Availability Set              |
     | +----------------------------+  |  | +----------------------------+  |
     | | VM 1     | VM 2     | VM 3  |  |  | | VM 4     | VM 5     | VM 6  |  |
     | +----------+----------+-------+  |  | +----------+----------+-------+  |
     +----------------------------------+  +----------------------------------+



**Diagram to illustrate the Load Balancer and scaling methods:**
                      +---------------------+
                      |     Load Balancer   |
                      |     (Public IP)     |
                      +---------+-----------+
                                |
          +---------------------+---------------------+
          |                     |                     |
    +-----+-----+         +-----+-----+         +-----+-----+
    |    VM 1   |         |    VM 2   |         |    VM 3   |
    |(Web Server)|         |(Web Server)|         |(Web Server)|
    +-----+-----+         +-----+-----+         +-----+-----+
          |                     |                     |
    +-----+-----+         +-----+-----+         +-----+-----+
    |   Health  |         |   Health  |         |   Health  |
    |   Probe   |         |   Probe   |         |   Probe   |
    +-----------+         +-----------+         +-----------+

Vertical Scaling:
-----------------

    +---------------------+
    |   Load Balancer     |
    |    (Public IP)      |
    +---------+-----------+
              |
      +-------+-------+
      |      VM 1     |      <- Larger VM
      | (Web Server)  |
      +-------+-------+
              |
      +-------+-------+
      |   Health      |
      |   Probe       |
      +---------------+

Horizontal Scaling:
-------------------

                      +---------------------+
                      |     Load Balancer   |
                      |     (Public IP)     |
                      +---------+-----------+
                                |
          +---------------------+---------------------+---------------------+
          |                     |                     |                     |
    +-----+-----+          +-----+-----+         +-----+-----+         +-----+-----+
    |    VM 1    |         |    VM 2    |        |    VM 3    |        |    VM 4    |
    |(Web Server)|         |(Web Server)|        |(Web Server)|        |(Web Server)|
    +-----+-----+          +-----+-----+         +-----+-----+         +-----+-----+
          |                     |                     |                     |
    +-------+-------+   +-------+-------+     +-------+-------+     +-------+-------+
    |   Health      |   |   Health      |     |   Health      |     |   Health      |
    |   Probe       |   |   Probe       |     |   Probe       |     |   Probe       |
    +---------------+   +---------------+     +---------------+     +-------+-------+




## Availability Sets and Availability Zones
### Availability Set
- **Definition**: An availability set is a logical grouping of VMs within an Azure data center that ensures your application remains available during planned or unplanned maintenance events, such as hardware failures or VM reboots.
- **Usage**: By distributing VMs across multiple fault domains within an availability set, Azure ensures that at least one VM remains available and accessible even if a physical server or rack fails.
- **Advantages**: Enhances reliability and availability of applications by minimizing downtime due to hardware maintenance or failures.

### Availability Zones
- **Definition**: Availability zones are physically separate data centers within an Azure region. Each availability zone is made up of one or more data centers equipped with independent power, cooling, and networking.
- **Usage**: Distributing applications across availability zones ensures high availability and resiliency. If one zone becomes unavailable due to an outage, applications can failover to resources in another zone without interruption.
- **Advantages**: Provides higher fault tolerance and ensures applications remain operational even during data center failures or maintenance events affecting a single zone.

## Example Scenario
Imagine a web application with multiple VM instances behind a Basic Load Balancer:
- Users connect to the Load Balancer's public IP.
- The Load Balancer distributes incoming requests across VM instances in the backend pool.
- Health probes monitor the VMs; if one VM fails, traffic is redirected to healthy VMs.

## Inbound NAT Rules
- Allow accessing individual VMs through the Load Balancer using specific port numbers.
When a Network Security Group (NSG) is not connected or attached to a VM, external access to that VM is restricted. Disassociating the public IP addresses from each VM replaces them with a single IP address from the Load Balancer.

Without individual public IPs, direct access to each VM for modifications or content updates becomes challenging. To address this, you can configure inbound NAT rules on the Load Balancer. These rules forward incoming traffic, based on IP address and port combinations, to specific virtual machines within your network.

By setting up inbound NAT rules, you can access each VM using the Load Balancer's IP address along with designated port numbers assigned to each VM during the NAT rule configuration process. This setup ensures efficient management and accessibility to VMs while leveraging the Load Balancer for traffic distribution and security.

## Conclusion
Azure Load Balancer and Basic Load Balancer play crucial roles in distributing traffic and ensuring application availability. By understanding their differences and configurations, you can effectively scale and manage your Azure applications.

---

This .md file provides an overview of Azure Load Balancer and Basic Load Balancer, explaining their roles, types, key features, and deployment steps. It also covers availability sets and availability zones, essential for ensuring high availability and resilience of Azure applications.
