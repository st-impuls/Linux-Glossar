[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Befehle](../commands/index.md)
- [Wichtige Verzeichnisse](../directories.md)

## Grundlagen
- [Reguläre Ausdrücke](regular-expressions.md)
- [Signale](signals.md)
- [Streams & Umleitungen](streams-redirects.md)
- [Verkettung & Hintergrund](command-chaining.md)

[← Zurück zur Übersicht](index.md)

# Globbing & Platzhalter

Die Shell ersetzt bestimmte Sonderzeichen automatisch durch passende Datei- und Verzeichnisnamen, *bevor* der Befehl ausgeführt wird (Globbing):

- `*` – beliebig viele Zeichen (auch keine), z. B. `*.txt` = alle Dateien mit der Endung `.txt`.
- `?` – genau ein beliebiges Zeichen, z. B. `datei?.txt` passt auf `datei1.txt` oder `dateiA.txt`.
- `[…]` – genau ein Zeichen aus der Menge, z. B. `datei[12].txt` = `datei1.txt` oder `datei2.txt`; Bereiche mit `-`, z. B. `[a-z]`.

Beispiele: `ls *.md`, `rm bild_??.png`, `cp datei[1-3].txt /backup/`
