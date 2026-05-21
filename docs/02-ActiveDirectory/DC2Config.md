# Sekundärer Domänencontroller (DC2)

## Ziel

DC2 dient als zusätzlicher replizierender Domänencontroller innerhalb der Domäne:

```text
firma.intern
```

Der zweite Domänencontroller erhöht:

- Verfügbarkeit der Active Directory Umgebung
- Redundanz der Authentifizierung
- DNS-Ausfallsicherheit
- Replikationssicherheit innerhalb der Domäne

---

# Voraussetzungen

Vor der Konfiguration wird:

- der bevorzugte DNS-Server auf DC1 konfiguriert
- die Maschine der Domäne `firma.intern` hinzugefügt

| Einstellung | Wert |
|---|---|
| Hostname | DC2 |
| IP-Adresse | 172.16.1.11 |
| Bevorzugter DNS | 172.16.1.10 |

---

# Installation der AD DS Rolle

Pfad:

```text
Server-Manager
→ Rollen und Features hinzufügen
```

Installierte Rolle:

```text
Active Directory-Domänendienste (AD DS)
```

Zusätzliche Features werden automatisch übernommen.

---

# Heraufstufung zum zusätzlichen Domänencontroller

Pfad:

```text
Server-Manager
→ Benachrichtigung
→ Diesen Server zu einem Domänencontroller heraufstufen
```

Bereitstellungskonfiguration:

```text
Domänencontroller zu einer vorhandenen Domäne hinzufügen
```

Domäne:

```text
firma.intern
```

Anmeldedaten:

```text
firma\Administrator
```

---

# Domänencontrolleroptionen

Konfiguration:

- DNS-Server aktivieren
- Globaler Katalog (GC) aktivieren
- Replikation von beliebigem Domänencontroller

Zusätzlich wird ein DSRM-Kennwort definiert.

---

# DNS-Integration (Forward & Reverse Lookup)

DC2 wird vollständig in die bestehende DNS-Infrastruktur der Domäne integriert.

Nach der Heraufstufung zum Domänencontroller erfolgt automatisch:

- Registrierung eines A-Records im Forward Lookup
- Registrierung eines PTR-Records im Reverse Lookup (sofern Reverse Zone vorhanden ist)
- DNS-Replikation der Zonen zwischen DC1 und DC2

## Forward Lookup Eintrag

```text
dc2.firma.intern → 172.16.1.11
```

## Reverse Lookup Eintrag

```text
172.16.1.11 → dc2.firma.intern
```

---

# Active Directory Replikation

Die Active Directory Datenbank wird automatisch zwischen DC1 und DC2 repliziert.

Dazu gehören:

- Benutzerkonten
- Gruppen
- Gruppenrichtlinien
- DNS-Einträge
- Active Directory Objekte

---

# Validierung

Die erfolgreiche Konfiguration wird anhand praktischer Tests überprüft.

- Erfolgreiche Replikation zwischen DC1 und DC2
- Sichtbarkeit beider Domänencontroller in Active Directory
- Funktionierende DNS-Auflösung innerhalb der Domäne

<img width="466" height="119" alt="image" src="https://github.com/user-attachments/assets/4c0c9ec2-0c54-42c2-a6c8-dbf36ce1ebec" />

Alle weiteren Funktionstests wurden ebenfalls erfolgreich durchgeführt.

---

# Zusammenfassung

DC2 wurde erfolgreich als zusätzlicher replizierender Domänencontroller eingerichtet.

Die Infrastruktur verfügt nun über:

- Redundante Active Directory Dienste
- Redundante DNS-Auflösung
- Verbesserte Hochverfügbarkeit
- Replikationssicherheit innerhalb der Domäne
