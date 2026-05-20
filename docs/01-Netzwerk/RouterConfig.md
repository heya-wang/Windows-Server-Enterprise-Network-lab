# Initial Netzerk Configuration

Die folgende Grundkonfiguration wird auf allen virtuellen Maschinen durchgeführt.  
Alle PowerShell-Befehle müssen als Administrator ausgeführt werden.

---

# 1. Servernetz – 172.16.1.0/24

## DC1

| Einstellung | Wert |
|---|---|
| Computername | `DC1` |
| IP-Adresse | `172.16.1.10` |
| Präfix | `/24` |
| Gateway | `172.16.1.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.1.10 `
-PrefixLength 24 `
-DefaultGateway 172.16.1.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName DC1 -Force -Restart
```

---

## DC2

| Einstellung | Wert |
|---|---|
| Computername | `DC2` |
| IP-Adresse | `172.16.1.11` |
| Präfix | `/24` |
| Gateway | `172.16.1.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.1.11 `
-PrefixLength 24 `
-DefaultGateway 172.16.1.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName DC2 -Force -Restart
```

---

## Fileserver

| Einstellung | Wert |
|---|---|
| Computername | `Fileserver` |
| IP-Adresse | `172.16.1.12` |
| Präfix | `/24` |
| Gateway | `172.16.1.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.1.12 `
-PrefixLength 24 `
-DefaultGateway 172.16.1.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName Fileserver -Force -Restart
```

---

## DHCPServer

| Einstellung | Wert |
|---|---|
| Computername | `DHCPServer` |
| IP-Adresse | `172.16.1.13` |
| Präfix | `/24` |
| Gateway | `172.16.1.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.1.13 `
-PrefixLength 24 `
-DefaultGateway 172.16.1.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName DHCPServer -Force -Restart
```

---

## Zabbix (geplant)

| Einstellung | Wert |
|---|---|
| Computername | `Zabbix` |
| IP-Adresse | `172.16.1.14` |
| Präfix | `/24` |
| Gateway | `172.16.1.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.1.14 `
-PrefixLength 24 `
-DefaultGateway 172.16.1.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName Zabbix -Force -Restart
```

---

# 2. DMZ – 172.16.10.0/24

## Webserver

| Einstellung | Wert |
|---|---|
| Computername | `Webserver` |
| IP-Adresse | `172.16.10.10` |
| Präfix | `/24` |
| Gateway | `172.16.10.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.10.10 `
-PrefixLength 24 `
-DefaultGateway 172.16.10.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName Webserver -Force -Restart
```

---

## Proxy

| Einstellung | Wert |
|---|---|
| Computername | `Proxy` |
| IP-Adresse | `172.16.10.11` |
| Präfix | `/24` |
| Gateway | `172.16.10.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.10.11 `
-PrefixLength 24 `
-DefaultGateway 172.16.10.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName Proxy -Force -Restart
```

---

# 3. Benutzernetz – 172.16.20.0/24

## CL1

| Einstellung | Wert |
|---|---|
| Computername | `CL1` |
| IP-Adresse | `172.16.20.10` |
| Präfix | `/24` |
| Gateway | `172.16.20.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.20.10 `
-PrefixLength 24 `
-DefaultGateway 172.16.20.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName CL1 -Force -Restart
```

---

## CL2

| Einstellung | Wert |
|---|---|
| Computername | `CL2` |
| IP-Adresse | `172.16.20.11` |
| Präfix | `/24` |
| Gateway | `172.16.20.1` |
| DNS-Server | `172.16.1.10` |

```powershell
New-NetIPAddress `
-InterfaceAlias Ethernet `
-IPAddress 172.16.20.11 `
-PrefixLength 24 `
-DefaultGateway 172.16.20.1

Set-DnsClientServerAddress `
-InterfaceAlias Ethernet `
-ServerAddresses 172.16.1.10

Rename-Computer -NewName CL2 -Force -Restart
```

---

# 4. Hinweise

- Alle Server verwenden statische IP-Adressen
- Die Clients können später optional per DHCP konfiguriert werden
- Die Domain wird später als:

```text
firma.intern
```

eingerichtet
- Die Routing- und ACL-Konfiguration erfolgt auf dem zentralen vRouter
