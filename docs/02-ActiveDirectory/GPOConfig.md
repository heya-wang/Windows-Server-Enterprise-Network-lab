# Gruppenrichtlinien (GPO) Konfiguration

## Ziel

Zur zentralen Verwaltung der Active-Directory-Domäne werden mehrere Gruppenrichtlinienobjekte (GPOs) eingesetzt.

Die Konfiguration dient der:

- Zentralen Benutzerverwaltung
- Automatischen Netzlaufwerkszuordnung
- Einschränkung unerwünschter Benutzeraktionen
- Erhöhung der Anmeldesicherheit
- Einheitlichen Client-Konfiguration

---

# Verwendete Gruppenrichtlinien

| Nr. | GPO | Zweck |
|---|---|---|
| 1 | Laufwerkszuordnung | Automatische Verbindung von Netzlaufwerken |
| 2 | Desktopcontroll | Einschränkung lokaler Speicher- und Desktopfunktionen |
| 3 | Login_Sicherheit | Sicherheitsrichtlinien für Benutzeranmeldung |
| 4 | Herunterfahrencontroll | Einschränkung von Herunterfahren- und Neustartfunktionen |

---

# Konfigurierte Richtlinien

## 1. Laufwerkszuordnung

Die Gruppenrichtlinie „Laufwerkszuordnung“ dient zur automatischen Bereitstellung zentraler Netzwerkfreigaben für Domänenbenutzer.

Pfad:

```text
Server-Manager
→ Tools
→ Gruppenrichtlinienverwaltung
→ Gruppenrichtlinienobjekte
→ Neu
→ Laufwerkszuordnung
→ Bearbeiten
```

Konfiguration:

```text
Benutzerkonfiguration
→ Einstellungen
→ Windows-Einstellungen
→ Laufwerkszuordnungen
→ Rechtsklick
→ Neu
→ Zugeordnetes Laufwerk
```

| Einstellung | Wert |
|---|---|
| Aktion | Erstellen |
| Speicherort | \\Fileserver\Firmendaten |
| Laufwerksbuchstabe | U: |
| Bezeichnung | Firmendaten |
| Wiederverbinden | Aktiviert |

Verknüpfung der GPO:

```text
OU_Benutzer
→ Vorhandenes GPO verknüpfen
→ Laufwerkszuordnung
```

Validierung:

- Benutzeranmeldung an einem Domänenclient
- Kontrolle des automatisch verbundenen Netzlaufwerks U:
- Zugriff auf die Freigabe „Firmendaten“
- Überprüfung mittels `gpresult /r`

---

## 2. Desktopcontroll

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

Konfiguration:

```text
Benutzerkonfiguration
→ Richtlinien
→ Administrative Vorlagen
→ Windows-Komponenten
→ Datei-Explorer
```

| Richtlinie | Wert |
|---|---|
| Zugriff auf Laufwerke vom Arbeitsplatz verhindern | Aktiviert |
| Einschränkung | Nur Laufwerk A, B und C einschränken |

Verknüpfung der GPO:

```text
OU_Benutzer
→ Vorhandenes GPO verknüpfen
→ Desktopcontroll
```

Validierung:

- Benutzeranmeldung am Domänenclient
- Desktop und Dokumente werden auf den Fileserver umgeleitet
- Benutzer kann nicht auf Laufwerk C: zugreifen
- Überprüfung mittels `gpresult /r`

---

## 3. Login_Sicherheit

Die Gruppenrichtlinie „Login_Sicherheit“ dient der Absicherung der Benutzeranmeldung innerhalb der Active-Directory-Domäne.

Pfad:

```text
Server-Manager
→ Tools
→ Gruppenrichtlinienverwaltung
→ Gruppenrichtlinienobjekte
→ Login_Sicherheit
→ Bearbeiten
```

Konfiguration:

```text
Computerkonfiguration
→ Richtlinien
→ Windows-Einstellungen
→ Sicherheitseinstellungen
```

| Bereich | Einstellung | Wert |
|---|---|---|
| Kontorichtlinien → Kennwortrichtlinie | Mindestkennwortlänge | 8 Zeichen |
| Kontorichtlinien → Kennwortrichtlinie | Kennwort muss Komplexitätsanforderungen entsprechen | Aktiviert |
| Kontorichtlinien → Kennwortrichtlinie | Maximales Kennwortalter | 90 Tage |
| Kontorichtlinien → Kennwortrichtlinie | Kennworthistorie erzwingen | 5 Kennwörter |
| Kontorichtlinien → Kontosperrungsrichtlinie | Kontosperrschwelle | 5 Fehlversuche |
| Kontorichtlinien → Kontosperrungsrichtlinie | Kontosperrdauer | 15 Minuten |
| Kontorichtlinien → Kontosperrungsrichtlinie | Kontosperrzähler zurücksetzen nach | 15 Minuten |
| Lokale Richtlinien → Sicherheitsoptionen | Interaktive Anmeldung: Letzten Benutzernamen nicht anzeigen | Aktiviert |
| Lokale Richtlinien → Sicherheitsoptionen | STRG+ALT+ENTF erforderlich | Aktiviert |
| Lokale Richtlinien → Sicherheitsoptionen | Gastkonto Status | Deaktiviert |

Verknüpfung der GPO:

```text
OU_Computer
→ Vorhandenes GPO verknüpfen
→ Login_Sicherheit
```

Validierung:

- Anmeldung mit Domänenbenutzer testen
- Falsche Kennworteingaben zur Kontosperrung prüfen
- Überprüfung mittels `gpresult /r`
- Kontrolle der angewendeten Sicherheitsrichtlinien auf dem Client

---

## 4. Herunterfahrencontroll

Die Gruppenrichtlinie „Herunterfahrencontroll“ dient zur Absicherung von Benutzerkonten gegen wiederholte fehlerhafte Anmeldeversuche.

Pfad:

```text
Server-Manager
→ Tools
→ Gruppenrichtlinienverwaltung
→ Gruppenrichtlinienobjekte
→ Herunterfahrencontroll
→ Bearbeiten
```

Konfiguration:

```text
Computerkonfiguration
→ Richtlinien
→ Windows-Einstellungen
→ Sicherheitseinstellungen
→ Kontorichtlinien
→ Kontosperrungsrichtlinie
```

| Richtlinie | Wert |
|---|---|
| Kontosperrschwelle | 3 Fehlversuche |
| Kontosperrdauer | 30 Minuten |
| Kontosperrzähler zurücksetzen nach | 30 Minuten |

Verknüpfung der GPO:

```text
OU_Computer
→ Vorhandenes GPO verknüpfen
→ Herunterfahrencontroll
```

Validierung:

- Mehrfache falsche Kennworteingabe testen
- Benutzerkonto wird nach drei Fehlversuchen gesperrt
- Anmeldung erst nach 30 Minuten wieder möglich
- Überprüfung mittels `gpresult /r`

---

# PowerShell-Überprüfung

Verwendeter Befehl:

```powershell
Get-GPO -All | Select-Object DisplayName, Id, CreationTime, ModificationTime
```

Verwendete GPOs:

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

Durch den Einsatz zentral verwalteter Gruppenrichtlinien wird die Administration der Windows-Domäne standardisiert und vereinfacht.

Die eingesetzten GPOs ermöglichen:

- Einheitliche Clientkonfiguration
- Zentrale Sicherheitsverwaltung
- Automatische Netzlaufwerksbereitstellung
- Einschränkung lokaler Benutzeraktionen
- Verbesserte Benutzer- und Systemkontrolle
