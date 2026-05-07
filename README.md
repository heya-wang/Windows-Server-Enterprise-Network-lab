# Enterprise Windows Infrastructure Lab

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

## Network Design

The environment is divided into three subnets:

- VLAN1: 192.168.1.0/24 → Servers Network  
- VLAN2: 192.168.2.0/24 → Users Network  
- VLAN3: 192.168.3.0/24 → DMZ Network  

---

## Architecture Overview
<img width="1030" height="810" alt="image" src="https://github.com/user-attachments/assets/b8d1f6e6-5db9-4ce5-a815-67754fbf84a7" />

---

## VLAN1: 192.168.1.0/24 - Servers Network

### Purpose
Core internal infrastructure services including domain control, file services, and routing.

| No | Machine      | Role                        | IP Address     |
|----|--------------|-----------------------------|----------------|
| 1  | DC1    | Primary Domain Controller   | 192.168.1.200   |
| 2  | DC2    | Secondary Domain Controller | 192.168.1.201   |
| 3  | Server1    | File Server                 | 192.168.1.1   |
| 4  | Server2   | Application Server          | 192.168.1.2   |
| 5  | Server3   | Router (RRAS Gateway)       | 192.168.1.3    |

### Hosted Services
- Active Directory Domain Services  
- DNS  
- File Sharing (SMB)  
- Internal applications  
- Inter-subnet routing  

---

## VLAN2: 192.168.2.0/24 - Users Network

### Purpose
End-user devices used for domain login, testing, and daily operations.

| No | Machine     | Role                      | IP Address     |
|----|-------------|---------------------------|----------------|
| 1  | CL1   | Windows 11 Client         | 192.168.2.10   |
| 2  | CL2   | Secondary Client  | 192.168.2.11   |

### Configuration Notes
- DNS: 192.168.2.10 / 192.168.2.11  
- Gateway: 192.168.1.1  

---

## VLAN3: 192.168.3.0/24 - DMZ Network

### Purpose
Isolated network for externally facing services.

| No | Machine       | Role                      | IP Address     |
|----|---------------|---------------------------|----------------|
| 1  | DMZ-WEB01     | Web Server (IIS)          | 192.168.3.10   |
| 2  | DMZ-WEB02     | Staging Web Server        | 192.168.3.20   |
| 3  | DMZ-PROXY01   | Reverse Proxy / Gateway    | 192.168.3.12   |

### Security Characteristics
- Not joined to the domain  
- No direct access to internal servers  
- Access controlled via EDGE router only  

---

## Routing Layer - EDGE-RT01

### Interfaces

| Interface | Network | IP Address   |
|----------|--------|--------------|
| NIC1     | Servers  | 192.168.1.254  |
| NIC2     | Users | 192.168.2.254  |
| NIC3     | DMZ    | 192.168.3.254  |

### Responsibilities
- Inter-subnet routing  
- Traffic segmentation  
- Logical access control between network zones  

---

## Security Model

- Users → Servers: Allowed (authenticated domain access)  
- Users → DMZ: Restricted  
- DMZ → Servers: Blocked  
- Servers → DMZ: Controlled access only  

---

## Active Directory Design

The environment is based on a single domain:

corp.local

Implemented using:

- Centralized authentication and authorization  
- Organizational Units (OU) structure  
- Group Policy Objects (GPO)  
- Two domain controllers for redundancy  

---

## Summary

This project demonstrates a realistic enterprise-style IT infrastructure with proper network segmentation, centralized identity management, and isolated service deployment. It simulates a small-to-medium enterprise environment and provides hands-on experience with Windows Server administration, Active Directory, and network architecture design.
