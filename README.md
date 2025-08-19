Auftrag M159: 1.1 Demodomäne

Anleitung: Installation eines Active Directory Servers
Ziel

In dieser Übung wird ein Windows Server 2019 aufgesetzt, welcher als Domänencontroller mit Active Directory dient. Am Ende soll ein funktionsfähiger Server mit einer eigenen Demodomäne bereitstehen.

1. Vorbereitung

Virtuelle Maschine starten:
Pfad: C:\VMs\WS4…

Anmeldung mit lokalem Administrator:
Benutzer: Administrator
Passwort: Riethuesli>12345 (bei Bedarf auf ein eigenes komplexes Passwort ändern).

2. Grundkonfiguration des Servers

Tastatur- und Spracheinstellungen prüfen

Falls das Tastaturlayout nicht korrekt ist:
Control Panel → Language → Deutsch (Schweiz) hinzufügen und nach oben verschieben.
Danach VM neu starten.

IP-Adresse und Netzwerk einstellen

Öffne den Server Manager → Local Server

Unter Properties bei „Ethernet“ die IPv4-Einstellungen anpassen:

IP: 10.x.230.10 (x = deine Position im Alphabet)

Subnetzmaske: 255.255.255.0

DNS-Server: 10.x.230.10

Gateway: 10.x.230.1 (vorerst irrelevant)

IPv6 deaktivieren.

Firewall ausschalten

Im Server Manager → „Turn Windows Firewall on or off“ → sowohl private als auch öffentliche Firewall deaktivieren.

Servernamen ändern

Neuer Name: adserver.y.demo (y = dein Nachname).

VM danach neu starten.

3. Installation der AD-Rolle

Server Manager öffnen → Manage → Add Roles and Features

Installationstyp: Role-based or feature-based Installation

Rolle auswählen: Active Directory Domain Services (AD DS)

Bestätigen mit „Add Features“.

Installation starten → „Restart if required“ aktivieren.

Nach Abschluss kann die Konfiguration gespeichert werden (DeploymentConfigTemplate.xml).

4. Server zum Domänencontroller hochstufen

Im Server Manager erscheint ein Hinweis (gelbes Ausrufezeichen).
→ Klick auf „Promote this server to a domain controller“.

Neue Gesamtstruktur erstellen:

Root Domain Name: y.demo (y = Nachname).

Functional Level: Windows Server 2016.

DNS-Server aktivieren.

Wiederherstellungspasswort setzen (z. B. Riethuesli>12345).

Hinweis wegen „Parent Zone“ ignorieren (gewollt).

Speicherorte für AD-Datenbanken auf Standard belassen:

C:\Windows\NTDS

C:\Windows\SYSVOL

Voraussetzungen prüfen → „All checks passed successfully“.
Installation starten und Server neu starten.

5. Abschluss

Anmelden als Domänenadministrator.

Funktionsprüfung im Server Manager durchführen.

Den Domänencontroller sichern – er dient als Grundlage für spätere Übungen.

✅ Ergebnis: Ein lauffähiger Domänencontroller mit Active Directory unter Windows Server 2019, konfiguriert mit eigener Demodomäne.
