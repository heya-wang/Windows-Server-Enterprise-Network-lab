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

# 3. IIS Website erstellen

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

Site-Name: Firmenportal
Physischer Pfad: C:\inetpub\wwwroot\firma
Typ: http
IP-Adresse: Alle nicht zugewiesen
Port: 80
Hostname: www.firma.intern

---

# 4. Standard Website deaktivieren

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

# 5. IP Binding hinzufügen

Pfad:

```text
IIS-Manager
→ Sites
→ Firmenportal
→ Bindungen...
```
Konfiguration:

| Einstellung | Wert                  |
| ----------- | --------------------- |
| Typ         | http                  |
| IP-Adresse  | Alle nicht zugewiesen |
| Port        | 80                    |
| Hostname    | leer lassen           |



---

# 6. Website starten

Die IIS-Website „Firmenportal“ wird gestartet und für den Zugriff im Netzwerk freigegeben.  
Der Webdienst läuft anschließend aktiv auf dem Server.

---

# 7. Testzugriff

Die Funktion der Website wird getestet:

- Zugriff über IP-Adresse: `http://172.16.10.10`
- Zugriff über DNS-Name: `http://www.firma.intern`

Beide Zugriffsarten müssen erfolgreich die Firmenportal-Webseite anzeigen.
