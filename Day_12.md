# Day 12: Azure Storage Services and Containers
 
# Table of Contents

1. [Introduction to Azure Storage Services](#introduction-to-azure-storage-services)
2. [Azure Storage Services Overview](#azure-storage-services-overview)
3. [Azure Storage Account and Containers](#azure-storage-account-and-containers)
4. [Redundancy and Replication](#redundancy-and-replication)
5. [Anonymous Access](#anonymous-access)
6. [Erroneous Deletion](#erroneous-deletion)
7. [Access Tiers](#access-tiers)
8. [Hierarchy](#hierarchy)
9. [Snapshot](#snapshot)
10. [Conclusion](#conclusion)

---

## Introduction to Azure Storage Services

Azure Storage Services provide scalable, durable, and highly available cloud storage solutions to store data reliably. They offer various types of storage to cater to different needs and scenarios.

## Azure Storage Services Overview

Azure Storage includes several services:

- **BLOB (Binary Large Object) Storage:** Stores unstructured data like images, documents, videos. It consists of block blobs, page blobs, and append blobs.
- **File Storage:** Provides fully managed file shares for cloud or on-premises deployments.
- **Disk Storage:** Offers durable, high-performance block storage for Azure Virtual Machines.
- **Queue Storage:** Stores messages to be processed asynchronously, supporting producer-consumer and publisher-subscriber patterns.
- **Archive Storage:** Cost-effective solution for storing rarely accessed data.
- **Data Lake Storage:** Optimized storage for big data analytics workloads.

Each service has specific use cases and benefits tailored to different storage requirements.

## Azure Storage Account and Containers

### BLOB Storage

BLOB Storage is ideal for:

- **Media Storage:** Hosting images, videos, and audio files.
- **Backup and Restore:** Storing backup data securely.
- **Application Hosting:** Serving static website content.

#### Benefits:
- Scalability: Easily scale storage capacity as per requirements.
- Durability: Ensures data reliability with replication options.
- Accessibility: Can be accessed globally via HTTP/HTTPS.

#### Steps to Create BLOB Storage:
1. Navigate to Azure portal.
2. Create a new Storage Account.
3. Configure BLOB storage within the account.
4. Create containers to organize BLOBs.

### File Storage

File Storage is suitable for:

- **Enterprise Applications:** Sharing files across VMs.
- **Development Environment:** Collaborative development using shared resources.
- **Migration from On-premises:** Lift-and-shift scenarios.

#### Benefits:
- Familiar Protocol Support: Supports standard SMB and NFS protocols.
- Managed Service: Azure handles maintenance and updates.
- Scalability: Easily scales storage capacity up or down.

### Disk Storage

Disk Storage is used for:

- **Virtual Machines:** Holding OS disks and data disks.
- **High-Performance Computing:** Handling I/O intensive workloads.
- **Databases:** Providing persistent storage for databases.

#### Benefits:
- Performance: Provides low-latency, high-throughput disk performance.
- Reliability: Ensures data persistence even during VM failures.
- Integration: Seamlessly integrates with Azure Virtual Machines.

### Queue Storage

Queue Storage is employed for:

- **Task Management:** Storing messages for asynchronous processing.
- **Background Jobs:** Implementing worker roles for parallel processing.
- **Message Queues:** Implementing reliable messaging between application components.

#### Benefits:
- Decoupling Applications: Enables scalable and reliable application architecture.
- Asynchronous Processing: Supports efficient handling of background tasks.
- Message Persistence: Ensures message durability and reliability.

### Archive Storage

Archive Storage is used for:

- **Long-Term Retention:** Storing regulatory compliance data.
- **Data Backup:** Archiving data that's seldom accessed.
- **Cost Efficiency:** Minimizing storage costs for infrequently accessed data.

#### Benefits:
- Cost-Effective: Low storage costs for long-term data retention.
- Compliance: Meets regulatory requirements with encryption and access controls.
- Scalability: Scales storage capacity based on archival needs.

### Data Lake Storage

Data Lake Storage is used for:

- **Big Data Analytics:** Storing and analyzing vast amounts of structured and unstructured data.
- **Machine Learning:** Serving as a data repository for training and inference.
- **Data Exploration:** Enabling complex queries and analytics on large datasets.

#### Benefits:
- Scalability: Scales to petabytes of data effortlessly.
- Integration: Integrates with Azure Data Services and third-party tools.
- Security: Provides fine-grained access control and encryption for data protection.


## Azure storage hierarchy
```
Tenant
   └── Subscription
         └── Resource Group (RG)
               └── Storage Account
                     ├── BLOB
                     │    ├── Block Blob
                     │    ├── Page Blob
                     │    └── Append Blob
                     ├── FILE
                     ├── TABLE
                     └── QUEUE
```
## Redundancy and Replication

### Redundancy Types:

- **LRS (Locally Redundant Storage):** Replicates data within a single data center.
- **GRS (Geo-Redundant Storage):** Replicates data to a secondary region for data durability.
- **ZRS (Zone-Redundant Storage):** Replicates data across availability zones within a region.
- **GZRS (Geo-Zone-Redundant Storage):** Combines GRS and ZRS for enhanced availability and durability.

### Replication Types:

- **Synchronous Replication:** Updates are replicated to secondary storage immediately.
- **Asynchronous Replication:** Updates are replicated to secondary storage with a delay, offering lower latency.

## Anonymous Access

Azure Storage allows anonymous access to public containers and blobs. Types include:

- **Public Access:** Anyone with the URL can access blobs and containers.
- **Private Access:** Requires a shared access signature (SAS) for access control.

## Erroneous Deletion

Erroneous deletion refers to accidental deletion of data. Azure Storage mitigates this with:

- **Soft Delete:** Temporarily retains deleted data, allowing recovery within a specified retention period.

## Access Tiers

Access tiers define the accessibility and cost of stored data:

- **Hot Access Tier:** Ideal for frequently accessed data with higher storage costs.
- **Cool Access Tier:** Suitable for infrequently accessed data with lower storage costs.
- **Archive Access Tier:** For rarely accessed data with the lowest storage costs.

## Hierarchy

Azure Storage services follow this hierarchy:

- **Tenant**
  - **Subscription**
    - **Resource Group (RG)**
      - **Storage Account**
        - **BLOB, File, Disk, Table, Queue (Storage Services)**

This hierarchical structure organizes and manages Azure resources efficiently.

## Snapshot

Snapshots are:

- **Point-in-time Copies:** Captures the state of a storage resource at a specific moment.
- **Usage:** Used for backup, data recovery, and versioning of storage resources.

## Steps to configure a Storage Account and BLOB Storage
1. **Create a Storage Account:**
- Navigate to the Azure portal, click on "Create a resource," select "Storage account," and fill in the necessary details.
- Choose the subscription, resource group, storage account name, and region. Select the desired performance (Standard or Premium), redundancy (LRS, GRS, etc.), and access tier (Hot, Cool, etc.).
2. **Create a Container in BLOB Storage:**

- Go to the newly created storage account, select "Containers" under the BLOB service section.
- Click on "+ Container," enter the name, and set the public access level (Private, Blob, or Container).
3. **Upload a Blob:**

- Inside the container, click on "Upload," select the file to upload, and click "Upload."
```
Tenant
   └── Subscription
         └── Resource Group (RG)
               └── Storage Account
                     ├── BLOB
                     │    ├── Container
                     │    │    └── Blob (e.g., image.jpg)
                     │    └── ...
                     ├── FILE
                     ├── TABLE
                     └── QUEUE

```
---

## Conclusion

Azure Storage Services offer a robust platform for storing and managing data in the cloud. Understanding the different storage types, their benefits, and usage scenarios helps in choosing the right storage solution based on application needs and cost considerations.
