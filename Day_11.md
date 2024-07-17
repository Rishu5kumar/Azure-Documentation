# Day 11: Standard Load Balancer and Outbound Rule Configuration

## Table of Contents

1. [Standard Load Balancer](#standard-load-balancer)
   - [Introduction to Load Balancer](#introduction-to-load-balancer)
   - [Key Concepts of Standard Load Balancer](#key-concepts-of-standard-load-balancer)
   - [Use Cases](#use-cases)
   - [Diagram: Standard Load Balancer Architecture](#diagram-standard-load-balancer-architecture)
   - [Steps to Create a Standard Load Balancer](#steps-to-create-a-standard-load-balancer)

2. [Outbound Rule Configuration](#outbound-rule-configuration)
   - [Introduction to Outbound Rules](#introduction-to-outbound-rules)
   - [Key Concepts of Outbound Rules](#key-concepts-of-outbound-rules)
   - [Use Cases](#use-cases-1)
   - [Example: Configuring Outbound Rules](#example-configuring-outbound-rules)
   - [Diagram: Outbound Rule Configuration](#diagram-outbound-rule-configuration)

---

## Standard Load Balancer

### Introduction to Load Balancer

A **Load Balancer** distributes incoming network traffic across multiple servers to ensure efficient utilization, high availability, and reliability of applications. There are two primary types in Azure:

### Key Concepts of Standard Load Balancer

- **Traffic Distribution**: Routes traffic to backend instances based on rules and health probes.
- **Availability Zones**: Supports deployment across multiple availability zones for fault tolerance.
- **Inbound and Outbound Flows**: Handles both incoming and outgoing traffic efficiently.
- **Requirements**:
**VM SKU**: The VMs must have a Standard SKU.
**Static IP**: The VMs must have a static IP address.

### Use Cases

- **High Traffic Web Applications**: Distributes incoming HTTP/HTTPS requests across multiple web servers.
- **Scalable Backend Services**: Provides redundancy and scalability for backend APIs and databases.

### Diagram: Standard Load Balancer Architecture

```
+-------------------------+       +-------------------------+
|     Internet            |       |     Internet            |
|         |               |       |         |               |
|  +------+-----+         |       |  +------+-----+         |
|  |   Public   |         |       |  |   Public   |         |
|  |   IP 1     |         |       |  |   IP 2     |         |
|  +-----+------+         |       |  +-----+------+         |
|        |                |       |        |                |
|  +-----+------+         |       |  +-----+------+         |
|  |  Standard  |         |       |  |  Standard  |         |
|  | Load Balancer|       |       |  | Load Balancer|       |
|  +-----+------+         |       |  +-----+------+         |
|        |                |       |        |                |
|  +-----+------+         |       |  +-----+------+         |
|  |  Backend   |         |       |  |  Backend   |         |
|  |  Pool 1    |         |       |  |  Pool 2    |         |
|  +-----+------+         |       |  +-----+------+         |
|        |                |       |        |                |
|  +-----+------+         |       |  +-----+------+         |
|  |   VM 1     |         |       |  |   VM 2     |         |
|  +-------------+        |       |  +-------------+        |
+-------------------------+       +-------------------------+
         VM level                       Subnet level

```

### Steps to Create a Standard Load Balancer

1. **Create a Standard Load Balancer Resource**:
   - Navigate to Azure Portal > Create a resource > Search for "Load Balancer" > Select "Standard Load Balancer" > Click "Create".

2. **Configure Load Balancer Settings**:
   - Define load balancer name, SKU (Standard), subscription, resource group, and region.

3. **Define Frontend IP Configuration**:
   - Assign a public IP address to the load balancer for incoming traffic.

4. **Configure Backend Pool**:
   - Add virtual machines (VMs) or virtual machine scale sets (VMSS) to the backend pool.

5. **Set Health Probes**:
   - Define health probes to monitor the health of backend instances.

6. **Create Load Balancing Rules**:
   - Specify rules to distribute incoming traffic based on protocol (HTTP, HTTPS, TCP) and port numbers.

7. **Review and Create**:
   - Review all configurations and create the load balancer.

---

## Outbound Rule Configuration

### Introduction to Outbound Rules

Outbound rules in Azure Load Balancer control how outbound traffic is handled from backend resources to the internet or other Azure services.

### Key Concepts of Outbound Rules

- **Source IP Address**: Specifies the source IP address range for outbound traffic.
- **Destination IP Address**: Defines the destination IP addresses or IP ranges for outbound traffic.
- **Port Ranges**: Determines the allowed outbound port ranges for communication.
- **Session Persistence**: Ensures that outbound traffic from a backend resource maintains the same session.

### Use Cases

- **Internet Access**: Allows VMs to access resources on the internet securely.
- **Azure Services**: Enables communication between VMs and other Azure services like Azure Storage or Azure SQL Database.

### Example: Configuring Outbound Rules

1. **Create Outbound Rule**:
   - Specify a name for the outbound rule.
   - Define the source IP address and port ranges.
   - Set the destination IP address and port ranges.
   - Choose the action (Allow or Deny) for outbound traffic.

2. **Apply Outbound Rule**:
   - Attach the outbound rule to the backend pool of the Standard Load Balancer.

### Diagram: Outbound Rule Configuration

```
+---------------------+
|  Outbound Rule      |
|  Configuration      |
|                     |
|  Source IP:         |
|  10.0.0.0/24        |
|                     |
|  Destination IP:    |
|  0.0.0.0/0 (Any)    |
|                     |
|  Port Range:        |
|  1024-65535         |
|                     |
|  Action: Allow      |
+---------------------+
```

---

This markdown file provides a detailed explanation of Standard Load Balancer and Outbound Rule Configuration in Azure, including concepts, examples, diagrams, and steps to create both components.
