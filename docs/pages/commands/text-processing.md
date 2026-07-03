[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Hilfe & Dokumentation](help-documentation.md)
- [Navigation & Suche](navigation-search.md)
- [Netzwerk & Download](network-download.md)
- [Paketverwaltung](package-management.md)
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)

[← Zurück zur Übersicht](index.md)

# Textbearbeitung & Filter

### awk
>**Funktion:** Textzeilen spalten- und musterbasiert verarbeiten<br />
>**Syntax:** `awk [optionen] '<programm>' [<datei>...]`<br />
>**Erklärung:** Mächtige Sprache zur Verarbeitung von Textzeilen. Zerlegt jede Zeile automatisch in Felder (`$1`, `$2`, …; `$0` = ganze Zeile) und führt für passende Zeilen Aktionen aus.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`'{print $1}'` gibt das erste Feld jeder Zeile aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-F <z>` legt das Feldtrennzeichen fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`'/muster/{...}'` Aktion nur für passende Zeilen<br />
>**Beispiel:** `awk '{print $1}' datei.txt`

<details markdown>
<summary>Aufbau & Variablen</summary>

| Element | Bedeutung |
|---|---|
| `$1`, `$2`, … | das 1., 2., … Feld der Zeile |
| `$0` | die gesamte Zeile |
| `NF` | Anzahl der Felder in der Zeile |
| `NR` | Nummer der aktuellen Zeile |
| `'muster {aktion}'` | führt die Aktion nur bei passendem Muster aus |
| `BEGIN {…}` / `END {…}` | Aktion vor der ersten / nach der letzten Zeile |
| `-F <z>` | Feldtrennzeichen (z. B. `-F:` oder `-F','`) |
| `-v <var>=<wert>` | setzt eine Variable vor dem Lauf |
| `-f <datei>` | liest das awk-Programm aus einer Datei |
| `--help` / `--version` | Hilfe / Version (gawk) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
awk '{print $1}' datei.txt                     # erstes Feld jeder Zeile
awk -F: '{print $1}' /etc/passwd               # Benutzernamen (Trenner :)
awk '{print $NF}' datei.txt                    # letztes Feld jeder Zeile
awk 'NR==1' datei.txt                          # nur die erste Zeile
awk '/fehler/ {print}' log.txt                 # Zeilen mit "fehler"
awk '{sum += $1} END {print sum}' zahlen.txt   # eine Spalte aufsummieren
awk -F, '$3 > 100 {print $1}' daten.csv        # nach Feldwert filtern
```

</details>

>**Hinweis:** `awk` glänzt bei spaltenorientierten Daten und Berechnungen – dort, wo `cut` zu starr und `grep` zu einfach ist. Mehrere aufeinanderfolgende Leerzeichen werden standardmäßig als ein Trenner behandelt (anders als bei `cut`).

---

### column
>**Funktion:** Eingabe in ausgerichtete Spalten formatieren<br />
>**Syntax:** `column [optionen] [<datei>...]`<br />
>**Erklärung:** Bringt Text in saubere, ausgerichtete Spalten – ideal, um Tabellen oder Listen lesbar darzustellen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t` erzeugt eine Tabelle mit ausgerichteten Spalten<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <z>` legt das Eingabe-Trennzeichen fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-N <namen>` setzt Spaltenüberschriften<br />
>**Beispiel:** `column -t -s: /etc/passwd`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-t`, `--table` | richtet die Eingabe als Tabelle aus |
| `-s <z>`, `--separator <z>` | Eingabe-Trennzeichen, am besten in Anführungszeichen (z. B. `-s ','` oder `-s '.'`) |
| `-o <z>`, `--output-separator <z>` | Trennzeichen der Ausgabe |
| `-N <namen>`, `--table-columns` | Spaltenüberschriften (kommagetrennt) |
| `-R <n>`, `--table-right` | richtet die Spalte(n) n rechtsbündig aus |
| `-J`, `--json` | gibt die Tabelle im JSON-Format aus (erfordert `-N`) |
| `-x`, `--fillrows` | füllt zuerst die Zeilen statt der Spalten |
| `-c <breite>`, `--output-width <breite>` | begrenzt die Gesamtbreite der Ausgabe |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
column -t -s: /etc/passwd                  # die :-getrennte Datei als Tabelle
mount | column -t                          # Ausgabe übersichtlich ausrichten
echo -e "Name Alter\nMax 30" | column -t   # Spalten ausrichten
column -t -s, daten.csv                    # CSV als Tabelle anzeigen
```

</details>

>**Hinweis:** `column -t` ist praktisch am Ende einer Pipe, um unübersichtliche Ausgaben auszurichten. Standardmäßig trennt es an Leerzeichen; für andere Trenner `-s` verwenden – das Trennzeichen am besten in Anführungszeichen setzen (`-s ','`, `-s '.'`), besonders bei Zeichen mit Sonderbedeutung in der Shell wie Leerzeichen, `;` oder `*`.

---

### comm
>**Funktion:** Zwei sortierte Dateien zeilenweise vergleichen<br />
>**Syntax:** `comm [optionen] <datei1> <datei2>`<br />
>**Erklärung:** Vergleicht zwei **sortierte** Dateien und teilt die Zeilen auf drei Spalten auf: Spalte 1 = Zeilen, die **nur** in Datei 1 stehen (nicht in Datei 2); Spalte 2 = Zeilen, die **nur** in Datei 2 stehen; Spalte 3 = Zeilen, die in **beiden** Dateien vorkommen. Gemeinsame Zeilen erscheinen also nur in Spalte 3, nicht zusätzlich in Spalte 1 oder 2.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-1` blendet Spalte 1 aus (nur in Datei 1)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-2` blendet Spalte 2 aus (nur in Datei 2)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-3` blendet Spalte 3 aus (gemeinsame Zeilen)<br />
>**Beispiel:** `comm datei1.txt datei2.txt`

<details markdown>
<summary>Optionen & Spalten</summary>

| Option | Wirkung |
|---|---|
| `-1` | unterdrückt Spalte 1 (Zeilen nur in Datei 1) |
| `-2` | unterdrückt Spalte 2 (Zeilen nur in Datei 2) |
| `-3` | unterdrückt Spalte 3 (Zeilen in beiden) |
| `-12` | zeigt nur die gemeinsamen Zeilen |
| `--check-order` | bricht ab, wenn die Dateien nicht sortiert sind |
| `--nocheck-order` | prüft die Sortierung nicht |
| `--output-delimiter=<z>` | legt das Trennzeichen zwischen den Spalten fest |
| `--total` | zeigt am Ende eine Zeile mit den Summen je Spalte |
| `-z`, `--zero-terminated` | trennt Zeilen mit dem Nullbyte |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
comm datei1.txt datei2.txt        # drei Spalten: nur1 / nur2 / beide
comm -12 a.txt b.txt              # nur gemeinsame Zeilen
comm -23 a.txt b.txt              # nur Zeilen, die ausschließlich in a stehen
comm -3 a.txt b.txt               # nur die Unterschiede
comm <(sort a.txt) <(sort b.txt)  # unsortierte Dateien vorher sortieren
```

</details>

>**Hinweis:** Beide Dateien müssen **sortiert** sein, sonst ist das Ergebnis falsch. Mit Prozesssubstitution `<(sort …)` lässt sich das direkt erledigen. Für detaillierte Unterschiede mit Kontext eignet sich `diff`.

---

### cut
>**Funktion:** Spalten oder Felder ausschneiden<br />
>**Syntax:** `cut [optionen] [<datei>...]`<br />
>**Erklärung:** Schneidet aus jeder Zeile bestimmte Teile heraus, z. B. einzelne Spalten.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <z>` legt das Trennzeichen fest (z. B. `-d ":"`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <n>` wählt das n-te Feld aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <n>` wählt bestimmte Zeichen-Positionen aus<br />
>**Beispiel:** `cut -d ":" -f 1 /etc/passwd`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-d <z>`, `--delimiter=<z>` | legt das Trennzeichen fest (Standard: Tabulator) |
| `-f <n>`, `--fields=<n>` | wählt Felder aus (`1,3` oder Bereich `2-4`) |
| `-c <n>`, `--characters=<n>` | wählt Zeichen-Positionen aus (`1-5`) |
| `-b <n>`, `--bytes=<n>` | wählt Byte-Positionen aus |
| `--complement` | kehrt die Auswahl um (alles außer den gewählten Feldern) |
| `-s`, `--only-delimited` | überspringt Zeilen ohne Trennzeichen |
| `--output-delimiter=<z>` | legt ein eigenes Trennzeichen für die Ausgabe fest |
| `-z`, `--zero-terminated` | trennt Zeilen mit dem Nullbyte statt Zeilenumbruch |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
cut -d ":" -f 1 /etc/passwd        # nur die Benutzernamen
cut -d ":" -f 1,3 /etc/passwd      # Name und UID
cut -c 1-10 datei.txt              # die ersten 10 Zeichen jeder Zeile
cut -d "," -f 2- daten.csv         # ab dem 2. Feld bis zum Ende
```

</details>

>**Hinweis:** `cut` eignet sich für einfach strukturierte Daten mit festem Trennzeichen. Bei mehreren aufeinanderfolgenden Leerzeichen als Trenner ist `awk` flexibler.

---

### diff
>**Funktion:** Unterschiede zwischen zwei Dateien anzeigen<br />
>**Syntax:** `diff [optionen] <datei1> <datei2>`<br />
>**Erklärung:** Vergleicht zwei Dateien zeilenweise und zeigt, welche Zeilen sich unterscheiden und wie man die eine in die andere überführt.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` einheitliches, kompaktes Format (unified)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert Groß-/Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` vergleicht Verzeichnisse rekursiv<br />
>**Beispiel:** `diff -u alt.txt neu.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-u`, `--unified` | kompaktes „unified"-Format (auch von `git` genutzt) |
| `-c`, `--context` | Format mit Kontextzeilen |
| `-i`, `--ignore-case` | ignoriert Groß-/Kleinschreibung |
| `-w`, `--ignore-all-space` | ignoriert alle Leerzeichen-Unterschiede |
| `-r`, `--recursive` | vergleicht Verzeichnisse rekursiv |
| `-q`, `--brief` | meldet nur, ob sich die Dateien unterscheiden |
| `-y`, `--side-by-side` | stellt beide Dateien nebeneinander dar |
| `-b`, `--ignore-space-change` | ignoriert Änderungen in der Menge der Leerzeichen |
| `-B`, `--ignore-blank-lines` | ignoriert hinzugefügte/entfernte Leerzeilen |
| `-N`, `--new-file` | behandelt eine fehlende Datei als leer |
| `-a`, `--text` | behandelt alle Dateien als Text |
| `--color[=<wann>]` | hebt Unterschiede farblich hervor |
| `-s`, `--report-identical-files` | meldet auch, wenn Dateien gleich sind |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
diff alt.txt neu.txt                         # Standardausgabe der Unterschiede
diff -u alt.txt neu.txt                       # unified-Format (üblich für Patches)
diff -y alt.txt neu.txt                       # Gegenüberstellung nebeneinander
diff -q a.txt b.txt                           # nur: unterscheiden sich? (ja/nein)
diff -r ordner1 ordner2                       # ganze Verzeichnisse vergleichen
diff -u alt.txt neu.txt > aenderung.patch     # Unterschiede als Patch speichern
```

</details>

>**Hinweis:** Das `-u`-Format ist Standard für Patches (`patch < aenderung.patch`) und entspricht der Anzeige von `git diff`. Für den Vergleich **sortierter** Mengen (gemeinsame/eindeutige Zeilen) ist `comm` besser geeignet.

---

### grep
>**Funktion:** Nach Text suchen<br />
>**Syntax:** `grep [optionen] <muster> [<datei>...]`<br />
>**Erklärung:** Durchsucht Dateien oder Ausgaben nach Zeilen, die zu einem Muster passen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` sucht rekursiv in Verzeichnissen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` zeigt die Zeilennummern der Treffer an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt nur Zeilen, die NICHT passen<br />
>**Beispiel:** `grep -i "fehler" logfile.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-i`, `--ignore-case` | ignoriert Groß- und Kleinschreibung |
| `-r`, `--recursive` | sucht rekursiv in Verzeichnissen |
| `-n`, `--line-number` | zeigt die Zeilennummern der Treffer an |
| `-v`, `--invert-match` | zeigt nur Zeilen, die NICHT passen |
| `-c`, `--count` | zählt nur die Treffer pro Datei |
| `-l`, `--files-with-matches` | zeigt nur die Namen der Dateien mit Treffern |
| `-w`, `--word-regexp` | findet nur ganze Wörter |
| `-o`, `--only-matching` | zeigt nur den passenden Teil der Zeile |
| `-E`, `--extended-regexp` | erweiterte reguläre Ausdrücke (`grep -E` = `egrep`) |
| `-A <n>` / `-B <n>` / `-C <n>` | zeigt n Zeilen nach / vor / um den Treffer |
| `--color=auto` | hebt die Treffer farblich hervor |
| `-F`, `--fixed-strings` | sucht nach festem Text statt regulärem Ausdruck (`fgrep`) |
| `-P`, `--perl-regexp` | nutzt Perl-kompatible reguläre Ausdrücke |
| `-f <datei>`, `--file=<datei>` | liest die Suchmuster aus einer Datei |
| `-x`, `--line-regexp` | findet nur Zeilen, die komplett passen |
| `-m <n>`, `--max-count=<n>` | hört nach n Treffern je Datei auf |
| `--include=<muster>` / `--exclude=<muster>` | beschränkt die durchsuchten Dateien |
| `-q`, `--quiet` | gibt nichts aus, nur Erfolg/Misserfolg im Status |
| `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
grep -i "fehler" logfile.txt        # ohne Beachtung der Groß-/Kleinschreibung
grep -rn "TODO" .                   # rekursiv mit Zeilennummern
grep -v "^#" config.txt             # alle Zeilen außer Kommentaren
grep -c "fehler" logfile.txt        # Anzahl der Treffer
grep -A 2 "Exception" log.txt       # Treffer plus 2 Folgezeilen
ps aux | grep firefox               # Ausgabe einer Pipe durchsuchen
---
grep -E "fehler|warnung" log.txt        # Zeilen mit "fehler" ODER "warnung"
grep -E "colou?r" datei.txt             # findet "color" und "colour" (? = optional)
grep -E "ab+c" datei.txt                # "abc", "abbc", "abbbc" … (+ = einmal oder mehr)
grep -E "[0-9]{3}-[0-9]{4}" datei.txt   # Muster wie 123-4567 ({n} = genaue Anzahl)
grep -E "^(Mo|Di|Mi)" plan.txt          # Zeilen, die mit Mo, Di oder Mi beginnen
grep -E "\.(jpg|png|gif)$" liste.txt    # Zeilen, die auf .jpg, .png oder .gif enden

```

</details>

>**Hinweis:** Oft mit einer Pipe kombiniert, z. B. `ps aux | grep firefox`. Für erweiterte Muster `-E` verwenden (Alternativen mit `|`, Quantoren wie `+`).

---

### join
>**Funktion:** Zwei Dateien über ein gemeinsames Feld verbinden<br />
>**Syntax:** `join [optionen] <datei1> <datei2>`<br />
>**Erklärung:** Verknüpft die Zeilen zweier Dateien, die im angegebenen Feld denselben Wert haben (ähnlich einem Datenbank-Join). Beide Dateien müssen nach diesem Feld sortiert sein.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-1 <n>` / `-2 <n>` Verknüpfungsfeld in Datei 1 / 2<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t <z>` Feldtrennzeichen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a <n>` zeigt auch Zeilen ohne Partner aus Datei n<br />
>**Beispiel:** `join datei1.txt datei2.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-1 <n>`, `-2 <n>` | legt das Verknüpfungsfeld in Datei 1 bzw. 2 fest |
| `-j <n>` | gemeinsames Feld in beiden Dateien |
| `-t <z>` | Feldtrennzeichen (Standard: Leerzeichen) |
| `-a <n>` | gibt auch Zeilen ohne Treffer aus Datei n aus (wie ein outer join) |
| `-o <format>` | legt fest, welche Felder ausgegeben werden |
| `-i` | ignoriert Groß-/Kleinschreibung beim Vergleich |
| `-v <n>` | gibt nur Zeilen ohne Partner aus Datei n aus |
| `-e <text>` | ersetzt fehlende Felder durch den angegebenen Text |
| `--header` | behandelt die erste Zeile jeder Datei als Kopfzeile |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
join a.txt b.txt                  # über das jeweils 1. Feld verbinden
join -t, -1 1 -2 1 a.csv b.csv    # CSV über das 1. Feld verbinden
join -a 1 a.txt b.txt             # auch Zeilen aus a ohne Partner zeigen
join <(sort a.txt) <(sort b.txt)  # unsortierte Dateien vorher sortieren
```

</details>

>**Hinweis:** Wie bei `comm` müssen beide Dateien nach dem Verknüpfungsfeld **sortiert** sein. Für eine einfache positionsbasierte Zusammenführung (ohne gemeinsames Feld) reicht `paste`.

---

### nl
>**Funktion:** Zeilen nummerieren<br />
>**Syntax:** `nl [optionen] [<datei>...]`<br />
>**Erklärung:** Versieht die Zeilen einer Datei mit Nummern – flexibler als `cat -n`, z. B. nur für nicht leere Zeilen oder mit eigenem Format.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b a` nummeriert alle Zeilen, `-b t` nur nicht leere<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-w <n>` Breite der Nummernspalte<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <z>` Trennzeichen nach der Nummer<br />
>**Beispiel:** `nl datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-b a` / `-b t` | nummeriert alle / nur nicht leere Zeilen (Standard: `t`) |
| `-n <format>` | Zahlenformat: `ln` linksbündig, `rn` rechtsbündig, `rz` mit führenden Nullen |
| `-w <n>`, `--number-width=<n>` | Breite der Nummernspalte |
| `-s <z>`, `--number-separator=<z>` | Trennzeichen zwischen Nummer und Text |
| `-v <n>` | Startnummer |
| `-i <n>` | Schrittweite |
| `-h <stil>`, `--header-numbering=<stil>` | Nummerierung für den Kopfbereich |
| `-f <stil>`, `--footer-numbering=<stil>` | Nummerierung für den Fußbereich |
| `-d <z>`, `--section-delimiter=<z>` | Kennzeichen für einen Abschnittswechsel |
| `-p`, `--no-renumber` | zählt über Abschnitte hinweg weiter |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
nl datei.txt                 # nur nicht leere Zeilen nummerieren
nl -b a datei.txt            # alle Zeilen nummerieren (auch leere)
nl -w 3 -s '. ' datei.txt    # Format "  1. Text"
nl -ba -nrz -w3 skript.sh    # mit führenden Nullen (001, 002, …)
```

</details>

>**Hinweis:** Im Gegensatz zu `cat -n` nummeriert `nl` standardmäßig nur nicht leere Zeilen und lässt Format, Breite und Startwert frei einstellen.

---

### paste
>**Funktion:** Zeilen mehrerer Dateien spaltenweise zusammenfügen<br />
>**Syntax:** `paste [optionen] [<datei>...]`<br />
>**Erklärung:** Fügt die Zeilen mehrerer Dateien nebeneinander (spaltenweise) zusammen, standardmäßig durch einen Tabulator getrennt.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <z>` legt das Trennzeichen fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` fügt die Zeilen einer Datei zu einer einzigen Zeile zusammen<br />
>**Beispiel:** `paste namen.txt alter.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-d <z>`, `--delimiters=<z>` | Trennzeichen statt Tabulator (z. B. `-d,`) |
| `-s`, `--serial` | verbindet die Zeilen je Datei zu einer Zeile |
| `-z`, `--zero-terminated` | trennt mit Null-Byte statt Zeilenumbruch |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
paste namen.txt alter.txt    # zwei Dateien als zwei Spalten (Tab-getrennt)
paste -d, a.txt b.txt        # mit Komma als Trenner (CSV)
paste -s -d, zahlen.txt      # eine Spalte zu einer Zeile: 1,2,3
paste - - < datei.txt        # je zwei Zeilen nebeneinander legen
```

</details>

>**Hinweis:** Anders als `join` arbeitet `paste` rein positionsbasiert (Zeile 1 zu Zeile 1) und braucht weder ein gemeinsames Feld noch sortierte Dateien.

---

### sed
>**Funktion:** Text strömend bearbeiten (Stream-Editor)<br />
>**Syntax:** `sed [optionen] '<befehl>' [<datei>...]`<br />
>**Erklärung:** Bearbeitet Text zeilenweise nach festen Regeln – am häufigsten zum Suchen und Ersetzen. Liest aus Datei oder Pipe und schreibt das Ergebnis auf die Standardausgabe.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`s/alt/neu/` ersetzt das erste Vorkommen je Zeile<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`s/alt/neu/g` ersetzt alle Vorkommen je Zeile<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ändert die Datei direkt<br />
>**Beispiel:** `sed 's/alt/neu/g' datei.txt`

<details markdown>
<summary>Befehle & Optionen</summary>

| Ausdruck | Wirkung |
|---|---|
| `s/alt/neu/` | ersetzt das erste Vorkommen pro Zeile |
| `s/alt/neu/g` | ersetzt alle Vorkommen pro Zeile (global) |
| `s/alt/neu/gi` | zusätzlich ohne Beachtung der Groß-/Kleinschreibung |
| `<n>d` / `/muster/d` | löscht Zeile n bzw. passende Zeilen |
| `-n '<n>p'` | gibt nur Zeile n aus (mit `-n` = keine automatische Ausgabe) |
| `-i`, `--in-place` | bearbeitet die Datei direkt (mit `-i.bak` = Sicherung) |
| `-E`, `-r` | erweiterte reguläre Ausdrücke |
| `-e <befehl>` | mehrere Befehle hintereinander angeben |
| `-f <datei>`, `--file=<datei>` | liest die Befehle aus einer Datei |
| `-s`, `--separate` | behandelt mehrere Dateien getrennt statt als einen Strom |
| `-z`, `--null-data` | trennt Zeilen mit dem Nullbyte |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sed 's/alt/neu/g' datei.txt              # alle "alt" durch "neu" ersetzen (nur Ausgabe)
sed -i 's/alt/neu/g' datei.txt           # Ersetzung direkt in der Datei
sed -i.bak 's/alt/neu/g' datei.txt       # wie oben, mit Sicherung datei.txt.bak
sed -n '5,10p' datei.txt                 # nur die Zeilen 5 bis 10 ausgeben
sed '/^#/d' config.txt                   # alle Kommentarzeilen entfernen
sed '2d' datei.txt                       # die zweite Zeile löschen
echo "Hallo Welt" | sed 's/Welt/Linux/'  # in einer Pipe
```

</details>

>**Hinweis:** Als Trennzeichen muss nicht `/` dienen: Das Zeichen direkt nach dem `s` legt das Trennzeichen für diesen Befehl fest – steht dort z. B. ein `#`, gilt `#` als Trenner (`s#alt#neu#`). Bei Pfaden ist das oft lesbarer, z. B. `s#/alt/pfad#/neu/pfad#`. Kommt das Trennzeichen im Such- oder Ersetzungstext selbst vor, muss es mit `\` maskiert werden (z. B. `s/a\/b/c/`) – ein anderes Trennzeichen erspart das oft. `-i` ändert die Datei unwiderruflich; vorher ohne `-i` testen oder mit `-i.bak` eine Sicherung anlegen.

---

### sort
>**Funktion:** Zeilen sortieren<br />
>**Syntax:** `sort [optionen] [<datei>...]`<br />
>**Erklärung:** Sortiert die Zeilen einer Datei oder Eingabe, standardmäßig alphabetisch.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` sortiert numerisch statt alphabetisch<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` kehrt die Reihenfolge um (absteigend)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` entfernt doppelte Zeilen<br />
>**Beispiel:** `sort -n zahlen.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n`, `--numeric-sort` | sortiert numerisch statt alphabetisch |
| `-r`, `--reverse` | kehrt die Reihenfolge um (absteigend) |
| `-u`, `--unique` | gibt jede Zeile nur einmal aus (entfernt Dubletten) |
| `-k <n>`, `--key=<n>` | sortiert nach der n-ten Spalte |
| `-t <z>`, `--field-separator=<z>` | legt das Spalten-Trennzeichen fest |
| `-h`, `--human-numeric-sort` | sortiert lesbare Größen (z. B. `2K`, `1M`, `3G`) |
| `-f`, `--ignore-case` | ignoriert Groß-/Kleinschreibung |
| `-o <datei>`, `--output=<datei>` | schreibt das Ergebnis in eine Datei |
| `-b`, `--ignore-leading-blanks` | ignoriert führende Leerzeichen |
| `-c`, `--check` | prüft nur, ob bereits sortiert ist |
| `-m`, `--merge` | führt bereits sortierte Dateien zusammen |
| `-s`, `--stable` | stabile Sortierung (Reihenfolge gleicher Zeilen bleibt) |
| `-V`, `--version-sort` | sortiert Versionsnummern natürlich (`v2` vor `v10`) |
| `-z`, `--zero-terminated` | trennt Zeilen mit dem Nullbyte |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sort namen.txt                  # alphabetisch sortieren
sort -n zahlen.txt              # numerisch sortieren
sort -nr zahlen.txt             # numerisch absteigend
sort -u liste.txt               # sortieren und Dubletten entfernen
sort -t ":" -k 3 -n /etc/passwd # nach 3. Feld (UID) numerisch sortieren
```

</details>

>**Hinweis:** Oft mit `uniq` kombiniert, z. B. `sort datei.txt | uniq -c` zählt gleiche Zeilen. `uniq` setzt voraus, dass gleiche Zeilen bereits beieinanderstehen – daher meist vorher `sort`.

---

### split
>**Funktion:** Eine Datei in mehrere Teile zerlegen<br />
>**Syntax:** `split [optionen] [<datei> [<präfix>]]`<br />
>**Erklärung:** Teilt eine große Datei in mehrere kleinere Stücke – nach Zeilenzahl, Größe oder Anzahl. Die Teile heißen standardmäßig `xaa`, `xab`, …<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l <n>` Teile mit je n Zeilen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b <größe>` Teile mit fester Größe (z. B. `10M`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <n>` in genau n Teile aufteilen<br />
>**Beispiel:** `split -l 1000 gross.txt teil_`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l <n>`, `--lines=<n>` | je Teil n Zeilen |
| `-b <größe>`, `--bytes=<größe>` | je Teil eine feste Größe (`K`, `M`, `G`) |
| `-n <n>`, `--number=<n>` | in genau n gleich große Teile |
| `-d`, `--numeric-suffixes` | numerische Endungen (00, 01, …) statt aa, ab |
| `-a <n>`, `--suffix-length=<n>` | Länge der Endung |
| `--additional-suffix=<z>` | hängt eine feste Endung an (z. B. `.txt`) |
| `-C <größe>`, `--line-bytes=<größe>` | je Teil höchstens diese Größe, aber an Zeilengrenzen |
| `-e`, `--elide-empty-files` | erzeugt keine leeren Teildateien |
| `--verbose` | meldet jede erstellte Datei |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
split -l 1000 gross.txt teil_       # Stücke mit je 1000 Zeilen: teil_aa, teil_ab …
split -b 10M video.bin teil_        # Stücke mit je 10 MB
split -n 4 datei.txt teil_          # in 4 gleich große Teile
split -d -l 500 daten.csv teil_     # mit Zahlen-Endungen: teil_00, teil_01 …
cat teil_* > wiederhergestellt.txt  # Teile wieder zusammenfügen
```

</details>

>**Hinweis:** Mit `cat teil_* > datei` lassen sich die Stücke wieder zusammensetzen – die alphabetische Reihenfolge der Endungen sorgt für die richtige Abfolge. Praktisch, um große Dateien zu versenden oder portionsweise zu verarbeiten.

---

### tac
>**Funktion:** Datei in umgekehrter Zeilenreihenfolge ausgeben<br />
>**Syntax:** `tac [optionen] [<datei>...]`<br />
>**Erklärung:** Gibt die Zeilen einer Datei in umgekehrter Reihenfolge aus – die letzte Zeile zuerst (umgekehrtes `cat`).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <z>` legt den Zeilentrenner fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` behandelt den Trenner als regulären Ausdruck<br />
>**Beispiel:** `tac datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-s <z>`, `--separator=<z>` | verwendet z als Zeilentrenner statt des Zeilenumbruchs |
| `-r`, `--regex` | interpretiert den Trenner als regulären Ausdruck |
| `-b`, `--before` | setzt den Trenner vor statt nach den Abschnitt |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
tac datei.txt        # letzte Zeile zuerst
tac log.txt | head   # die neuesten Log-Zeilen zuerst ansehen
who | tac            # Ausgabe einer Pipe umkehren
```

</details>

>**Hinweis:** Der Name ist `cat` rückwärts gelesen. Praktisch bei Logdateien, um die neuesten Einträge oben zu sehen (oft `tac datei | less`).

---

### tee
>**Funktion:** Eingabe gleichzeitig auf den Bildschirm und in Datei(en) schreiben<br />
>**Syntax:** `tee [optionen] [<datei>...]`<br />
>**Erklärung:** Liest von der Standardeingabe und schreibt sie zugleich auf die Standardausgabe und in eine oder mehrere Dateien („T-Stück" in einer Pipe).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` hängt an die Datei an, statt sie zu überschreiben<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert das Unterbrechungssignal (`Ctrl + C`)<br />
>**Beispiel:** `ls -l | tee liste.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-a`, `--append` | hängt an die Datei(en) an, statt sie zu überschreiben |
| `-i`, `--ignore-interrupts` | ignoriert das Unterbrechungssignal (`Ctrl + C`) |
| `-p` | bricht bei Schreibfehlern in Pipes weniger streng ab |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
ls -l | tee liste.txt                 # Ausgabe anzeigen und speichern
ls -l | tee a.txt b.txt               # in mehrere Dateien zugleich schreiben
echo "neu" | tee -a log.txt           # an eine Datei anhängen
ps aux | tee prozesse.txt | grep ssh  # speichern und gleichzeitig weiterfiltern
echo "wert" | sudo tee /proc/sys/datei  # mit sudo in eine geschützte Datei schreiben
```

</details>

>**Hinweis:** Praktisch, um eine Ausgabe zu speichern und trotzdem weiterzuverarbeiten. `sudo befehl > datei` schlägt bei geschützten Dateien fehl (die Shell leitet ohne root-Rechte um) – `befehl | sudo tee datei` löst das.

---

### tr
>**Funktion:** Zeichen ersetzen oder löschen<br />
>**Syntax:** `tr [optionen] <satz1> [<satz2>]`<br />
>**Erklärung:** Wandelt oder entfernt einzelne Zeichen aus einem Eingabestrom (translate).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` löscht die angegebenen Zeichen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` fasst wiederholte Zeichen zu einem zusammen<br />
>**Beispiel:** `echo "hallo" | tr 'a-z' 'A-Z'`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-d`, `--delete` | löscht die angegebenen Zeichen |
| `-s`, `--squeeze-repeats` | fasst wiederholte Zeichen zu einem zusammen |
| `-c`, `--complement` | wirkt auf alle Zeichen **außer** den angegebenen |
| `-t`, `--truncate-set1` | kürzt Satz 1 auf die Länge von Satz 2 |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
echo "hallo" | tr 'a-z' 'A-Z'       # in Großbuchstaben umwandeln
echo "a-b-c" | tr '-' '_'           # Bindestriche durch Unterstriche ersetzen
echo "Hallo Welt" | tr -d ' '       # alle Leerzeichen löschen
echo "aaa   bbb" | tr -s ' '        # mehrfache Leerzeichen zusammenfassen
cat datei.txt | tr -cd '0-9'        # nur die Ziffern behalten
```

</details>

>**Hinweis:** `tr` liest nur aus der Eingabe (Pipe oder Umleitung mit `<`), nicht direkt aus einer als Argument genannten Datei.

---

### uniq
>**Funktion:** Aufeinanderfolgende doppelte Zeilen entfernen<br />
>**Syntax:** `uniq [optionen] [<datei>...]`<br />
>**Erklärung:** Fasst direkt aufeinanderfolgende gleiche Zeilen zu einer zusammen. Da nur benachbarte Zeilen verglichen werden, ist die Datei vorher meist mit `sort` zu sortieren.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` stellt jeder Zeile die Anzahl ihrer Vorkommen voran (count)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt nur Zeilen, die mehrfach vorkommen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` zeigt nur Zeilen, die genau einmal vorkommen<br />
>**Beispiel:** `sort datei.txt | uniq`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-c`, `--count` | stellt jeder Zeile die Anzahl ihrer Vorkommen voran |
| `-d`, `--repeated` | zeigt nur Zeilen, die mehrfach vorkommen |
| `-u`, `--unique` | zeigt nur Zeilen, die genau einmal vorkommen |
| `-i`, `--ignore-case` | ignoriert Groß- und Kleinschreibung beim Vergleich |
| `-f <n>`, `--skip-fields=<n>` | überspringt beim Vergleich die ersten n Felder |
| `-s <n>`, `--skip-chars=<n>` | überspringt beim Vergleich die ersten n Zeichen |
| `-w <n>`, `--check-chars=<n>` | vergleicht nur die ersten n Zeichen je Zeile |
| `-D` | zeigt alle doppelten Zeilen, nicht nur eine je Gruppe |
| `--group` | zeigt alle Zeilen, Gruppen durch eine Leerzeile getrennt |
| `-z`, `--zero-terminated` | trennt Zeilen mit dem Nullbyte |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sort datei.txt | uniq            # doppelte Zeilen entfernen (vorher sortieren!)
sort datei.txt | uniq -c         # mit Anzahl der Vorkommen je Zeile
sort datei.txt | uniq -d         # nur die mehrfach vorkommenden Zeilen
sort datei.txt | uniq -u         # nur die einmaligen Zeilen
sort zugriffe.log | uniq -c | sort -nr   # häufigste Zeilen zuerst (Rangliste)
```

</details>

>**Hinweis:** `uniq` vergleicht nur **benachbarte** Zeilen – ohne vorheriges `sort` bleiben weiter auseinanderliegende Dubletten erhalten. `sort -u` entfernt Dubletten in einem Schritt, kann aber nicht zählen; für eine Häufigkeitsliste ist `sort | uniq -c` der Weg.

---

### wc
>**Funktion:** Zeilen, Wörter und Zeichen zählen<br />
>**Syntax:** `wc [optionen] [<datei>...]`<br />
>**Erklärung:** Zählt standardmäßig Zeilen, Wörter und Bytes einer Datei oder Eingabe (word count).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zählt nur die Zeilen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-w` zählt nur die Wörter<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` zählt nur die Bytes<br />
>**Beispiel:** `wc -l datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--lines` | zählt nur die Zeilen |
| `-w`, `--words` | zählt nur die Wörter |
| `-c`, `--bytes` | zählt nur die Bytes |
| `-m`, `--chars` | zählt nur die Zeichen (bei UTF-8 nicht identisch mit den Bytes) |
| `-L`, `--max-line-length` | gibt die Länge der längsten Zeile aus |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
wc datei.txt                # Zeilen, Wörter und Bytes
wc -l datei.txt             # nur die Zeilenanzahl
wc -w aufsatz.txt           # nur die Wörter zählen
ls | wc -l                  # Anzahl der Einträge im Verzeichnis
grep "fehler" log.txt | wc -l   # Anzahl der Treffer
```

</details>

>**Hinweis:** Sehr nützlich am Ende einer Pipe, um Ergebnisse zu zählen, z. B. `grep … | wc -l` für die Anzahl der Treffer.

---

### xargs
>**Funktion:** Eingabe in Argumente für einen anderen Befehl umwandeln<br />
>**Syntax:** `xargs [optionen] [<befehl>]`<br />
>**Erklärung:** Liest Elemente von der Standardeingabe und übergibt sie als Argumente an den angegebenen Befehl. Nützlich, wenn ein Befehl die Daten nicht über eine Pipe, sondern als Argumente erwartet (z. B. `rm`).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <z>` höchstens z Argumente pro Aufruf<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-I <zeichen>` setzt jedes Element an die Stelle des Platzhalters ein<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-0` Elemente sind durch Null-Bytes getrennt (mit `find -print0`)<br />
>**Beispiel:** `ls *.txt | xargs rm`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n <z>`, `--max-args=<z>` | höchstens z Argumente pro Befehlsaufruf |
| `-I <zeichen>`, `--replace=<zeichen>` | ersetzt den Platzhalter durch jedes Element (z. B. `-I {}`) |
| `-0`, `--null` | Elemente sind durch Null-Bytes getrennt (sicher bei Leerzeichen; mit `find -print0`) |
| `-d <z>`, `--delimiter=<z>` | legt ein eigenes Trennzeichen fest |
| `-p`, `--interactive` | fragt vor jedem Aufruf nach |
| `-r`, `--no-run-if-empty` | führt den Befehl bei leerer Eingabe nicht aus |
| `-P <n>`, `--max-procs=<n>` | führt bis zu n Aufrufe parallel aus |
| `-t`, `--verbose` | zeigt den jeweils ausgeführten Befehl an |
| `-a <datei>`, `--arg-file=<datei>` | liest die Elemente aus einer Datei statt von der Standardeingabe |
| `-L <n>`, `--max-lines=<n>` | höchstens n Eingabezeilen pro Befehlsaufruf |
| `-s <n>`, `--max-chars=<n>` | begrenzt die Länge der Befehlszeile auf n Zeichen |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
ls *.txt | xargs rm                          # alle .txt-Dateien löschen
find . -name "*.log" | xargs rm              # gefundene Dateien löschen
find . -name "*.log" -print0 | xargs -0 rm   # sicher bei Leerzeichen in Namen
echo "a b c" | xargs -n 1 echo               # je Argument ein eigener Aufruf
ls *.jpg | xargs -I {} cp {} /backup/        # jede Datei einzeln kopieren ({} = Platzhalter)
cat urls.txt | xargs -P 4 -n 1 wget          # bis zu 4 Downloads parallel
```

</details>

>**Hinweis:** Bei Dateinamen mit Leerzeichen `find … -print0 | xargs -0` verwenden, sonst werden Namen falsch aufgeteilt. Mit `-I {}` legt man die Position der Argumente frei fest, mit `-P` arbeitet `xargs` parallel.

---