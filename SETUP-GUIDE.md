# Hyper-V Virtual Machine Installation 


## 1. Hyper-V Installation

- Open **Server Manager → Add Roles and Features**
- Select **Role-based or feature-based installation**
- Choose target server
- Select **Hyper-V role**
- Accept required features
- Configure:
  - Virtual Switch: select network adapter
  - Default storage path: `C:\VM`
- Enable **automatic restart if required**
- Click **Install**
- Wait for installation and server reboot
- Log in again as Administrator to complete setup

  
---

## 2. Virtual Switches 

Create 3 isolated virtual switches for the lab network:

- Users Network → `Privat-Users`
- Servers Network → `Privat-Servers`
- DMZ Network → `Privat-DMZ`

---

## Virtual Machines Overview

The following virtual machines will be created:

- DC1 (Domain Controller)
- DC2 (Domain Controller backup) 
- Server1
- Server2
- Server3
- Server4
- Server5
- CL1 (Windows 11 Client)
- CL2 (Windows 11 Client)

Common settings:

- Generation 2
- OS 
  - Servers: Windows_Server_2022_German.iso
  - Windows 11: Win11_German_22h2.iso
- Network:
  - DC1, DC2, Server1, Server2, Server3: `Privat-Servers`
  - CL1, CL2: `Privat-Users`
  - Server5, Server6: `Privat-DMZ`
- RAM:
  - Servers: 2 GB
  - Windows 11: 4 GB
