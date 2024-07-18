# Day 17: Azure Application Gateway and Active Directory

## Table of Contents

1. [Azure Application Gateway](#azure-application-gateway)
2. [Active Directory](#active-directory)
3. [Active Directory Domain Services (AD DS)](#active-directory-domain-services-ad-ds)

---

## Azure Application Gateway

### Overview:
Azure Application Gateway is a web traffic load balancer that enables you to manage traffic to your web applications. It operates at the application layer (OSI Layer 7) and offers advanced routing capabilities, SSL offloading, and web application firewall (WAF) features.

### Key Features:

1. **Load Balancing:**
   - Distributes incoming traffic across multiple backend servers.
   - Ensures no single server becomes overwhelmed.

2. **Web Application Firewall (WAF):**
   - Protects your applications from common web vulnerabilities and attacks.

3. **SSL Offloading:**
   - Moves the SSL encryption/decryption process to the Application Gateway.
   - Enhances backend server performance and simplifies SSL certificate management.
   - Example: Secure e-commerce website handling a high volume of traffic.

4. **Cookie-Based Affinity:**
   - Ensures a user's session is consistently handled by the same backend server.
   - Important for maintaining session data and providing a seamless user experience.
   - Ensures a user's requests are always directed to the same backend server during their session.
   - Example: An online store where the user's shopping cart state is maintained consistently.

5. **Connection Draining:**
   - Ensures existing connections can complete before a server is taken offline.
   - Prevents abrupt termination of active requests during maintenance or updates.
   - Allows existing connections to complete before a server is taken offline.
   - Example: An online store updating a backend server without interrupting ongoing transactions.

6. **Multi-Site Hosting:**
   - Hosts multiple websites on the same application gateway.
   - Useful for managing different domains or subdomains under one gateway.
   - Example: Hosting main website, blog, and shop on the same gateway.
     
7. **URL-Based Routing:**
   - Routes requests to different backend servers based on the URL path.

8. **Automatic Scaling:**
   - Adjusts the number of instances in response to traffic load.

9. **Redirection:**
   - Supports HTTP to HTTPS redirection, URL redirection, and more.
     
10. **HTTP to HTTPS Rerouting:**
   - Ensures all traffic uses HTTPS for secure communication.
   - Example: Ensuring secure communication for all visitors to your website.

### Scenario: Bot Attack Mitigation with Azure Application Gateway

Imagine your e-commerce website, www.mystore.com, is experiencing a bot attack. Malicious bots are sending a high volume of requests to your site, attempting to overwhelm your servers. This can lead to degraded performance for legitimate users, increased operational costs, and potential security breaches.

**Mitigating Bot Attacks with Azure Application Gateway:**
Azure Application Gateway, with its WAF capabilities, can help protect your website from such bot attacks. WAF can detect that requests come from the same IP and can block the bot attack.

### Types of Azure Application Gateway:

1. **Standard Application Gateway:**
   - Basic load balancing based on HTTP/HTTPS traffic.
   - SSL offloading.
   - URL-based routing.
   - Basic health monitoring and diagnostics.
   - Manual scaling and configuration updates.
   - **Example:** Small e-commerce website with predictable traffic.

2. **Standard_v2 Application Gateway:**
   - Includes all features of Standard.
   - Autoscaling based on traffic load.
   - Zone redundancy for high availability.
   - Faster provisioning and configuration updates.
   - Enhanced diagnostics and logging capabilities.
   - **Example:** Dynamic online service with varying traffic.

3. **WAF_v1 Application Gateway:**
   - Includes all features of Standard.
   - Web Application Firewall (WAF) for protecting against common web vulnerabilities and attacks.
   - Supports predefined WAF rulesets.
   - **Example:** Corporate web application processing sensitive user data.

4. **WAF_v2 Application Gateway:**
   - Includes all features of WAF_v1.
   - Enhanced security capabilities with customizable WAF rules.
   - Integrated Azure Monitor for advanced diagnostics and monitoring.
   - **Example:** Enterprise-level application handling extensive user interactions and transactions.

### Comparison of AG Types:
```

| Feature                      | Standard | Standard_v2 | WAF_v1 | WAF_v2 |
|------------------------------|----------|-------------|--------|--------|
| Autoscaling                  | No       | Yes         | No     | Yes    |
| Zone Redundancy              | No       | Yes         | No     | Yes    |
| URL and Path-based Routing   | Yes      | Yes         | Yes    | Yes    |
| SSL Offloading               | Yes      | Yes         | Yes    | Yes    |
| Custom Error Pages           | No       | Yes         | Yes    | Yes    |
| Advanced Diagnostics         | Basic    | Enhanced    | Enhanced| Enhanced |
| Web Application Firewall (WAF)| No      | No          | Yes    | Yes    |
| Custom WAF Rules             | No       | No          | No     | Yes    |
| Azure Monitor Integration    | Basic    | Advanced    | Basic  | Advanced|
| Deployment Speed             | Standard | Faster      | Standard| Faster |
```

### How It Works:

1. **Client Request:** A user sends a request to your application’s URL.
2. **DNS Resolution:** The domain name resolves to the public IP of the Application Gateway.
3. **Application Gateway:** The gateway receives the request and determines the best backend server to handle it.
4. **Routing:** The request is routed to the chosen backend server.
5. **Response:** The backend server processes the request and sends the response back to the Application Gateway.
6. **Client Response:** The Application Gateway forwards the response to the client.


### Differences Between Azure Application Gateway and Azure Load Balancer:
```

| Feature                       | Azure Application Gateway         | Azure Load Balancer           |
|-------------------------------|-----------------------------------|-------------------------------|
| OSI Layer                     | Layer 7 (Application Layer)       | Layer 4 (Transport Layer)     |
| Protocol                      | HTTP/HTTPS (Layer 7)              | TCP/UDP (Layer 4)             |
| SSL Termination               | Yes, supports SSL offloading      | No                            |
| Web Application Firewall (WAF)| Yes, includes WAF for security    | No                            |
| URL-based Routing             | Yes, can route traffic based on URL paths | No                    |
| Multi-site Hosting            | Yes, can host multiple websites   | No                            |
| Session Affinity              | Yes, via cookie-based affinity    | No, uses source IP affinity   |
| Health Probes                 | Layer 7 health probes (HTTP/S)    | Layer 4 health probes (TCP/HTTP) |
| Ideal Use Cases               | Web applications needing advanced routing and security | General network traffic distribution across VMs |
| Features                      | URL-based routing, SSL offloading, WAF, multi-site hosting, connection draining, rewrite HTTP headers, redirect HTTP to HTTPS | Basic load balancing, high availability, failover, low latency |

```
### When to Use Each:

- **Azure Application Gateway:** For complex web applications requiring advanced routing and security features, SSL offloading, web application firewall, and URL-based routing.
- **Azure Load Balancer:** For general load balancing across virtual machines, low latency, high throughput, and simple traffic distribution at the network level.

### Step-by-Step Guide to Create Azure Application Gateway:

1. Sign in to the Azure Portal:
   - Go to [Azure Portal](https://portal.azure.com).
   - Sign in with your Azure account.

2. Create a Resource Group (RG).

3. Create Application Gateway.

4. Basics Tab: Configure Basics:
   - Subscription: Select your Azure subscription.
   - Resource Group: Select the resource group you created in step 2.
   - Name: Enter a unique name for your Application Gateway.
   - Region: Choose the Azure region where you want to deploy the Application Gateway.
   - SKU: Select the SKU type (e.g., Standard_v2, WAF_v2) based on your requirements.

5. Configure Virtual Network Settings:
   - Virtual Network: Select the virtual network where your backend resources are located.
   - Subnet: Choose the subnet within the selected virtual network where the Application Gateway will be deployed.

6. Frontend IP Configuration:
   - Frontend IP Address Type: Choose "Public" if you want a public-facing application gateway or "Private" for an internal-facing application gateway.
   - Public IP Address: If you selected "Public," create a new public IP address or select an existing one.
        - Click "Add new" under "Public IP address."
        - Name: Enter a name for the new public IP address.
        - Click "OK" to create the public IP address.
7. Backend Pool Configuration:
    - Backend Pool: Create a new backend pool by clicking "Add new."
    - Name: Enter a name for your backend pool.
    - Target Type: Select the target type for your backend pool (IP address, Virtual Machine, App Service, etc.).
    - Add your backend targets (IP addresses, VMs, etc.) to the pool.
    - Click "Add" to create the backend pool.

8. Routing Rules:
    - Click "Add new" under the "Routing rules" section.
    - Enter a name for your routing rule. Then create a new listener.
    - Enter a name for the listener, then Select the frontend IP you configured earlier.
    - Protocol: Choose the protocol (HTTP or HTTPS). Then Enter the port number (e.g., 80 for HTTP, 443 for HTTPS).
    - If using HTTPS, configure the SSL settings (upload an SSL certificate).
    - Backend Target: Select the backend pool you created earlier.
    - HTTP Setting: Create a new HTTP setting, enter a name for this.
    - Backend Protocol: Choose the protocol (HTTP or HTTPS).
    - Backend Port: Enter the backend port number.
    - Configure any additional settings as needed (e.g., Cookie-based affinity, Connection draining).
    - Click "Add" to create the routing rule.

9. Web Application Firewall (WAF):
    - If using WAF_v2 SKU, configure the WAF settings. Click on the "Web Application Firewall" tab.
    - Enable WAF, choose the mode (Detection or Prevention), and configure the WAF rules.

10. Review + Create:
    - Click "Review + create." Once validation passes, click "Create" to deploy the Application Gateway.

---

## Active Directory

### Overview

Azure Active Directory (Azure AD) is Microsoft's cloud-based identity and access management service. It provides identity services that authenticate and authorize users to access Azure resources and other Microsoft Online Services like Office 365, Dynamics 365, and more.
Think of it as a directory service similar to the on-premises Active Directory, but hosted in the cloud and integrated with Azure services.

**Scenario**: A company named Contoso wants to manage access to its applications and resources securely and efficiently using Azure AD.


### Key Features:
- **Identity Management:** Centralized user and group management.
- **Access Control:** Authentication and authorization for applications and resources.
- **Single Sign-On (SSO):** Enables users to access multiple applications with one set of credentials.
- **Device Management:** Manages devices accessing corporate resources.
- **Security:** Implements security policies and multifactor authentication.
  
### Key Concepts

#### Users and Groups

- **Users:** Represent individuals who need access to Azure resources or applications.
- **Groups:** Containers for managing access to resources. Users are added to groups to grant them permissions collectively.

#### Applications and Service Principals

- **Applications:** Represent apps that you integrate with Azure AD to authenticate users.
- **Service Principals:** Represent the app's identity and permissions.

#### Roles and Role-Based Access Control (RBAC)

- **Roles:** Define permissions that users or groups can have.
- **RBAC:** Enables fine-grained access management for Azure resources.


### Setting Up Azure AD

#### Creating an Azure AD Tenant

1. **Sign in to the Azure portal:**
   - Navigate to Azure Active Directory > Create New Tenant.

2. **Configure basic settings:**
   - Choose a unique domain name (e.g., yourcompany.onmicrosoft.com).

3. **Complete the setup:**
   - Follow the prompts to finish creating your Azure AD tenant.

#### Adding Users and Groups

1. **Navigate to Azure AD:**
   - Add users individually or in bulk.
   - Create groups based on organizational needs.

#### Integrating Applications

1. **Register an Application:**
   - Navigate to Azure AD > App registrations > New registration.
   - Configure authentication settings for your application.

#### Configuring Roles and RBAC

1. **Define Roles:**
   - Navigate to Azure AD > Roles and administrators.
   - Create custom roles or assign built-in roles.
---
## Active Directory Domain Services (AD DS)

### Overview
AD DS is the core service within Active Directory that provides authentication and authorization services, storing information about network objects. It's essential for managing Windows-based networks and ensuring secure access to resources.

It is like a phonebook for your computer network, where you can find information about all the users, computers, and other resources in your organization. It helps you manage and secure these resources efficiently.

### Difference between AD and AD DS
```
| Feature                         | Active Directory (AD)                                | Active Directory Domain Services (AD DS)                        |
|---------------------------------|------------------------------------------------------|-----------------------------------------------------------------|
|   Scope                         | Umbrella term covering multiple directory services   | Core directory service within Active Directory                  |
|   Components                    | Includes AD DS, AD LDS, AD CS, AD FS, AD RMS         | Focuses specifically on managing domains and directory objects  |
|   Function                      | Broad identity and access management                 | Provides authentication, authorization, and resource management |
|   Example Services              | AD DS, AD LDS, AD CS, AD FS, AD RMS                  | Core service for managing domains, users, groups, and computers |
```
---
