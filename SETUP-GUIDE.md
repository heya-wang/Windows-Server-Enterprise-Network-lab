# Hyper-V Installation und VM-Konfiguration

## 1. Hyper-V Installation

- Öffnen Sie den **Server-Manager → Rollen und Features hinzufügen**
- Wählen Sie:
  - **Rollenbasierte oder featurebasierte Installation**
- Zielserver auswählen
- Rolle **Hyper-V** aktivieren
- Benötigte Features bestätigen
- Konfiguration durchführen:
  - Virtueller Switch: entsprechenden Netzwerkadapter auswählen
  - Standardpfad für virtuelle Maschinen:
    ```text
    C:\VM
    ```
- Option **Automatischen Neustart bei Bedarf durchführen** aktivieren
- Installation starten
- Nach Abschluss den Server neu starten
- Anschließend erneut als Administrator anmelden

---

# 2. Virtuelle Switches

Für die Laborumgebung werden mehrere virtuelle Netzwerke erstellt:

| Virtueller Switch | Typ | Zweck |
|---|---|---|
| `Extern` | External | WAN / Internetzugang |
| `Privat-Servers` | Private | Servernetz |
| `Privat-Users` | Private | Benutzernetz |
| `Privat-DMZ` | Private | DMZ-Netzwerk |

---

# 3. Erstellung der virtuellen Maschinen

Folgende virtuelle Maschinen werden erstellt:

| VM-Name | Rolle |
|---|---|
| DC1 | Primärer Domain Controller |
| DC2 | Sekundärer Domain Controller |
| Fileserver | SMB-Dateiserver |
| DHCPServer | DHCP-Server |
| Zabbix | Monitoring-Server |
| Webserver | IIS-Webserver |
| Proxy | Reverse Proxy |
| CL1 | Windows 11 Client |
| CL2 | Windows 11 Client |

---

# 4. Gemeinsame VM-Konfiguration

## Generation

Alle virtuellen Maschinen verwenden:

```text
Generation 2
```

---

## Betriebssysteme

| Systemtyp | ISO-Datei |
|---|---|
| Windows Server | `Windows_Server_2022_German.iso` |
| Windows 11 | `Win11_German_22h2.iso` |

---

## Virtuelle Festplatten

| Systemtyp | Festplattengröße |
|---|---|
| Windows Server | 40 GB |
| Windows 11 Clients | 64 GB |

Dynamischer Arbeitsspeicher wird für Clients verwendet.
  
---

## Netzwerkzuordnung

| VM | Netzwerk |
|---|---|
| DC1 | `Benutzernetz` |
| DC2 | `Benutzernetz` |
| Fileserver | `Servernetz` |
| DHCPServer | `Servernetz` |
| Zabbix | `Servernetz` |
| Webserver | `DMZ` |
| Proxy | `DMZ` |
| CL1 | `Benutzernetz` |
| CL2 | `Benutzernetz` |

---

## Arbeitsspeicher (RAM)

| Systemtyp | RAM |
|---|---|
| Windows Server | 2 GB |
| Windows 11 Clients | 4 GB |

---

## CPU-Konfiguration

Für alle Windows 11 Clients:

- `2 virtuelle Prozessoren`

---

## TPM-Konfiguration

Für die Windows 11 Clients (`CL1`, `CL2`):

- TPM aktivieren
- Secure Boot aktiviert lassen

---

# 5. Hinweise

- Alle Server erhalten statische IP-Adressen
- Die Clients beziehen ihre IP-Konfiguration über DHCP
- Gateways werden manuell konfiguriert
- Die Domain wird später als:
  
```text
firma.intern
```

eingerichtet.
