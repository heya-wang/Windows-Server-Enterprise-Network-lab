# Gruppenrichtlinien (GPO) Konfiguration

## Zweck

Zur zentralen Verwaltung der Domänenumgebung werden mehrere Gruppenrichtlinienobjekte (GPOs) eingesetzt.

Die Konfiguration dient der:

- Zentralen Benutzerverwaltung
- Einschränkung unerwünschter Benutzeraktionen
- Automatischen Netzlaufwerkszuordnung
- Erhöhung der Anmeldesicherheit
- Einheitlichen Client-Konfiguration

---

# Verwendete Gruppenrichtlinien

| GPO | Zweck |
|---|---|
| Laufwerkszuordnung | Automatische Verbindung von Netzlaufwerken |
| Herunterfahrencontroll | Einschränkung der Herunterfahren-/Neustartfunktionen |
| Desktopcontroll | Einschränkung und Konfiguration der Desktopumgebung |
| Login_Sicherheit | Zusätzliche Sicherheitsrichtlinien für Benutzeranmeldung |
| Default Domain Policy | Standardrichtlinien der Active Directory Domäne |
| Default Domain Controllers Policy | Standardrichtlinien für Domänencontroller |

---

# Erstellung und Verwaltung

# Konfigurierte Richtlinien

## 1. Laufwerkszuordnung
Die Gruppenrichtlinie „Laufwerkszuordnung“ dient zur automatischen Bereitstellung zentraler Netzwerkfreigaben für Domänenbenutzer.

Pfad:

```text
Server-Manager
→ Tools
→ Gruppenrichtlinienverwaltung
→ Domäne firma.intern
→ Gruppenrichtlinienobjekte
→ Neu
→ Name: Laufwerkszuordnung
```

Anschließend:

```text
Rechtsklick auf „Laufwerkszuordnung“
→ Bearbeiten
```

Konfiguration im Gruppenrichtlinienverwaltungs-Editor:

```text
Benutzerkonfiguration
→ Einstellungen
→ Windows-Einstellungen
→ Laufwerkszuordnungen
→ Rechtsklick
→ Neu
→ Zugeordnetes Laufwerk
```

Konfiguration:

| Einstellung | Wert |
|---|---|
| Aktion | Erstellen |
| Speicherort | \\Fileserver\Firmendaten |
| Laufwerksbuchstabe | U: |
| Bezeichnung | Firmendaten |
| Wiederverbinden | Aktiviert |

Anschließend wird die Gruppenrichtlinie mit der entsprechenden Benutzer-OU verknüpft:

```text
OU_Benutzer
→ Rechtsklick
→ Vorhandenes GPO verknüpfen
→ Laufwerkszuordnung
```

Zur Validierung werden folgende Tests durchgeführt:

- Benutzeranmeldung an einem Domänenclient
- Kontrolle des automatisch verbundenen Netzlaufwerks U:
- Zugriff auf die Freigabe „Firmendaten“
- Überprüfung mittels `gpresult /r`

## Desktopcontroll

Die Gruppenrichtlinie „Desktopcontroll“ dient zur Zentralisierung von Benutzerdaten sowie zur Einschränkung lokaler Speichermöglichkeiten auf Clientsystemen.

Pfad:

```text
Server-Manager
→ Tools
→ Gruppenrichtlinienverwaltung
→ Gruppenrichtlinienobjekte
→ Desktopcontroll
→ Bearbeiten
```

---

### Ordnerumleitung (Folder Redirection)

Konfiguration:

```text
Benutzerkonfiguration
→ Richtlinien
→ Windows-Einstellungen
→ Ordnerumleitung
```

Folgende Benutzerordner werden auf den Fileserver umgeleitet:

- Desktop
- Dokumente

Einstellungen:

| Einstellung | Wert |
|---|---|
| Einstellung | Erweitert – Orte für verschiedene Benutzergruppen bestimmen |
| Zielordner | In den folgenden Pfad umleiten |
| Pfad | \\Fileserver\Homelaufwerk$\%username%\Desktop |

Beispiel Dokumente:

```text
\\Fileserver\Homelaufwerk$\%username%\Dokumente
```

---

### Zugriff auf lokale Laufwerke einschränken

Pfad:

```text
Benutzerkonfiguration
→ Richtlinien
→ Administrative Vorlagen
→ Windows-Komponenten
→ Datei-Explorer
```

Folgende Richtlinie wird aktiviert:

```text
Zugriff auf Laufwerke vom Arbeitsplatz verhindern
```

Konfiguration:

| Einstellung | Wert |
|---|---|
| Status | Aktiviert |
| Option | Nur Laufwerk A, B und C einschränken |

---

### Verknüpfung der GPO

```text
OU_Benutzer
→ Vorhandenes GPO verknüpfen
→ Desktopcontroll
```

---

### Validierung

- Benutzeranmeldung am Domänenclient
- Desktop und Dokumente werden auf den Fileserver umgeleitet
- Benutzer kann nicht auf Laufwerk C: zugreifen
- Überprüfung mittels `gpresult /r`

  
## Login_Sicherheit

Zweck:

```text
Erhöhung der Sicherheit bei der Benutzeranmeldung innerhalb der Domäne.
```

Beispiele:

- Sicherheitsrichtlinien
- Anmeldebeschränkungen
- Benutzerkontrolle

Konfiguration über:

```text
Computerkonfiguration
→ Windows-Einstellungen
→ Sicherheitseinstellungen
```

---

# Gruppenrichtlinien-Verknüpfung

Die Gruppenrichtlinien werden mit den entsprechenden Organisationseinheiten (OU) verknüpft.

Beispiele:

| OU | Zugewiesene GPO |
|---|---|
| OU_Benutzer | Desktopcontroll |
| OU_Benutzer | Laufwerkszuordnung |
| OU_Computer | Login_Sicherheit |

---

# Überprüfung der Richtlinien

Zur Validierung der Gruppenrichtlinien werden folgende Prüfungen durchgeführt:

- Kontrolle der vorhandenen Gruppenrichtlinien in der Gruppenrichtlinienverwaltung
- Überprüfung der angewendeten Richtlinien mittels `gpresult /r`
- Kontrolle der automatischen Netzlaufwerkszuordnung am Client
- Überprüfung der eingeschränkten Benutzerfunktionen auf Windows-Clients

---

# PowerShell-Überprüfung

Verwendeter Befehl:

```powershell
Get-GPO -All | Select-Object DisplayName, Id, CreationTime, ModificationTime
```

Beispielausgabe:

```text
Laufwerkszuordnung
Herunterfahrencontroll
Desktopcontroll
Login_Sicherheit
Default Domain Policy
Default Domain Controllers Policy
```

---

# Verwendete GPOs

```text
Laufwerkszuordnung
GUID: 08559a82-74e4-4e10-83fd-6cf8ddf2f92f

Herunterfahrencontroll
GUID: 0f1beaa3-c26c-4723-a5a7-41cdf71163e3

Desktopcontroll
GUID: 2ad3f10a-e71b-4e5e-acbe-6ca209699a63

Login_Sicherheit
GUID: 6640e1a6-9cfe-4963-8552-91dffc4d574d
```

---

# Zusammenfassung

Durch den Einsatz zentral verwalteter Gruppenrichtlinien wird die Administration der Windows-Domäne vereinfacht und standardisiert.

Die GPO-Struktur ermöglicht:

- Einheitliche Clientkonfiguration
- Zentrale Sicherheitsverwaltung
- Automatisierte Ressourcenbereitstellung
- Einschränkung unerwünschter Benutzeraktionen
- Verbesserte Benutzer- und Systemkontrolle
