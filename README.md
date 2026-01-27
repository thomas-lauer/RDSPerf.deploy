# RDS Performance Monitor (WinForms, .NET Framework 4.8)

Diese Anwendung misst Performance Counter in einer Benutzer-Session auf einem Terminalserver (RDS). Die Messung läuft live im 5‑Sekunden‑Intervall (konfigurierbar) und verwendet sprachunabhängige Counter-Namen, indem sie die englischen Counter-Bezeichnungen automatisch auf die lokal installierte Sprache abbildet.

<img width="1111" height="621" alt="image" src="https://github.com/user-attachments/assets/27d91eef-f8aa-4652-a309-be8f78c014b5" />


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

