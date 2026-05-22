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

Zur Validierung werden folgende Tests durchgeführt:

- Kontrolle der Ordnerstruktur auf Laufwerk `E:`
- Überprüfung der SMB-Freigaben mittels:

```powershell
Get-SmbShare
```

- Testzugriff von einem Domänenclient
- Überprüfung der Netzwerkerreichbarkeit des Fileservers
