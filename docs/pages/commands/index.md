[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

# Befehle

## Kategorien
- [Archivierung & Kompression](archiving-compression.md)
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Datenträger & Dateisysteme](disk-filesystems.md)
- [Hilfe & Dokumentation](help-documentation.md)
- [Navigation & Suche](navigation-search.md)
- [Netzwerk & Download](network-download.md)
- [Paketverwaltung](package-management.md)
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)


## Aufbau eines Befehls

Ein Linux-Befehl ist in der Regel nach folgendem Schema aufgebaut:

`kommando [optionen] <argumente>`

- **kommando** – der Befehl selbst, also das Programm oder die eingebaute Funktion, die ausgeführt wird (z. B. `ls`, `cp`).
- **optionen** – Schalter, die festlegen, *wie* der Befehl arbeitet (z. B. `-l`, `-r`). Meist optional.
- **argumente** – das Objekt, *womit* der Befehl arbeitet, z. B. eine Datei oder ein Verzeichnis.

Schreibweise der Platzhalter in diesem Glossar:

- `<…>` – Platzhalter: hier etwas Eigenes einsetzen (z. B. einen Dateinamen).
- `[…]` – optional: kann angegeben werden, muss aber nicht.
- `…` – wiederholbar: die Angabe kann mehrfach vorkommen.

## Globbing (Platzhalter für Dateinamen)

Die Shell ersetzt bestimmte Sonderzeichen automatisch durch passende Datei- und Verzeichnisnamen, *bevor* der Befehl ausgeführt wird (Globbing):

- `*` – beliebig viele Zeichen (auch keine), z. B. `*.txt` = alle Dateien mit der Endung `.txt`.
- `?` – genau ein beliebiges Zeichen, z. B. `datei?.txt` passt auf `datei1.txt` oder `dateiA.txt`.
- `[…]` – genau ein Zeichen aus der Menge, z. B. `datei[12].txt` = `datei1.txt` oder `datei2.txt`; Bereiche mit `-`, z. B. `[a-z]`.

Beispiele: `ls *.md`, `rm bild_??.png`, `cp datei[1-3].txt /backup/`

**Umleitung und Pipes**

Standardmäßig liest ein Befehl von der Tastatur (Eingabe) und schreibt sein Ergebnis ins Terminal (Ausgabe). Beides lässt sich umleiten:

- `befehl > datei` – schreibt die Ausgabe in eine Datei; eine vorhandene Datei wird **überschrieben** (bzw. neu angelegt).
- `befehl >> datei` – **hängt** die Ausgabe an eine bestehende Datei an.
- `befehl < datei` – liest die Eingabe aus einer Datei statt von der Tastatur.
- `befehl1 | befehl2` – leitet die Ausgabe von `befehl1` direkt als Eingabe an `befehl2` weiter (Pipe).
- `befehl 2> datei` – leitet nur die Fehlerausgabe (stderr) um.

Beispiele: `echo "Hallo" > notiz.txt`, `ls -l >> liste.txt`, `cat datei.txt | grep "fehler"`

## Befehle verketten und im Hintergrund starten

Mehrere Befehle lassen sich in einer Zeile verbinden oder in den Hintergrund schicken. Ausschlaggebend ist dabei oft der **Exit-Code** des vorigen Befehls (`0` = Erfolg):

- `befehl1 ; befehl2` – führt beide **nacheinander** aus, unabhängig vom Ergebnis.
- `befehl1 && befehl2` – führt `befehl2` nur aus, wenn `befehl1` **erfolgreich** war (Exit-Code `0`).
- `befehl1 || befehl2` – führt `befehl2` nur aus, wenn `befehl1` **fehlgeschlagen** ist (Exit-Code ≠ `0`).
- `befehl &` – startet den Befehl im **Hintergrund**; die Shell blockiert nicht, sondern ist sofort wieder benutzbar.

Beispiele: `sudo apt update && sudo apt upgrade` · `mkdir bilder || echo "schon vorhanden"` · `./langlaeufer.sh &`

Ein mit `&` gestarteter Befehl wird zu einem **Job** der Shell. Solche Hintergrundjobs steuert man mit `jobs` (auflisten), `fg` (in den Vordergrund holen), `bg` (angehalten im Hintergrund fortsetzen) und `Ctrl + Z` (Vordergrundprozess anhalten). Damit ein Hintergrundprozess auch das **Schließen des Terminals** übersteht, dienen `nohup` und `disown` (siehe Kategorie „Prozesse & Steuerung").

## Here-Document (`<<`)

Mit einem Here-Document wird einem Befehl ein mehrzeiliger Text direkt als Eingabe (stdin) übergeben – bis zu einer frei wählbaren Schlusszeile (üblich: `EOF`):

- `befehl <<EOF … EOF` – im Text werden Variablen (`$name`), Befehlssubstitution (`$(…)`) und Backticks **ersetzt**.
- `befehl <<'EOF' … EOF` – der Text wird **wörtlich** übernommen, nichts wird ersetzt (gleichbedeutend: `<<"EOF"`, `<<\EOF`).
- `befehl <<-EOF … EOF` – führende **Tabs** der Zeilen werden entfernt, sodass die Schlusszeile eingerückt sein darf.

Die Schlusszeile darf nur den Begrenzer enthalten und muss am Zeilenanfang stehen (ohne vorangestellte Leerzeichen). Bei `<<-EOF` darf die Schlusszeile mit Tabs eingerückt sein, aber nicht mit Leerzeichen.

Beispiel – Text in eine Datei schreiben, ohne dass `$WERT` ersetzt wird:

```sh
cat <<'EOF' > config.txt
key = $WERT
EOF
```

## Das letzte Argument wiederverwenden (`$_`)

`$_` ist eine spezielle Shell-Variable, die das **letzte Argument des zuvor ausgeführten Befehls** enthält. In Kombination mit `&&` spart das Tipparbeit und vermeidet Wiederholungen:

- `mkdir projekt && cd $_` – Ordner anlegen und direkt hineinwechseln (`$_` = `projekt`).
- `chmod +x install.sh && ./$_` – Skript ausführbar machen und sofort starten (`./$_` wird zu `./install.sh`).
- `touch notiz.txt && nano $_` – Datei anlegen und gleich im Editor öffnen.

**Hinweis:** `$_` bezieht sich immer auf das **letzte Wort** der vorherigen Befehlszeile. Die gleiche Wirkung hat die Tastenkombination `Alt + .`, die das letzte Argument direkt in die Eingabe einfügt.
