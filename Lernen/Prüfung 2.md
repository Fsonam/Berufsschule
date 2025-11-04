# 📘 Lernziele – Schriftlicher Teil: Active Directory & DNS

## 🧩 1. Domänenmodell

### 🎯 Lernziel
> Sie kennen die grafische Darstellung von Domänen und erklären die Eigenschaften (Sicherheitsgrenzen, Schutzschema, DC-Empfehlungen, Replikation, DC, RODC).

### 🧠 Erklärung
Eine **Domäne** ist die **logische Verwaltungseinheit** im Active Directory.  
Sie enthält Benutzer, Gruppen, Computer und Richtlinien.

### 🔐 Sicherheitsgrenzen
- Eine Domäne bildet eine **Sicherheitsgrenze**.  
- Benutzer und Gruppen sind **nur innerhalb dieser Domäne** gültig.  
- Zugriff auf andere Domänen nur über **Trusts (Vertrauensstellungen)** möglich.  
- Passwortrichtlinien und Sicherheitsrichtlinien gelten domänenweit.

### 🧱 Schutzschema
- Jede Domäne kann eigene **Administratoren und Richtlinien** haben.  
- Trennung von Verantwortlichkeiten schützt vor Fehlkonfigurationen.  
- Sicherheitskontext gilt nur innerhalb der jeweiligen Domäne.

### 🖥️ Domain Controller (DC) Empfehlungen
- **Mindestens zwei DCs** pro Domäne (Redundanz).  
- DCs auf **verschiedenen Hosts oder Standorten** betreiben.  
- Keine Alltagsarbeiten direkt auf DCs (nur Administration).

### 🔁 Replikation
- Änderungen (z. B. neue Benutzer) werden **automatisch zwischen DCs repliziert**.  
- Replikation ist **bidirektional** und wird über den **KCC (Knowledge Consistency Checker)** gesteuert.  
- AD-Datenbank bleibt auf allen DCs synchron.

### 🧩 RODC (Read-Only Domain Controller)
- **Schreibgeschützte Kopie** der AD-Datenbank.  
- Einsatz in **unsicheren oder entfernten Standorten** (z. B. Filialen).  
- Vorteile:
  - Keine Änderungen lokal möglich  
  - Höhere Sicherheit bei Diebstahl  
  - Weniger Replikationslast

---

## 🧩 2. Organizational Unit (OU)

### 🎯 Lernziel
> Welche Strukturierungsmöglichkeiten bietet die OU.  
> Nennen Sie Praxisbeispiele zu den Themen «Abbilden der Firmenstruktur», «Verwaltungstätigkeiten», «Gruppenrichtlinien» und «Sichtbarkeit».

### 🧠 Erklärung
Eine **OU (Organizational Unit)** dient zur **logischen Strukturierung von AD-Objekten** innerhalb einer Domäne.

### 🏢 Abbilden der Firmenstruktur
Beispielhafte OU-Struktur:

➡️ Jede Abteilung oder jeder Standort kann separat verwaltet werden.

### ⚙️ Verwaltungstätigkeiten
- Verwaltung kann **delegiert** werden.  
- Beispiel: IT-Leiter Zürich darf nur Benutzer in `OU=Zürich` verwalten.  
- Delegation erlaubt **feingranulare Rechtevergabe**.

### 🧭 Gruppenrichtlinien (GPOs)
- **GPOs** können direkt auf OUs angewendet werden.  
- Beispiel:  
  - `OU=Schule` → GPO: „USB-Ports deaktivieren“.  
- Alle Benutzer/Computer in der OU erben die Richtlinie.

### 👁️ Sichtbarkeit
- Benutzer und Admins sehen nur Objekte, für die sie Berechtigungen besitzen.  
- Erhöht Übersichtlichkeit und Sicherheit.

### ⚠️ Wichtig
> Eine **OU ist keine Sicherheitsgrenze**, sondern eine **Verwaltungsgrenze**!

---

## 🧩 3. Strukturtypen von Active Directory

### 🎯 Lernziel
> Sie kennen die Unterschiede zwischen Einzeldomäne, Domänenstruktur (Tree), Gesamtstruktur (Forest) und Mehrgesamtstruktur (Multi-Forest).

### 🧱 Vergleichstabelle

| Strukturtyp | Beschreibung | Beispiel | Besonderheiten |
|--------------|--------------|-----------|----------------|
| **Einzeldomäne** | Eine einzige Domäne verwaltet alle Objekte. | `wondertoys.local` | Einfachste Struktur, zentral verwaltet. |
| **Domänenstruktur (Tree)** | Mehrere Domänen mit gemeinsamer Namenshierarchie. | `wondertoys.local`, `work.wondertoys.local` | Gemeinsame DNS-Basis, automatische Trusts. |
| **Gesamtstruktur (Forest)** | Sammlung von Domänen mit gemeinsamer AD-Datenbank, Schema & Global Catalog. | `wondertoys.local` + `sales.wondertoys.local` | Gemeinsame Richtlinien, geteilte Ressourcen. |
| **Mehrgesamtstruktur (Multi-Forest)** | Mehrere unabhängige Forests. | `wondertoys.local` + `contoso.com` | Keine automatische Vertrauensstellung; bei Fusionen üblich. |

### 💡 Merksatz
> **Tree = gemeinsame DNS-Hierarchie**  
> **Forest = gemeinsame AD-Datenbank und Schema**  
> **Multi-Forest = komplett getrennte Systeme**

---

## 🧩 4. Unterschiedliche Sichtweisen

### 🎯 Lernziel
> Die Lernenden kennen die Darstellungsform und Aufgaben von der logischen sowie physischen Sicht.

### 🧠 Erklärung

| Sichtweise | Beschreibung | Typische Elemente | Zweck |
|-------------|--------------|------------------|--------|
| **Logische Sicht** | Zeigt die **Verwaltungsstruktur** des AD. | Domänen, OUs, Benutzer, Gruppen, GPOs | Dient der Organisation und Verwaltung. |
| **Physische Sicht** | Zeigt die **Netzwerktopologie und Replikation**. | Standorte (Sites), Subnetze, Replikationsverbindungen | Optimiert Datenverkehr und Replikationswege. |

### 🏗️ Beispiele

**Logische Sicht:**
→ Spiegelt die **Organisationsstruktur** wider.

**Physische Sicht:**
→ Replikation über WAN, zeigt **tatsächliche Netzwerkstruktur**.

### 🔍 Unterschied

| Vergleich | Logische Sicht | Physische Sicht |
|------------|----------------|----------------|
| **Fokus** | Verwaltung | Netzwerk & Replikation |
| **Zweck** | Strukturierung von Objekten | Optimierung von Datenverkehr |
| **Beispiel** | OU=Marketing | Standort=Zürich |

### ✅ Ziel
> Logische Struktur = Abbildung der Organisation  
> Physische Struktur = Optimierung der Replikation im Netzwerk

---

## 🧾 Zusammenfassung

| Thema | Kurz erklärt |
|--------|---------------|
| **Domäne** | Sicherheitsgrenze im AD, gemeinsame Richtlinien & Authentifizierung. |
| **OU** | Verwaltungseinheit zur logischen Strukturierung, Delegation & GPO-Steuerung. |
| **Tree / Forest / Multi-Forest** | Verschiedene Hierarchieebenen und Vertrauensbeziehungen. |
| **Logisch / Physisch** | Logisch = Organisation, Physisch = Netzwerk & Replikation. |

---

💡 **Tipp zum Lernen:**  
Lies jedes Lernziel laut vor und erkläre es in deinen eigenen Worten –  
so merkst du dir die Begriffe schneller und verstehst die Zusammenhänge wirklich.
