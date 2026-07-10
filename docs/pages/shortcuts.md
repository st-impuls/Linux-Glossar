[← Home](../index.md)
- [Grundlagen](basics/index.md)
- [Abkürzungen / Begriffe](abbreviations.md)
- [Befehle](commands/index.md)
- [Wichtige Verzeichnisse](directories.md)

# Tastenkürzel

## Inhalt

- [Allgemeine Tastenkürzel](#allgemeine-tastenkurzel)
- [Linux-Konsole und grafische Oberfläche](#linux-konsole-und-grafische-oberflache)
- [Terminal, Shell und Kommandozeile](#terminal-shell-und-kommandozeile)
- [Prozesssteuerung](#prozesssteuerung)


## Allgemeine Tastenkürzel

| Tastenkürzel | Bedeutung |
|---|---|
| `Super-Taste` | (Windows-Taste) Aktivitäten / Suche öffnen |
| `Super + L` | Bildschirm sperren |
| `Ctrl + Alt + Pfeiltaste (links/rechts)` | Zwischen Arbeitsflächen wechseln |
| `Ctrl + Q` | Schließt das aktuell geöffnete Programm (GUI) |
| `Ctrl + Alt + Entf` | Neustart (je nach Konfiguration) |
| `Super + D` |	(Windows-Taste) Alle Fenster minimieren / Desktop anzeigen

## Linux-Konsole und grafische Oberfläche

| Tastenkürzel | Bedeutung |
|---|---|
| `Ctrl + Alt + F1…F6` | zwischen virtuellen Konsolen (TTYs) wechseln |
| `Ctrl + Alt + F7 (oder F2)` | zurück zur grafischen Oberfläche. Die genaue Taste hängt von Distribution und Desktop-Umgebung ab. |


## Terminal, Shell und Kommandozeile

| Tastenkürzel | Bedeutung |
|---|---|
| `Ctrl + Alt + T` | Terminal öffnen |
| `Ctrl + Shift + C` | Text im Terminal kopieren |
| `Ctrl + Shift + V` | Text im Terminal einfügen |
| | |
| `Tab` | Befehl, Dateiname oder Verzeichnisname automatisch vervollständigen |
| `Pfeil nach oben` | vorherigen Befehl aus dem Befehlsverlauf anzeigen |
| `Pfeil nach unten` | nächsten Befehl aus dem Befehlsverlauf anzeigen |
| `Ctrl + L` | Terminal-Bildschirm leeren |
| `Ctrl + A` | Zum Anfang der aktuellen Zeile springen |
| `Ctrl + E` | Zum Ende der aktuellen Zeile springen |
| `Ctrl + U` | Text vor dem Cursor löschen |
| `Ctrl + K` | Text nach dem Cursor löschen |
| `Ctrl + W` | Wort vor dem Cursor löschen |
| `Ctrl + T` | Zwei Zeichen vertauschen |
| `Ctrl + Y` | zuletzt gelöschten Text wieder einfügen (yank) |
| `Ctrl + _` | Letzte Eingabe rückgängig machen |
| `Alt + B / Alt + F` | wortweise rückwärts / vorwärts springen |
| `Alt + D` | Wort nach dem Cursor löschen |
| `Alt + .` | letztes Argument des vorherigen Befehls einfügen (gleichbedeutend mit `Esc` dann `.`) |
| | |
| `Ctrl + R` | Befehlsverlauf durchsuchen |
| `Ctrl + Shift + N` | neues Terminalfenster |
| `Ctrl + Shift + T` | Neuer Tab |


## Prozesssteuerung

| Tastenkürzel | Bedeutung |
|---|---|
| `Ctrl + C` | Beendet den aktuell laufenden Befehl im Terminal |
| `Ctrl + Z` | Pausiert den aktuell laufenden Prozess im Terminal |
| `Ctrl + D` | EOF senden, also das Ende der Eingabe signalisieren. Bei leerer Shell-Eingabe wird die Shell-Sitzung beendet. |
| `Ctrl + S` | Bildschirmausgabe anhalten (XOFF) |
| `Ctrl + Q` | Bildschirmausgabe fortsetzen (XON, Shell), wenn sie vorher mit `Ctrl + S` angehalten wurde |
| `Ctrl + \` | Laufenden Prozess mit SIGQUIT beenden. Dabei kann ein Core-Dump erzeugt werden. |