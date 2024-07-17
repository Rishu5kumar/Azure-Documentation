# Day 15 Content: Azure File Sync, Snapshot, and Swap OS Disk

## Table of Contents

1. [Azure File Sync](#azure-file-sync)
2. [Snapshot](#snapshot)
3. [Swap OS Disk](#swap-os-disk)
4. [Conclusion](#conclusion)

---

## Azure File Sync

### Overview
Azure File Sync is a service that enables synchronization and centralization of files from on-premises Windows servers to Azure file shares. It provides cloud tiering to optimize storage usage.
It enables synchronization between Windows VMs and Azure file shares, ensuring that data changes made on-premises are automatically synced with Azure file shares.

Key points:
- **Sync Functionality:** Azure File Sync synchronizes data between on-premises Windows Servers and Azure file shares, allowing organizations to leverage cloud storage benefits while maintaining local data access.
- **Data Protection:** It facilitates robust data protection and disaster recovery strategies by syncing data offsite to Azure file shares.
- **Local Access:** Users can access their files locally even though they are synced with Azure file shares.
- **Usage Scenarios:** Useful when organizations need to manage limited on-premises storage while utilizing Azure file shares effectively.
- **Limitations:** Azure File Sync does not support syncing for the C drive on Windows VMs.

### Use Cases
- Hybrid cloud environments requiring local access to files synced with Azure.
- Robust data protection and disaster recovery strategies.

### Cloud Tiering
Cloud tiering allows optimization of storage usage:
- **Volume Policy**: Defines how much space should be kept free locally.
- **Date Policy**: Sets retention rules for data on-premises.

### Setup Steps
1. **Create a Resource Group and Windows VM**
   - Provision a Windows VM in Azure.

2. **Attach a Disk to VM and Create Volume**
   - Attach a disk to the VM and create a volume.

3. **Set up Azure File Sync**
   - Install Azure File Sync agent on the VM.
   - Create a storage account and file share.
   - Configure the Agent:Sign in with your Azure account credentials and enter the tenant ID to configure the agent on your Windows Server.
   - Configure the sync group and add server endpoints.

### Important Note:

- This solution is applicable only on Windows servers.
- Frequently accessed data is kept on-premises, while infrequent data is stored on Azure.
- The sync is one-way: data from on-premises is synced to Azure, but not the reverse.

### Diagram: On-Premises and Azure Sync Setup
```
+-------------------+       +-------------------+
| On-Premises       |       | Azure             |
| Windows Server    |       | Storage Account   |
+-------------------+       +-------------------+
         |                           |
         v                           v
+-------------------+       +-------------------+
| Frequently Accessed|       | Infrequently Accessed|
| Data               |       | Data                |
+-------------------+       +-------------------+
         |                           |
         v                           v
+-------------------+       +-------------------+
| Azure File Sync   |       | Azure File Sync   |
+-------------------+       +-------------------+
```

### Example
Imagine a scenario where a company has critical files stored on their on-premises Windows Server. They decide to use Azure File Sync to sync these files to Azure for backup and disaster recovery purposes. By setting up Azure File Sync, any changes made to files on the on-premises server are automatically synchronized to Azure File Share, ensuring data consistency and availability in case of on-premises failures.

---

## Snapshot

### Overview
Snapshots capture the state of a disk at a point in time. They are used for backup, migration, and replication.

When using Azure Storage Explorer for migration, you typically copy a disk from one Azure account to another to create a VM. However, this method requires access credentials from both Azure accounts. If access is denied by the client, an alternative method involves using snapshots.

A snapshot captures the state of an OS disk or any disk at a specific moment. By creating a snapshot using tools like Azure Storage Explorer, you can then create a new disk from this snapshot. Subsequently, you can use this new disk to create a VM, effectively replicating or migrating the original VM's configuration.

- Snapshot creation captures the current state of a disk.
- This snapshot can then be used to create a new disk.
- The new disk is employed to configure a VM, facilitating replication or migration tasks without requiring direct access to another Azure account.

### Use Cases
- Backup and restore of OS or data disks.
- Migration of VM configurations across subscriptions.

### Creating Snapshots
- Use Azure Portal or Azure CLI to create snapshots.
- Snapshots can be used to create new disks or VMs.

### Limitations
- Cannot decrease disk size, only increase.
- Requires VM deallocation for certain operations.

### Diagram
```
+-------------------+        +-------------------+
| Existing Disk     |        | Snapshot          |
+-------------------+        +-------------------+
         |                          |
         v                          v
+-------------------+        +-------------------+
| Create Snapshot   |        | New VM            |
+-------------------+        +-------------------+
         |                          |
         v                          v
+-------------------+        +-------------------+
| New VM from       |        | Snapshot          |
| Snapshot          |        |                   |
+-------------------+        +-------------------+
```
### Example
Suppose an IT administrator needs to perform a major update on a production VM. Before making changes, they create a snapshot of the VM's OS disk using Azure Portal. This snapshot serves as a backup, allowing them to revert to the previous state if the update causes issues. After creating the snapshot, they can also use it to create a new VM for testing purposes without impacting the production environment.

---

## Swap OS Disk

### Overview
Swap OS Disk allows switching OS disks between VMs without changing IP or MAC addresses.

### Use Cases
- Safely apply configuration changes to VMs.
- Maintain IP whitelisting during VM updates.

### Implementation Steps
1. **Create a Snapshot**
   - Create a snapshot of the original VM's OS disk.

2. **Create a VM from Snapshot**
   - Provision a new VM using the snapshot as its OS disk.

3. **Swap OS Disks**
   - Swap the OS disk of the running VM with the snapshot disk.

### Diagram
```
+-------------------+       +-------------------+
| Current OS Disk   |       | Backup OS Disk    |
+-------------------+       +-------------------+
         |                           |
         v                           v
+-------------------+       +-------------------+
| Detach Current    |       | Attach Backup     |
| OS Disk           |       | OS Disk           |
+-------------------+       +-------------------+
         |                           |
         v                           v
+-------------------+       +-------------------+
| VM with Backup    |       | OS Disk           |
+-------------------+       +-------------------+
```
### Example
Consider a scenario where a financial services company needs to update the software on their production VM but cannot afford any downtime or changes to the VM's IP and MAC addresses due to strict regulatory requirements. They use Swap OS Disk to create a snapshot of the VM's OS disk before making changes. Once the snapshot is created, they provision a new VM using this snapshot, apply the necessary updates, and then swap the OS disk back to the original VM seamlessly, maintaining compliance with IP whitelisting rules.

---

## Conclusion

This markdown file covered Azure File Sync for syncing on-premises files with Azure file shares, Snapshot for capturing disk states for backup and migration, and Swap OS Disk for safely applying configuration changes to VMs. These Azure services provide robust solutions for managing and optimizing storage and VM operations in hybrid cloud environments.
