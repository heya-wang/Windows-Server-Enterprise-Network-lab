# OU- und Benutzerstruktur – Automatisierte Active Directory Konfiguration

## Ziel

Diese Konfiguration erstellt eine vollständige, standardisierte Active Directory Struktur inklusive OU-Hierarchie, Benutzer, Gruppen (AGDLP-Modell) sowie initialer Sicherheitsmaßnahmen.

Die Umsetzung erfolgt automatisiert mittels PowerShell, um eine reproduzierbare und skalierbare AD-Struktur zu simulieren.

---

## Hinweis zur Ausführung

Die Ausführung erfolgt auf dem Domänencontroller (DC1) mit administrativen Rechten:

```text
gfnlab\Administrator
```

---

## 1. Erstellung der OU-Struktur (Top-Level + Sub-OUs)

Die OU-Struktur wird dynamisch aufgebaut und gegen versehentliches Löschen geschützt.

```powershell
# Modul 1: Top-Level OU erstellen
try {
    $Eingabe = Read-Host "Name der neuen OU"

    New-ADOrganizationalUnit `
        -Name $Eingabe `
        -ProtectedFromAccidentalDeletion $true
}
catch {
    Write-Host "Fehler: OU existiert bereits oder konnte nicht erstellt werden."
    break
}

# Modul 2: Standard OU-Struktur
$basisOU = Get-ADOrganizationalUnit -Filter "Name -eq '$Eingabe'"

$struktur = @(
    "OU_Geschaeftsfuehrung",
    "OU_Vertrieb",
    "OU_Software",
    "OU_Personal",
    "OU_Controlling",
    "OU_Buchhaltung",
    "OU_Empfang"
)

$struktur2 = @(
    "OU_Benutzer",
    "OU_Computer",
    "OU_Gruppen"
)

foreach ($neueOU in $struktur) {

    $parent = New-ADOrganizationalUnit `
        -Name $neueOU `
        -Path $basisOU.DistinguishedName `
        -ProtectedFromAccidentalDeletion $true

    foreach ($child in $struktur2) {

        New-ADOrganizationalUnit `
            -Name $child `
            -Path $parent.DistinguishedName `
            -ProtectedFromAccidentalDeletion $true
    }
}
```

---

## 2. Benutzerimport aus CSV

Die Benutzer werden aus einer CSV-Datei automatisch in die entsprechenden OUs importiert.

CSV-Pfad:

```text
C:\Users\Administrator\Desktop\Neue_Mitarbeiter.csv
```

```powershell
Import-Csv -Path "C:\Users\Administrator\Desktop\Neue_Mitarbeiter.csv" -Encoding Default | ForEach-Object {

    Write-Host "Erstelle Benutzer:" $_.Name

    New-ADUser `
        -Name $_.Name `
        -GivenName $_.GivenName `
        -Surname $_.Surname `
        -SamAccountName $_.SamAccountName `
        -UserPrincipalName $_.UserPrincipalName `
        -Path $_.Path `
        -AccountPassword (ConvertTo-SecureString $_.TempPass -AsPlainText -Force) `
        -Enabled $true `
        -ChangePasswordAtLogon $true
}
```

---

## 3. Erstellung der AGDLP-Gruppenstruktur

Für jede Abteilung werden Global Groups (GG) und Domain Local Groups (DL) erstellt.

```powershell
# Beispiel: Buchhaltung
New-ADGroup -Name "GG_Buchhaltung" -GroupScope Global -Path "OU=OU_Gruppen,OU=OU_Buchhaltung,DC=firma,DC=intern"
New-ADGroup -Name "DL_Buchhaltung" -GroupScope DomainLocal -Path "OU=OU_Gruppen,OU=OU_Buchhaltung,DC=firma,DC=intern"
```

(Gleiches Prinzip für alle weiteren Abteilungen)

---

## 4. Gruppenverschachtelung (G → DL)

Global Groups werden in Domain Local Groups verschachtelt.

```powershell
Add-ADGroupMember -Identity "DL_Buchhaltung" -Members "GG_Buchhaltung"
Add-ADGroupMember -Identity "DL_Vertrieb" -Members "GG_Vertrieb"
Add-ADGroupMember -Identity "DL_Software" -Members "GG_Software"
Add-ADGroupMember -Identity "DL_Personal" -Members "GG_Personal"
Add-ADGroupMember -Identity "DL_Controlling" -Members "GG_Controlling"
Add-ADGroupMember -Identity "DL_Empfang" -Members "GG_Empfang"
```

---

## 5. Benutzerzuweisung zu Global Groups (A → G)

Benutzer werden entsprechend ihrer Abteilung den Global Groups zugewiesen.

```powershell
Add-ADGroupMember -Identity "GG_Buchhaltung" -Members "Harry Meyer"
Add-ADGroupMember -Identity "GG_Buchhaltung" -Members "Petra Stahlhut"
Add-ADGroupMember -Identity "GG_Vertrieb" -Members "Juergen Hoeller"
Add-ADGroupMember -Identity "GG_Software" -Members "Thomas Joos"
Add-ADGroupMember -Identity "GG_Controlling" -Members "Sandra Schroll"
Add-ADGroupMember -Identity "GG_Personal" -Members "Jens Meier"
Add-ADGroupMember -Identity "GG_Geschaeftsfuehrung" -Members "Andrea Schroll"
```

---

## 6. Validierung

Nach der Ausführung werden folgende Punkte überprüft:

- OU-Struktur im Active Directory vorhanden

 <img width="543" height="473" alt="image" src="https://github.com/user-attachments/assets/c0c92cca-807a-4987-a003-bef45a2029af" />

- Gruppenstruktur (AGDLP) korrekt aufgebaut

  <img width="407" height="219" alt="image" src="https://github.com/user-attachments/assets/9a172d10-d68b-4b48-924e-b6461ad05142" />

- Mitgliedschaften korrekt zugewiesen

---

## Zusammenfassung

Diese PowerShell-basierte Konfiguration erstellt eine vollständige Active Directory Unternehmensstruktur inklusive:

- OU-Hierarchie
- Benutzerverwaltung (CSV-basiert)
- AGDLP Gruppenmodell
- Automatisierte Gruppenverschachtelung
- Skalierbare AD-Architektur
