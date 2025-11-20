# Anleitung: Active Directory Forest (it.intra, abc.intra, west.abc.intra)

## Zielsetzung

Einrichten eines AD-Forests mit:

* **Root-Domain:** it.intra
* **Tree-Domain:** abc.intra
* **Child-Domain:** west.abc.intra

DNS-Zonen: AD-integriert, sichere dynamische Updates, Forward/Reverse-Auflösung.

---

## IP-Konzept

**Standort (1.–3. Byte):**

* 10.25.0 = Schweiz (C)

**Gerätetyp (4. Byte):**

* 31–40 = Domain Controller
* 41–250 = Member & Clients

### Server-IP-Zuteilung

| Server | Rolle                 | IP         | Subnetz       | Gateway   | DNS        |
| ------ | --------------------- | ---------- | ------------- | --------- | ---------- |
| WS4    | DC it.intra           | 10.25.0.31 | 255.255.255.0 | 10.25.0.1 | 127.0.0.1  |
| WS5    | DC abc.intra          | 10.25.0.32 | 255.255.255.0 | 10.25.0.1 | 10.25.0.31 |
| WS6    | Member west.abc.intra | 10.25.0.41 | 255.255.255.0 | 10.25.0.1 | 10.25.0.32 |

---

## Namenskonzept

6-stellige Rechnernamen:

1. Land (C/I/K/H/S/T/A)
2. OS-Version (Server 2019 = 9)
3. Gerätetyp (D/M/C)
   4–6: Nummer

**Beispiele:**

* C9D001 → C9D001.it.intra
* C9D002 → C9D002.abc.intra
* C9M001 → C9M001.west.abc.intra

---

## Vorbereitung

* IPs konfigurieren
* DNS einstellen
* Servername setzen
* Laufwerk **E:\ADS** erstellen
* Netzwerk testen (Ping)

---

## WS4 – Root Domain (it.intra)

* AD DS installieren
* Promote → **Add new forest** → it.intra
* Pfade: **E:\ADS**
* DSRM-Passwort setzen
* Neustart
* Anmeldung: `it\Administrator`

**Tests:**

```
nslookup it.intra
ping c9d001.it.intra
```

---

## WS5 – Tree Domain (abc.intra)

* DNS: 10.25.0.31
* AD DS installieren
* Promote → **Add new domain (Tree)**
* Parent: it.intra
* Domain: abc.intra
* Pfade: E:\ADS
* Neustart
* Anmeldung: `abc\Administrator`

**Tests:**

```
nslookup abc.intra
ping c9d002.abc.intra
```

---

## DNS – Conditional Forwarder

### Auf WS4 (it.intra):

* Forwarder: abc.intra → 10.25.0.32

### Auf WS5 (abc.intra):

* Forwarder: it.intra → 10.25.0.31

Beide: „Store in Active Directory“ aktivieren.

---

## WS6 – Child Domain (west.abc.intra)

* DNS: 10.25.0.32
* AD DS installieren
* Promote → **Add new domain (Child)**
* Parent: abc.intra
* Domain Name: west
* Neustart
* Anmeldung: `west\Administrator`

**Tests:**

```
nslookup west.abc.intra
ping c9m001.west.abc.intra
```

---

## Reverse Lookup Zone (WS4)

* Zone erstellen: **0.25.10.in-addr.arpa**
* AD-integriert, nur sichere Updates
* PTR-Einträge für WS4, WS5, WS6 prüfen

---

## PowerShell-Objekte (WS5 – abc.intra)

Speichern unter: **E:\ADS\AD_Objekte.ps1**

```powershell
New-ADOrganizationalUnit -Name "Vertrieb"
New-ADOrganizationalUnit -Name "Schweiz"

New-ADGroup -Name "Direktvertrieb" -GroupScope Global -Path "OU=Vertrieb,DC=abc,DC=intra"

New-ADUser -Name "MeierA" -AccountPassword (ConvertTo-SecureString "Riethuesli>12345" -AsPlainText -Force) -Enabled $true -Path "OU=Vertrieb,DC=abc,DC=intra"

Add-ADGroupMember -Identity "Direktvertrieb" -Members "MeierA"

New-ADGroup -Name "KatalogLesen" -GroupScope DomainLocal -Path "OU=Schweiz,DC=abc,DC=intra"
Add-ADGroupMember -Identity "KatalogLesen" -Members "Direktvertrieb"
```

Falls blockiert:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

## Tests

**DNS:**

```
nslookup c9d001.it.intra
nslookup c9d002.abc.intra
nslookup c9m001.west.abc.intra
```

**Replikation:**

```
repadmin /syncall > E:\ADS\repadmin.txt
```

**Diagnose:**

```
dcdiag > E:\ADS\dcdiag.txt
```

**Netz:**

```
ipconfig /all > E:\ADS\ipconfig.txt
```

---

## Kontrolle

* OU „Vertrieb“ und „Schweiz“ vorhanden
* Benutzer **MeierA** aktiv
* Gruppe **Direktvertrieb** Mitglied in **KatalogLesen**
* DNS & Reverselookup funktionsfähig
* Replikation erfolgreich

---

## Endzustand

* Forest mit **it.intra**, **abc.intra**, **west.abc.intra** vollständig
* DNS korrekt eingerichtet
* AD-Struktur (OUs, Gruppen, Benutzer) vollständig
* Dokumentation in E:\ADS auf jedem DC
