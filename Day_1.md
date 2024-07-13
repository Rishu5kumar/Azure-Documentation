## Table of Contents

1. [Introduction to Networking and Cloud Computing](#day-1-introduction-to-networking-and-cloud-computing)
2. [Cloud Service and Deployment Models](#day-2-cloud-service-and-deployment-models)
3. [Azure Key Components and Services](#day-3-azure-key-components-and-services)
4. [Security in Cloud Computing](#day-4-security-in-cloud-computing)

---

## Part 1: Introduction to Networking and Cloud Computing

### Networking Overview

- **Network Devices**: Switches, routers, firewalls.
- **Computing Devices**: Servers, clients, virtual machines.

### Cloud Computing Overview

**Definition**: Cloud computing is a way to use the internet to access and store data, applications, and services on remote servers instead of on your own computer. This allows you to use powerful computing resources and storage without needing to have all the hardware and software on your local device.

When working in the IT industry, you generally build various types of software (web apps, databases, digital segments like data analytics, data engineering, data science, hosting, etc.) that are publicly accessible.

### Client-Server Model

- **Client**: The computer that requests data or services from another computer (e.g., Computer 1).
- **Server**: The computer that accepts requests and provides responses (e.g., Computer 2).

### Cloud Computing Explained

Cloud computing is the delivery of computing services—including servers, storage, databases, networking, software, analytics, and intelligence—over the internet (“the cloud”) to offer faster innovation, flexible resources, and economies of scale. Instead of owning and maintaining physical data centers and servers, businesses and individuals can access these services on-demand from cloud providers, such as Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform (GCP).

### Key Characteristics

1. **On-Demand Self-Service**: Users can provision and manage computing resources as needed without human intervention from the service provider.
2. **Broad Network Access**: Services are accessible over the network via standard mechanisms (e.g., web browsers, mobile apps), allowing access from a variety of devices.
3. **Resource Pooling**: Cloud providers pool their computing resources to serve multiple customers using a multi-tenant model, dynamically allocating and reallocating resources according to demand.
4. **Rapid Elasticity**: Resources can be rapidly and elastically provisioned to quickly scale out and rapidly released to quickly scale in, appearing to be unlimited to the user.
5. **Measured Service**: Cloud systems automatically control and optimize resource use by leveraging a metering capability, typically on a pay-per-use or subscription basis.

### Benefits of Cloud Computing

- **Cost Efficiency**: Eliminates the capital expense of buying hardware and software.
- **Flexibility**: You can easily scale up or down your usage.
- **Scalability**: Easily scales resources up or down as business needs change.
- **Performance**: Offers high-performance computing power with low latency and large-scale storage.
- **Security**: Provides a broad set of policies, technologies, and controls that strengthen security.
- **Disaster Recovery and Backup**: Simplifies data backup, disaster recovery, and business continuity.
- **Automatic Updates**: Provides automatic software updates and patches.

---

## Part 2: Cloud Service and Deployment Models

### Cloud Service Models

1. **Infrastructure as a Service (IaaS)**
   - Provides virtualized computing resources over the internet.
   - Examples: Amazon EC2, Microsoft Azure VMs, Google Compute Engine.
2. **Platform as a Service (PaaS)**
   - Offers hardware and software tools over the internet, typically for application development.
   - Examples: Google App Engine, Microsoft Azure App Services, Heroku.
3. **Software as a Service (SaaS)**
   - Delivers software applications over the internet, on a subscription basis.
   - Examples: Google Workspace, Microsoft Office 365, Google Drive.

### Cloud Deployment Models

1. **Public Cloud**
   - Services are delivered over the public internet and shared across multiple organizations.
   - Examples: AWS, Azure, GCP.
2. **Private Cloud**
   - Services are maintained on a private network, dedicated to a single organization.
   - Examples: On-premises data centers, private clouds managed by vendors like VMware.
3. **Hybrid Cloud**
   - Combines public and private clouds, allowing data and applications to be shared between them.
   - Examples: An organization uses both on-premises servers and public cloud services.
4. **Multi-Cloud**
   - Uses multiple cloud computing services from different providers.
   - Examples: A company might use AWS for development and Azure for business analytics.

---

## Part 3: Azure Key Components and Services

### Compute Services

- **Virtual Machines (VMs)**: Provides on-demand computing resources with customizable VM configurations.
- **App Services**: Managed platform for building, deploying, and scaling web apps.
- **Azure Kubernetes Service (AKS)**: Managed Kubernetes service for deploying and managing containerized applications.

### Storage Services

- **Blob Storage**: Scalable object storage for unstructured data.
- **File Storage**: Managed file shares in the cloud.
- **Queue Storage**: Message queuing for large workloads.

### Database Services

- **Azure SQL Database**: Fully managed relational database with SQL Server capabilities.
- **Cosmos DB**: Globally distributed, multi-model database service.
- **Azure Database for MySQL/PostgreSQL**: Managed relational databases for MySQL and PostgreSQL.

### Networking Services

- **Virtual Network**: Enables secure, private network connections in the cloud.
- **Azure DNS**: Domain Name System hosting.
- **Content Delivery Network (CDN)**: Distributes content to users globally with high performance.

### Analytics and AI

- **Azure Synapse Analytics**: Integrated analytics service combining big data and data warehousing.
- **Azure Machine Learning**: Service for building, training, and deploying machine learning models.
- **Azure Cognitive Services**: Pre-built APIs for vision, speech, language, and decision-making.

### Developer Tools and DevOps

- **Azure DevOps**: Development collaboration tools including CI/CD pipelines.
- **Azure Pipelines**: Continuous integration and continuous delivery (CI/CD) service.
- **Azure Repos**: Source code management with Git repositories.

### Security and Management

- **Azure Active Directory (AAD)**: Identity and access management service.
- **Azure Security Center**: Unified security management and advanced threat protection.
- **Azure Monitor**: Full-stack monitoring and diagnostics.

---

## Part 4: Security in Cloud Computing

### Security Mechanisms

1. **Data Encryption**
   - In-Transit Encryption: Protects data being transmitted over the internet.
   - At-Rest Encryption: Secures data stored on physical media.
2. **Identity and Access Management (IAM)**
   - User Authentication: Verifies user identities (e.g., passwords, multi-factor authentication).
   - Access Controls: Manages permissions using role-based or attribute-based access controls.
3. **Firewalls**
   - Network Firewalls: Control incoming and outgoing network traffic.
   - Web Application Firewalls (WAF): Protect web applications from threats.
4. **Intrusion Detection and Prevention Systems (IDPS)**
   - Intrusion Detection Systems (IDS): Monitor network traffic for suspicious activity.
   - Intrusion Prevention Systems (IPS): Actively block detected threats.
5. **Security Information and Event Management (SIEM)**
   - Log Management: Collects and analyzes log data to identify security incidents.
   - Incident Response: Automates the response to detected threats.
6. **Data Loss Prevention (DLP)**
   - Content Inspection: Scans data for sensitive information.
   - Endpoint Protection: Ensures data protection on accessing devices.
7. **Application Security**
   - Secure Coding Practices: Develops applications with security best practices.
   - Application Security Testing: Regularly tests applications for vulnerabilities.
8. **Network Security**
   - Virtual Private Networks (VPNs): Create secure connections over public networks.
   - Segmentation: Divides the network to limit threat spread.
9. **Backup and Recovery**
   - Regular Backups: Protects against data loss.
   - Disaster Recovery Plans: Establishes procedures for recovery after incidents.
10. **Compliance and Auditing**
    - Compliance Frameworks: Adheres to industry standards and regulations.
    - Regular Audits: Assesses and improves security posture.

### Examples of Cloud Security Tools

- **AWS**: IAM, Key Management Service, Shield, CloudTrail.
- **Azure**: Security Center, Active Directory, Key Vault, Sentinel.
- **GCP**: IAM, Key Management, Cloud Armor, Security Command Center.

---

Hosting a Website:

Deploy applications on a server.
Use a unique IP address and domain name.
Manage traffic with load balancers.

### Everyday Analogy

Cloud computing is like using a public library instead of buying all the books you want to read. You go to the library, pick the books you need, use them for as long as you want, and then return them. You don't have to worry about buying, storing, or maintaining the books.

---

Thank you for visiting my Azure training documentation. I hope these notes help you understand the key concepts and services in Azure. Feel free to explore and contribute to this repository.

---
