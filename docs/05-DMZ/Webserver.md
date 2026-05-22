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

# 2. Test-Webseite erstellen

```powershell
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

Eine neue IIS-Website wird erstellt und auf das Verzeichnis `C:\inetpub\wwwroot\firma` gebunden.  
Die Website wird unter dem Namen „Firmenportal“ im IIS registriert und ist über Port 80 erreichbar.  
Als Hostname wird `www.firma.intern` verwendet.

---

# 4. Standard Website deaktivieren

Die Standardwebsite „Default Web Site“ wird deaktiviert, um Konflikte mit der neuen Firmenportal-Website zu vermeiden.  
Damit ist nur noch die neue Unternehmenswebsite aktiv erreichbar.

---

# 5. IP Binding hinzufügen

Zusätzlich zur Hostheader-Konfiguration wird ein IP-Binding eingerichtet.  
Dadurch ist die Website auch direkt über die IP-Adresse `172.16.10.10` erreichbar, ohne DNS-Namen.

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
