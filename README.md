🖥️ Anleitung: Installation eines Active Directory Servers
🎯 Ziel

In dieser Übung wird ein Windows Server 2019 als Domänencontroller mit Active Directory aufgesetzt.
Am Ende steht ein lauffähiger Server mit eigener Demodomäne.

⚙️ Vorbereitung

Virtuelle Maschine starten:

Pfad: C:\VMs\WS4…


Anmeldung mit lokalem Administrator:

Benutzer: Administrator
Passwort: Riethuesli>12345


(Bei Bedarf auf ein eigenes komplexes Passwort ändern.)

🛠️ Grundkonfiguration des Servers
🔤 1. Tastatur- & Spracheinstellungen

Falls das Tastaturlayout falsch ist:
Control Panel → Language → Deutsch (Schweiz) hinzufügen und nach oben verschieben.
➡️ Danach VM neu starten.

🌐 2. Netzwerk konfigurieren

Server Manager → Local Server

Unter Properties bei „Ethernet“ folgende IPv4-Daten setzen:

Einstellung	Wert
IP-Adresse	10.1.230.10
Subnetz	255.255.255.0
DNS-Server	10.1.230.10
Gateway	10.1.230.1

➡️ IPv6 deaktivieren.

🔒 3. Firewall ausschalten

Server Manager → „Turn Windows Firewall on or off“
→ Private & öffentliche Firewall deaktivieren.

🏷️ 4. Servernamen ändern
Neuer Hostname: ADSERVER-FEDERER


➡️ VM neu starten.

📦 Installation der AD-Rolle

Server Manager → Manage → Add Roles and Features

Installationstyp: Role-based or feature-based Installation

Rolle auswählen: Active Directory Domain Services (AD DS)
→ „Add Features“ bestätigen

Installation starten → „Restart if required“ aktivieren

Optional: Einstellungen exportieren (DeploymentConfigTemplate.xml)

🌐 Server zum Domänencontroller hochstufen

Server Manager → Gelbes Ausrufezeichen →
Promote this server to a domain controller

Neue Gesamtstruktur erstellen:

Root Domain Name: federer.demo


Functional Level: Windows Server 2016

DNS-Server aktivieren

Wiederherstellungspasswort setzen: Riethuesli>12345

Hinweis wegen „Parent Zone“ ignorieren

Speicherorte belassen:

C:\Windows\NTDS
C:\Windows\SYSVOL


Voraussetzungen prüfen → „All checks passed successfully“

Installation starten & Server neu starten

✅ Abschluss

Anmeldung als Domänenadministrator

Funktionsprüfung im Server Manager durchführen

Domänencontroller sichern (Basis für weitere Übungen)

📸 Screenshots (Platzhalter)

👉 Hier kannst du deine Screenshots einfügen, z. B. aus dem Server Manager:

![Server Manager Screenshot](./screenshots/server_manager.png)

✍️ Ergebnis

Ein funktionsfähiger Domänencontroller mit Active Directory unter Windows Server 2019,
konfiguriert mit der Domäne federer.demo,
Hostname ADSERVER-FEDERER,
und IP-Adresse 10.1.230.10.
