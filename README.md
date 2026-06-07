# Active Directory Homelab

## Overview

This project demonstrates the deployment and administration of a Windows-based Active Directory environment designed to simulate core enterprise identity and infrastructure services.

The lab was built using Windows Server 2019 and Windows 10 within VirtualBox and includes:

- Active Directory Domain Services (AD DS)
- Domain Name System (DNS)
- Dynamic Host Configuration Protocol (DHCP)
- Network Address Translation (NAT)
- Organizational Units (OUs)
- Group Policy Objects (GPOs)
- Domain-joined client management
- Automated user provisioning with Python

The objective of this project was to gain hands-on experience with identity management, centralized authentication, policy enforcement, network services, and automation commonly found in enterprise environments.

---

## Environment

| Component | Purpose |
|------------|------------|
| VirtualBox | Virtualization platform |
| Windows Server 2019 | Domain Controller |
| Windows 10 | Domain-joined client |
| Active Directory | Identity and access management |
| DNS | Internal name resolution |
| DHCP | Dynamic IP assignment |
| NAT/RRAS | Internet access for internal clients |
| Group Policy | Centralized policy enforcement |
| Python + pyad | Automated user provisioning |

---

## Network Topology

### Domain Controller

The Windows Server 2019 system functions as the Domain Controller and hosts:

- Active Directory Domain Services
- DNS
- DHCP
- Routing and Remote Access Services (RRAS)
- Group Policy Management

### Client Workstation

The Windows 10 workstation:

- Receives IP configuration from DHCP
- Uses domain DNS for name resolution
- Joins the Active Directory domain
- Receives Group Policy settings
- Authenticates with domain user accounts

---

## Project Objectives

- Build a functional Active Directory environment
- Configure DNS and DHCP services
- Create Organizational Units and administrative structure
- Implement Group Policy security controls
- Join client systems to the domain
- Configure NAT for outbound connectivity
- Automate user account creation using Python
- Validate authentication and policy enforcement

---

# Phase 1: Network Configuration

## Configure Server Network Interfaces

The Domain Controller was configured with two network adapters:

### Internal Network Adapter

| Setting | Value |
|----------|----------|
| IP Address | 10.2.22.1 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | None |
| DNS Server | 127.0.0.1 |

### External Network Adapter

Configured to obtain IP configuration automatically through DHCP for internet access.

### Purpose

The internal adapter provides connectivity for domain services while the external adapter allows controlled internet access for updates and testing.

![image1](images/phase1.png)

---

# Phase 2: Active Directory Installation

## Install Active Directory Domain Services

The AD DS role was installed through Server Manager and the server was promoted to a Domain Controller.

### Domain Information

| Setting | Value |
|----------|----------|
| Domain Name | streetrack.com |
| Forest Functional Level | Windows Server 2016+ |
| Server Name | DC |

### Outcome

The server was successfully promoted and configured as the first Domain Controller in a new forest.

[Insert Screenshot]

---

# Phase 3: Organizational Units and Administrative Accounts

## Create Organizational Units

Two Organizational Units were created:

- _ADMINS
- _USERS

This structure separates administrative accounts from standard user accounts and provides granular policy targeting.

[Insert Screenshot]

## Create Administrative User

An administrative account was created and added to the Domain Admins group.

### Tasks Completed

- Created domain administrator account
- Assigned administrative privileges
- Validated successful authentication

[Insert Screenshot]

---

# Phase 4: DHCP Deployment

## Install DHCP Server

The DHCP Server role was installed and authorized within Active Directory.

### DHCP Scope Configuration

| Setting | Value |
|----------|----------|
| Scope Name | Internal Network |
| Start Address | 10.2.22.100 |
| End Address | 10.2.22.200 |
| Subnet Mask | 255.255.255.0 |
| Gateway | 10.2.22.1 |
| DNS Server | 10.2.22.1 |
| Lease Duration | 8 Days |

### Outcome

Clients automatically receive valid IP configuration from the Domain Controller.

[Insert Screenshot]

---

# Phase 5: NAT and Internet Connectivity

## Configure RRAS and NAT

Routing and Remote Access Services (RRAS) was installed to provide internet connectivity to internal clients.

### Configuration Tasks

- Installed Remote Access role
- Enabled Routing
- Configured NAT
- Selected external interface for internet access

### Outcome

Domain-joined systems can access external resources while remaining on the internal network.

[Insert Screenshot]

---

# Phase 6: Domain Join

## Join Windows 10 Client to Domain

The Windows 10 workstation was joined to the Active Directory domain.

### Verification

- Domain membership confirmed
- Domain credentials accepted
- Successful authentication against Domain Controller

[Insert Screenshot]

---

# Phase 7: Connectivity Testing

## Validate Network Services

Several tests were performed to verify infrastructure functionality.

### DHCP Validation

```cmd
ipconfig /all
```

### DNS Validation

```cmd
nslookup streetrack.com
```

### Internet Connectivity

```cmd
ping google.com
```

### Results

- DHCP assigned correct IP address
- DNS resolved domain resources
- Internet connectivity verified
- Domain authentication successful

[Insert Screenshot]

---

# Phase 8: Automated User Provisioning

## User Creation with Python

To reduce manual account creation, a Python script was developed using the pyad library.

### Functionality

- Reads user information from a text file
- Generates usernames automatically
- Creates Active Directory user accounts
- Assigns account attributes
- Sets initial passwords
- Places users into the designated Organizational Unit

### Technologies Used

- Python
- pyad
- pywin32
- LDAP

### Example Workflow

1. Import user list
2. Parse names
3. Generate usernames
4. Create AD accounts
5. Configure attributes
6. Set passwords
7. Verify successful creation

[Insert Screenshot]

---

# Phase 9: Group Policy Management

## Security Hardening with Group Policy

A custom Group Policy Object was created and linked to the _USERS Organizational Unit.

### Security Controls Implemented

#### Password Policy

- Password complexity enabled
- Minimum length: 14 characters

#### Account Lockout Policy

- Lockout threshold: 3 failed attempts

#### Administrative Restrictions

- Block access to Control Panel
- Disable Command Prompt
- Prevent software installation
- Disable Microsoft Store
- Disable OneDrive usage
- Restrict removable storage access

### Policy Deployment

Policies were applied using:

```cmd
gpupdate /force
```

### Validation

All restrictions were verified from the Windows 10 client workstation.

[Insert Screenshot]

---

# Key Takeaways

This project provided practical experience deploying and managing a Windows Active Directory environment from initial server configuration through policy enforcement and user lifecycle management. It reinforced core enterprise IT concepts including identity management, infrastructure services, security administration, and automation.
