
📘 Lernziele – Schriftlicher Teil
1️⃣ Domänenmodell
→ Sie kennen die grafische Darstellung von Domänen und erklären die Eigenschaften (Sicherheitsgrenzen, Schutzschema, DC-Empfehlungen, Replikation, DC, RODC).

Antwort:

Eine Domäne ist die logische Verwaltungseinheit im Active Directory.
Sie enthält Benutzer, Gruppen, Computer und Richtlinien.

Sicherheitsgrenze:

Jede Domäne ist eine eigene Sicherheitsgrenze.

Benutzer und Gruppen können nur innerhalb dieser Domäne direkt verwaltet werden.

Zugriff auf andere Domänen ist nur über Trusts (Vertrauensstellungen) möglich.

Schutzschema:

Jede Domäne kann eigene Administratoren und Richtlinien haben.

Sicherheitsrichtlinien gelten domänenweit.

Schutz vor Fehlkonfigurationen durch Trennung der Verwaltung.

DC-Empfehlungen (Domain Controller):

Mindestens zwei DCs pro Domäne für Ausfallsicherheit.

DCs sollten auf verschiedenen Hosts oder Standorten laufen.

Keine Benutzerarbeiten direkt auf DCs durchführen.

Replikation:

Änderungen im AD werden automatisch zwischen allen DCs repliziert.

Replikation ist mehrstufig und bidirektional.

Verwaltung über den Knowledge Consistency Checker (KCC).

RODC (Read-Only Domain Controller):

Ein DC mit schreibgeschützter AD-Datenbank.

Wird in unsicheren Standorten (z. B. Filialen) eingesetzt.

Nur lokale Authentifizierungsdaten werden gespeichert.

Keine Änderungen am AD möglich → mehr Sicherheit.

2️⃣ Organisational Unit (OU)
→ Welche Strukturierungsmöglichkeiten bietet die OU. Nennen Sie Praxisbeispiele zu den Themen «Abbilden der Firmenstruktur», «Verwaltungstätigkeiten», «Gruppenrichtlinien» und «Sichtbarkeit».

Antwort:

Eine OU (Organizational Unit) dient zur logischen Strukturierung von Objekten innerhalb einer Domäne.

Abbilden der Firmenstruktur:

Beispiel: OU=Zentrale, OU=Filiale, OU=IT, OU=Marketing.

Damit kann man die Unternehmensstruktur direkt im AD darstellen.

Verwaltungstätigkeiten:

Verwaltung kann delegiert werden.

Beispiel: IT-Leiter Zürich darf nur Benutzer in der OU=Zürich verwalten.

Gruppenrichtlinien (GPOs):

GPOs können gezielt auf OUs angewendet werden.

Beispiel: OU=Schule → GPO „USB-Ports deaktivieren“.

Alle Benutzer/Computer in dieser OU erben die Richtlinie.

Sichtbarkeit:

Benutzer oder Administratoren sehen nur die Objekte,
für die sie Berechtigungen haben.

Dadurch wird die Verwaltung übersichtlicher und sicherer.

Wichtig:

Eine OU ist keine Sicherheitsgrenze, sondern eine Verwaltungsgrenze.

3️⃣ Unterschiede zwischen Einzeldomäne, Domänenstruktur (Tree), Gesamtstruktur (Forest) und Mehrgesamtstruktur (Multi-Forest)

Antwort:

Strukturtyp	Beschreibung	Beispiel	Besonderheiten
Einzeldomäne	Eine einzelne Domäne verwaltet alle Objekte.	wondertoys.local	Einfach, zentral, keine Trusts nötig.
Domänenstruktur (Tree)	Mehrere Domänen mit gemeinsamer Namenshierarchie.	wondertoys.local, work.wondertoys.local	Automatische Vertrauensstellung, gemeinsame DNS-Basis.
Gesamtstruktur (Forest)	Sammlung von Domänen mit gemeinsamem AD-Schema und Global Catalog.	wondertoys.local + sales.wondertoys.local	Gemeinsame Richtlinien, aber unterschiedliche Domänen.
Mehrgesamtstruktur (Multi-Forest)	Mehrere unabhängige Forests mit eigenen Schemata.	wondertoys.local + contoso.com	Keine automatische Vertrauensstellung, z. B. bei Firmenfusionen.

Forest = oberste Verwaltungsebene im AD.

Alle Domänen in einem Forest teilen:

Schema, Global Catalog, Trusts und Replikation.

4️⃣ Unterschiedliche Sichtweisen
→ Die Lernenden kennen die Darstellungsform und Aufgaben von der logischen sowie physischen Sicht.

Antwort:

Logische Sicht:

Zeigt die Verwaltungsstruktur des AD.

Enthält Domänen, OUs, Benutzer, Gruppen und Gruppenrichtlinien.

Dient der Organisation und Verwaltung von Ressourcen.

Beispiel:

OU=IT

OU=Finanzen

OU=Schule
→ Jede Abteilung ist logisch abgebildet.

Physische Sicht:

Zeigt die Netzwerktopologie und Replikationsstruktur.

Enthält Sites (Standorte) und Subnetze.

Steuert, wie und wann Daten zwischen DCs repliziert werden.

Beispiel:

Standort Zürich (DC01)

Standort Bern (DC02)

Replikation über WAN.

Unterschied:

Logisch = Organisation & Verwaltung

Physisch = Netzwerk & Replikation

Ziel:

Logische Struktur spiegelt die Firma wider.

Physische Struktur optimiert Datenverkehr und Geschwindigkeit.

✅ Zusammenfassung (Kurzüberblick)
Lernziel	Kurz erklärt
Domänenmodell	Domäne = Sicherheitsgrenze mit eigenen DCs, Replikation und Schutzmechanismen.
OU-Struktur	Verwaltungseinheit für logische Ordnung, Delegation und GPOs.
Tree / Forest / Multi-Forest	Verschiedene Hierarchieebenen und Trust-Beziehungen im AD.
Logisch / Physisch	Logisch = Verwaltung; Physisch = Standort und Replikation.
