# config.json Referenz

Die Datei `config.json` steuert das Messintervall und die zu überwachenden Performance Counter.

## Beispiel

```json
{
  "intervalSeconds": 5,
  "counters": [
    "\\Processor Information(_Total)\\% Processor Time",
    "\\RemoteFX Graphics({sessionName})\\Frame Quality",
    "\\Terminal Services\\Active Sessions"
  ]
}
```

## Felder

### `intervalSeconds`

- **Typ:** Ganzzahl
- **Standard:** `5`
- **Beschreibung:** Messintervall in Sekunden. Werte ≤ 0 werden auf 5 Sekunden korrigiert.

### `counters`

- **Typ:** Array von Strings
- **Beschreibung:** Liste der Performance Counter Pfade. Die Anwendung erwartet englische Counter-Namen und übersetzt diese automatisch auf die lokale Sprache.

#### Platzhalter

- `{sessionId}` → aktuelle Session-ID.
- `{sessionName}` → aktueller Session-Name in Counter-Schreibweise (Beispiel: `RDP-Tcp 0`).

## Hinweise

- Leerzeilen oder leere Einträge werden ignoriert.
- Fehlerhafte Counter-Pfade werden in der Oberfläche als Fehlerstatus angezeigt.
