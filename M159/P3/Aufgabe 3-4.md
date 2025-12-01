A159 – Computer / Gruppe / Benutzer
Lösungsanleitung (Active Directory)
🎯 Ziel der Aufgabe

Computer in die Domäne integrieren

Benutzer planen & anlegen

Homes & Profiles korrekt einrichten

Rollenbasierte Rechteverwaltung (AGDLP) einsetzen

Standort-Ordner absichern

Memberserver aufnehmen

1. Client in die Domäne integrieren
Schritte

IPv4 konfigurieren (siehe Beispielseite im PDF):

IP: 10.x.1.101

DNS: 10.x.1.24

Computername setzen (z. B. NYX1CL0001)

Neustarten

Domänenbeitritt:

work.wondertoys.local


Auf dem DC prüfen und Computerkonto in korrekte OU verschieben

2. Benutzer erstellen
Benutzer	OU	Logon Name
Hans Müller	Intern/Houston/Informatik/Users	hamue_ho_inf
Eva Muster	Intern/NewYork/Informatik/Users	evmus_ny_inf

Passwortänderung bei erstem Login deaktivieren.

3. Homes & Profiles einrichten
Ordnerstruktur
F:\Shared\Homes
F:\Shared\Profiles

Freigaben

Homes$ → Everyone: Full

Profiles$ → Everyone: Full

NTFS

Homes:

Nur Admins → Full Control

Vererbung deaktivieren

Profiles:

Admins → Full Control

Users → Read & Execute

4. Benutzer konfigurieren
Profile Path
\\Server\Profiles$\%username%

Home Folder

Drive: H:

Path:

\\Server\Homes$\%username%

5. Funktionstest

Anmelden Hans/Eva

Datei in H: ablegen

Hintergrund ändern

Abmelden → wieder anmelden

Änderungen müssen erhalten bleiben

6. Standortordner & AGDLP
Ordnerstruktur
F:\Shared\Sites\Houston
F:\Shared\Sites\NewYork

Gruppenmodell
Globale Gruppen (G)

HO_INF_AllUsers_G

HO_BUC_AllUsers_G

NY_INF_AllUsers_G

NY_BUC_AllUsers_G

Domänenlokale Gruppen (DL)

ACL_HOFolders_Edit_L

ACL_NYFolders_Edit_L

NTFS-Rechte
Ordner	Gruppe	Recht
Houston	ACL_HOFolders_Edit_L	Modify
NewYork	ACL_NYFolders_Edit_L	Modify
7. Memberserver in Domäne aufnehmen

Name setzen (NYW9DB01)

IPv4 definieren

Domänenbeitritt wie beim Client
