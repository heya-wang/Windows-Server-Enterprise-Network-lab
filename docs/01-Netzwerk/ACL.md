# DMZ – RRAS ACL Konfiguration

## Ziel

Diese Konfiguration beschreibt die Filterregeln der DMZ-Schnittstelle im Windows RRAS Router.  
Standardmäßig gilt: **Alle nicht explizit erlaubten Pakete werden verworfen (DROP).**

---

# Konfigurationspfad

```text
RRAS
→ IPv4
→ Allgemein
→ DMZ Interface auswählen
→ Eigenschaften
→ Eingehende Filter (Inbound Filter)
```

---

# Grundparameter

- Filtertyp: INPUT  
- Standardaktion: DROP  
- Schnittstelle: DMZ (172.16.10.0/24)

---

# Erlaubte Regeln

Quelle: 172.16.10.0/24 (DMZ)

Ziel: 172.16.1.0/24 (Servernetz)  
Protokoll: UDP  
Port: 53  

---

Quelle: 172.16.10.0/24 (DMZ)

Ziel: 172.16.1.0/24 (Servernetz)  
Protokoll: TCP  
Port: 389  

---

Quelle: 172.16.10.0/24 (DMZ)

Ziel: 0.0.0.0/0  
Protokoll: ICMP  
Ports: ANY  

---

Quelle: 172.16.10.0/24 (DMZ)

Ziel: 0.0.0.0/0  
Protokoll: TCP  
Port: 80  

---

Quelle: 172.16.10.0/24 (DMZ)

Ziel: 0.0.0.0/0  
Protokoll: TCP  
Port: 443  

---

# Sicherheitsprinzip

- Default Deny (DROP)
- Nur benötigte Dienste werden explizit erlaubt
- Zugriff auf interne AD-Dienste wird gezielt über LDAP/DNS ermöglicht
- Webzugriff (HTTP/HTTPS):  Erlaubt die Bereitstellung von Webseiten über die DMZ, sodass externe Clients HTTP/HTTPS-Anfragen an den Webserver stellen und die entsprechenden Antwortdaten (HTTP Response) zurück an die externen Clients übertragen werden.

---
# Validierung

Die Funktion der DMZ-ACL wird direkt nach der Konfiguration überprüft.

Zur Validierung werden folgende Tests durchgeführt:

- DNS-Auflösung über den internen DNS-Server mittels `nslookup` (z. B. dc1.firma.intern)
- Zugriff des Clients auf den DMZ-Webserver und erfolgreiche Anzeige der Webseite im Browser
- Überprüfung der DMZ-Zugriffsregeln:
  - LDAP (TCP 389) ist erlaubt
  - SMB (TCP 445) ist blockiert

Die angehängten Screenshots zeigen beispielhaft:

- DNS/AD-Kommunikation (nslookup und Port 389 ， 445 Test)
 <img width="940" height="290" alt="image" src="https://github.com/user-attachments/assets/d1aff8e1-9ff5-48af-8ea6-07278217aa13" />
 <img width="617" height="224" alt="image" src="https://github.com/user-attachments/assets/06e5436f-cdf8-4906-ac47-4f5e8d623abd" />




- Webzugriff eines Clients auf den DMZ-Webserver (HTTP/HTTPS)
<img width="568" height="312" alt="image" src="https://github.com/user-attachments/assets/21e54081-6f51-4d2a-9621-f66d7e4fcfc9" />


  
# Zusammenfassung

Die DMZ-Schnittstelle ist so konfiguriert, dass nur definierter Traffic in das Netzwerk gelangt.  
Die Konfiguration erfolgt ausschließlich über die **Eingehenden Filter der DMZ-Netzwerkkarte im RRAS Manager**.
```
