# Routing und RRAS-Konfiguration

## Ziel

Der virtuelle Router stellt das zentrale Routing zwischen allen Netzwerksegmenten bereit.  
Die Routing-Funktion wird über den Windows Server Dienst **RRAS (Routing and Remote Access Service)** implementiert.

Der Router übernimmt folgende Aufgaben:

- Routing zwischen den internen Netzwerken
- Bereitstellung der Gateway-Adressen
- DHCP-Relay zwischen den Subnetzen
- Grundlage für spätere NAT-, ACL- und VPN-Konfigurationen

---

# Netzwerkinterfaces

Der Router besitzt mehrere virtuelle Netzwerkadapter.

## WAN Interface (Extern)

Das WAN-Interface verbindet den Router mit dem externen Netzwerk der virtuellen Umgebung.

- IP-Konfiguration: DHCP
- Zugewiesene IP-Adresse: **10.100.20.22**
- Zweck: Externe Kommunikation / Internet-Verbindung

## Interne Interfaces

| Interface | Netzwerk | IP-Adresse |
|---|---|---|
| Servernetz | 172.16.1.0/24 | 172.16.1.1 |
| DMZ | 172.16.10.0/24 | 172.16.10.1 |
| Benutzernetz | 172.16.20.0/24 | 172.16.20.1 |

---

# Installation von RRAS

## Server Manager

Die RRAS-Rolle wird über den Server Manager installiert.

Pfad:

```text
Server Manager
→ Add Roles and Features
→ Remote Access
→ Routing
```

## Installation abschließen

Nach der Installation muss der Server gegebenenfalls neu gestartet werden.

---

# Aktivierung von RRAS

## RRAS öffnen

```text
Tools
→ Routing and Remote Access
```

## RRAS konfigurieren

Rechtsklick auf den Server:

```text
Routing und RAS konfigurieren und aktivieren
```

## Konfigurationstyp

Folgende Option wird ausgewählt:

```text
Benutzerdefinierte Konfiguration
```

Aktivierte Funktion:

- LAN Routing

Weitere Funktionen wie NAT oder VPN werden später ergänzt.

## Dienst starten

Nach Abschluss des Assistenten wird der RRAS-Dienst gestartet.

---

# Gateway-Konfiguration

Die Clients und Server verwenden den Router als Standardgateway.

| Netzwerk | Gateway |
|---|---|
| Servernetz | 172.16.1.1 |
| DMZ | 172.16.10.1 |
| Benutzernetz | 172.16.20.1 |

---

# DHCP Relay

## Zweck

Da sich der zentrale DHCP-Server im Servernetz befindet, werden DHCP-Anfragen aus anderen Netzwerken über DHCP Relay weitergeleitet.

DHCP-Server:

```text
172.16.1.13
```

---

## DHCP Relay Agent hinzufügen

Pfad:

```text
RRAS
→ IPv4
→ Allgemein
→ Neues Routingprotocoll
→ DHCP Relay Agent
```

---

## DHCP-Server konfigurieren

Im DHCP Relay Agent wird der zentrale DHCP-Server eingetragen:

```text
172.16.1.13
```

---

## Relay-Interfaces

DHCP Relay wird auf folgenden Interfaces aktiviert:

- DMZ
- Benutzernetz

Optional:

- VPN-Netz

---

# NAT-Konfiguration

Für den Internetzugriff der internen Netzwerke wird NAT auf der WAN-Schnittstelle aktiviert.

Pfad:

```text
RRAS
→ IPv4
→ NAT
```

Die Extern-Schnittstelle wird als:

```text
Öffentliche Schnittstelle mit aktiviertem NAT
```

konfiguriert.

Die internen Netzwerke (Servernetz, DMZ und VPN-netz) verwenden den ROUTER anschließend als Gateway für externe Verbindungen.

Die Netzwerke sind logisch voneinander getrennt und kommunizieren ausschließlich über den zentralen Router.

---

# Funktionstest

## Routing testen

Beispiel:

- CL1 → DC1
- CL1 → Fileserver
- DMZ → Router

## DHCP testen

Clients sollten automatisch eine gültige IP-Konfiguration vom zentralen DHCP-Server erhalten.

---

# Zusammenfassung

Die RRAS-Konfiguration bildet die Grundlage der gesamten Netzwerkkommunikation innerhalb der Infrastruktur.

Der Router übernimmt:

- Interne Netzwerkweiterleitung
- Gateway-Funktion
- DHCP Relay
- Zentrale Steuerung der Netzwerkkommunikation

Weitere Funktionen wie:

- ACL-Regeln
- VPN

werden später ergänzt.
