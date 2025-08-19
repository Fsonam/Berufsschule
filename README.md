# 🖥️ Anleitung: Installation eines Active Directory Servers

## 🎯 Ziel
In dieser Übung wird ein **Windows Server 2019** als **Domänencontroller mit Active Directory** aufgesetzt.  
Am Ende steht ein lauffähiger Server mit eigener **Demodomäne**.

---

## ⚙️ Vorbereitung
- Virtuelle Maschine starten:  
  `Pfad: C:\VMs\WS4…`
- Anmeldung mit lokalem Administrator:  
  ```
  Benutzer: Administrator
  Passwort: *********
  ```

---

## 🛠️ Grundkonfiguration des Servers

### 🌐 1. Netzwerk konfigurieren
- Server Manager → **Local Server**
- Unter Properties bei „Ethernet“ folgende IPv4-Daten setzen:

| Einstellung    | Wert          |
|----------------|---------------|
| **IP-Adresse** | 10.1.230.10   |
| **Subnetz**    | 255.255.255.0 |
| **DNS-Server** | 10.1.230.10   |
| **Gateway**    | 10.1.230.1    |

➡️ IPv6 deaktivieren.

### 🔒 2. Firewall ausschalten
Server Manager → „Turn Windows Firewall on or off“  
→ Private & öffentliche Firewall deaktivieren.

### 🏷️ 3. Servernamen ändern
`ADSERVER-FEDERER-DEMO`  
➡️ VM neu starten.

---

## 📦 Installation der AD-Rolle
1. Server Manager → **Manage → Add Roles and Features**
2. Installationstyp: *Role-based or feature-based Installation*
3. Rolle auswählen: **Active Directory Domain Services (AD DS)**  
   → „Add Features“ bestätigen
4. Installation starten → „Restart if required“ aktivieren

---

## 🌐 Server zum Domänencontroller hochstufen
1. Server Manager → Gelbes Ausrufezeichen →  
   **Promote this server to a domain controller**
2. Neue Gesamtstruktur erstellen:  
   `Root Domain Name: federer.demo`
3. Functional Level: **Windows Server 2016**
   - DNS-Server aktivieren  
   - Wiederherstellungspasswort setzen: *********
4. Hinweis wegen „Parent Zone“ ignorieren
5. Speicherorte belassen:  
   - `C:\Windows\NTDS`  
   - `C:\Windows\SYSVOL`
6. Voraussetzungen prüfen → „All checks passed successfully“
7. Installation starten & Server neu starten

---

## ✅ Abschluss
- Anmeldung als **Domänenadministrator**
- Funktionsprüfung im Server Manager durchführen

---

## 📸 Screenshots (Platzhalter)
👉 Hier kannst du deine Screenshots einfügen, z. B.:  
`![Server Manager Screenshot](.image.png)`

---

## ✍️ Ergebnis
Ein funktionsfähiger **Domänencontroller mit Active Directory** unter Windows Server 2019,  
konfiguriert mit der Domäne **federer.demo**,  
Hostname **ADSERVER-FEDERER-DEMO**,  
und IP-Adresse **10.1.230.10**.
