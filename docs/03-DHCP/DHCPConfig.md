# DHCP-Konfiguration

## Ziel

Der Server „DHCPserver“ stellt die automatische IPv4-Adressvergabe für das Netzwerk `172.16.20.0/24` bereit.

Da sich der DHCP-Server in einem anderen Netzwerksegment befindet, erfolgt die Weiterleitung der DHCP-Anfragen über den ROUTER als DHCP-Relay-Agent.

DHCP-Relay auf dem ROUTER ist schon configuriert.

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
| Bereichsname | Benutzernetz |
| Netzwerk | 172.16.20.0 |
| Subnetzmaske | 255.255.255.0 |
| Adressbereich | 172.16.20.10 – 172.16.20.100 |
| Ausschlussbereich | Optional |
| Lease-Dauer | Standard |

---


# Validierung

Zur Validierung werden folgende Tests durchgeführt:

- Client im Netzwerk `172.16.20.0/24` auf DHCP umstellen
- Überprüfung der automatischen IPv4-Adressvergabe
- Kontrolle von Gateway und DNS-Server mittels `ipconfig /all`
- Erreichbarkeit des Gateways testen
- DNS-Auflösung mittels `nslookup`
- Überprüfung der Lease im DHCP-Manager
