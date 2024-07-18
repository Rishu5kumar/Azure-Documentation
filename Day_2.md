## Day 2: IT Process Management and Introduction to DevOps

### Topics Covered:
1. [IT Process Management](#it-process-managament)
2. [Introduction to DevOps](#introduction-to-devops)
3. [Networking](#networking)

### IT Process Management
IT Process Management involves planning, organizing, and managing IT resources to achieve organizational goals. It ensures efficient and effective IT operations.

**Key Processes:**
- **Incident Management:** Handling incidents to restore normal service operation.
- **Change Management:** Managing changes to minimize disruption.
- **Problem Management:** Identifying and resolving root causes of incidents.
- **Service Desk:** Central point of contact for IT support.

### Introduction to DevOps
DevOps combines development (Dev) and operations (Ops) to improve collaboration and productivity by automating workflows and continuously measuring application performance.

**Key Principles:**
1. **Collaboration:** Bridging the gap between development and operations teams.
2. **Automation:** Automating repetitive tasks for faster delivery.
3. **Continuous Integration:** Merging code changes frequently.
4. **Continuous Delivery:** Ensuring code is always in a deployable state.
5. **Monitoring:** Continuously monitoring application performance and infrastructure.

**Benefits of DevOps:**
- Faster Time to Market
- Improved Collaboration
- Higher Quality Software
- Increased Efficiency
- Better Security

**DevOps Tools:**
- **Version Control:** Git, GitHub
- **CI/CD:** Jenkins, Azure DevOps
- **Configuration Management:** Ansible, Puppet
- **Containerization:** Docker, Kubernetes
- **Monitoring:** Prometheus, Nagios

### Azure Cloud and Its Capabilities

Azure supports various operating systems, programming languages, frameworks, databases, and devices, allowing a broad range of options for building, deploying, and managing applications. It is widely used by businesses for its scalability, flexibility, and integration with other Microsoft products and services.

**Hosting a Website:**
When a company builds a website and wants it to be accessible to everyone, they need a server to deploy it and make it publicly available. This server is converted into a web server by installing necessary tools, and the application data is deployed on it. The server has a unique IP address mapped with a domain name, so when the domain is accessed, it resolves to the IP address of the server, making the application accessible to users.

To handle high traffic, load balancers distribute incoming traffic across multiple servers, preventing any single server from getting overwhelmed. This ensures that the website remains accessible even during peak times.

**Cloud Computing Solution:**
Cloud computing resolves the issue of underutilized resources by renting unused resources to others. Hypervisors virtualize physical systems, enabling access to computing resources from anywhere. Azure, AWS, and other cloud providers open data centers worldwide, offering virtualized resources accessible over the internet.

### Networking in Azure Cloud

Networking is the technique of connecting computers using networking devices to share resources. Network service providers like Airtel, Jio, and Idea provide higher bandwidth for faster connections to remote computers.

**Key Networking Devices:**
1. **Hub:** A non-intelligent, broadcasting device with a single collision domain.
2. **Switch:** An intelligent device with multiple collision domains for private resource sharing.
3. **Router:** Connects different networks, enabling communication between them.

**Benefits of Networking:**
- Saves resource cost
- Allows resource sharing

### Azure Cloud Networking Components

1. **Virtual Network (VNet):**
   - A VNet is a private network in the Azure cloud for placing virtual machines and other resources.
   - It acts like a private neighborhood where all your devices can securely communicate.

2. **Subnets:**
   - Subnets are smaller sections within a VNet.
   - They help organize and secure resources by grouping them together.

**Example:**
A company network (192.168.0.0/16) divided into subnets:
- Subnet 1 (192.168.1.0/24)
- Subnet 2 (192.168.2.0/24)
- Subnet 3 (192.168.3.0/24)

Each subnet can contain up to 254 devices with unique IP addresses.

3. **Network Security Groups (NSGs):**
   - NSGs control incoming and outgoing traffic to resources in a VNet.
   - They ensure only authorized users or services can access the resources.

4. **Azure Load Balancer:**
   - Distributes incoming traffic across multiple servers to maintain performance and availability.

5. **Virtual Private Network (VPN):**
   - A secure connection from an on-premises network to an Azure VNet.
   - Acts like a secure tunnel between your home office and your Azure network.

6. **ExpressRoute:**
   - A private connection from your premises to Azure, bypassing the public internet.
   - Provides more reliability and faster speeds.

### Networking Protocols in Azure

Protocols facilitate communication between services and resources.

1. **HTTP/HTTPS:**
   - HTTP (Hypertext Transfer Protocol): Used for accessing web services and applications.
   - HTTPS (Hypertext Transfer Protocol Secure): An encrypted version of HTTP for secure communication.

2. **TCP/IP (Transmission Control Protocol/Internet Protocol):**
   - TCP: Ensures reliable, ordered, and error

-checked delivery of data.
   - IP: Delivers data packets from source to destination based on IP addresses.

3. **UDP (User Datagram Protocol):**
   - A connectionless protocol for applications where speed is critical, such as streaming services.

4. **DNS (Domain Name System):**
   - Translates domain names into IP addresses for identifying and communicating between resources.

5. **SMTP (Simple Mail Transfer Protocol):**
   - Used for sending emails.

6. **SSH (Secure Shell):**
   - A protocol for securely accessing and managing VMs over an unsecured network.

7. **RDP (Remote Desktop Protocol):**
   - Used for remote access to Windows VMs.

8. **ICMP (Internet Control Message Protocol):**
   - Used for diagnostic and error-reporting purposes, such as the ping command.

**Applications in Azure:**
- **HTTP/HTTPS:** Used by Azure App Services for hosting web applications.
- **TCP/IP and UDP:** Fundamental for network communications in Azure.
- **DNS:** Managed by Azure DNS to resolve domain names to IP addresses.
- **SMTP:** Used by Azure Logic Apps and Azure Functions for sending emails.
- **SSH:** Used to securely connect to Linux VMs.
- **RDP:** Used to remotely manage Windows VMs.
- **ICMP:** Used for network diagnostics and monitoring.

In summary, networking in Azure cloud is about creating and managing virtual networks that connect and protect your cloud resources, ensuring they can communicate securely and efficiently. Protocols facilitate secure, reliable, and efficient communication between services and resources, enabling a wide range of functionalities and applications in the cloud environment.

---
