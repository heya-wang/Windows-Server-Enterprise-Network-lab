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

# Validierung

Die erfolgreiche AD-Installation wird anhand folgender Punkte überprüft:

- Anmeldung an der Domäne möglich (mit "firma\Administrator" und Kennwort)
- Active Directory Benutzer und Computer verfügbar
- Domänencontroller erreichbar

---

# Zusammenfassung

Die Active Directory Umgebung wurde erfolgreich bereitgestellt.

Die Domäne:

```text
firma.intern
```

dient als zentrale Authentifizierungs- und Verwaltungsplattform der gesamten Infrastruktur.
