# Day 25: Kubernetes in Azure: A Comprehensive Guide

## 1. Introduction to Kubernetes

### The Problem Before Kubernetes
Before the advent of Kubernetes, developers faced several challenges when deploying and managing containerized applications:
- **Manual Scaling**: Scaling applications manually across multiple machines was complex and error-prone.
- **Inconsistent Environments**: Differences in development, testing, and production environments led to inconsistencies and bugs.
- **Complex Orchestration**: Managing dependencies, communication between services, and failover strategies required custom scripts and manual intervention.

### How Kubernetes Solves These Problems
Kubernetes automates the deployment, scaling, and management of containerized applications:
- **Automated Scaling**: Kubernetes automatically scales applications based on resource usage.
- **Consistent Environments**: Kubernetes ensures consistency across all environments by running containers with the same configuration.
- **Orchestration**: Kubernetes handles the orchestration of containers, including networking, load balancing, and failover.

### Scenario and Example
Imagine you have an e-commerce application with several microservices (e.g., payment, inventory, user management). Manually deploying these services on multiple servers can lead to errors and downtime. Kubernetes automates this process, ensuring that each microservice runs in its container, with automatic scaling and self-healing capabilities.

---

## 2. Introduction to Docker

### What is Docker?
Docker is a platform for developing, shipping, and running applications inside containers. A container packages an application and its dependencies together, ensuring that it runs consistently across different environments.

### Docker and Kubernetes Relationship
Docker is the container runtime used by Kubernetes. While Docker handles the creation and management of containers, Kubernetes orchestrates and manages these containers across a cluster of machines.

### Differences between Docker and Kubernetes
- **Docker**: Focuses on containerizing applications. It creates and runs containers on a single node.
- **Kubernetes**: Manages and orchestrates containers across a cluster of nodes, handling tasks like scaling, networking, and storage.

---

## 3. Kubernetes Architecture

### Overview of Kubernetes Architecture
Kubernetes architecture is based on a master-slave (control plane and worker nodes) model. The **control plane** manages the overall state of the cluster, while **worker nodes** run the application workloads.

### Master Node Components
- **Kubernetes API Server**: The API server acts as the frontend for the Kubernetes control plane. It exposes the Kubernetes API, which is used by internal and external components to interact with the cluster.
- **etcd**: A distributed key-value store that holds all the cluster data, such as configuration and state information. It acts as the source of truth for the cluster.
- **Kube-Scheduler**: The scheduler assigns pods (the smallest deployable units in Kubernetes) to nodes based on resource availability and other constraints.
- **Kube-Controller-Manager**: This component runs controllers that regulate the state of the cluster, such as the node controller, replication controller, and endpoint controller.

### Worker Node Components
- **Kubelet**: An agent that runs on each worker node. It ensures that the containers in a pod are running as expected.
- **Kube-Proxy**: Manages network communication for pods, enabling them to communicate with each other and with services inside and outside the cluster.
- **Container Runtime (Docker)**: The software responsible for running the containers. Docker is the most commonly used container runtime in Kubernetes.

### Single Master with Multiple Worker Nodes Setup
In a typical setup, you have a single master node managing multiple worker nodes. The master node handles control plane tasks, while the worker nodes run the actual application workloads.

### One Master and Two Worker Nodes Scenario
Consider a scenario with one master and two worker nodes. The master node manages the cluster, while the two worker nodes run the applications. This setup provides high availability and load balancing.

### Explanation of Terms like Kubelet, Kube-Proxy, etc.
- **Kubelet**: Monitors and manages the containers on a worker node.
- **Kube-Proxy**: Handles networking for the Kubernetes pods, ensuring proper routing and load balancing.
- **Pod**: The smallest deployable unit in Kubernetes, usually containing one or more containers.

---

## 4. Kubernetes Clusters

### What is a Kubernetes Cluster?
A Kubernetes cluster is a set of nodes (machines) that run containerized applications managed by the Kubernetes control plane. The cluster ensures that your applications run as intended, even as nodes fail or scale.

### Setting up a Kubernetes Cluster in Azure
Azure Kubernetes Service (AKS) simplifies the deployment, management, and operations of Kubernetes clusters in Azure. AKS handles critical tasks like scaling, monitoring, and upgrades.

### Components and Architecture of a Kubernetes Cluster
A Kubernetes cluster typically consists of a control plane (master node) and multiple worker nodes. The control plane manages the state of the cluster, while the worker nodes run the containerized applications.

### Setting up Multiple Nodes in Azure
In Azure, you can set up multiple VMs in the same virtual network and configure them as nodes in a Kubernetes cluster. This setup allows you to scale your applications across multiple machines.

---

## 5. Setting Up Kubernetes in Azure

### Prerequisites for Setting Up Kubernetes
- **Azure Subscription**: You need an active Azure subscription.
- **Azure CLI**: Install the Azure CLI to interact with Azure services from your terminal.
- **Kubernetes Tools**: Install `kubectl`, `kubeadm`, and `kubelet` on your machines.
- **Networking**: Ensure that all VMs can communicate with each other and have access to the internet.
- **Swap Memory**: Disable swap memory on all nodes to ensure Kubernetes runs smoothly.

### Detailed Step-by-Step Setup on Azure

1. **Create an Azure Kubernetes Service (AKS) Cluster**

   - **Step 1**: Log in to the Azure Portal.
   - **Step 2**: Navigate to **Azure Kubernetes Service** by searching for "Kubernetes services" in the search bar.
   - **Step 3**: Click on **Create**.
   - **Step 4**: Fill in the required details:
     - **Resource Group**: Create a new resource group or select an existing one.
     - **Kubernetes Cluster Name**: Give your cluster a name.
     - **Region**: Select the region closest to you.
     - **Node Size**: Choose the size of your nodes (e.g., Standard_DS2_v2).
     - **Node Count**: Select the number of nodes (e.g., 2).
   - **Step 5**: Configure the **Authentication** and **Networking** settings as required. For basic setups, you can use the defaults.
   - **Step 6**: Review and create the cluster.

2. **Accessing the Kubernetes Dashboard**

   - **Step 1**: Once the cluster is created, navigate to the resource group and select your AKS cluster.
   - **Step 2**: Under **Cluster Monitoring**, select **View Kubernetes dashboard**.
   - **Step 3**: Follow the instructions to set up access. You may need to create a service account and get the `kubeconfig` file.

3. **Configuring Nodes**

   - Azure AKS automatically configures the nodes as part of the cluster creation. You can scale the nodes later by going to the **Node pools** section in the AKS cluster.

### Swap Memory Management
- **Azure**: Swap memory management in Azure Kubernetes is handled automatically by the AKS service, so you typically don't need to manage this manually unless you're dealing with specific workloads.

### Network Configuration Using Azure Portal
- **Network Plugin**: During the AKS setup, you can select the network plugin (Azure CNI or Kubenet) that best fits your needs. Azure CNI is recommended for advanced networking scenarios.


### Detailed Step-by-Step Setup on Azure

1. **Create Virtual Machines (VMs)**
   - Log in to your Azure account:
     ```bash
     az login
     ```
   - Create a resource group:
     ```bash
     az group create --name KubernetesResourceGroup --location eastus
     ```
   - Create VMs for the master and worker nodes:
     ```bash
     az vm create --resource-group KubernetesResourceGroup --name MasterNode --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
     az vm create --resource-group KubernetesResourceGroup --name WorkerNode1 --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
     az vm create --resource-group KubernetesResourceGroup --name WorkerNode2 --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
     ```

2. **Disable Swap Memory**
   - SSH into each VM:
     ```bash
     ssh azureuser@<VM_PUBLIC_IP>
     ```
   - Disable swap memory:
     ```bash
     sudo swapoff -a
     ```

3. **Install Docker on Each Node**
   - Update package information and install prerequisites:
     ```bash
     sudo apt-get update
     sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
     ```
   - Add Docker’s official GPG key:
     ```bash
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
     ```
   - Set up the stable repository:
     ```bash
     sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
     ```
   - Install Docker:
     ```bash
     sudo apt-get update
     sudo apt-get install -y docker-ce
     ```
   - Verify Docker installation:
     ```bash
     sudo systemctl status docker
     ```

4. **Install Kubernetes Tools (kubectl, kubeadm, kubelet)**
   - Add the Kubernetes apt repository:
     ```bash
     curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
     sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
     ```
   - Install Kubernetes tools:
     ```bash
     sudo apt-get update
     sudo apt-get install -y kubelet kubeadm kubectl
     ```
   - Hold these packages at their current version:
     ```bash
     sudo apt-mark hold kubelet kubeadm kubectl
     ```

### Configuring the Master Node
- **Initialize the Kubernetes Cluster**:
  ```bash
  sudo kubeadm init --pod-network-cidr=10.244.0.0/16
  ```
- **Set up the Kubernetes configuration file**:
  ```bash
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
  ```

### Configuring Worker Nodes
- **Join the Worker Nodes to the Cluster**:
  On the worker nodes, run the command provided by `kubeadm init` (from the master node) to join them to the cluster:
  ```bash
  sudo kubeadm join <MASTER_NODE_IP>:<PORT> --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH>
  ```

### Using `kubeadm` to Initialize the Cluster
`kubeadm` simplifies the process of setting up a Kubernetes cluster by automating the configuration of control plane components.

### Setting Up Network Plugins (CNI)
Install a network plugin to allow communication between pods:
- For Flannel:
  ```bash
  kubectl apply -f https://raw.githubusercontent.com/coreos

/flannel/master/Documentation/kube-flannel.yml
  ```
- For Calico:
  ```bash
  kubectl apply -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml
  ```

---

## 6. Deploying Applications on Kubernetes

### Deploying an Application in a Kubernetes Cluster
- **Create a Deployment**:
  ```bash
  kubectl create deployment nginx --image=nginx
  ```
- **Expose the Deployment as a Service**:
  ```bash
  kubectl expose deployment nginx --port=80 --type=LoadBalancer
  ```
- **Check the Status**:
  ```bash
  kubectl get pods
  kubectl get services
  ```

### Accessing the Application via Public IP
Once the service is created with a `LoadBalancer` type, Kubernetes will provision a public IP address for the service. You can access the application using this IP.

### Rolling Updates and Deployment Strategies
- **Rolling Update**:
  Update an application image with zero downtime:
  ```bash
  kubectl set image deployment/nginx nginx=nginx:1.19.3
  ```
- **Rolling Back**:
  Roll back to the previous deployment:
  ```bash
  kubectl rollout undo deployment/nginx
  ```

---










## 6. Swap Memory in Kubernetes

### What is Swap Memory?
Swap memory is a space on a disk used when the system's RAM is fully utilized. It's slower than RAM but provides additional memory resources.

### Importance in Kubernetes Setup
Kubernetes relies on predictable performance, and swap memory can introduce variability. Disabling swap ensures consistent behavior.

### How to Manage Swap Memory
- **Disable Swap Temporarily**:
  ```bash
  sudo swapoff -a
  ```
- **Disable Swap Permanently**:
  Edit the `/etc/fstab` file and comment out the swap entry:
  ```bash
  sudo nano /etc/fstab
  # Comment out the line that includes "swap"
  ```

---

## 7. Deploying Applications on Kubernetes

### Deploying an Application in a Kubernetes Cluster

1. **Use the Azure Kubernetes Dashboard**
   - **Step 1**: Access the Kubernetes dashboard as described in the previous section.
   - **Step 2**: Click on **Create** and choose to upload a YAML file or use the form to deploy your application.
   - **Step 3**: Fill in the details or upload your YAML file to create a deployment.

2. **Expose the Deployment as a Service**
   - **Step 1**: Once the deployment is created, select it from the dashboard.
   - **Step 2**: Under the **Services** tab, click on **Expose** to create a service for your deployment.
   - **Step 3**: Choose the type of service (e.g., LoadBalancer) to expose your application to the internet.

### Accessing the Application via Public IP
- After creating a service with the LoadBalancer type, the Azure Portal will automatically provision a public IP for your application. You can find this IP under the **Services** section in the Kubernetes dashboard.

### Rolling Updates and Deployment Strategies

1. **Rolling Update**
   - Use the Azure Kubernetes dashboard or `kubectl` to update your deployments. For a rolling update:
   - **Step 1**: Modify the deployment file or update the image directly from the dashboard.
   - **Step 2**: Kubernetes will automatically perform the rolling update without downtime.

2. **Rolling Back**
   - If something goes wrong, you can roll back to a previous version directly from the Kubernetes dashboard or using `kubectl`.

---

## 8. Kubernetes API-Server

### What is the Kubernetes API-Server?
The Kubernetes API-Server is the control plane component that exposes the Kubernetes API. It serves as the primary entry point for all administrative tasks in the cluster.

### Interaction with Other Components
- **kubectl**: The command-line tool interacts with the API-Server to manage resources.
- **Scheduler and Controller-Manager**: Communicate with the API-Server to get the current state of the cluster and make decisions.

---

## 9. Other Kubernetes Components

### kubelet API
- **Description**: The `kubelet` API is responsible for managing the lifecycle of containers on a node.
- **Key Functions**: Ensures containers are running as expected, manages pod configurations, and interacts with the container runtime.

### kube-scheduler
- **Description**: The `kube-scheduler` assigns pods to nodes based on resource requirements, affinity rules, and other constraints.
- **Key Functions**: Monitors the cluster state, schedules new pods, and optimizes resource usage.

### kube-controller-manager
- **Description**: Runs multiple controllers that regulate the cluster state, such as the replication controller, node controller, and endpoint controller.
- **Key Functions**: Ensures the desired state of resources is maintained, manages replication, and handles node failures.

### kubectl
- **Description**: The command-line tool for interacting with the Kubernetes API-Server.
- **Key Functions**: Used for creating, modifying, and deleting resources, as well as retrieving cluster status.

### kubeadm
- **Description**: A tool that simplifies the setup of a Kubernetes cluster.
- **Key Functions**: Automates the initialization of the control plane and the joining of worker nodes.

---

## 10. Conclusion

### Summary
Kubernetes provides a robust platform for managing containerized applications across multiple nodes, automating tasks like scaling, networking, and deployment.

### Best Practices
- **Use YAML Files**: Store configurations in YAML files for consistency and version control.
- **Monitor Cluster Health**: Use tools like Prometheus and Grafana to monitor your cluster.
- **Secure Your Cluster**: Implement role-based access control (RBAC) and network policies.
- **Regular Backups**: Regularly back up etcd data to ensure cluster recovery.

---
