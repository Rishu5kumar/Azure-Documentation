# Day 3: Azure Account for Students Subscription and Virtual Machine Deep Dive

## Table of Contents
1. [Azure Account for Students Subscription Steps](#azure-account-for-students-subscription-steps)
2. [Virtual Machine Deep Dive](#virtual-machine-deep-dive)
   - [What is a Virtual Machine?](#what-is-a-virtual-machine)
   - [Key Components](#key-components)
   - [Steps to Create a Virtual Machine](#steps-to-create-a-virtual-machine)
   - [Managing and Monitoring VMs](#managing-and-monitoring-vms)

## Azure Account for Students Subscription Steps

### Step-by-Step Guide to Create an Azure for Students Account

1. **Sign Up for Azure for Students**
   - Visit the [Azure for Students sign-up page](https://azure.microsoft.com/en-us/free/students/).
   - Click on the "Activate now" button.

2. **Sign In with Your School Email**
   - Sign in with your school email address to verify your student status.
   - If you don't have a school email, you might need to provide proof of student status.

3. **Verify Your Identity**
   - Provide necessary details like your name, email, and school name.
   - Verify your identity through the provided verification process (usually involves entering a code sent to your email).

4. **No Credit Card Required**
   - Azure for Students does not require a credit card for verification.
   - You receive a $100 credit for 12 months and access to popular free services.

5. **Complete Registration**
   - Once verified, complete the registration process.
   - You will have access to the Azure portal to start using Azure services.

## Virtual Machine Deep Dive

### What is a Virtual Machine?

A Virtual Machine (VM) is an emulation of a physical computer. It runs an operating system and applications just like a physical computer. VMs provide the flexibility of running multiple operating systems on a single physical machine and can be easily created, modified, and deleted.

### Key Components of Azure Virtual Machines

1. **Resource Group**
   - A container that holds related resources for an Azure solution.
   - Example: A resource group named `MyResourceGroup`.

2. **Virtual Network (VNet)**
   - A logical isolation of the Azure cloud dedicated to your subscription.
   - Example: A VNet named `MyVNet` with the address space `10.0.0.0/16`.

3. **Subnet**
   - A range of IP addresses in the VNet.
   - Example: A subnet named `MySubnet` with the address range `10.0.0.0/24`.

4. **Network Interface Card (NIC)**
   - Connects the VM to the VNet.
   - Example: A NIC named `MyNIC`.

5. **Public IP Address**
   - An IP address assigned to the VM to communicate with the internet.
   - Example: A public IP named `MyPublicIP`.

6. **Network Security Group (NSG)**
   - Controls inbound and outbound traffic to the VM.
   - Example: An NSG named `MyNSG` with rules allowing SSH and RDP access.

### Steps to Create a Virtual Machine in Azure

1. **Log in to the Azure Portal**
   - Go to [Azure Portal](https://portal.azure.com/) and log in with your credentials.

2. **Create a Resource Group**
   - Navigate to "Resource groups" and click "Add."
   - Enter a name for your resource group and select a region.
   - Click "Review + create" and then "Create."

3. **Create a Virtual Machine**
   - Navigate to "Virtual machines" and click "Add."
   - Select your subscription and the resource group you created.
   - Enter a name for your VM, choose a region, and select an image (e.g., Ubuntu Server).
   - Choose a size based on your requirements.

4. **Configure the Administrator Account**
   - Enter a username and password or choose SSH public key for authentication.

5. **Configure Networking**
   - Select or create a new virtual network.
   - Choose or create a subnet.
   - Select or create a public IP address.
   - Attach a Network Security Group.

6. **Review and Create**
   - Review all the settings.
   - Click "Review + create" and then "Create."
   - Wait for the VM to be deployed.

7. **Access the VM**
   - Once deployed, go to the "Virtual machines" section.
   - Click on your VM and copy the public IP address.
   - Use SSH (Linux) or RDP (Windows) to connect to your VM.

### Managing and Monitoring VMs

1. **Start, Stop, and Restart**
   - Use the Azure portal to start, stop, and restart your VM.

2. **Scaling VMs**
   - Scale up or down based on your workload requirements.
   - Change the VM size through the portal.

3. **Monitoring Performance**
   - Use Azure Monitor to track VM performance metrics.
   - Set up alerts for critical metrics.

4. **Backup and Recovery**
   - Use Azure Backup to create backups of your VM.
   - Restore from backups as needed.

5. **Security Best Practices**
   - Regularly update your OS and applications.
   - Use NSGs and Azure Firewall to protect your VM.
   - Implement Azure Bastion for secure and seamless RDP and SSH access.

By following these steps, you can create and manage Azure Virtual Machines, leveraging the flexibility and scalability of the Azure cloud to meet your computing needs.
