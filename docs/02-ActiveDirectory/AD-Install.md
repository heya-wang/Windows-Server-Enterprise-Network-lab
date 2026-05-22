# Active Directory Konfiguration

## Ziel

Diese Konfiguration beschreibt die Einrichtung der Active Directory Domäne innerhalb der Server-Infrastruktur.

Die Domäne dient als zentrale Verwaltungsplattform für:

- Benutzerkonten
- Gruppenrichtlinien (GPO)
- Authentifizierung
- DNS-Integration
- Zentrale Ressourcenverwaltung

---

# Installation der Active Directory Domain Services (AD DS)

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

# Bereitstellung als Domänencontroller

Nach der Installation wird der Server zu einem neuen Domänencontroller heraufgestuft.

Pfad:

```text
Server-Manager
→ Benachrichtigung
→ Diesen Server zu einem Domänencontroller heraufstufen
```

---

# Gesamtstruktur-Konfiguration

Bereitstellungstyp:

```text
Neue Gesamtstruktur hinzufügen
```

Domänenname:

```text
firma.intern
```

---

# Domänencontroller-Optionen

Konfiguration:

- DNS-Server aktivieren
- Globaler Katalog (GC)
- Gesamtstrukturfunktionsebene: Windows Server Standard
- Domänenfunktionsebene: Windows Server Standard

Zusätzlich wird ein Kennwort für den Verzeichnisdienste-Wiederherstellungsmodus (DSRM) definiert.

---

# DNS-Konfiguration

Der Domänencontroller übernimmt zusätzlich die DNS-Funktion für die interne Namensauflösung.

Interne DNS-Zone:

```text
firma.intern
```

---

# Abschluss der Installation

Nach erfolgreicher Überprüfung der Voraussetzungen wird die Installation gestartet.

Der Server wird anschließend automatisch neu gestartet.

---

# Domänenbeitritt der Clients

Die Windows-Clients werden der Active-Directory-Domäne `firma.intern` hinzugefügt, um zentrale Benutzerverwaltung, DNS-Auflösung und Gruppenrichtlinien zu nutzen.

Pfad:

```text
System
→ Erweiterte Systemeinstellungen
→ Computername
→ Ändern
```

Konfiguration:

| Einstellung | Wert |
|---|---|
| Mitglied von | Domäne |
| Domäne | firma.intern |
| Benutzername | firma\Administrator |

Nach erfolgreicher Authentifizierung wird der Client neu gestartet.

---

## Validierung

- Anmeldung mit Domänenbenutzer
- Kontrolle des Computerkontos in AD
- DNS-Test mittels `nslookup`
- Gruppenrichtlinien prüfen mittels `gpresult /r`

---

# Zusammenfassung

Die Active Directory Umgebung wurde erfolgreich bereitgestellt.

Die Domäne:

```text
firma.intern
```

dient als zentrale Authentifizierungs- und Verwaltungsplattform der gesamten Infrastruktur.
