# Anleitung: Gruppenrichtlinien (GPO)

## Zielsetzung

1. Ordnerumleitung einrichten
2. Netzwerk-Wartezeit aktivieren
3. Registry sperren
4. Passwortgeschützten Screensaver setzen

---

## 1. Vorbereitung

1. OU-Struktur prüfen
2. Client + DC verbunden
3. GPMC installiert

---

## 2. Ordnerumleitung „Documents“

1. Neues GPO erstellen:
   Ordner 'Documents' wird umgeleitet
2. Pfad öffnen:
   User → Policies → Windows Settings → Folder Redirection → Documents
3. Einstellung:
   Redirect to → Home Folder
4. Verknüpfen mit OU:
   Intern → Houston → Informatik → Users
5. Testen:
   Documents zeigt Home-Folder

---

## 3. GPO: Always Wait for Network

1. Neues GPO:
   Bei Neustart und Anmeldung immer auf Netzwerk warten
2. Pfad:
   Computer → Admin Templates → System → Logon
3. Einstellung:
   Enabled
4. Verknüpfen:
   Domänenroot

---

## 4. Registry sperren & Screensaver

1. Neues GPO:
   Kein Registryzugriff & Kennwortschutz
2. Screensaver-Einstellungen:
   User → Admin Templates → Personalization
   * Timeout: 60 sec
   * Password protect: Enabled
3. Registry-Sperre:
   User → Admin Templates → System
   * Prevent access → Enabled
4. Verknüpfen:
   Intern → Houston → Informatik → Users

---

## 5. Tests

1. Registry blockiert
2. Screensaver startet
3. Passwort nötig
4. gpupdate /force funktioniert

---

## 6. Kontrolle

1. Alle GPOs greifen
2. Keine Konflikte
3. Benutzer-Einstellungen korrekt
