📘 Anleitung: Active Directory & DNS – wondertoys.local & work.wondertoys.local
1️⃣ Einrichtung der Stammdomäne (Root Domain)
Vorbereitung

Servername: NYW9DC01

IP-Adresse: 10.0.1.21

Subnetzmaske: 255.255.255.0

Gateway: 10.0.1.1

DNS-Server: 127.0.0.1 (lokal)

Installationsort für AD-Daten: E:\AD

Schritte

Rolle installieren:

Server Manager → Add Roles and Features → Active Directory Domain Services.

Zum DC hochstufen:

Meldung „Post-deployment Configuration“ → Promote this server to a domain controller.

Option: Add a new forest.

Root domain name: wondertoys.local.

Administrator-Passwort setzen.

Pfade: E:\AD wählen.

Installation starten → Neustart.

DNS prüfen:

Eingabeaufforderung:

nslookup wondertoys.local
ping nyw9dc01.wondertoys.local


Beide Abfragen müssen funktionieren.

2️⃣ Einrichtung der Subdomäne (Child Domain)
Vorbereitung

Servername: NYW9DC04

IP-Adresse: 10.0.1.24

Subnetzmaske: 255.255.255.0

Gateway: 10.0.1.1

DNS-Server (vor Installation): 10.0.1.21 (Parent-DC)

Installationsort: E:\AD

Schritte

Rolle installieren:

Server Manager → Add Roles and Features → Active Directory Domain Services.

Zum DC hochstufen:

Promote this server to a domain controller.

Option: Add a new domain to an existing forest.

Domain type: Child Domain.

Parent domain name: wondertoys.local.

New domain name: work.

Credentials: wondertoys.local\Administrator.

Pfade: E:\AD.

Installation starten → Neustart.

DNS-Konfiguration:

Auf NYW9DC04:

Unbedingte Weiterleitung auf 10.0.1.21 (Parent) einrichten.

Nach Installation: DNS-Server-Adresse im Adapter auf 10.0.1.24 (eigene IP) umstellen.

Delegierung (Parent-DC NYW9DC01):

DNS-Manager → Zone wondertoys.local.

Neue Delegierung → Name: work.

Verweis auf NYW9DC04 (10.0.1.24).

Rückwärtsauflösung:

Auf NYW9DC01 neue Primäre Zone für Netz 10.0.1.0/24.

Einträge für NYW9DC01 und NYW9DC04 hinzufügen.

DNS-Server neu starten.

Tests:

nslookup nyw9dc01.wondertoys.local

nslookup nyw9dc04.work.wondertoys.local

Beide müssen mit IP-Adresse antworten.

3️⃣ PowerShell-Übung – IT-Objekte anlegen
Vorbereitung

Arbeiten auf NYW9DC04 (work.wondertoys.local).

Explorer → Dateiendungen anzeigen.

Neue Datei: E:\AD\ITObjektErzeugen.ps1.

PowerShell-Skript
# Globale Gruppe erstellen
New-ADGroup -Name "VerkaufsGruppeGlobal" -GroupScope Global -GroupCategory Security -Path "OU=Users,DC=work,DC=wondertoys,DC=local"

# Lokale Gruppe erstellen
New-ADGroup -Name "ProduktOrdnerLesenGruppeLokal" -GroupScope DomainLocal -GroupCategory Security -Path "OU=Users,DC=work,DC=wondertoys,DC=local"

# Globale Gruppe in lokale Gruppe aufnehmen
Add-ADGroupMember -Identity "ProduktOrdnerLesenGruppeLokal" -Members "VerkaufsGruppeGlobal"

# Benutzer erstellen
New-ADUser -Name "Valentin HEFTI" -GivenName "Valentin" -Surname "HEFTI" -SamAccountName "vhefti" -AccountPassword (ConvertTo-SecureString "Passwort123!" -AsPlainText -Force) -Enabled $true -Path "OU=Users,DC=work,DC=wondertoys,DC=local"

# Benutzer zur lokalen Gruppe hinzufügen
Add-ADGroupMember -Identity "ProduktOrdnerLesenGruppeLokal" -Members "vhefti"

Read-Host "Skript beendet. Drücken Sie Enter zum Schließen"

Ausführung

Rechtsklick → Edit → PowerShell ISE öffnen.

Mit F5 ausführen.

Falls Fehler wegen Policies:

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

Kontrolle

AD Administrative Center oder AD Users and Computers öffnen.

Unter „View“ → Advanced Features aktivieren.

Prüfen, ob:

Gruppe VerkaufsGruppeGlobal existiert.

Gruppe ProduktOrdnerLesenGruppeLokal existiert und die erste Gruppe enthält.

Benutzer Valentin HEFTI existiert und Mitglied in der lokalen Gruppe ist.

