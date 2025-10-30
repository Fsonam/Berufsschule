1. Ziel der Aufgabe

In dieser Übung habe ich in der Domäne work.wondertoys.local eine Organisationsstruktur erstellt.
Dazu musste ich:

OUs per PowerShell anlegen (CreateOUs.ps1)

Die gleiche Struktur später mit einer Batch-Datei replizieren (CreateOUs.bat)

Einen Benutzer in einer bestimmten OU erstellen (New-ADUser)

Ziel war es, eine logische Firmenstruktur nachzubauen, sodass Standorte eigene IT-Bereiche verwalten können.

2. Planung & OU-Struktur

Bevor ich mit dem Skript begonnen habe, habe ich die OU-Struktur geplant.

Foto schlussendliche AD Struktur:

![Screenshot](Screenshot Ad Struktur.png)

Die OU Server wurde ebenfalls erstellt, wie in der Aufgabe beschrieben.

📎 Hier Screenshot einfügen: OU-Plan (aus PDF oder selbst gezeichnet)

3. PowerShell Vorbereitung

Bevor ich das Skript ausgeführt habe, musste ich sicherstellen, dass das AD-Modul geladen ist:

Import-Module ActiveDirectory


Dann habe ich geprüft, ob ich in der richtigen Domäne bin:

Get-ADDomain


📎 Screenshot: Ausgabe von Get-ADDomain

4. PowerShell-Skript CreateOUs.ps1 ausführen

Ich habe das Skript im PowerShell-ISE/VS-Code erstellt und gespeichert.

Um das Skript zu starten:

.\CreateOUs.ps1


📎 Screenshot: Skript im Editor öffnen
📎 Screenshot: Skript-Ausführung in PowerShell

Nach der Ausführung habe ich kontrolliert, ob die OUs im AD sichtbar sind.

📎 Screenshot: OU-Struktur im AD „Active Directory-Benutzer und -Computer“

5. Benutzerkonto erstellen

Dann habe ich einen Benutzer in folgender OU erstellt:

Intern → NewYork → Informatik → Groups → Security

Beispiel-Befehl:

New-ADUser -Name "Peter Muster" `
 -GivenName "Peter" `
 -Surname "Muster" `
 -SamAccountName "PeterMuster" `
 -UserPrincipalName "PeterMuster@work.wondertoys.local" `
 -Path "OU=Security,OU=Groups,OU=Informatik,OU=NewYork,OU=Intern,DC=work,DC=wondertoys,DC=local" `
 -AccountPassword (ConvertTo-SecureString "Passwort123!" -AsPlainText -Force) `
 -Enabled $true


📎 Screenshot: Benutzer im AD sichtbar

6. Ergebnis

Ich habe erfolgreich:

Die OU-Struktur in work.wondertoys.local erstellt

Die Struktur automatisiert per PowerShell erstellt

Einen Benutzer in die korrekte OU angelegt

Damit ist die Aufgabe abgeschlossen.
