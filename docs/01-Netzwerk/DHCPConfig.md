# DHCP Configuration

## Overview

Der DHCP-Dienst stellt die automatische IP-Adressvergabe für das Benutzernetzwerk bereit.

Der Dienst wird auf `DHCPServer` installiert und verwaltet die Adressvergabe für das Subnetz `172.16.20.0/24`.

---

# DHCP Server

| Einstellung | Wert |
|---|---|
| Servername | `DHCPServer` |
| IP-Adresse | `172.16.1.13` |
| Netzwerk | `Privat-Servers` |

---

# DHCP-Installation

## DHCP-Rolle installieren

1. Öffnen Sie den **Server-Manager**
2. Klicken Sie auf:
   - `Rollen und Features hinzufügen`
3. Aktivieren Sie:
   - `DHCP-Server`
4. Benötigte Verwaltungstools hinzufügen
5. Installation abschließen
6. DHCP-Konfiguration autorisieren

---

# DHCP-Scope

## Benutzernetzwerk

### Scope-Einstellungen

| Einstellung | Wert |
|---|---|
| Scope-Name | `Clients` |
| Netzwerk | `172.16.20.0/24` |
| Adressbereich | `172.16.20.10 - 172.16.20.100` |
| Subnetzmaske | `255.255.255.0` |
| Lease-Dauer | `8 Tage` |

---

## DHCP-Optionen

| Option | Wert |
|---|---|
| Gateway | `172.16.20.1` |
| DNS-Server | `172.16.1.10` |
| DNS-Domain | `firma.intern` |

Nach der Konfiguration wird der Scope aktiviert.

---

# Client-Konfiguration

| Einstellung | Wert |
|---|---|
| IP-Adresse | Automatisch beziehen |
| DNS-Server | Automatisch beziehen |

---

# Validation

## DHCP-Manager

```text
dhcpmgmt.msc
```

Überprüfen:

- Aktiver DHCP-Scope
- Vergebene DHCP-Leases
- DHCP-Optionen
- Erreichbarkeit der Clients

---

## Client-Validierung

Auf einem Client:

```text
ipconfig /all
```

Überprüfen:

- Zugewiesene IP-Adresse
- Gateway
- DNS-Server
- DHCP-Server

---

# Hinweise

- Der DHCP-Server befindet sich im Servernetzwerk `172.16.1.0/24`
- DHCP-Anfragen aus anderen Subnetzen werden über einen DHCP-Relay-Agent auf dem vRouter weitergeleitet
- Server verwenden statische IP-Adressen
- Nur Clients erhalten Adressen per DHCP
