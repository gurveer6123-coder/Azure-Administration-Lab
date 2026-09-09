# Microsoft Azure Administration Lab

## Overview

This project demonstrates hands-on administration of a Microsoft Azure environment.

The lab focuses on practical tasks commonly performed by Cloud Support Technicians, IT Support Specialists, Junior System Administrators, and Azure Administrators, including:

- Resource organization
- Virtual networking
- Network security
- Windows Server virtual machine deployment
- Remote administration
- Monitoring and logging
- Role-Based Access Control (RBAC)
- Resource tagging and locking
- Cost management
- Azure Advisor
- Service Health alerts
- Azure Blob Storage
- Shared Access Signatures (SAS)
- Cross-region VNet peering

The goal of this project was to build a small Azure environment while also practicing security, troubleshooting, monitoring, and cost-control techniques.

---

# Technologies Used

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
- Azure RBAC
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

# Lab Architecture

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
- Private IP: `10.1.0.4`

The two virtual networks were later connected using Azure Global VNet Peering.

---

# 1. Resource Group

A resource group named:

`Azure-Admin-Lab`

was used to organize the Azure resources created throughout the project.

Resource Groups provide a logical management boundary for related Azure resources and make administration, monitoring, access control, and cleanup easier.

![Azure Resource Group](01-resource-group.png)

---

# 2. Virtual Network

A virtual network named:

`Lab-VNet-India`

was created in the Central India Azure region.

The VNet used the following address space:

`10.1.0.0/16`

The virtual network provides private IP addressing and network connectivity for Azure resources.

![Virtual Network](02-vnet-india.png)

---

# 3. Subnet Configuration

A subnet was configured inside `Lab-VNet-India`.

Configuration:

- Address Space: `10.1.0.0/16`
- Subnet Range: `10.1.0.0/24`
- Starting Address: `10.1.0.0`

The subnet provides a smaller network segment where the Windows Server virtual machine was deployed.

![Subnet Configuration](03-subnet.png)

---

# 4. Network Security Group

A Network Security Group named:

`LAB-NSG-India`

was created to control inbound and outbound traffic.

Network Security Groups evaluate traffic using rules based on:

- Source
- Destination
- Port
- Protocol
- Priority
- Allow or Deny action

The NSG was associated with the lab network and virtual machine.

![NSG Overview](04-nsg-overview.png)

---

# 5. Securing RDP Access

Remote Desktop Protocol uses TCP port:

`3389`

Initially, the VM could not be reached through RDP because inbound traffic was blocked by the default NSG security rules.

A custom rule named:

`ALLOWRDP`

was created.

Configuration included:

- Service: RDP
- Protocol: TCP
- Destination Port: `3389`
- Action: Allow
- Priority: `100`
- Source: Restricted administrator public IP

Restricting the source IP instead of allowing RDP from the entire Internet reduces unnecessary exposure of the administrative port.

![RDP Security Rule](05-rdp-security-rule.png)

---

# 6. Windows Server Virtual Machine

A Windows Server virtual machine was deployed in Azure.

Configuration:

- Name: `Lab-WinVM`
- Operating System: Windows Server 2022 Datacenter
- Architecture: x64
- VM Size: Standard B-series
- Region: Central India
- Virtual Network: `Lab-VNet-India`
- Subnet: `default`
- Private IP: `10.1.0.4`
- Public IP: Configured for lab RDP access
- OS Disk: Standard SSD

Standard SSD storage and a smaller VM size were selected to reduce unnecessary lab costs.

![Windows Server VM](06-windows-vm-overview.png)

---

# 7. Verify VM Network Configuration

After connecting to the Windows Server VM, the following command was used:

```cmd
ipconfig
```

The VM received the following network configuration:

- IPv4 Address: `10.1.0.4`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `10.1.0.1`

This confirmed that the VM was correctly connected to the Azure subnet.

![VM IP Configuration](08-vm-ipconfig.png)

---

# 8. Azure VM Monitoring

Azure Monitor was used to review the health and performance of the virtual machine.

Metrics included:

- VM availability
- CPU utilization
- Network traffic
- Disk activity
- Disk operations

Monitoring helps administrators detect resource utilization problems and troubleshoot performance issues.

![VM Monitoring](09-vm-monitoring.png)

---

# 9. Azure Activity Log

The Azure Activity Log was reviewed to track administrative operations performed on the VM.

Examples included:

- Create or Update Virtual Machine
- Add Management Locks

The Activity Log provides information about:

- Operation performed
- Status
- Timestamp
- Resource
- Subscription
- User or service that initiated the change

This is useful for troubleshooting and auditing administrative activity.

![Activity Log](10-activity-log.png)

---

# 10. Role-Based Access Control

Azure Access Control (IAM) was reviewed to understand Role-Based Access Control.

The lab account inherited the `Owner` role from the subscription scope.

Common Azure roles include:

### Reader

Can view Azure resources but cannot modify them.

### Contributor

Can create and manage resources but cannot assign Azure RBAC permissions.

### Virtual Machine Contributor

Can manage virtual machines without receiving unrestricted control over all resources.

### Owner

Provides full resource management access and permission to assign Azure roles.

RBAC allows organizations to apply the principle of least privilege.

![Azure RBAC](11-rbac-iam.png)

---

# 11. Resource Tags

A resource tag was added to identify the VM as a lab resource.

Tag:

`Environment : LAB`

Tags can be used to organize Azure resources by:

- Environment
- Department
- Project
- Owner
- Cost center
- Application

They are also useful for reporting and cost analysis.

![Azure Resource Tags](12-resource-tags.png)

---

# 12. Resource Lock

A Delete Lock was configured on the virtual machine.

Lock name:

`Protect-LabVM`

Lock type:

`Delete`

The lock protects the VM from accidental deletion.

The lock must first be removed before intentionally deleting the protected resource.

![Resource Lock](13-resource-lock.png)

---

# 13. Cost Management

Azure Cost Management was reviewed to understand:

- Current cloud spending
- Forecasted spending
- Resource costs
- Budgeting
- Cost alerts

Cost monitoring is especially important in cloud environments because resources can continue generating charges while they remain deployed.

![Azure Cost Management](14-cost-budget.png)

> Note: Sensitive billing and account information should always be removed before publishing screenshots publicly.

---

# 14. Resource Visualizer

Azure Resource Visualizer was used to view relationships between the resources in the lab.

The diagram displayed dependencies between resources such as:

- Virtual Machine
- Network Interface
- Public IP
- Virtual Networks
- Network Security Groups
- OS Disk
- Storage Account
- Service Health Alert
- Action Group

This is useful for understanding how Azure resources depend on each other.

![Azure Resource Visualizer](15-resource-visualizer.png)

---

# 15. Azure Advisor

Azure Advisor was reviewed for recommendations relating to:

- Cost
- Security
- Reliability
- Operational Excellence
- Performance

Advisor analyzes deployed resources and provides recommendations based on Azure best practices.

Not every recommendation was implemented because some production-oriented features would unnecessarily increase costs for a temporary lab.

Examples of recommendations reviewed included:

- Larger VM sizes
- Premium storage
- NAT Gateway
- Higher availability
- VM Scale Sets

This demonstrated that administrators should evaluate recommendations based on business and technical requirements rather than automatically implementing every suggestion.

![Azure Advisor](16-azure-advisor.png)

---

# 16. Azure Service Health Alert

Azure Service Health was configured to provide notifications about Microsoft Azure infrastructure problems.

A Service Health alert named:

`serviceHealth`

was created.

The alert monitors:

- Azure service issues
- Selected Azure regions
- Subscription-level service health

The `Microsoft.Insights` resource provider was also registered at the subscription level to support Azure Monitor alert functionality.

Service Health alerts allow administrators to respond quickly when Azure infrastructure issues affect their workloads.

![Service Health Alert](17-service-health-alert.png)

---

# 17. Azure Storage

An Azure Storage Account named:

`labstorageazure2026`

was created.

The storage environment used:

- Standard performance
- Locally Redundant Storage
- Secure transfer
- TLS 1.2
- Microsoft-managed encryption
- Anonymous Blob access disabled

Standard LRS was selected because it provides a cost-effective storage configuration for a temporary lab.

![Azure Storage](18-storage-account.png)

---

# 18. Azure Blob Storage

A Blob Storage container named:

`labcontainer`

was created.

A test file named:

`azure-test.txt`

was uploaded to the container.

The storage structure is:

```text
Storage Account
      |
      +-- Container
             |
             +-- Blob
```

For this lab:

```text
labstorageazure2026
      |
      +-- labcontainer
             |
             +-- azure-test.txt
```

Azure Blob Storage can be used for:

- Documents
- Images
- Videos
- Application files
- Backups
- Archives
- Logs
- Analytics datasets

![Blob Container](19-blob-container.png)

---

# 19. Shared Access Signature

The Blob container was configured as private.

A Shared Access Signature was then generated to provide temporary delegated access to the storage resource.

The SAS configuration demonstrated concepts including:

- Read-only access
- Expiration time
- HTTPS access
- Temporary authorization

A SAS token should be treated like a credential and should never be committed to a public GitHub repository.

The actual SAS token used in the lab is intentionally not included in this repository.

---

# 20. Global VNet Peering

The two Azure virtual networks were connected using VNet Peering.

Networks:

### Canada Central

```text
Lab-VNet
10.0.0.0/16
```

### Central India

```text
Lab-VNet-India
10.1.0.0/16
```

The address ranges did not overlap, allowing Azure to establish the peering.

The resulting status showed:

`Connected`

and:

`Fully Synchronized`

Because the virtual networks are located in different Azure regions, this represents Global VNet Peering.

Global VNet Peering allows Azure resources in different regions to communicate privately using Microsoft's network infrastructure.

![VNet Peering](20-vnet-peering.png)

---

# Troubleshooting

## RDP Connection Failure

One of the main troubleshooting scenarios in this project occurred when Remote Desktop initially failed to connect to the Windows Server VM.

Azure NSG diagnostics showed that traffic to port:

`3389`

was being denied.

The blocking rule was the default:

`DenyAllInBound`

### Resolution

A custom inbound NSG rule was created with:

```text
Protocol: TCP
Port: 3389
Source: Administrator Public IP
Action: Allow
Priority: 100
```

After the rule was applied, RDP connectivity was successfully established.

This demonstrated how Network Security Groups can directly affect Azure VM connectivity.

---

# Security Practices Demonstrated

The project included several security concepts.

## Restricted RDP

Port `3389` was limited to the administrator source IP instead of allowing access from all Internet addresses.

## Private Blob Container

Anonymous Blob access was disabled.

## Shared Access Signature

Temporary access was provided through a limited SAS instead of making the storage container public.

## HTTPS

Secure transport was used when accessing Azure Storage.

## Azure RBAC

Role assignments were reviewed through Azure IAM.

## Resource Lock

A Delete Lock protected the VM from accidental deletion.

## TLS

Azure Storage was configured to require TLS 1.2.

---

# Cost Optimization

Because the lab used limited Azure credits, several cost-control practices were used.

These included:

- Selecting a lower-cost VM size
- Using Standard SSD instead of Premium SSD
- Using Standard LRS storage
- Avoiding unnecessary additional disks
- Avoiding Azure Firewall
- Avoiding Azure Bastion
- Avoiding NAT Gateway
- Avoiding unnecessary backup services
- Reviewing Azure Advisor before applying recommendations
- Monitoring current Azure spending
- Using budgets and alerts
- Deallocating the VM when it was not required

A VM should be stopped from the Azure portal until its status becomes:

`Stopped (deallocated)`

When a VM is deallocated, compute billing stops, although storage and some related resources may still generate small charges.

---

# Skills Demonstrated

This project demonstrates practical experience with:

- Microsoft Azure administration
- Resource Groups
- Azure Virtual Networks
- IPv4 addressing
- Subnetting
- Network Security Groups
- Windows Server deployment
- Remote Desktop administration
- Azure VM troubleshooting
- Azure Monitor
- Azure Activity Logs
- Azure RBAC
- Azure Tags
- Resource Locks
- Azure Cost Management
- Azure Advisor
- Azure Service Health
- Azure Storage Accounts
- Azure Blob Storage
- Shared Access Signatures
- VNet Peering
- Cross-region networking
- Cloud security
- Cloud cost optimization

---

# Key Takeaways

This lab provided practical experience building and administering an Azure environment from the ground up.

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
RDP Administration
      |
      v
Monitoring + Logging
      |
      v
RBAC + Resource Protection
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

The project also demonstrated that Azure administration involves more than deploying resources. Administrators must continuously consider:

- Security
- Availability
- Monitoring
- Troubleshooting
- Permissions
- Cost
- Resource organization

---

# Cleanup

After completing the project, unused Azure resources should be stopped or removed to prevent unnecessary charges.

Recommended cleanup process:

1. Stop the Windows Server VM.
2. Verify the VM shows `Stopped (deallocated)`.
3. Remove the `Protect-LabVM` Delete Lock before deleting the VM.
4. Delete unused Public IP resources.
5. Delete unused Network Interfaces.
6. Delete unused Managed Disks.
7. Delete temporary Blob Storage resources.
8. Remove VNet peering if it is no longer required.
9. Delete the `Azure-Admin-Lab` Resource Group when the environment is no longer needed.

---

## Project Status

**Completed**

This lab was created as part of my hands-on practice in Microsoft Azure administration, networking, security, monitoring, storage, and cloud resource management.
