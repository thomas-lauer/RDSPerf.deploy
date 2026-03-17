# Download unter folgendem Link  
[DOWNLOAD](https://it-s.de/download)  
  
  
# RDS Performance Monitor (.NET Framework 4.8)

RDSPerf überwacht Windows Performance Counter im Kontext von Remote Desktop Services (RDS). Aktuelle Version: 3.1. Die Anwendung unterstützt drei Betriebsarten:

GUI (Standard ohne Parameter): Live-Ansicht für die aktuelle Session (ohne manuellen CSV-Button).
Console (-c): zyklische Erfassung und Protokollierung aller aktiven RDS-Sessions.
Windows Service (-s, -u): Installation/Start bzw. Deinstallation des Services, der im Hintergrund alle aktiven Sessions überwacht.
Die Protokollierung erfolgt über NLog und wird über NLog.config gesteuert.

Zusätzlich wird bei jeder Messung automatisch eine CSV-Zeile in logs/rdsperf-values-YYYYMMDD.csv geschrieben (GUI und Background-Modi).

<img width="1106" height="615" alt="image" src="https://github.com/user-attachments/assets/1252e5a8-1aa9-4ca6-89c4-1fb5eda6327a" />



## Features

- Automatische Erkennung der aktuellen Benutzer-Session (Session-ID und Session-Name).
- Konfigurierbares Messintervall (`intervalSeconds`).
- Frei definierbare Counter-Liste über `config.json`.
- Sprachneutrale Counter-Namen durch Mapping von Englisch → lokale Sprache.
- Live-Ansicht der Messwerte in einer WinForms-Oberfläche.
- CSV-Export aller Messwerte seit Start der Anwendung über die Oberfläche.

## Voraussetzungen

- Windows Server/Windows 10+ mit installierten Performance Countern.
- .NET Framework 4.8.
- Ausführung in der gewünschten Benutzer-Session (z. B. im RDS User Context).

## Start

1. `RdsPerfMonitor.exe` zusammen mit `config.json` im selben Ordner ablegen.
2. Anwendung starten – die aktuelle Session wird automatisch erkannt.
3. Werte werden alle `intervalSeconds` Sekunden aktualisiert.

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

## Dateien

- `config.json`: Konfiguration für Intervall und Counter.
- `config_json.md`: Dokumentation der Konfiguration.
- `RdsPerfMonitor.exe`: Anwendung.

## Hinweise

- Einige Counter liefern beim ersten Abruf noch 0/keinen Wert. Nach dem zweiten Intervall sind die Werte korrekt.
- Falls ein Counter nicht existiert (z. B. RemoteFX nicht installiert), wird ein Fehlerstatus in der Oberfläche angezeigt.



