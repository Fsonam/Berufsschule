Anleitung Aufgabe 3.4

Mittwoch, 5. November 2025
12:20

Auftrag 3.4 – Active Directory Forest mit it.intra, abc.intra, west.abc.intra

1. Zielsetzung
Ziel ist die Einrichtung eines Active Directory-Forests mit drei Domänen:
	• Stammdomäne (Root): it.intra
	• Tree-Domäne: abc.intra
	• Untergeordnete Domäne (Child): west.abc.intra
Alle DNS-Zonen sind Active Directory-integriert, erlauben nur sichere dynamische Updates und ermöglichen vollständige Forward- und Reverse-Auflösung.

2. IP-Konzept (nach Vorgabe)
1.–3. Byte → Standort
1.–3. Byte	Kürzel	Land
10.25.0	C	Schweiz
10.25.1	I	Italien
10.25.2	K	Kroatien
10.25.3	H	Ungarn
10.25.4	S	Slowenien
10.25.5	T	Tschechien
10.25.6	A	Österreich

4. Byte → Gerätetyp
4. Byte	Bedeutung
1–30	Netzwerkgeräte und Drucker
31–40	Domain Controller
41–250	Memberserver und Clients
251–254	Reserve

Zuteilung für die drei Server
Server	Rolle	IP-Adresse	Subnetz	Gateway	DNS
WS4	DC it.intra (Root)	10.25.0.31	255.255.255.0	10.25.0.1	127.0.0.1
WS5	DC abc.intra (Tree)	10.25.0.32	255.255.255.0	10.25.0.1	10.25.0.31
WS6	Member west.abc.intra (Child)	10.25.0.41	255.255.255.0	10.25.0.1	10.25.0.32

3. Namenskonzept
Jeder Rechnername besteht aus 6 Zeichen.
Jede Position hat eine definierte Bedeutung.
Position	Bedeutung	Beschreibung
1	Land	Landkürzel (C, I, K, H, S, T, A)
2	Betriebssystem	Wenn D/M: 6=2012, 7=2012R2, 8=2018, 9=2019
		Wenn C: 1=Win10, 7=Win7, 8=Win8, 9=Win8.1
3	Gerätetyp	D=Domain Controller, M=Memberserver, C=Client
4–6	Nummer	001–999 (laufende Nummer pro Gerätetyp)

Beispiele
Rechner	Name	Bedeutung	Vollständiger FQDN
WS4	C9D001	Schweiz, Server 2019, DC #1	C9D001.it.intra
WS5	C9D002	Schweiz, Server 2019, DC #2	C9D002.abc.intra
WS6	C9M001	Schweiz, Server 2019, Member #1	C9M001.west.abc.intra

4. Vorbereitung
	1. IP-Adressen gemäß IP-Konzept eintragen.
	2. DNS-Einstellungen:
		○ WS4 → 127.0.0.1
		○ WS5 → 10.25.0.31
		○ WS6 → 10.25.0.32
	3. Servernamen nach Namensschema vergeben (z. B. C9D001).
	4. Laufwerk E: erstellen → Ordner E:\ADS.
	5. Netzwerkverbindungen zwischen allen Systemen testen (Ping).

5. Installation WS4 – Root Domain (it.intra)
	1. Rolle Active Directory Domain Services installieren.
	2. Nach der Installation: „Promote this server to a domain controller“.
	3. Option: Add a new forest.
	4. Root domain name: it.intra.
	5. Pfade: Database, Logs, SYSVOL → E:\ADS.
	6. Directory Services Restore Mode Password: Riethuesli>12345.
	7. Installation starten → Neustart.
	8. Anmeldung: it\Administrator.
	9. Test:

nslookup it.intra
ping c9d001.it.intra

6. Installation WS5 – Tree Domain (abc.intra)
	1. DNS-Adresse: 10.25.0.31.
	2. Rolle AD DS installieren.
	3. Promote to domain controller → Add a new domain to an existing forest.
	4. Domain type: Tree Domain.
	5. Parent Domain: it.intra.
	6. New Domain: abc.intra.
	7. Credentials: it\Administrator.
	8. Pfade: E:\ADS.
	9. Directory Services Restore Mode Password: Riethuesli>12345.
	10. Installation starten → Neustart.
	11. Anmeldung: abc\Administrator.
	12. Test:

nslookup abc.intra
ping c9d002.abc.intra

7. DNS – Conditional Forwarder
Auf WS4 (it.intra):
	• DNS Manager → Conditional Forwarders → New.
	• DNS Domain: abc.intra.
	• IP-Adresse: 10.25.0.32.
	• „Store this conditional forwarder in Active Directory“ aktivieren.
	• OK.
Auf WS5 (abc.intra):
	• DNS Manager → Conditional Forwarders → New.
	• DNS Domain: it.intra.
	• IP-Adresse: 10.25.0.31.
	• „Store this conditional forwarder in Active Directory“ aktivieren.
	• OK.

8. Installation WS6 – Child Domain (west.abc.intra)
	1. DNS-Adresse: 10.25.0.32.
	2. Rolle AD DS installieren.
	3. Promote this server to a domain controller.
	4. Option: Add a new domain to an existing forest.
	5. Domain type: Child Domain.
	6. Parent Domain: abc.intra.
	7. New Domain Name: west.
	8. Credentials: abc\Administrator.
	9. Pfade: E:\ADS.
	10. Password: Riethuesli>12345.
	11. Installation starten → Neustart.
	12. Anmeldung: west\Administrator.
	13. Test:

nslookup west.abc.intra
ping c9m001.west.abc.intra

9. Reverse Lookup Zone
Auf WS4 (Root-DC):
	1. DNS Manager → New Zone → Primary Zone → AD-integrated → Secure updates only.
	2. Zone Name: 0.25.10.in-addr.arpa.
	3. PTR-Einträge für WS4, WS5, WS6 prüfen oder hinzufügen.


10. PowerShell-Objekte (auf WS5 – abc.intra)
PowerShell ISE → Datei speichern als E:\ADS\AD_Objekte.ps1


New-ADOrganizationalUnit -Name "Vertrieb"
New-ADOrganizationalUnit -Name "Schweiz"

New-ADGroup -Name "Direktvertrieb" -GroupScope Global -Path "OU=Vertrieb,DC=abc,DC=intra"
New-ADUser -Name "MeierA" -AccountPassword (ConvertTo-SecureString "Riethuesli>12345" -AsPlainText -Force) -Enabled $true -Path "OU=Vertrieb,DC=abc,DC=intra"
Add-ADGroupMember -Identity "Direktvertrieb" -Members "MeierA"

New-ADGroup -Name "KatalogLesen" -GroupScope DomainLocal -Path "OU=Schweiz,DC=abc,DC=intra"
Add-ADGroupMember -Identity "KatalogLesen" -Members "Direktvertrieb"


Falls PowerShell blockiert:

Set-ExecutionPolicy RemoteSigned -Scope CurrentUser


11. Tests
DNS-Auflösung:

nslookup c9d001.it.intra
nslookup c9d002.abc.intra
nslookup c9m001.west.abc.intra
Replikation:

repadmin /syncall > E:\ADS\repadmin.txt
Diagnose:

dcdiag > E:\ADS\dcdiag.txt
Netzwerk:

ipconfig /all > E:\ADS\ipconfig.txt

12. Kontrolle
In Active Directory:
	• OU „Vertrieb“ und „Schweiz“ vorhanden
	• Benutzer MeierA aktiv
	• Gruppe Direktvertrieb ist Mitglied in KatalogLesen

13. Dokumentation
In E:\ADS auf jedem DC:
	• repadmin.txt
	• dcdiag.txt
	• ipconfig.txt
	• Screenshots:
		○ DNS-Manager (Forwarders, Zonen)
		○ AD-Struktur (OUs, Gruppen, Benutzer)
		○ PowerShell-Ausführung (Skript erfolgreich)

14. Endzustand
	• Forest besteht aus it.intra, abc.intra und west.abc.intra
	• DNS-Auflösung (Forward & Reverse) funktioniert
	• Replikation erfolgreich
	• OU-, Gruppen- und Benutzerstruktur vollständig
	• Netz- und Namenskonzept korrekt umgesetzt


