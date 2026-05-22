# DHCP-Konfiguration

## Ziel

Der Server „DHCPserver“ stellt die automatische IPv4-Adressvergabe für das Netzwerk `172.16.20.0/24` bereit.

Da sich der DHCP-Server in einem anderen Netzwerksegment befindet, erfolgt die Weiterleitung der DHCP-Anfragen über den ROUTER als DHCP-Relay-Agent.

---

# DHCP-Server Installation

Pfad:

```text
Server-Manager
→ Rollen und Features hinzufügen
```

Installation:

```text
Serverrollen
→ DHCP-Server
→ Benötigte Features hinzufügen
→ Weiter
→ Installieren
```

Nach Abschluss:

```text
Benachrichtigung
→ DHCP-Konfiguration abschließen
→ Autorisieren
→ Schließen
```

---

# DHCP-Bereich konfigurieren

Pfad:

```text
Server-Manager
→ Tools
→ DHCP
```

Konfiguration:

```text
DHCP
→ IPv4
→ Rechtsklick
→ Neuer Bereich
```

---

## Bereichseinstellungen

| Einstellung | Wert |
|---|---|
| Bereichsname | LAN20 |
| Netzwerk | 172.16.20.0 |
| Subnetzmaske | 255.255.255.0 |
| Adressbereich | 172.16.20.100 – 172.16.20.200 |
| Ausschlussbereich | Optional |
| Lease-Dauer | Standard |

---

## DHCP-Optionen

| Option | Wert |
|---|---|
| Router (Gateway) | 172.16.20.1 |
| DNS-Server | 172.16.1.10 |
| DNS-Domäne | firma.intern |

---

# DHCP-Relay auf dem ROUTER

Da sich der DHCP-Server nicht im gleichen Broadcast-Domain wie die Clients befindet, wird ein DHCP-Relay-Agent auf dem ROUTER konfiguriert.

Pfad:

```text
Routing und RAS
→ IPv4
→ Allgemein
→ Rechtsklick auf LAN20-Schnittstelle
→ Eigenschaften
```

DHCP-Relay-Agent hinzufügen:

```text
IPv4
→ Allgemein
→ Neues Routingprotokoll
→ DHCP-Relay-Agent
```

DHCP-Server hinzufügen:

```text
DHCP-Relay-Agent
→ Rechtsklick
→ Neuer Server
→ 172.16.1.13
```

Anschließend die Schnittstelle konfigurieren:

```text
DHCP-Relay-Agent
→ Rechtsklick auf LAN20-Schnittstelle
→ Eigenschaften
```

| Einstellung | Wert |
|---|---|
| DHCP-Server-Adresse | 172.16.1.13 |
| Hop-Count-Schwelle | Standard |
| Boot-Threshold | Standard |

---

# Validierung

Zur Validierung werden folgende Tests durchgeführt:

- Client im Netzwerk `172.16.20.0/24` auf DHCP umstellen
- Überprüfung der automatischen IPv4-Adressvergabe
- Kontrolle von Gateway und DNS-Server mittels `ipconfig /all`
- Erreichbarkeit des Gateways testen
- DNS-Auflösung mittels `nslookup`
- Überprüfung der Lease im DHCP-Manager
