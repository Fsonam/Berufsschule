# Anleitung: Computer, Benutzer, Gruppen & Standort-Ressourcen (work.wondertoys.local)

## Zielsetzung

Einrichten einer funktionsfähigen Domänenumgebung:

* Integration eines Clients und eines Memberservers
* Anlegen von Benutzern gemäss Namenskonzept
* Einrichten von Homes und Profiles
* Umsetzung eines rollenbasierten Rechtekonzepts (AGDLP)
* Standortbezogene Ressourcenverzeichnisse
* Testen und Dokumentieren der Resultate

---

## IP-Konzept

Beispielzuteilung (gemäss Übung):

| Gerät       | Rolle           | IP          | Subnetz         | Gateway   | DNS        |
|-------------|-----------------|-------------|------------------|-----------|------------|
| NYX1CL0001  | Client New York | 10.x.1.101  | 255.255.255.0    | 10.x.1.1  | 10.x.1.24  |
| NYW9DB01    | Memberserver    | 10.x.1.41   | 255.255.255.0    | 10.x.1.1  | 10.x.1.24  |

---

## Namenskonzept

6-stellige Rechnernamen:

* NYX1CL0001 → Client New York
* NYW9DB01 → Memberserver

Benutzernamen:

* Hans Müller → hamue_ho_inf
* Eva Muster → evmus_ny_inf

---

## Vorbereitung

* IPv4 konfigurieren
* DNS eintragen
* Rechnername setzen
* Netzwerk prüfen (ping)
* OU-Struktur vorbereiten

---

## NYX1CL0001 – Client in Domäne aufnehmen

* IP konfigurieren
* Rechnername setzen
* Neustart
* Domänenbeitritt:

work.wondertoys.local

Danach:

* Client erscheint unter "Computers"
* In die passende OU verschieben

---

## Benutzer erstellen

| Benutzer     | OU                                   | Logon-Name     |
|--------------|----------------------------------------|----------------|
| Hans Müller  | Intern/Houston/Informatik/Users        | hamue_ho_inf   |
| Eva Muster   | Intern/NewYork/Informatik/Users        | evmus_ny_inf   |

* Passwort setzen
* Passwortwechsel deaktivieren

---

## Serverordner für Homes & Profiles

### Ordnerstruktur:

F:\Shared\Homes  
F:\Shared\Profiles

### Freigaben:

| Ordner   | Share     | Rechte          |
|----------|-----------|------------------|
| Homes    | Homes$    | Everyone: Full   |
| Profiles | Profiles$ | Everyone: Full   |

### NTFS-Rechte:

Homes:
* Administratoren → Full Control
* Vererbung deaktivieren

Profiles:
* Administratoren → Full Control
* Users → Read & Execute

---

## Benutzer konfigurieren

Home Folder:
H: → \\Server\Homes$\%username%

Roaming Profile:
\\Server\Profiles$\%username%

Der Home-Ordner wird beim Speichern erstellt.  
Der Profilordner wird beim ersten Login erzeugt.

---

## Tests (NYX1CL0001)

* Datei auf H:\ speichern
* Hintergrund ändern
* Desktop-Symbole ändern
* Abmelden → Anmelden
* Einstellungen müssen übernommen werden

---

## Standortverzeichnisse

Ordnerstruktur:

F:\Shared\Sites\Houston  
F:\Shared\Sites\NewYork

### Globale Gruppen (G):

* HO_INF_AllUsers_G
* HO_BUC_AllUsers_G
* NY_INF_AllUsers_G
* NY_BUC_AllUsers_G

### Domänenlokale Gruppen (DL):

* ACL_HOFolders_Edit_L
* ACL_NYFolders_Edit_L

### NTFS-Rechte:

| Ordner  | Gruppe                | Recht  |
|---------|------------------------|--------|
| Houston | ACL_HOFolders_Edit_L  | Modify |
| New York | ACL_NYFolders_Edit_L | Modify |

---

## Memberserver einbinden (NYW9DB01)

* Firewall deaktivieren
* IPv4 setzen
* Name setzen: NYW9DB01
* Neustart
* Domänenbeitritt:

work.wondertoys.local

---

## Kontrolle

* Benutzerprofile laden korrekt
* H:\ und S:\ (Standortlaufwerk) sichtbar
* Kein Zugriff auf andere Standorte
* Memberserver sichtbar im AD
* Homes & Profiles funktionieren
