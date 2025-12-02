# Anleitung: Computer, Benutzer, Gruppen & Standort-Ressourcen (work.wondertoys.local)

## Zielsetzung

1. Integration eines Clients und eines Memberservers
2. Anlegen von Benutzern gemäss Namenskonzept
3. Einrichten von Homes und Profiles
4. Umsetzung eines rollenbasierten Rechtekonzepts (AGDLP)
5. Standortbezogene Ressourcenverzeichnisse einrichten
6. Tests und Kontrolle

---

## 1. IP-Konzept

Beispielzuteilung (gemäss Übung):

| Gerät       | Rolle           | IP          | Subnetz         | Gateway   | DNS        |
|-------------|-----------------|-------------|------------------|-----------|------------|
| NYX1CL0001  | Client New York | 10.x.1.101  | 255.255.255.0    | 10.x.1.1  | 10.x.1.24  |
| NYW9DB01    | Memberserver    | 10.x.1.41   | 255.255.255.0    | 10.x.1.1  | 10.x.1.24  |

---

## 2. Namenskonzept

1. NYX1CL0001 → Client New York
2. NYW9DB01 → Memberserver
3. Benutzer:
   * Hans Müller → hamue_ho_inf
   * Eva Muster → evmus_ny_inf

---

## 3. Vorbereitung

1. IPv4 konfigurieren
2. DNS eintragen
3. Rechnername setzen
4. Netzwerk prüfen (ping)
5. OU-Struktur vorbereiten

---

## 4. Client NYX1CL0001 in Domäne aufnehmen

1. IP konfigurieren
2. Rechnername setzen
3. Neustarten
4. Domänenbeitritt ausführen:
   work.wondertoys.local
5. Client erscheint unter "Computers"
6. In korrekte OU verschieben

---

## 5. Benutzer erstellen

| Nr. | Benutzer     | OU                                   | Logon-Name     |
|-----|--------------|----------------------------------------|----------------|
| 1   | Hans Müller  | Intern/Houston/Informatik/Users        | hamue_ho_inf   |
| 2   | Eva Muster   | Intern/NewYork/Informatik/Users        | evmus_ny_inf   |

1. Passwort setzen
2. Passwortwechsel bei Login deaktivieren

---

## 6. Homes & Profiles einrichten

### 6.1 Ordnerstruktur

1. F:\Shared\Homes erstellen
2. F:\Shared\Profiles erstellen

### 6.2 Freigaben

| Ordner   | Share     | Rechte          |
|----------|-----------|------------------|
| Homes    | Homes$    | Everyone: Full   |
| Profiles | Profiles$ | Everyone: Full   |

### 6.3 NTFS-Rechte

**Homes:**
1. Administratoren → Full Control
2. Vererbung deaktivieren

**Profiles:**
1. Administratoren → Full Control
2. Users → Read & Execute

### 6.4 Benutzer konfigurieren

1. Home-Drive:
   H: → \\Server\Homes$\%username%
2. Profilpfad:
   \\Server\Profiles$\%username%

---

## 7. Tests (NYX1CL0001)

1. Datei in H:\ speichern
2. Hintergrund ändern
3. Desktop-Symbole ändern
4. Abmelden → anmelden
5. Prüfen, ob Einstellungen übernommen wurden

---

## 8. Standortverzeichnisse (Houston / New York)

### 8.1 Ordnerstruktur

1. F:\Shared\Sites\Houston
2. F:\Shared\Sites\NewYork

### 8.2 Globale Gruppen (G)

1. HO_INF_AllUsers_G
2. HO_BUC_AllUsers_G
3. NY_INF_AllUsers_G
4. NY_BUC_AllUsers_G

### 8.3 Domänenlokale Gruppen (DL)

1. ACL_HOFolders_Edit_L
2. ACL_NYFolders_Edit_L

### 8.4 NTFS-Rechte

| Ordner  | Gruppe                | Recht  |
|---------|------------------------|--------|
| Houston | ACL_HOFolders_Edit_L  | Modify |
| New York | ACL_NYFolders_Edit_L | Modify |

---

## 9. Memberserver NYW9DB01 in Domäne aufnehmen

1. Firewall deaktivieren
2. IPv4 setzen
3. Rechnername setzen: NYW9DB01
4. Neustart
5. Domänenbeitritt:
   work.wondertoys.local

---

## 10. Kontrolle

1. Benutzerprofile laden korrekt
2. H:\ sichtbar
3. S:\ (Standortlaufwerk) sichtbar
4. Kein Zugriff auf andere Standorte
5. Memberserver sichtbar im AD
6. Homes & Profiles funktionieren
