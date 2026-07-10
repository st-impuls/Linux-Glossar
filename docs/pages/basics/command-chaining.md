[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Befehle](../commands/index.md)
- [Wichtige Verzeichnisse](../directories.md)

## Grundlagen
- [Globbing & Platzhalter](globbing.md)
- [Reguläre Ausdrücke](regular-expressions.md)
- [Streams & Umleitungen](streams-redirects.md)

[← Zurück zur Übersicht](index.md)

# Verkettung & Hintergrund

Mehrere Befehle lassen sich in einer Zeile verbinden oder in den Hintergrund schicken. Ausschlaggebend ist dabei oft der **Exit-Code** des vorigen Befehls (`0` = Erfolg):

- `befehl1 ; befehl2` – führt beide **nacheinander** aus, unabhängig vom Ergebnis.
- `befehl1 && befehl2` – führt `befehl2` nur aus, wenn `befehl1` **erfolgreich** war (Exit-Code `0`).
- `befehl1 || befehl2` – führt `befehl2` nur aus, wenn `befehl1` **fehlgeschlagen** ist (Exit-Code ≠ `0`).
- `befehl &` – startet den Befehl im **Hintergrund**; die Shell blockiert nicht, sondern ist sofort wieder benutzbar.

Beispiele: `sudo apt update && sudo apt upgrade` · `mkdir bilder || echo "schon vorhanden"` · `./langlaeufer.sh &`

Ein mit `&` gestarteter Befehl wird zu einem **Job** der Shell. Solche Hintergrundjobs steuert man mit `jobs` (auflisten), `fg` (in den Vordergrund holen), `bg` (angehalten im Hintergrund fortsetzen) und `Ctrl + Z` (Vordergrundprozess anhalten). Damit ein Hintergrundprozess auch das **Schließen des Terminals** übersteht, dienen `nohup` und `disown` (siehe Kategorie [Prozesse & Steuerung](../commands/process-control.md)).

## Das letzte Argument wiederverwenden (`$_`)

`$_` ist eine spezielle Shell-Variable, die das **letzte Argument des zuvor ausgeführten Befehls** enthält. In Kombination mit `&&` spart das Tipparbeit und vermeidet Wiederholungen:

- `mkdir projekt && cd $_` – Ordner anlegen und direkt hineinwechseln (`$_` = `projekt`).
- `chmod +x install.sh && ./$_` – Skript ausführbar machen und sofort starten (`./$_` wird zu `./install.sh`).
- `touch notiz.txt && nano $_` – Datei anlegen und gleich im Editor öffnen.

**Hinweis:** `$_` bezieht sich immer auf das **letzte Wort** der vorherigen Befehlszeile. Die gleiche Wirkung hat die Tastenkombination `Alt + .`, die das letzte Argument direkt in die Eingabe einfügt.
