# windows-server-enterprise-network-lab

## Project Overview

This project simulates a small-to-medium enterprise IT infrastructure using a virtualized environment built on Hyper-V.  
The goal is to practice and demonstrate real-world skills in:

- Active Directory administration
- Network segmentation and routing
- Server role design
- Security isolation (DMZ)
- Basic infrastructure operations

The design focuses on simplicity, scalability, and realism, reflecting how real SMB environments are structured.

---

## Architecture Overview

The environment is divided into three network segments:

### Users Network
- Subnet: 192.168.10.0/24
- Purpose: End-user devices

### Servers Network
- Subnet: 192.168.20.0/24
- Purpose: Internal infrastructure services

### DMZ Network
- Subnet: 192.168.30.0/24
- Purpose: Public-facing services (isolated from internal domain)

---

## Virtual Machines & Roles

## Domain Services (Servers Network)

| Machine | Role | IP Address |
|--------|------|------------|
| DC | Primary Domain Controller | 192.168.20.10 |
| GFN-DC | Secondary Domain Controller | 192.168.20.11 |

Functions:
- Active Directory Domain Services
- DNS
- User and group management
- Redundancy and replication

---

## Internal Servers (Servers Network)

| Machine | Role | IP Address |
|--------|------|------------|
| Server1 | File Server | 192.168.20.20 |
| Server2 | Application / Management Server | 192.168.20.21 |

Functions:
- File sharing (SMB)
- NTFS permissions
- Internal services and management tools

---

## Network & Routing

| Machine | Role |
|--------|------|
| Server3 | Router (RRAS) |

Interfaces:
- 192.168.10.1 → Users Network
- 192.168.20.1 → Servers Network
- 192.168.30.1 → DMZ Network

Functions:
- Inter-subnet routing
- Traffic segmentation
- Logical access control

---

## DMZ Environment

| Machine | Role | IP Address |
|--------|------|------------|
| GFN-Server1 | Web Server (IIS) | 192.168.30.10 |
| GFN-Server2 | Staging / Test Web Server | 192.168.30.11 |
| GFN-Server3 | Optional Proxy / Service Host | 192.168.30.12 |

Characteristics:
- Not joined to domain
- Isolated from internal servers
- Simulates external-facing services

---

## Client Machines (Users Network)

| Machine | Role | IP Address |
|--------|------|------------|
| Win11 | Domain-joined client | 192.168.10.10 |
| GFN-11 | Domain-joined client | 192.168.10.11 |

---

## Security Design

The architecture implements basic enterprise security principles:

- Network Segmentation  
  Users, Servers, and DMZ are isolated

- Domain Isolation  
  DMZ systems are not part of the Active Directory domain

- Access Control  
  Users → Servers allowed  
  Users → DMZ restricted  
  DMZ → Servers blocked  
  Servers → DMZ limited access  

---

## Active Directory Design

The environment uses a single domain:  gfn.local


Structure:
- Users organized by departments (OU-based design)
- Computer and server separation
- Centralized authentication and policy management

Features:
- Group Policy Objects (GPO)
- Centralized user management
- Domain redundancy (2 Domain Controllers)

---

## Network Design Summary
Users Network Servers Network DMZ Network
192.168.10.0 192.168.20.0 192.168.30.0
| | |
Win11 DC / File / App / Router Web Servers
GFN-11 (Core Systems) (Isolated)


---

## Technologies Used

- Hyper-V (Virtualization)
- Windows Server
- Active Directory Domain Services
- DNS / DHCP
- RRAS (Routing)
- IIS (Web Server)
- Windows 11 Clients

---

## Project Goals

- Simulate a real SMB IT infrastructure
- Practice Windows Server administration
- Understand network segmentation
- Learn Active Directory architecture
- Build a portfolio-ready IT lab environment

---

## Screenshots (To Be Added)

- Network topology diagram
- Active Directory structure
- Domain join process
- GPO configuration
- DMZ isolation tests
- Ping / routing validation

---

## Future Improvements

- PowerShell automation for user creation
- Group Policy hardening templates
- Firewall rule simulation
- Backup & recovery strategy
- Monitoring system (optional)

---

## Key Learning Outcomes

- Enterprise network design basics
- Active Directory architecture
- Role-based server deployment
- Network isolation (DMZ concept)
- Virtualized infrastructure management

---

## Author

Project built as a hands-on IT infrastructure lab for system administration practice and job preparation.
