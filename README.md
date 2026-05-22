# Enterprise Windows Infrastructure Lab

## Projektübersicht

Dieses Projekt simuliert eine kleine bis mittelgroße Unternehmensinfrastruktur innerhalb einer virtualisierten Hyper-V Umgebung.  
Der Fokus liegt auf einer realistischen Netzwerkstruktur, zentralisierten Windows-Diensten sowie grundlegenden Sicherheits- und Infrastrukturkonzepten.

Ziel des Projekts ist es, praktische Erfahrungen in folgenden Bereichen zu sammeln:

- Active Directory Administration
- Netzwerksegmentierung
- Routing und Zugriffskontrolle
- VPN-Integration
- DMZ-Design
- Infrastruktur-Monitoring
- Zentrale Windows Server Verwaltung

---

# Netzwerkdesign

Die Infrastruktur ist in mehrere logisch getrennte Netzwerkbereiche aufgeteilt.

| Netzwerk | Subnetz | Zweck |
|---|---|---|
| Servernetz | 172.16.1.0/24 | Interne Infrastruktur und zentrale Dienste |
| DMZ | 172.16.10.0/24 | Öffentlich erreichbare Dienste |
| Benutzernetz | 172.16.20.0/24 | Client- und Arbeitsplatzsysteme |
| VPN-Netz | 172.16.30.0/24 | Remote Access für externe Benutzer |

---

# Architekturübersicht

<img width="873" height="688" alt="image" src="https://github.com/user-attachments/assets/3a11e085-f9a0-4b97-95a3-1aa24dbcdcf5" />


Die gesamte Kommunikation zwischen den Netzwerken erfolgt über einen zentralen virtuellen Router, welcher Routing, ACL-Regeln sowie VPN-Zugriffe verwaltet.

---

# Servernetz – 172.16.1.0/24

## Zweck

Das Servernetz enthält die zentrale Unternehmensinfrastruktur sowie interne Verwaltungs- und Netzwerkdienste.

## Systeme

| Hostname | Rolle | IP-Adresse |
|---|---|---|
| DC1 | Primary Domain Controller | 172.16.1.10 |
| DC2 | Secondary Domain Controller | 172.16.1.11 |
| Fileserver | Datei- und Verwaltungsserver | 172.16.1.12 |
| DHCPServer | DHCP Service | 172.16.1.13 |

## Dienste

- Active Directory
- DNS
- DHCP
- SMB-Freigaben
- Windows Admin Center (WAC)

---

# Benutzernetz – 172.16.20.0/24

## Zweck

Das Benutzernetz simuliert Arbeitsplatzrechner innerhalb der Unternehmensdomäne.

## Systeme

| Hostname | Rolle | IP-Adresse |
|---|---|---|
| CL1 | Windows Client | 172.16.20.10 |
| CL2 | Windows Client | 172.16.20.11 |

## Funktionen

- Anmeldung an der Active Directory Domäne
- Zugriff auf interne Ressourcen
- Testumgebung für GPOs und Benutzerverwaltung

---

# DMZ – 172.16.10.0/24

## Zweck

Die DMZ enthält Dienste, die potenziell aus externen Netzwerken erreichbar sind.

## Systeme

| Hostname | Rolle | IP-Adresse |
|---|---|---|
| Webserver | IIS Webserver | 172.16.10.10 |

## Sicherheitskonzept

- Getrenntes Netzwerksegment
- Eingeschränkter Zugriff auf interne Systeme
- Zugriffskontrolle über ACL-Regeln

---

# VPN-Netz – 172.16.30.0/24

## Zweck

Das VPN-Netz ermöglicht sicheren Remote-Zugriff auf interne Netzwerkressourcen.

## Konfiguration

| Funktion | Details |
|---|---|
| VPN-Typ | Remote Access VPN |
| Adresspool | 172.16.30.100 – 172.16.30.200 |
| Zugriffspunkt | vRouter |

## Besonderheiten

- Logisches Netzwerk ohne eigenes physisches Interface
- Zugriff erfolgt über das externe Router-Interface
- Kommunikation wird über ACL-Regeln kontrolliert

---

# Routing und Netzwerksteuerung

## vRouter

Der zentrale virtuelle Router verbindet sämtliche Netzwerksegmente miteinander.

## Aufgaben

- Routing zwischen den Subnetzen
- ACL-basierte Zugriffskontrolle
- Verwaltung der VPN-Verbindungen
- Netzwerksegmentierung

## Gateway-Adressen

| Netzwerk | Gateway |
|---|---|
| Servernetz | 172.16.1.1 |
| DMZ | 172.16.10.1 |
| Benutzernetz | 172.16.20.1 |

---

# Sicherheitskonzept

Die Infrastruktur orientiert sich an grundlegenden Sicherheitsprinzipien moderner Unternehmensnetzwerke.

- Netzwerksegmentierung
- ACL-Regeln
- Trennung interner und externer Dienste
- Redundante Domain Controller

---

# Active Directory

Die Umgebung basiert auf einer zentralen Windows-Domäne:

```text
firma.intern
```

Implementierte Funktionen:

- Benutzer- und Gruppenverwaltung
- Gruppenrichtlinien (GPO)
- DNS-Integration
- Redundante Domain Controller

---

# Verwendete Technologien

| Bereich | Technologie |
|---|---|
| Virtualisierung | Hyper-V |
| Betriebssysteme | Windows Server / Windows 11 |
| Verzeichnisdienst | Active Directory |
| DNS | Windows DNS |
| DHCP | Windows DHCP |
| Webserver | IIS |
| VPN | OpenVPN / WireGuard |
| Routing | RRAS / Virtueller Router |

---

# Geplante Erweiterungen

- VLAN-Segmentierung
- Erweiterte Firewall-Regeln in DMZ Zone
- Proxy-Konfiguration
- Windows Admin Center (WAC) Verwaltung auf Fileserver
- PowerShell-Automatisierung

---

# Zusammenfassung

Dieses Projekt demonstriert eine realitätsnahe Unternehmensinfrastruktur mit Netzwerksegmentierung, zentralem Identitätsmanagement, VPN-Zugriff sowie isolierten Netzwerkzonen.

Der Aufbau orientiert sich an typischen SMB-Umgebungen und dient als praktische Lern- und Demonstrationsumgebung für:

- Windows Server Administration
- Netzwerkdesign und Segmentierung
- Active Directory Management
- Zentrale Systemverwaltung mit Windows Admin Center (WAC)
- Grundlegende IT-Sicherheit

- Projekt befindet sich in aktiver Weiterentwicklung (nicht produktionsreif)
