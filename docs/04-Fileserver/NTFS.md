# NTFS-Berechtigungen auf dem Fileserver

## Ziel

Die NTFS-Berechtigungen werden so konfiguriert, dass:

- jeder Benutzer nur auf seinen eigenen Bereich zugreifen kann
- Abteilungsordner nur über die jeweiligen DL-Gruppen zugänglich sind
- unerwünschte Standardberechtigungen (Benutzer / Jeder) entfernt werden
- kontrollierter Lese-/Schreibzugriff gewährleistet ist

---

# Grundprinzip

Es gilt:

- SMB-Freigabe = allgemeiner Zugriffspunkt
- NTFS-Rechte = tatsächliche Zugriffskontrolle

Effektive Berechtigungen ergeben sich aus der Kombination beider Ebenen.

---

# Berechtigungsstruktur

## Firmendaten (Abteilungsordner)

Pfad:

```text
E:\Firmendaten
```

Jeder Abteilungsordner erhält folgende Struktur:

| Berechtigung | Gruppe | Rechte |
|---|---|---|
| Vollzugriff (optional Admin) | Domain Admins | Vollzugriff |
| Lesen/Schreiben | DL_<Abteilung> | Schreiben |
| Entfernen | Users / Benutzer / Jeder | gelöscht |

---

## Beispiel: Abteilung Vertrieb

Pfad:

```text
E:\Firmendaten\Vertrieb
```

Konfiguration:

- Vererbung deaktivieren
- Vorhandene Berechtigungen entfernen:
  - Benutzer
  - Jeder
- Neue Berechtigung hinzufügen:

| Gruppe | Rechte |
|---|---|
| firma\DL_Vertrieb | Lesen + Schreiben |

---

## PowerShell-Konfiguration (optional automatisiert)

```powershell
$abteilungen = @(
    "Geschaeftsfuehrung",
    "Vertrieb",
    "Software",
    "Personal",
    "Controlling",
    "Buchhaltung",
    "Empfang"
)

foreach ($abt in $abteilungen)
{
    $path = "E:\Firmendaten\$abt"

    # Vererbung deaktivieren
    icacls $path /inheritance:r

    # Standardgruppen entfernen
    icacls $path /remove "Users"
    icacls $path /remove "Benutzer"
    icacls $path /remove "Jeder"

    # DL-Gruppe berechtigen
    icacls $path /grant "firma\DL_$abt:(OI)(CI)M"
}
```

---

# Homelaufwerk (Benutzerverzeichnisse)

Pfad:

```text
E:\Homelaufwerk
```

Konfiguration:

| Berechtigung | Gruppe | Rechte |
|---|---|---|
| Vollzugriff | Creator Owner | Vollzugriff |
| Vollzugriff | Administrators | Vollzugriff |
| Entfernen | Users / Jeder | entfernt |

PowerShell:

```powershell
icacls "E:\Homelaufwerk" /inheritance:r
icacls "E:\Homelaufwerk" /remove "Users"
icacls "E:\Homelaufwerk" /remove "Benutzer"
icacls "E:\Homelaufwerk" /remove "Jeder"
```

---

# Validierung

- Zugriffstest mit Domänenbenutzer
- Prüfung:
  - Zugriff nur auf eigene Abteilung
  - Zugriff auf fremde Abteilungen verweigert
- Kontrolle mit:

```powershell
icacls E:\Firmendaten\Vertrieb
```

- Test eines Benutzer-Homelaufwerks nach Anmeldung
