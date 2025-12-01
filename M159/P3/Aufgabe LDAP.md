# Anleitung: LDAP – Active Directory Datenstrukturen verwalten

## Zielsetzung

* Benutzer über LDAP finden
* Distinguished Names bestimmen
* Attribute ändern
* Gruppenmitgliedschaften verwalten
* LDAP-Diagnose durchführen
* Externe LDAP-Tools nutzen

---

## Vorbereitung

* Verbindung zum Domain Controller sicherstellen
* LDAP-Tool (z. B. Softerra LDAP Browser) installieren
* Neustart durchführen

---

## Benutzer suchen & DN bestimmen

Beispiel: Hans Müller

AD-Pfad:
work.wondertoys.local/Intern/Houston/Informatik/Users/Hans Müller

Distinguished Name:

CN=Hans Müller,OU=Users,OU=Informatik,OU=Houston,OU=Intern,DC=work,DC=wondertoys,DC=local

---

## LDAP-Suche durchführen

Beispiel-Suchfilter:

Eva*

Ergebnis:
* zeigt alle Benutzer, deren Name mit „Eva“ beginnt

---

## LDAP-Attribute ändern

Beispiel: Autokennzeichen (carLicense) setzen

Neuer Wert:

SG 123 456

* Attribut bearbeiten
* Speichern
* Im AD kontrollieren (Attribute Editor)

---

## Gruppenmitgliedschaft ändern

Beispiel: Eva soll Mitglied von „Backup Operators“ werden

Vorgehen:

* Benutzerobjekt öffnen
* Tab: Member Of
* Gruppe hinzufügen
* Speichern
* Kontrolle im AD

---

## LDAP-Diagnose überwachen

Pfad:

Computer Management → Performance → Data Collector Sets → System → Active Directory Diagnostics

Bericht abrufen unter:

Reports → System → Active Directory Diagnostics

---

## LDAP von externem Client testen

* Externe VM starten
* LDAP-Browser starten
* Mit Domain Controller verbinden:

ldap://work.wondertoys.local

---

## Kontrolle

* DN korrekt bestimmt
* Attribute erfolgreich geändert
* Gruppenmitgliedschaften korrekt gesetzt
* LDAP-Diagnose erstellt
* Externer LDAP-Zugriff erfolgreich
