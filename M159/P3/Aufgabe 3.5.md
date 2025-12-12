# D159 – Active Directory Standorte (Sites) mit Replikation

## Ziel der Aufgabe
Die bestehende Active-Directory-Domäne **work.wondertoys.local** wird von einem Standort auf **zwei Standorte (NewYork und Houston)** erweitert.  
Dabei werden:
- zwei Domänencontroller auf unterschiedliche Standorte verteilt
- Routing zwischen zwei Subnetzen eingerichtet
- die Active-Directory-Replikation optimiert, zeitgesteuert und getestet

---

## Ausgangslage

### Systeme
| Server | Rolle |
|------|------|
| WS4 | Domänencontroller (NewYork) |
| WS5 | Domänencontroller (Houston) |
| WS6 | Router / Memberserver (RT6) |

### IP-Adressierung
| Standort | Subnetz |
|--------|--------|
| NewYork | 10.0.0.0/24 |
| Houston | 10.0.1.0/24 |
| Gesamtnetz | 10.0.0.0/16 |

---

## Schritt 1: Router (WS6 / RT6) einrichten

### 1.1 WS6 zur Domäne hinzufügen
1. WS6 starten und als **lokaler Administrator** anmelden
2. **Server Manager → Local Server**
   - IPv6 deaktivieren
   - IPv4 konfigurieren:
     - IP-Adresse: `10.0.0.41/24`
     - DNS-Server: `10.0.0.24`
   - Firewall deaktivieren
3. Computer umbenennen in **RT6**
4. Zur Domäne **work.wondertoys.local** hinzufügen
5. Neustart
6. Anmeldung als `work\Administrator`

---

### 1.2 Zweite Netzwerkkarte hinzufügen
1. VMware Player → **VM Settings**
2. **Add → Network Adapter → Host-Only**
3. In Windows beide Netzwerkkarten identifizieren
4. Umbenennen:
   - `NIC 1 NY`
   - `NIC 2 HO`

---

### 1.3 IP-Konfiguration der NICs
**NIC 1 NY**
- IP: `10.0.0.1/24`
- IPv6 deaktiviert
- Gateway & DNS leer

**NIC 2 HO**
- IP: `10.0.1.1/24`
- IPv6 deaktiviert
- Gateway & DNS leer

---

### 1.4 Routing-Rolle installieren
1. **Server Manager → Add Roles and Features**
2. Rolle: `Remote Access`
3. Role Service: `Routing`
4. Installation abschliessen
5. Tools → **Routing and Remote Access**
6. Rechtsklick auf RT6 → *Configure and Enable*
7. **Custom Configuration**
8. **LAN Routing**
9. Dienst starten

---

### 1.5 Statische Routen konfigurieren
Routing and Remote Access → IPv4 → Static Routes:

| Zielnetz | Interface |
|--------|----------|
| 10.0.0.0/24 | NIC 1 NY |
| 10.0.1.0/24 | NIC 2 HO |

Danach den RAS-Dienst neu starten.

---

## Schritt 2: WS5 ins Subnetz Houston verschieben

### Netzwerkkonfiguration WS5
- IP-Adresse: `10.0.1.25/24`
- Default Gateway: `10.0.1.1`
- DNS-Server: `10.0.1.25`

### Verbindung testen
```bash
ping 10.0.1.1
ping 10.0.0.24


## Schritt 3: AD-Standorte und Subnetze erstellen

### Auf WS4
- Anmeldung als `wondertoys.local\Administrator`
- **Active Directory Sites and Services** öffnen

### Neue Standorte erstellen
- `NewYork`
- `Houston`

### Subnetze anlegen und zuweisen

| Subnetz       | Standort |
|--------------|----------|
| 10.0.0.0/24  | NewYork  |
| 10.0.1.0/24  | Houston  |

---

## Schritt 4: Domänencontroller zuordnen

### In *Active Directory Sites and Services*
- `WS4` → Standort **NewYork**
- `WS5` → Standort **Houston**

### Auf WS5
- `NTDS Settings` öffnen
- **Global Catalog** aktivieren

---

## Schritt 5: Replikation konfigurieren

### 5.1 Site Link konfigurieren
- `Inter-Site Transports` → `IP`
- `DEFAULTIPSITELINK`
- Eigenschaften:
  - Replikation alle **15 Minuten**

### 5.2 Zeitplan einstellen

| Zeit            | Replikation |
|-----------------|-------------|
| 07:00 – 18:00   | deaktiviert |
| 18:00 – 07:00   | aktiviert   |
| Tests           | 00:00 – 24:00 aktiviert |

---

## Schritt 6: DNS-Konfiguration anpassen

### 6.1 Forward Lookup Zones
- IP-Adressen der Domänencontroller aktualisieren
- Delegation `work` überprüfen und anpassen

### 6.2 Reverse Lookup Zones
- Neue Reverse Zone für `10.0.1.0/24` erstellen
- PTR-Einträge anlegen für:
  - `WS4`
  - `WS5`
  - `RT6`
- Alte oder falsche Einträge löschen
- DNS-Server neu starten

---

## Schritt 7: DNS-Funktion testen

Auf allen Servern:
```bash
ipconfig
nslookup

## Schritt 8: AD-Replikation testen

### Test mit GlobalNames-Zone
- Forward Lookup Zone `GlobalNames` erstellen
- Replikation auf **Forest-weit** setzen

---

### Test 1: WS5 → WS4
- Auf `WS5`:
  - A-Record erstellen:
    - **Name:** `intranet`
    - **IP-Adresse:** `10.0.0.200`
- Replikation abwarten (max. **15 Minuten**)
- Prüfen, ob der Eintrag auf `WS4` erscheint

---

### Test 2: WS4 → WS5
- Auf `WS4`:
  - A-Record `intranet` ändern auf:
    - **IP-Adresse:** `10.0.1.200`
- Replikation abwarten
- Prüfen, ob die Änderung auf `WS5` übernommen wurde

---

## Ergebnis
- Zwei funktionierende AD-Standorte
- Routing zwischen Subnetzen aktiv
- Zeitgesteuerte AD-Replikation
- Korrekte DNS-Auflösung
- Erfolgreich getestete Multimaster-Replikation

