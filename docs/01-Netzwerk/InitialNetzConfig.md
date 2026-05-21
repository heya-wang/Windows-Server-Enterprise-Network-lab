# Initial Network Configuration

Die folgende Grundkonfiguration wird auf allen virtuellen Maschinen durchgeführt.

---

# 1. Servernetz – 172.16.1.0/24

| Maschine | Computername | IP-Adresse | Gateway | DNS-Server |
|---|---|---|---|---|
| Domain Controller | `DC1` | `172.16.1.10/24` | `172.16.1.1` | `172.16.1.10` |
| Secondary Domain Controller | `DC2` | `172.16.1.11/24` | `172.16.1.1` | `172.16.1.10` |
| File Server | `Fileserver` | `172.16.1.12/24` | `172.16.1.1` | `172.16.1.10` |
| DHCP Server | `DHCPServer` | `172.16.1.13/24` | `172.16.1.1` | `172.16.1.10` |

---

# 2. DMZ – 172.16.10.0/24

| Maschine | Computername | IP-Adresse | Gateway | DNS-Server |
|---|---|---|---|---|
| IIS Webserver | `Webserver` | `172.16.10.10/24` | `172.16.10.1` | `172.16.1.10` |
| Reverse Proxy (geplant) | `Proxy` | `172.16.10.11/24` | `172.16.10.1` | `172.16.1.10` |

---

# 3. Benutzernetz – 172.16.20.0/24

| Maschine | Computername | IP-Adresse |
|---|---|---|
| Windows 11 Client | `CL1` | `172.16.20.10` |
| Windows 11 Client | `CL2` | `172.16.20.11` |

---

# 5. Hinweise

- Alle Server verwenden statische IP-Adressen
- Clients erhalten ihre Netzwerkkonfiguration per DHCP
- Die Domain wird später als `firma.intern` eingerichtet
- Routing, VPN und ACL-Regeln werden zentral über den vRouter verwaltet
