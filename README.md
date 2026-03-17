# Download unter folgendem Link  
[DOWNLOAD](https://it-s.de/download)  
  
  
# RDS Performance Monitor (.NET Framework 4.8)

RDSPerf überwacht Windows Performance Counter im Kontext von Remote Desktop Services (RDS). Aktuelle Version: 3.1. Die Anwendung unterstützt drei Betriebsarten:

GUI (Standard ohne Parameter): Live-Ansicht für die aktuelle Session (ohne manuellen CSV-Button).
Console (-c): zyklische Erfassung und Protokollierung aller aktiven RDS-Sessions.
Windows Service (-s, -u): Installation/Start bzw. Deinstallation des Services, der im Hintergrund alle aktiven Sessions überwacht.
Die Protokollierung erfolgt über NLog und wird über NLog.config gesteuert.

Zusätzlich wird bei jeder Messung automatisch eine CSV-Zeile in logs/rdsperf-values-YYYYMMDD.csv geschrieben (GUI und Background-Modi).

<img width="1101" height="614" alt="image" src="https://github.com/user-attachments/assets/5e97d67e-aeee-4d5d-bbf8-9a987c8d4d59" />




## Features

- Automatische Erkennung der aktuellen Benutzer-Session (Session-ID und Session-Name).
- Konfigurierbares Messintervall (`intervalSeconds`).
- Frei definierbare Counter-Liste über `config.json`.
- Sprachneutrale Counter-Namen durch Mapping von Englisch → lokale Sprache.
- Live-Ansicht der Messwerte in einer WinForms-Oberfläche.
- CSV-Export aller Messwerte seit Start der Anwendung über die Oberfläche.

## Architektur

### 1) GUI-Modus (ohne Parameter)

- Startet die WinForms-Oberfläche.
- Zeigt die Versionsnummer 3.1 im Fenstertitel und in der Statusanzeige.
- Ermittelt die **aktuelle** Session (`SessionInfo.GetCurrent`).
- Lädt `config.json`, ersetzt Session-Platzhalter und zeigt Live-Werte an.
- Schreibt bei jeder Prüfung automatisch eine CSV-Zeile nach `logs/rdsperf-values-YYYYMMDD.csv`.

### 2) Console-Modus (`-c`)

- Lädt `config.json` und startet den `RdsLoggingMonitor`.
- Ermittelt pro Intervall alle **aktiven** RDS-Sessions via `WTSEnumerateSessions`.
- Für jede aktive Session werden alle konfigurierten Counter gelesen und per NLog protokolliert.
- Beenden per `STRG+C`.

### 3) Service-Modus

- `-s`: installiert den Windows Service `RdsPerfMonitorService` und startet ihn.
- Der Service wird mit Parameter `--service` gestartet.
- `-u`: stoppt und entfernt den Service.

## Voraussetzungen

- Windows (RDS-/Terminalserver-Szenario empfohlen)
- .NET Framework 4.8
- Verfügbare Performance Counter auf dem Zielsystem
- Für `-s` und `-u`: Administratorrechte

## Startparameter

```bash
RdsPerfMonitor.exe -c   # Console-Betrieb
RdsPerfMonitor.exe -s   # Service installieren + starten
RdsPerfMonitor.exe -u   # Service stoppen + deinstallieren
RdsPerfMonitor.exe      # GUI-Betrieb (Standard)
```

## Logging mit NLog und CSV

Die NLog-Konfiguration liegt in `NLog.config`.

Standardmäßig:

- Datei-Log unter: `logs/rdsperf-YYYY-MM-DD.log`
- CSV-Messwerte unter: `logs/rdsperf-values-YYYYMMDD.csv`
- Console-Log aktiv
- Mindestlevel: `Info`
- tägliche Archivierung, 30 Archive

Beispiel Logzeile:

```text
2026-01-10 08:15:00.1234|INFO|SessionId=3; SessionName=RDP-Tcp#0; Counter=\Terminal Services\Active Sessions; Value=2.00; Status=OK
```

## Sprache der Performance Counter

Die Anwendung übersetzt die englischen Counter- und Kategorie-Namen der `config.json` zur Laufzeit über die Perflib-Registry (`HKLM\...\Perflib`). Damit funktionieren die gleichen Counter-Pfade auch auf nicht-englischen Windows-Installationen.

## Session-Platzhalter in der config.json

Die Konfiguration unterstützt Platzhalter:

- `{sessionId}`: Aktuelle Session-ID (z. B. `3`).
- `{sessionName}`: Aktueller Session-Name in Counter-Schreibweise (z. B. `RDP-Tcp 0`).

Beispiel:

```
\\RemoteFX Graphics({sessionName})\\Frame Quality
```

## Hinweise

- Einige Counter liefern beim ersten Abruf noch 0/keinen Wert. Nach dem zweiten Intervall sind die Werte korrekt.
- Falls ein Counter nicht existiert (z. B. RemoteFX nicht installiert), wird ein Fehlerstatus in der Oberfläche angezeigt.



