# SMB-Freigaben auf dem Fileserver

## Ziel

Auf dem Fileserver wird ein separates Datenlaufwerk eingerichtet, um zentrale Benutzer- und Abteilungsfreigaben bereitzustellen.

Die Konfiguration umfasst:

- Netzlaufwerk „Firmendaten“
- Persönliche Benutzerverzeichnisse „Homelaufwerk“
- Abteilungsordner für gemeinsame Datenablage
- SMB-Freigaben für Domänenbenutzer

---

# Zusätzliche Festplatte vorbereiten

Pfad:

```text
Server-Manager
→ Tools
→ Computerverwaltung
→ Datenträgerverwaltung
```

Konfiguration:

- Neue virtuelle Festplatte initialisieren
- Neues Volume erstellen
- Laufwerksbuchstaben `E:` vergeben
- NTFS formatieren

---

# Ordnerstruktur und SMB-Freigaben erstellen

Die komplette Ordnerstruktur sowie die SMB-Freigaben werden automatisiert per PowerShell erstellt.

Verwendetes Skript:

```powershell
# Abteilungen definieren
$abteilungen = @(
    "Geschaeftsfuehrung",
    "Vertrieb",
    "Software",
    "Personal",
    "Controlling",
    "Buchhaltung",
    "Empfang"
)

# Hauptordner erstellen
New-Item -Path "E:\Firmendaten" -ItemType Directory -Force
New-Item -Path "E:\Homelaufwerk" -ItemType Directory -Force

# SMB-Freigaben erstellen
New-SmbShare -Name "Firmendaten" -Path "E:\Firmendaten" -FullAccess "Authentifizierte Benutzer"
New-SmbShare -Name "Homelaufwerk" -Path "E:\Homelaufwerk" -FullAccess "Authentifizierte Benutzer"

# Abteilungsordner erstellen
foreach ($abt in $abteilungen)
{
    New-Item -Path "E:\Firmendaten\$abt" -ItemType Directory -Force
}
```

Erstellte Struktur:

```text
E:\Firmendaten
├── Geschaeftsfuehrung
├── Vertrieb
├── Software
├── Personal
├── Controlling
├── Buchhaltung
└── Empfang

E:\Homelaufwerk
```

Freigaben:

| Freigabe | Pfad |
|---|---|
| Firmendaten | E:\Firmendaten |
| Homelaufwerk | E:\Homelaufwerk |

---

# Validierung

<img width="580" height="325" alt="image" src="https://github.com/user-attachments/assets/b2eca8b1-8634-4409-8e65-877d42b600f4" />

<img width="636" height="541" alt="image" src="https://github.com/user-attachments/assets/ff8b458e-8d9a-41b5-9af7-bcafc0595476" />
