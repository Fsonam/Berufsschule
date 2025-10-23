🧾 Anleitung – Einrichtung einer Subdomäne mit DNS und Active Directory
1️⃣ Ausgangslage

Hauptdomänencontroller (Parent): 10.3.1.21 – NYW9DC01

Neuer Domänencontroller (Child): 10.3.1.24 – NYW9DC04

Subnetz: 10.3.1.0/24, Gateway: 10.3.1.1

Ziel:
Eine Subdomäne „work.wondertoys.local“ wird erstellt und über DNS korrekt mit der Hauptdomäne „wondertoys.local“ verbunden.

2️⃣ Installation und Grundkonfiguration

Neue virtuelle Festplatte (10 GB, SCSI, „Split into multiple files“) hinzufügen.

Windows Server 2019 installieren und alle Updates durchführen.

Computernamen anpassen (z. B. NYW9DC04) und neu starten.

Netzwerkkonfiguration:

IP-Adresse: 10.3.1.24

Subnetzmaske: 255.255.255.0

Gateway: 10.3.1.1

DNS: 10.3.1.21 (Parent-DNS)

Verbindung prüfen:

ipconfig /all

ping 10.3.1.21

3️⃣ Zweite Festplatte einbinden

Im Server Manager → File and Storage Services neuen Storage Pool erstellen.

Zweite Festplatte hinzufügen.

Virtuelle Disk (Layout Simple, Typ Fixed) anlegen.

Neues Volume E: mit NTFS-Dateisystem erstellen.

Ordner E:\AD anlegen.

4️⃣ Active Directory installieren und zum Domänencontroller hochstufen

Active Directory Domain Services installieren.

Bei der Hochstufung wählen: Child Domain.

Domainname: work.wondertoys.local

Funktionsebene: Windows Server 2016

DNS-Server und Global Catalog aktiviert lassen.

Anmeldung mit: WONDERTOYS\Administrator

DSRM-Kennwort festlegen.

Speicherorte:

Datenbank & Logs: E:\AD\NTDS

SYSVOL: E:\AD\SYSVOL

Installation starten, danach Neustart.

5️⃣ DNS auf dem neuen Domänencontroller konfigurieren

Prüfen, ob Parent erreichbar ist:
nslookup nyw9dc01.wondertoys.local

Zone work.wondertoys.local sollte sichtbar sein (AD-integriert, dynamische Updates erlaubt).

Im DNS-Manager unter Forwarders den Parent-DNS (10.3.1.21) eintragen.
→ Damit leitet der neue Server externe Anfragen weiter.

Danach in den Netzwerkeinstellungen den DNS auf 10.3.1.24 (sich selbst) ändern.

6️⃣ DNS-Delegation auf dem Parent-DNS einrichten

Auf NYW9DC01 (Parent) im DNS-Manager die Zone wondertoys.local öffnen.

Neue Delegation erstellen:

Name: work

Nameserver-IP: 10.3.1.24

Prüfen, ob der Delegationseintrag sichtbar ist.

7️⃣ Rückwärtsauflösung konfigurieren

Auf dem Parent eine neue primäre Zone erstellen: 1.3.10.in-addr.arpa

Einträge hinzufügen für:

10.3.1.21 (Parent)

10.3.1.24 (Child)

8️⃣ Überprüfung

Namensauflösung testen:

nslookup nyw9dc01.wondertoys.local
nslookup nyw9dc04.work.wondertoys.local
nslookup work.wondertoys.local


Beide Server gegenseitig anpingen.

FSMO-Rollen prüfen:

netdom query fsmo


→ Alle Rollen bleiben beim Parent-DC.

Test: Zugriff von Benutzern der Parent-Domain auf Ressourcen der Subdomain.

✅ Ergebnis

Stammdomäne: wondertoys.local → DC: NYW9DC01

Subdomäne: work.wondertoys.local → DC: NYW9DC04

DNS-Delegation funktioniert, Namensauflösung geprüft, Gesamtstruktur vollständig integriert.
