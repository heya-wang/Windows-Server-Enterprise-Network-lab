# Initial Network Configuration
Run the following PowerShell commands inside each virtual machine as Administrator.

## DC1

- Computer Name: `DC1`
- IP Address: `192.168.1.200`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.1.200 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName DC1 -Force -Restart
```

---

## DC2

- Computer Name: `DC2`
- IP Address: `192.168.1.201`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell

New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.1.201 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName DC2 -Force -Restart
```
---

## Server1

- Computer Name: `Server1`
- IP Address: `192.168.1.1`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.1.1 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName Server1 -Force -Restart
```

---

## Server2

- Computer Name: `Server2`
- IP Address: `192.168.1.2`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.1.2 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName Server2 -Force -Restart

```

---

## Server3

- Computer Name: `Server3`
- IP Address: `192.168.1.3`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.1.3 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName Server3 -Force -Restart
```

---

## Server4 (DMZ)

- Computer Name: `Server4`
- IP Address: `192.168.3.4`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.3.4 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName Server4 -Force -Restart
```

---

## Server5 (DMZ)

- Computer Name: `Server5`
- IP Address: `192.168.3.5`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.3.5 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName Server5 -Force -Restart
---
```
## CL1

- Computer Name: `CL1`
- IP Address: `192.168.2.10`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.2.10 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName CL1 -Force -Restart
```

---

## CL2

- Computer Name: `CL2`
- IP Address: `192.168.2.11`
- Subnet Mask: `255.255.255.0`
- DNS Server: `192.168.1.200`

```powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.2.11 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 192.168.1.200

Rename-Computer -NewName CL2 -Force -Restart
```
---
