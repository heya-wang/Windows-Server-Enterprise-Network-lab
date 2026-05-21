# VPN-Konfiguration (L2TP/IPsec)

## Ziel

Diese Konfiguration ermöglicht einen sicheren Remote-Zugriff auf das interne Netzwerk über L2TP/IPsec mittels RRAS (Routing und RAS).

Der VPN-Zugriff verwendet:

- L2TP/IPsec
- Pre-Shared Key (PSK)
- MS-CHAP v2 Authentifizierung
- Lokale Benutzerkonten

---

# VPN-Netzwerk

| Funktion | Wert |
|---|---|
| VPN-Typ | L2TP/IPsec |
| VPN-Netz | 172.16.30.0/24 |
| Adresspool | 172.16.30.100 – 172.16.30.200 |
| Zugriffspunkt | ROUTER |
| Authentifizierung | MS-CHAP v2 |

---

# Server-Konfiguration (ROUTER)

## Benutzerkonto vorbereiten

Lokalen VPN-Benutzer erstellen:

```text
lusrmgr.msc
```

Für den Benutzer:

- Einwählrechte auf „Zugriff zulassen“ setzen

---

# RRAS-Konfiguration

## Eigenschaften öffnen

```text
rrasmgmt.msc
→ Rechtsklick auf Server
→ Eigenschaften
```

---

## Sicherheit

Folgende Option aktivieren:

```text
L2TP-Verschlüsselung mit vorinstalliertem Schlüssel erlauben
```

Beispiel für den Pre-Shared Key:

```text
vpnfirma
```

---

## IPv4 Adresspool

IPv4-Adresszuweisung:

```text
Statischer Adresspool
```

Adressbereich:

```text
172.16.30.100 – 172.16.30.200
```

Der Bereich darf nicht mit bestehenden DHCP-Scopes kollidieren.

---

# Hyper-V Konfiguration

Da der ROUTER als virtuelle Maschine betrieben wird, muss MAC-Adresse-Spoofing aktiviert werden.

Pfad:

```text
Hyper-V Manager
→ ROUTER VM
→ Einstellungen
→ Netzwerkkarte
→ Erweiterte Funktionen
```

Folgende Option aktivieren:

```text
MAC-Adresse-Spoofing aktivieren
```

Anschließend die VM neu starten.

---

# NPS-Konfiguration

Da der Server kein Domain Controller ist, muss die lokale NPS-Richtlinie angepasst werden.

Da der ROUTER nicht Mitglied der Active Directory Domäne ist, erfolgt die VPN-Authentifizierung über lokale Benutzerkonten des ROUTERS.

Bei einer Domänenmitgliedschaft könnten stattdessen zentrale Domänenbenutzer für den VPN-Zugriff verwendet werden.

Pfad:

```text
nps.msc
→ Richtlinien
→ Netzwerkrichtlinien
→ Verbindung mit Microsoft Routing und Remotezugriffsserver
```

Zugriffsberechtigung:

```text
Zugriff gewähren
```

---

# Windows Defender Firewall

Für L2TP/IPsec müssen eingehende UDP-Ports freigegeben werden.

Pfad:

```text
wf.msc
→ Eingehende Regeln
→ Neue Regel
```

Konfiguration:

| Einstellung | Wert |
|---|---|
| Typ | Port |
| Protokoll | UDP |
| Ports | 500, 4500, 1701 |
| Aktion | Verbindung zulassen |

Regelname:

```text
L2TP-VPN-Inbound
```

---

# Client-Konfiguration (Windows 11)

## VPN-Verbindung erstellen

Pfad:

```text
Einstellungen
→ Netzwerk und Internet
→ VPN
→ VPN hinzufügen
```

Konfiguration:

| Einstellung | Wert |
|---|---|
| VPN-Typ | L2TP/IPsec mit vorinstalliertem Schlüssel |
| Schlüssel | vpnfirma |

---

# Erweiterte Sicherheitseinstellungen

Pfad:

```text
Win + R
→ ncpa.cpl
→ Eigenschaften der VPN-Verbindung
→ Sicherheit
```

Datenverschlüsselung:

```text
Verschlüsselung erforderlich
(Verbindung trennen, wenn Server ablehnt)
```

Authentifizierungsprotokolle:

- Nur Microsoft CHAP Version 2 (MS-CHAP v2)
- Alle anderen Protokolle bleiben deaktiviert

---

# Sicherheitsprinzip

Server und Client müssen identische Authentifizierungsprotokolle verwenden.

Die lokale NPS-Richtlinie des ROUTER verwendet standardmäßig MS-CHAP v2 für lokale Benutzerkonten.

Bei nicht übereinstimmenden Protokollen schlägt der VPN-Handshake fehl.

---

# Validierung

Die VPN-Konfiguration wurde erfolgreich getestet.

Die Screenshots zeigen beispielhaft:

- Erfolgreichen Aufbau der VPN-Verbindung
-
  <img width="426" height="195" alt="image" src="https://github.com/user-attachments/assets/078be03d-ccb2-428e-ba97-c9678bb191bd" />
  

- Zuweisung einer IP-Adresse aus dem VPN-Adresspool
 
  <img width="491" height="289" alt="image" src="https://github.com/user-attachments/assets/920d58fe-a4f2-4bcf-8e48-e97065d63906" />
  
Alle weiteren Funktionstests verliefen ebenfalls erfolgreich.

---

# Zusammenfassung

Die L2TP/IPsec-Konfiguration ermöglicht einen sicheren Remote-Zugriff auf das interne Netzwerk über RRAS.

Die Umgebung verwendet:

- L2TP/IPsec mit Pre-Shared Key
- MS-CHAP v2 Authentifizierung
- Statischen VPN-Adresspool
- Lokale Benutzerkonten
- Windows Defender Firewall Regeln für IPsec/L2TP
