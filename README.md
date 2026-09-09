# Microsoft Azure Administration Lab

## Overview

This project demonstrates hands-on administration of a Microsoft Azure environment.

The lab was designed to practice common tasks performed by Cloud Support Technicians, IT Support Specialists, Junior System Administrators, and Azure Administrators.

The project covers:

- Azure Resource Groups
- Virtual Networks and Subnets
- Network Security Groups
- Windows Server Virtual Machines
- Remote Desktop administration
- Azure monitoring and logging
- Role-Based Access Control
- Resource tags and locks
- Cost management
- Azure Advisor
- Service Health alerts
- Azure Storage
- Blob Storage
- Shared Access Signatures
- Global VNet Peering
- Security and cost optimization

---

# Technologies and Services Used

- Microsoft Azure
- Azure Resource Manager
- Azure Virtual Networks
- Azure Subnets
- Network Security Groups
- Azure Virtual Machines
- Windows Server 2022
- Remote Desktop Protocol
- Azure Monitor
- Azure Activity Log
- Azure IAM / RBAC
- Azure Resource Tags
- Azure Resource Locks
- Azure Cost Management
- Azure Advisor
- Azure Service Health
- Azure Storage Accounts
- Azure Blob Storage
- Shared Access Signatures
- Global VNet Peering

---

# Environment Overview

The lab used Azure resources across two regions.

## Canada Central

- Virtual Network: `Lab-VNet`
- Address Space: `10.0.0.0/16`
- Network Security Group: `LAB-NSG`

## Central India

- Virtual Network: `Lab-VNet-India`
- Address Space: `10.1.0.0/16`
- Subnet: `10.1.0.0/24`
- Network Security Group: `LAB-NSG-India`
- Virtual Machine: `Lab-WinVM`
- Operating System: Windows Server 2022
- Private IP Address: `10.1.0.4`

The two virtual networks were later connected using Azure Global VNet Peering.

---

# 1. Azure Resource Group

A Resource Group named:

`Azure-Admin-Lab`

was used to organize all resources created during the project.

Azure Resource Groups provide a logical container for resources and simplify:

- Resource organization
- Permissions
- Monitoring
- Cost tracking
- Deployment
- Cleanup

![Azure Resource Group](01-resource-group.png)

---

# 2. Azure Virtual Network

A Virtual Network named:

`Lab-VNet-India`

was configured in the Central India Azure region.

Address space:

`10.1.0.0/16`

Virtual Networks allow Azure resources to communicate using private IP addressing.

![Azure Virtual Network](02-vnet-india.png)

---

# 3. Subnet Configuration

A subnet was configured inside `Lab-VNet-India`.

Configuration:

- VNet Address Space: `10.1.0.0/16`
- Subnet Range: `10.1.0.0/24`
- Starting Address: `10.1.0.0`

The Windows Server VM was deployed inside this subnet.

![Azure Subnet](03-subnet.png)

---

# 4. Network Security Group

A Network Security Group named:

`LAB-NSG-India`

was created to control traffic entering and leaving the Azure environment.

NSG rules can evaluate traffic based on:

- Source
- Destination
- Source Port
- Destination Port
- Protocol
- Priority
- Allow or Deny action

The NSG was associated with the lab network resources.

![NSG Overview](04-nsg-overview.png)

---

# 5. RDP Security Rule

Remote Desktop Protocol uses TCP port:

`3389`

Initially, RDP connectivity to the Azure VM failed because inbound traffic was being blocked.

A custom inbound security rule named:

`ALLOWRDP`

was configured.

The rule used:

```text
Protocol: TCP
Service: RDP
Destination Port: 3389
Action: Allow
Priority: 100
Source: Restricted administrator public IP
```

Instead of allowing RDP from the entire Internet, the rule was restricted to the administrator's public IP address.

This reduces unnecessary exposure of the RDP service.

![RDP Security Rule](05-rdp-security-rule.png)

---

# 6. Windows Server Virtual Machine

A Windows Server virtual machine was deployed in Azure.

Configuration:

- VM Name: `Lab-WinVM`
- Operating System: Windows Server 2022 Datacenter
- Architecture: x64
- Region: Central India
- Virtual Network: `Lab-VNet-India`
- Subnet: `default`
- Private IP: `10.1.0.4`
- Public IP: Configured for lab RDP access
- OS Disk: Standard SSD
- VM Size: Standard B-series

A lower-cost VM size and Standard SSD were selected because this environment was created for temporary lab use.

![Windows Server VM](06-windows-vm-overview.png)

---

# 7. VM Network Verification

After successfully connecting to the Windows Server VM, the following command was executed:

```cmd
ipconfig
```

The VM received:

```text
IPv4 Address: 10.1.0.4
Subnet Mask: 255.255.255.0
Default Gateway: 10.1.0.1
```

This confirmed that the virtual machine was correctly connected to the Azure subnet.

![VM IP Configuration](08-vm-ipconfig.png)

---

# 8. Azure VM Monitoring

Azure Monitor was used to observe virtual machine performance and health.

Metrics reviewed included:

- VM availability
- CPU utilization
- Network traffic
- Disk activity
- Disk operations

Monitoring helps administrators identify performance problems and investigate abnormal resource utilization.

![Azure VM Monitoring](09-vm-monitoring.png)

---

# 9. Azure Activity Log

The Azure Activity Log was reviewed to track administrative operations performed on the virtual machine.

Examples included:

- Create or Update Virtual Machine
- Add Management Locks

The Activity Log can help determine:

- What operation occurred
- When the operation occurred
- Whether it succeeded or failed
- Which Azure resource was affected

This information is useful for both troubleshooting and auditing.

![Azure Activity Log](10-activity-log.png)

---

# 10. Azure Role-Based Access Control

Azure Access Control (IAM) was reviewed to understand Role-Based Access Control.

The account used during the lab inherited the:

`Owner`

role from the subscription.

Common Azure roles include:

## Reader

Can view resources but cannot modify them.

## Contributor

Can create and manage Azure resources but cannot assign RBAC permissions.

## Virtual Machine Contributor

Can manage virtual machines without having full control over every Azure resource.

## Owner

Provides full resource management access and can assign Azure roles.

Azure RBAC helps organizations implement the principle of least privilege.

![Azure RBAC](11-rbac-iam.png)

---

# 11. Azure Resource Tags

A resource tag was added to the VM.

Tag:

`Environment : LAB`

Tags can help organize Azure resources based on:

- Environment
- Project
- Department
- Application
- Owner
- Cost center

Tags are also useful when analyzing cloud spending.

![Azure Resource Tags](12-resource-tags.png)

---

# 12. Azure Resource Lock

A Delete Lock was configured on the VM.

Lock name:

`Protect-LabVM`

Lock type:

`Delete`

The lock protects the virtual machine from accidental deletion.

The lock must be removed before intentionally deleting the protected resource.

![Azure Resource Lock](13-resource-lock.png)

---

# 13. Azure Cost Management

Azure Cost Management was reviewed to understand cloud resource spending and budgeting.

Topics practiced included:

- Current spending
- Forecasted spending
- Resource costs
- Monthly budgets
- Cost alerts
- Cost optimization

Cloud administrators must monitor resource usage because deployed services can continue generating charges even when they are not actively being used.

![Azure Cost Management](14-cost-budget.png)

> The budget image in this repository is a sanitized/illustrative view used to document the cost-management portion of the lab. No account credentials or sensitive billing information are included.

---

# 14. Azure Resource Visualizer

Azure Resource Visualizer was used to understand relationships between deployed resources.

The visualizer displayed components such as:

- Virtual Machine
- Network Interface
- Public IP
- Virtual Networks
- Network Security Groups
- Managed Disk
- Storage Account
- Service Health Alert
- Action Group

This provides administrators with a visual understanding of resource dependencies.

![Azure Resource Visualizer](15-resource-visualizer.png)

---

# 15. Azure Advisor

Azure Advisor was reviewed to examine Microsoft recommendations for the Azure environment.

Advisor provides recommendations across areas including:

- Cost
- Security
- Reliability
- Performance
- Operational Excellence

Some recommendations were intentionally not implemented because they were designed for production workloads and would unnecessarily increase the cost of a temporary lab environment.

Examples included:

- Larger VM sizes
- Premium storage
- NAT Gateway
- Higher availability configurations
- Virtual Machine Scale Sets

This demonstrated that administrators should evaluate recommendations based on workload requirements rather than automatically implementing every suggestion.

![Azure Advisor](16-azure-advisor.png)

---

# 16. Azure Service Health

Azure Service Health was configured to monitor Azure infrastructure problems.

A Service Health alert named:

`serviceHealth`

was created.

The alert monitored Azure service issues affecting the selected subscription and region.

Azure Service Health can provide information about:

- Service incidents
- Planned maintenance
- Health advisories

The `Microsoft.Insights` resource provider was also registered at the subscription level to support Azure monitoring and alert functionality.

![Azure Service Health Alert](17-service-health-alert.png)

---

# 17. Azure Storage Account

An Azure Storage Account named:

`labstorageazure2026`

was created.

The storage configuration included:

- Standard performance
- Locally Redundant Storage (LRS)
- Secure transfer enabled
- TLS 1.2
- Microsoft-managed encryption
- Anonymous Blob access disabled

LRS was selected because it provided an appropriate low-cost configuration for this temporary lab.

![Azure Storage Account](18-storage-account.png)

---

# 18. Azure Blob Storage

A Blob Storage container named:

`labcontainer`

was created.

A small test file named:

`azure-test.txt`

was uploaded to the container.

Azure Blob Storage follows the hierarchy:

```text
Storage Account
      |
      +-- Container
             |
             +-- Blob
```

For this project:

```text
labstorageazure2026
      |
      +-- labcontainer
             |
             +-- azure-test.txt
```

Common Blob Storage use cases include:

- Documents
- Images
- Videos
- Application files
- Backups
- Archives
- Log files
- Analytics datasets

![Azure Blob Container](19-blob-container.png)

---

# 19. Shared Access Signature

The Blob container was configured without anonymous public access.

A Shared Access Signature was generated to provide temporary access to the storage resource.

The SAS configuration demonstrated:

- Temporary authorization
- Read-only access
- Expiration times
- HTTPS access
- Controlled access to private data

A SAS token functions similarly to a temporary credential and should never be committed to a public GitHub repository.

The actual SAS token used during the lab is therefore not included in this project.

---

# 20. Global VNet Peering

The Canada Central and Central India virtual networks were connected using Azure VNet Peering.

The networks were:

## Canada Central

```text
Lab-VNet
10.0.0.0/16
```

## Central India

```text
Lab-VNet-India
10.1.0.0/16
```

Because the IP address spaces did not overlap, Azure could establish the peering successfully.

The peering displayed:

```text
Peering State: Connected
Peering Sync Status: Fully Synchronized
```

Because the VNets were located in different Azure regions, this configuration demonstrated Global VNet Peering.

Global VNet Peering allows Azure resources in separate regions to communicate privately through Microsoft's network infrastructure.

![Azure VNet Peering](20-vnet-peering.png)

---

# Troubleshooting Scenario

## RDP Connection Failure

One of the major troubleshooting exercises during this lab involved Remote Desktop access.

Initially, the Windows Server VM could not be reached over RDP.

Azure networking diagnostics showed that inbound traffic to TCP port:

`3389`

was being denied.

The default NSG rule:

`DenyAllInBound`

was blocking the connection.

## Resolution

A custom inbound NSG rule was created.

```text
Rule Name: ALLOWRDP
Protocol: TCP
Destination Port: 3389
Source: Administrator Public IP
Action: Allow
Priority: 100
```

After applying the security rule, the RDP connection succeeded.

This exercise demonstrated how Network Security Groups affect Azure VM connectivity and how security rules can be used to troubleshoot network access.

---

# Security Practices Demonstrated

Several security practices were included in the lab.

## Restricted RDP Access

RDP port `3389` was limited to a specific administrator source IP instead of being open to the entire Internet.

## Network Security Groups

NSGs were used to control inbound and outbound Azure network traffic.

## Private Blob Storage

Anonymous public access to the Blob container was disabled.

## Shared Access Signature

Temporary delegated access was provided using a SAS rather than making the container publicly available.

## HTTPS and TLS

Secure transfer and TLS 1.2 were used for Azure Storage.

## Azure RBAC

Azure IAM role assignments were reviewed to understand least-privilege access.

## Resource Lock

A Delete Lock protected the virtual machine from accidental deletion.

## Sensitive Information Protection

Passwords, SAS tokens, subscription IDs, public IP addresses, and other sensitive values were removed from screenshots before publishing the project.

---

# Cost Optimization

Because the project used limited Azure credits, cost control was considered throughout the lab.

Cost-saving practices included:

- Using a smaller B-series VM
- Selecting Standard SSD
- Using Standard LRS storage
- Avoiding unnecessary additional disks
- Avoiding Azure Firewall
- Avoiding Azure Bastion
- Avoiding NAT Gateway
- Avoiding unnecessary backup services
- Reviewing Advisor recommendations before applying them
- Monitoring Azure costs
- Reviewing forecasted spending
- Using budgets and alerts
- Deallocating the VM when not required

When a virtual machine is not required, it should be stopped from the Azure portal until the status becomes:

`Stopped (deallocated)`

When a VM is deallocated, Azure compute billing stops, although storage and some associated resources may continue generating small charges.

---

# Skills Demonstrated

This project demonstrates practical experience with:

- Microsoft Azure administration
- Azure Resource Groups
- Azure Virtual Networks
- IPv4 networking
- Subnetting
- Network Security Groups
- Windows Server deployment
- Remote Desktop administration
- Azure networking troubleshooting
- Azure Monitor
- Azure Activity Logs
- Azure RBAC
- Resource Tags
- Resource Locks
- Azure Cost Management
- Azure Advisor
- Azure Service Health
- Azure Storage Accounts
- Azure Blob Storage
- Shared Access Signatures
- VNet Peering
- Cross-region networking
- Azure security
- Cloud cost optimization

---

# Key Takeaways

This lab provided hands-on experience creating and administering a small Azure environment.

The overall workflow included:

```text
Resource Group
      |
      v
Virtual Network
      |
      v
Subnet
      |
      v
Network Security Group
      |
      v
Windows Server VM
      |
      v
Remote Administration
      |
      v
Monitoring and Logging
      |
      v
RBAC and Resource Protection
      |
      v
Cost Management
      |
      v
Azure Storage
      |
      v
Global VNet Peering
```

The project demonstrated that Azure administration involves more than creating cloud resources.

Administrators must also consider:

- Security
- Networking
- Permissions
- Monitoring
- Troubleshooting
- Cost
- Resource organization
- Availability

---

# Cleanup

After completing the project, unused Azure resources should be stopped or deleted to prevent unnecessary charges.

Recommended cleanup steps:

1. Stop the Windows Server VM.
2. Confirm the status is `Stopped (deallocated)`.
3. Remove the `Protect-LabVM` Delete Lock before deleting the VM.
4. Delete unused Public IP resources.
5. Delete unused Network Interfaces.
6. Delete unused Managed Disks.
7. Delete temporary Blob Storage resources if no longer needed.
8. Remove VNet Peering if it is no longer needed.
9. Delete the `Azure-Admin-Lab` Resource Group after all documentation is complete.

---

# Project Status

**Completed**

This project was created as part of my hands-on practice with Microsoft Azure administration, networking, Windows Server, security, monitoring, storage, access control, troubleshooting, and cloud cost management.
