# Azure Administration Lab

## Overview

This project demonstrates hands-on administration of Microsoft Azure infrastructure, including resource organization, virtual networking, network security, Windows Server deployment, monitoring, role-based access control, cost management, storage, and VNet peering.

The lab was designed to simulate common Azure administration tasks that may be performed by an IT Support Technician, Cloud Support Technician, Junior System Administrator, or Azure Administrator.

---

## Technologies and Services Used

- Microsoft Azure
- Azure Resource Groups
- Azure Virtual Networks
- Azure Subnets
- Network Security Groups (NSGs)
- Azure Virtual Machines
- Windows Server 2022
- Remote Desktop Protocol (RDP)
- Azure Monitor
- Azure Activity Log
- Azure RBAC / IAM
- Azure Resource Locks
- Azure Tags
- Azure Cost Management
- Azure Advisor
- Azure Service Health
- Azure Storage Accounts
- Azure Blob Storage
- Shared Access Signatures (SAS)
- Global VNet Peering

---

# Lab Architecture

The environment used two Azure virtual networks in separate Azure regions.

## Canada Central

- Resource Group: `Azure-Admin-Lab`
- Virtual Network: `Lab-VNet`
- Address Space: `10.0.0.0/16`
- Subnet: `Lab-Subnet`
- Subnet Range: `10.0.1.0/24`

## Central India

- Virtual Network: `Lab-VNet-India`
- Address Space: `10.1.0.0/16`
- Subnet Range: `10.1.0.0/24`
- Network Security Group: `LAB-NSG-India`
- Windows VM: `Lab-WinVM`
- Private IPv4 Address: `10.1.0.4`

The Canada Central and Central India virtual networks were later connected using Azure Global VNet Peering.

---

# 1. Resource Group Creation

A resource group named `Azure-Admin-Lab` was created to organize the resources used throughout the project.

Azure Resource Groups provide a logical container for related cloud resources and simplify management, monitoring, access control, and cleanup.

![Resource Group](screenshots/01-resource-group.png)

---

# 2. Virtual Network Configuration

A virtual network was created to provide private networking for Azure resources.

The Central India environment used:

- VNet: `Lab-VNet-India`
- Address Space: `10.1.0.0/16`
- Subnet: `10.1.0.0/24`

This allows Azure resources to communicate using private IP addresses.

![Virtual Network](screenshots/02-vnet-india.png)

---

# 3. Subnet Configuration

The virtual network was divided into a smaller subnet.

Subnet:

`10.1.0.0/24`

The Windows Server VM was later assigned the private IP address:

`10.1.0.4`

![Subnet](screenshots/03-subnet.png)

---

# 4. Network Security Group

A Network Security Group named `LAB-NSG-India` was created to control inbound and outbound network traffic.

Azure NSGs operate using security rules based on:

- Source
- Destination
- Protocol
- Port
- Priority
- Allow/Deny action

![NSG](screenshots/04-nsg-overview.png)

---

# 5. Securing Remote Desktop Access

Remote Desktop Protocol uses TCP port:

`3389`

Initially, RDP connectivity failed because the default NSG rule was blocking inbound traffic.

Azure Network Diagnostics identified:

`DefaultRule_DenyAllInBound`

as the rule blocking RDP.

A custom inbound rule was created to allow TCP port `3389`.

For improved security, RDP access was restricted to my current public IP address instead of allowing connections from the entire Internet.

This follows the principle of reducing unnecessary exposure of administrative ports.

![RDP Security Rule](screenshots/05-rdp-security-rule.png)

---

# 6. Windows Server Virtual Machine

A Windows Server virtual machine was deployed in Central India.

Configuration:

- VM Name: `Lab-WinVM`
- Operating System: Windows Server 2022 Datacenter
- Architecture: x64
- VM Size: `Standard_B2as_v2`
- OS Disk: Standard SSD
- Virtual Network: `Lab-VNet-India`
- Subnet: `10.1.0.0/24`
- NSG: `LAB-NSG-India`
- Public IP: Enabled for lab RDP connectivity
- Azure Spot Instance: Disabled
- Backup: Disabled for this temporary lab

Standard SSD storage was selected instead of Premium SSD to reduce unnecessary lab costs.

![Windows VM Overview](screenshots/06-windows-vm-overview.png)

---

# 7. Remote Desktop Administration

The Windows Server VM was administered remotely using Remote Desktop Protocol.

The VM was accessed using its Azure public IP and the administrator account created during deployment.

RDP connectivity was successfully established after the NSG security rule was corrected.

![Windows Server RDP](screenshots/07-rdp-windows-server.png)

---

# 8. Verify VM Network Configuration

Inside Windows Server, the following command was used:

```cmd
ipconfig
