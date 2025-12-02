# Anleitung: LDAP – Datenstrukturen verwalten

## Zielsetzung

1. Benutzer via LDAP finden
2. DN bestimmen
3. Attribute ändern
4. Gruppenmitgliedschaften verwalten
5. LDAP-Diagnose ausführen
6. Externen LDAP-Client verwenden

---

## 1. Vorbereitung

1. Softerra LDAP installieren
2. Neustarten
3. Verbindung zum DC sicherstellen

---

## 2. Benutzer suchen & DN bestimmen

1. Benutzer im AD suchen
2. Pfad prüfen:
   work.wondertoys.local/Intern/Houston/Informatik/Users/Hans Müller
3. DN bestimmen:

CN=Hans Müller,OU=Users,OU=Informatik,OU=Houston,OU=Intern,DC=work,DC=wondertoys,DC=local

---

## 3. LDAP-Suche durchführen

1. LDAP-Browser öffnen
2. Suche ausführen:
   Eva*
3. Resultate kontrollieren

---

## 4. Attribute ändern

1. Benutzer öffnen
2. "Show empty attributes" aktivieren
3. Attribut bearbeiten:
   carLicense → SG 123 456
4. Änderungen speichern
5. Im AD überprüfen

---

## 5. Gruppenmitgliedschaften verwalten

1. Benutzer öffnen
2. Tab „Member Of“
3. Gruppe hinzufügen:
   Backup Operators
4. Speichern
5. Kontrolle im AD

---

## 6. LDAP-Diagnose

1. Computer Management öffnen
2. Performance
3. Data Collector Sets → System
4. Active Directory Diagnostics starten
5. Bericht lesen unter:
   Reports/System/Active Directory Diagnostics

---

## 7. Externe LDAP-Verbindung testen

1. VM starten
2. LDAP-Browser öffnen
3. Verbinden mit:
   ldap://work.wondertoys.local

---

## 8. Kontrolle

1. DN korrekt bestimmt
2. Attribute geändert
3. Gruppen korrekt gesetzt
4. Diagnose erstellt
5. Externer LDAP-Zugriff erfolgreich
