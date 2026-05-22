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

Pfad:

```text
Server-Manager
→ Tools
→ Gruppenrichtlinienverwaltung
```

---

# Konfigurierte Richtlinien

## Laufwerkszuordnung

Zweck:

```text
Automatische Bereitstellung zentraler Netzwerkfreigaben für Domänenbenutzer.
```

Beispiel:

```text
\\Fileserver\Freigaben
```

Konfiguration über:

```text
Benutzerkonfiguration
→ Einstellungen
→ Windows-Einstellungen
→ Laufwerkszuordnungen
```

---

## Herunterfahrencontroll

Zweck:

```text
Verhindert unerwünschtes Herunterfahren oder Neustarten von Clientsystemen durch Standardbenutzer.
```

Konfiguration über:

```text
Benutzerkonfiguration
→ Administrative Vorlagen
→ Startmenü und Taskleiste
```

---

## Desktopcontroll

Zweck:

```text
Einschränkung der Desktopumgebung sowie Vereinheitlichung der Benutzeroberfläche.
```

Beispiele:

- Desktop-Einstellungen
- Einschränkung von Systemeinstellungen
- Kontrolle der Benutzeroberfläche

Konfiguration über:

```text
Benutzerkonfiguration
→ Administrative Vorlagen
```

---

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
