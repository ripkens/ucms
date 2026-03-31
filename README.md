# USB Malware Scanner

## Was ist das?

Dieses Script erstellt eine vollständig isolierte Umgebung zum Scannen von USB-Sticks auf Malware, Viren und Backdoors. Das Host-System bleibt dabei immutable (unveränderlich).

## Sicherheitsfunktionen

### Netzwerk-Isolation (STRIKT)
- Prüft ob das System mit dem Internet verbunden ist
- **NUR SSID "R_Guest" ist erlaubt**
- Alle Ethernet- und VPN-Verbindungen werden deaktiviert
- Nur WLAN-Adapter bleibt aktiv
- System darf nicht im Firmennetzwerk sein (SSID-Beschränkung)

### System-Isolation
- USB-Stick wird **read-only** gemountet (kein Schreibzugriff möglich)
- Alle Scanner laufen in isolierten Docker-Containern
- Archive werden in temporaeren Verzeichnissen extrahiert
- Host-System bleibt vollständig geschützt

## Installierte Scanner

- **ClamAV**: Open-Source Virenscanner mit Signatur-Datenbank
- **YARA**: Pattern-basierter Malware-Scanner
- **Chkrootkit**: Rootkit-Erkennung
- **Rkhunter**: Backdoor-Erkennung

## Unterstuetzte Archive (rekursiv)

- ZIP, RAR, 7Z
- TAR, TAR.GZ, TAR.BZ2, TAR.XZ
- GZ, BZ2, XZ
- ISO, IMG
- JAR und mehr...

Verschachtelte Archive werden automatisch extrahiert.

## Benutzung

### Vorbereitung

1. System neu starten mit **NUR** SSID "R_Guest" verbunden
2. Alle Ethernet-Kabel entfernt
3. VPN ausgeschaltet
4. USB-Stick einstecken
5. Script auf das System kopieren

### Ausfuhrung

```bash
sudo ./usb-malware-scan.sh
```

### Der Scan-Prozess

1. **Sicherheitspruefung**
   - Prüft ob nur WLAN aktiv ist
   - Prüft ob mit "R_Guest" verbunden ist
   - Deaktiviert alle anderen Verbindungen
   - Prüft Internet-Verbindung

2. **Vorbereitung**
   - Installiert Docker (falls fehlt)
   - Baut Scanner-Image

3. **USB-Erkennung**
   - Listet verfuegbare USB-Sticks
   - Waehlt Ziel-Stick aus
   - Mountet read-only

4. **Archiv-Verarbeitung**
   - Kopiert alle Archive
   - Extrahiert rekursiv

5. **Scan-Durchlauf**
   - ClamAV: Viren/Malware
   - YARA: Pattern-Matching
   - Chkrootkit: Rootkits
   - Rkhunter: Backdoors

6. **Report**
   - Generiert detaillierten Bericht
   - Speichert im reports/ Ordner

7. **Aufräumen**
   - Entfernt Container
   - Loescht temporaere Dateien
   - Unmountet USB-Stick

## Ordnerstruktur

```
usb-malware-scanner/
├── usb-malware-scan.sh      # Haupt-Script
├── docker/
│   └── Dockerfile           # Scanner-Image
├── reports/                 # Scan-Reports
└── logs/                    # Scan-Logs
```

## Wichtige Warnungen

### VORHER prüfen:
- Nur mit SSID "R_Guest" verbinden
- Kein Ethernet angeschlossen
- VPN ausgeschaltet
- USB-Stick einstecken

### NICHT:
- Im Firmennetzwerk scannen
- Mit SSH/VPN verbunden sein
- System fuer andere Zwecke nutzen
- Mit unbekannten USB-Sticks scannen (nur vertrauenswürdige)

### NACHHER:
- USB-Stick sicher entfernen
- Report prüfen
- System neu starten (optional)

## Beispiel-Report

```
=======================================
SCAN-ZUSAMMENFASSUNG
=======================================
USB-Stick: /dev/sdb1
Dateien: 1573
Verzeichnisse: 234
Größe: 2.3 GB

=== CLAMAV SCAN ===
Found: 3
- /usb/trojan.exe: Trojan.GenericKD.123456
- /scan/extracted/malware.zip.001: EICAR-TEST-FILE
- /scan/extracted/virus.exe: Win32.FakeAV.B

=== YARA SCAN ===
Matched: 2
- /scan/extracted/backdoor: backdoor_rule_1
- /usb/rootkit: rootkit_pattern

=======================================
```

## Systemanforderungen

- Ubuntu 24.04 oder neuer
- WLAN (mit R_Guest SSID)
- Internet-Verbindung
- Min. 2GB freier Speicher
- sudo-Rechte

## Troubleshooting

### "Keine Internet-Verbindung"
- Mit "R_Guest" verbinden
- Internet prüfen mit `ping 8.8.8.8`

### "Ethernet ist aktiv"
- Ethernet-Kabel entfernen
- Alle Ethernet-Verbindungen trennen

### "Nicht mit R_Guest verbunden"
- WLAN auf SSID "R_Guest" stellen
- Keine andere SSID moeglich

### Docker-Fehler
- `sudo apt install docker.io`
- Oder Script laesst es automatisch installieren

## Lizenz

Dies ist ein Sicherheits-Werkzeug. Benutzung auf eigene Gefahr.

---

**WARNUNG:** Dieses Script sollte nur nach strengen Sicherheitsvorkehrungen verwendet werden. Immer die Netzwerk-Sicherheitsabfragen beachten!
