# DHCP and DNS Configuration

This document describes the configuration of DHCP and DNS services on `DC1` for the SMB lab environment.

---

# DHCP Server Installation

## Install DHCP Role

1. Open **Server Manager** on `DC1`
2. Click **Add Roles and Features**
3. Select:
   - `DHCP Server`
4. Add required management tools
5. Complete installation
6. Click:
   - `Complete DHCP Configuration`
7. Authorize the DHCP server

---

# Configure DHCP Scopes

##  Clients Network Scope
Open DHCP Manager:

```text
dhcpmgmt.msc
```

### Scope Settings

| Setting | Value |
|---|---|
| Scope Name | Clients |
| Network | 192.168.2.0/24 |
| IP Range | 192.168.2.50 - 192.168.2.200 |
| Subnet Mask | 255.255.255.0 |
| Lease Duration | 8 Days |

---

### DHCP Options

| Option | Value |
|---|---|
| Gateway | 192.168.2.254 |
| DNS Server | 192.168.1.200 |
| DNS Domain | smb-lab.local |

Activate the scope after configuration.

---

## DMZ Scope

### Scope Settings

| Setting | Value |
|---|---|
| Scope Name | DMZ |
| Network | 192.168.3.0/24 |
| IP Range | 192.168.3.50 - 192.168.3.200 |
| Subnet Mask | 255.255.255.0 |
| Lease Duration | 8 Days |

---

### DHCP Options

| Option | Value |
|---|---|
| Gateway | 192.168.3.254 |
| DNS Server | 192.168.1.200 |
| DNS Domain | smb-lab.local |

Activate the scope after configuration.

---

# DNS Forwarder Configuration

Configure external DNS forwarders for internet name resolution.

---

## PowerShell

```powershell
Add-DnsServerForwarder -IPAddress 8.8.8.8,8.8.4.4
```

---

## GUI Method

1. Open:

```text
dnsmgmt.msc
```

2. Right-click server name → Properties

3. Open:
   - Forwarders

4. Add:
   - 8.8.8.8
   - 8.8.4.4

5. Apply settings

---

# Validation

## Verify Active Directory

```powershell
Get-ADDomain

Get-ADDomainController -Filter *
```

---

## Verify DNS

### Internal DNS Resolution

```powershell
Resolve-DnsName smb-lab.local
```

### External DNS Resolution

```powershell
Resolve-DnsName google.com
```

---

## Verify DHCP

### View DHCP Scopes

```powershell
Get-DhcpServerv4Scope
```

### View DHCP Leases

```powershell
Get-DhcpServerv4Lease -ScopeId 192.168.2.0
```

---

# Client Configuration

## Client Network Settings

| Setting | Value |
|---|---|
| IP Address | Obtain automatically |
| DNS Server | Obtain automatically |

---

# Join Client to Domain

Run on client machines as Administrator:

```powershell
Add-Computer -DomainName "smb-lab.local" -Credential (Get-Credential) -Restart

Restart-Computer
```
