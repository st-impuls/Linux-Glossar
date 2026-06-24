[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Navigation & Suche](navigation-search.md)
- [Netzwerk & Download](network-download.md)
- [Paketverwaltung](package-management.md)
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Dateiinhalt anzeigen

### cat
>**Funktion:** Dateiinhalt anzeigen | Extern<br />
>**Syntax:** `cat [optionen] [<datei>...]`<br />
>**Erklärung:** Gibt den gesamten Inhalt einer Datei direkt im Terminal aus (concatenate).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` nummeriert alle Zeilen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` nummeriert nur nicht leere Zeilen<br />
>**Beispiel:** `cat datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n`, `--number` | nummeriert alle Zeilen |
| `-b`, `--number-nonblank` | nummeriert nur nicht leere Zeilen (überschreibt `-n`) |
| `-s`, `--squeeze-blank` | fasst mehrere Leerzeilen zu einer zusammen |
| `-E`, `--show-ends` | zeigt am Zeilenende ein `$` an |
| `-T`, `--show-tabs` | zeigt Tabulatoren als `^I` an |
| `-A`, `--show-all` | zeigt alle Sonderzeichen sichtbar an (entspricht `-vET`) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
cat datei.txt                   # Inhalt anzeigen
cat -n script.sh                # mit Zeilennummern
cat teil1.txt teil2.txt         # mehrere Dateien aneinanderhängen
cat datei1.txt datei2.txt > alles.txt   # zusammenführen und speichern
cat > notiz.txt                 # Eingabe direkt in eine Datei schreiben (Abschluss: Ctrl + D)
```

</details>

>**Hinweis:** Mehrere Dateien lassen sich aneinanderhängen, z. B. `cat teil1.txt teil2.txt`. Für lange Dateien ist `less` besser geeignet, da `cat` alles auf einmal ausgibt.

---

### head
>**Funktion:** Anfang einer Datei anzeigen | Extern<br />
>**Syntax:** `head [optionen] [<datei>...]`<br />
>**Erklärung:** Gibt standardmäßig die ersten 10 Zeilen einer Datei aus.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <z>` zeigt die ersten z Zeilen an (z. B. `-n 20`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <n>` zeigt die ersten n Bytes an<br />
>**Beispiel:** `head -n 20 datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n <z>`, `--lines=<z>` | zeigt die ersten z Zeilen an (`-n -5` = alle außer den letzten 5) |
| `-c <n>`, `--bytes=<n>` | zeigt die ersten n Bytes an |
| `-q`, `--quiet` | unterdrückt die Dateinamen-Kopfzeilen bei mehreren Dateien |
| `-v`, `--verbose` | zeigt immer die Dateinamen-Kopfzeile an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
head datei.txt              # die ersten 10 Zeilen
head -n 20 datei.txt        # die ersten 20 Zeilen
head -c 100 datei.txt       # die ersten 100 Bytes
head -n 5 *.log             # die ersten 5 Zeilen jeder Logdatei
```

</details>

>**Hinweis:** Mit `head` und `tail` lassen sich gezielt Zeilenbereiche zeigen, z. B. die Zeilen 11–20 mit `head -n 20 datei.txt | tail -n 10`.

---

### less
>**Funktion:** Datei seitenweise anzeigen | Extern<br />
>**Syntax:** `less [optionen] <datei>...`<br />
>**Erklärung:** Öffnet eine Datei im Lesemodus; man kann vor- und zurückblättern. Beenden mit `q`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-N` zeigt Zeilennummern an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` Suche ignoriert Groß-/Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`+F` folgt der Datei live (wie `tail -f`)<br />
>**Beispiel:** `less /var/log/syslog`

<details markdown>
<summary>Mehr Optionen</summary>

- `-?` zeigt eine Übersicht aller Befehle an (wie die Taste `h`)
- `-N` zeigt Zeilennummern an
- `-i` Suche ignoriert Groß-/Kleinschreibung (außer bei Großbuchstaben im Muster)
- `-I` Suche ignoriert Groß-/Kleinschreibung immer
- `-S` schneidet lange Zeilen ab, statt sie umzubrechen
- `-R` stellt Farb- und Steuerzeichen korrekt dar (raw)
- `-F` beendet sich automatisch, wenn der Inhalt auf eine Seite passt
- `-X` löscht den Bildschirm beim Beenden nicht (Inhalt bleibt im Terminal)
- `-E` beendet `less` automatisch beim ersten Erreichen des Dateiendes
- `-e` beendet `less` automatisch beim zweiten Erreichen des Dateiendes (wenn man am Dateiende erneut vorwärts blättert, zweites Erreichen)
- `-c` baut den Bildschirm beim Neuzeichnen von oben nach unten neu auf (statt zu scrollen)
- `-C` wie `-c`, löscht aber den Bildschirm vor dem Neuaufbau
- `+F` startet direkt im Folgemodus (wie `tail -f`)
- `+<n>` startet bei Zeile n (z. B. `+100`)
- `+/<muster>` springt direkt zur ersten Fundstelle

</details>

<details markdown>
<summary>Interaktive Befehle (Tasten im Programm)</summary>

| Taste | Wirkung |
|---|---|
| `Leertaste` / `f` | eine Seite vorwärts |
| `b` | eine Seite zurück |
| `j` / `↓` | eine Zeile vorwärts |
| `k` / `↑` | eine Zeile zurück |
| `g` | zum Anfang der Datei |
| `G` | zum Ende der Datei |
| `/Suchbegriff` | vorwärts suchen |
| `?Suchbegriff` | rückwärts suchen |
| `n` / `N` | nächster / vorheriger Treffer |
| `F` | dem Dateiende folgen (wie `tail -f`), Abbruch mit `Ctrl + C` |
| `h` | Hilfe anzeigen |
| `q` | beenden |
| `:n` | zur nächsten Datei wechseln |
| `:p` | zur vorherigen Datei wechseln |
| `m` + Buchstabe | Markierung an der aktuellen Position setzen |
| `'` + Buchstabe | zur gesetzten Markierung zurückspringen |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

- `less -N script.sh` – mit Zeilennummern öffnen
- `less +F /var/log/syslog` – Datei live verfolgen (Abbruch mit `Ctrl + C`)
- `less +/Fehler logfile.txt` – direkt zur ersten Fundstelle „Fehler" springen
- `grep -i fehler *.log | less` – die Ausgabe einer Pipe seitenweise lesen
- `less datei1.txt datei2.txt` – mehrere Dateien gleichzeitig öffnen (mit `:n` / `:p` zwischen ihnen wechseln)

</details>

>**Hinweis:** `less` ist der Standard-Pager von `man` und vielen anderen Programmen – die Tasten oben funktionieren dort genauso.

---

### more
>**Funktion:** Datei seitenweise anzeigen (nur vorwärts) | Extern<br />
>**Syntax:** `more [optionen] <datei>...`<br />
>**Erklärung:** Zeigt eine Datei schrittweise an. Im Gegensatz zu `less` kann nur vorwärts geblättert werden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt eine Bedienhilfe am unteren Rand an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-<n>` zeigt jeweils n Zeilen pro Bildschirm an<br />
>**Beispiel:** `more datei.txt`

<details markdown>
<summary>Interaktive Befehle (Tasten im Programm)</summary>

| Taste | Wirkung |
|---|---|
| `Leertaste` | eine Seite vorwärts |
| `Enter` | eine Zeile vorwärts |
| `/Suchbegriff` | vorwärts suchen |
| `q` | beenden |

</details>

>**Hinweis:** `less` ist moderner (auch Zurückblättern möglich) und meist die bessere Wahl.

---

### tail
>**Funktion:** Ende einer Datei anzeigen | Extern<br />
>**Syntax:** `tail [optionen] [<datei>...]`<br />
>**Erklärung:** Gibt standardmäßig die letzten 10 Zeilen einer Datei aus.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <z>` zeigt die letzten z Zeilen an (z. B. `-n 20`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` folgt der Datei live und zeigt neue Zeilen sofort an<br />
>**Beispiel:** `tail -f /var/log/syslog`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n <z>`, `--lines=<z>` | zeigt die letzten z Zeilen an (`-n +5` = ab Zeile 5 bis zum Ende) |
| `-c <n>`, `--bytes=<n>` | zeigt die letzten n Bytes an |
| `-f`, `--follow` | folgt der Datei live und zeigt neue Zeilen sofort an |
| `-F` | wie `-f`, folgt der Datei auch nach Rotation/Neuanlage |
| `-q`, `--quiet` | unterdrückt die Dateinamen-Kopfzeilen bei mehreren Dateien |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
tail datei.txt              # die letzten 10 Zeilen
tail -n 20 datei.txt        # die letzten 20 Zeilen
tail -f /var/log/syslog     # neue Zeilen live mitlesen (Abbruch mit Ctrl + C)
tail -n +5 datei.txt        # ab Zeile 5 bis zum Ende
```

</details>

>**Hinweis:** `-f` ist praktisch, um Logdateien in Echtzeit mitzulesen. Bei rotierenden Logs ist `-F` zuverlässiger, da es der Datei auch nach einem Neuanlegen weiter folgt.

---