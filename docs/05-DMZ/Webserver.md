# Webserver (IIS) + DNS Konfiguration – Firmenportal

## Ziel

Bereitstellung einer internen Website in der DMZ-Zone mit DNS-Auflösung über die Domäne `firma.intern`.

---

# 1. Webserver-Verzeichnis + Testseite erstellen

```powershell
New-Item -Path "C:\inetpub\wwwroot\firma" -ItemType Directory -Force

@"
<html>
<head><title>Firma Firmenportal</title></head>
<body>
<h1>Firma Firmenportal (DMZ)</h1>
<p>Server: $env:COMPUTERNAME</p>
<p>IP-Adresse: 172.16.10.10</p>
<p>Domäne: firma.intern</p>
<hr>
<p>Dies ist der Webserver in der DMZ-Zone</p>
<p>Letzter Zugriff: $(Get-Date)</p>
</body>
</html>
"@ | Out-File "C:\inetpub\wwwroot\firma\index.html" -Encoding UTF8
```

# 2. IIS Website erstellen

Pfad:

```text
Server-Manager
→ Tools
→ Internet Information Services (IIS)-Manager
```

Konfiguration:

Rechtsklick auf „Sites“
„Website hinzufügen“ auswählen

Einstellungen:

| Einstellung | Wert |
| --- | --- |
| Site-Name | Firmenportal |
| Physischer Pfad | C:\inetpub\wwwroot\firma |
| Typ | http |
| IP-Adresse | Alle nicht zugewiesen |
| Port | 80 |
| Hostname | www.firma.intern |

---

# 3. Standard Website deaktivieren

Pfad:

```text
Server-Manager
→ Tools
→ Internet Information Services (IIS)-Manager
→ Sites
```
Konfiguration:

„Default Web Site“ auswählen  
Rechtsklick  
„Stoppen“ auswählen

---

# 4. IP Binding hinzufügen

Pfad:

```text
IIS-Manager
→ Sites
→ Firmenportal
→ Bindungen...
```
Konfiguration:

| Einstellung | Wert |
| --- | --- |
| IP-Adresse | Alle nicht zugewiesen |
| Port | 80 |
| Hostname | www.firma.intern |

---

# 5. DNS-Eintrag für www.firma.intern erstellen

Pfad:

```text
Server-Manager
→ Tools
→ DNS
→ Server: DC1
→ Forward-Lookup-Zone: firma.intern

```
„Neuer Host (A oder AAAA)“ auswählen

| Einstellung | Wert |
| --- | --- |
| Name | www |
| IP-Adresse | 172.16.10.10 |
| „Zugehörigen PTR-Eintrag erstellen“ | auswahlen |

---

# 6. Website starten

Im IIS-Manager:  
`Firmenportal` → Rechtsklick → **„Website starten“**

---

# 7. Testzugriff

Die Funktion der Website wird getestet:

- Zugriff über IP-Adresse: `http://172.16.10.10`
- Zugriff über DNS-Name: `http://www.firma.intern`

Beide Zugriffsarten müssen erfolgreich die Firmenportal-Webseite anzeigen.
<img width="510" height="407" alt="image" src="https://github.com/user-attachments/assets/96f7c4eb-2cdb-4589-a581-e131e5a72a52" />

