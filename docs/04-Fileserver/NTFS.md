# NTFS-Berechtigungen auf dem Fileserver

Ziel: Die NTFS-Berechtigungen werden so konfiguriert, dass

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

## AD Benutzerzuordnung 

Diese Konfiguration erfolgt pro Benutzer im Active Directory (GUI):

Active Directory Benutzer und Computer  
→ OU_Benutzer (z. B. OU_Buchhaltung → OU_Benutzer)  
→ Benutzer auswählen (z. B. Harry Meyer)  
→ Eigenschaften  
→ Registerkarte: Profil  

---

## Home-Laufwerk konfigurieren

Home folder  
→ Connect  
→ Laufwerksbuchstabe: P:  
→ Pfad:  
\\fileserver\Homelaufwerk\%username%

## Berechtigungen

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

## Ergebnis

Nach Anmeldung:

- Gesamtstruktur Fileserver

 ```text
E:\
├── Firmendaten
│   ├── Geschaeftsfuehrung
│   ├── Vertrieb
│   ├── Software
│   ├── Personal
│   ├── Controlling
│   ├── Buchhaltung
│   └── Empfang
└── Homelaufwerk
    ├── user1
    ├── user2
    └── user3
    ├── ......

```
    
- Benutzer erhält automatisch Laufwerk P:
- Pfad wird automatisch aufgelöst zu:
  - Harry Meyer → \\fileserver\Homelaufwerk\harry.meyer
- Ordner wird automatisch auf dem Fileserver erstellt (falls Rechte korrekt gesetzt)
---

# Validierung

- Zugriffstest mit Domänenbenutzer
- Prüfung:
  - Zugriff nur auf eigene Abteilung
  - Zugriff auf fremde Abteilungen verweigert
- Vollzugriff auf eigene Laufwerk

  <img width="718" height="618" alt="image" src="https://github.com/user-attachments/assets/577bbd9d-a59d-4da3-9a24-c405059b1d1e" />

- Nur Mitglieder der jeweiligen DL-Gruppe haben auf den entsprechenden Abteilungsordner Lese- und Schreibrechte (Modify).

  <img width="636" height="541" alt="image" src="https://github.com/user-attachments/assets/cd841a9c-ea50-43b6-a24e-a9f875048ce3" />
