# DNS-Konfiguration

## Ziel

Diese Konfiguration beschreibt die Einrichtung und Verwaltung des internen DNS-Servers innerhalb der Domäne:

```text
firma.intern
```

Der DNS-Server ermöglicht:

- Interne Namensauflösung
- Active Directory Integration
- Forward- und Reverse-Lookups
- Zentrale Verwaltung interner Systeme

---

# DNS-Manager öffnen

Pfad:

```text
dnsmgmt.msc
```

---

# Forward Lookup Zone

## Neue Zone erstellen

Pfad:

```text
DNS-Manager
→ Forward Lookup Zones
→ Neue Zone
```

Konfiguration:

| Einstellung | Wert |
|---|---|
| Zonentyp | Primäre Zone |
| Speicherung | In Active Directory integriert |
| Replikation | An alle DNS-Server der Domäne |
| Zonenname | firma.intern |
| Dynamische Updates | Nur sichere dynamische Updates |

---

# Hosteinträge (A-Records)

Folgende Systeme werden in der Forward Lookup Zone eingetragen:

| Hostname | IP-Adresse |
|---|---|
| dc1 | 172.16.1.10 |
| dc2 | 172.16.1.11 |
| fileserver | 172.16.1.12 |
| dhcpserver | 172.16.1.13 |
| dns2 | 172.16.1.11 |
| webserver | 172.16.10.10 |

---

# Reverse Lookup Zone

## Neue Reverse Zone erstellen

Pfad:

```text
DNS-Manager
→ Reverse Lookup Zones
→ Neue Zone
```

Konfiguration:

| Einstellung | Wert |
|---|---|
| Zonentyp | Primäre Zone |
| Speicherung | In Active Directory integriert |
| Netzwerk-ID | 172.16.1 |
| Dynamische Updates | Nur sichere dynamische Updates |

---

# PTR-Einträge

Für die internen Systeme werden automatisch bzw. manuell PTR-Einträge erstellt.

Beispiel:

| IP-Adresse | PTR |
|---|---|
| 172.16.1.10 | dc1.firma.intern |
| 172.16.1.11 | dc2.firma.intern |
| 172.16.1.12 | fileserver.firma.intern |
| 172.16.10.10 | webserver.firma.intern |

---

# DNS-Weiterleitung (Optional)

Optional kann ein externer DNS-Forwarder konfiguriert werden.

Pfad:

```text
DNS-Manager
→ Servereigenschaften
→ Weiterleitungen
```

Beispiel:

```text
8.8.8.8
```

---

# Validierung

Die DNS-Konfiguration wird anhand praktischer Tests überprüft.

Verwendete Testbefehle (auf CL1):

```powershell
nslookup dc1.firma.intern
nslookup 172.16.1.10
```
Die Screenshots zeigen beispielhaft:

<img width="472" height="237" alt="image" src="https://github.com/user-attachments/assets/a2953887-0f2c-45f8-bb96-45eaa17550d3" />

Alle weiteren Tests wurden ebenfalls erfolgreich durchgeführt.

---

# Zusammenfassung

Der interne DNS-Server wurde erfolgreich eingerichtet und in Active Directory integriert.

Die Umgebung unterstützt:

- Forward Lookup
- Reverse Lookup
- Interne Namensauflösung
- DNS-Integration mit Active Directory
- Zentrale Verwaltung interner Hosteinträge
