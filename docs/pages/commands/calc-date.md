[← Home](../../index.md)
- [Grundlagen](../basics/index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

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
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Rechnen & Datum

### bc
>**Funktion:** Rechner mit beliebiger Genauigkeit<br />
>**Syntax:** `bc [optionen] [<datei>...]`<br />
>**Erklärung:** Wertet mathematische Ausdrücke aus – auch mit Nachkommastellen und sehr großen Zahlen. Im Kern eine kleine Programmiersprache mit Variablen, Schleifen und Funktionen. Liest aus der Eingabe (Pipe/Here-String) oder arbeitet interaktiv.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` lädt die mathematische Bibliothek und setzt `scale=20`<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-q` startet ohne Begrüßungstext (quiet)<br />
>**Beispiel:** `echo "scale=2; 10 / 3" | bc`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--mathlib` | lädt die mathematische Bibliothek (`sqrt`, `s`, `c`, `l`, `e`) und setzt `scale=20` |
| `-q`, `--quiet` | unterdrückt den Begrüßungstext beim Start |
| `-P`, `--no-prompt` | zeigt keine Eingabeaufforderung an |
| `-i`, `--interactive` | erzwingt den interaktiven Modus |
| `-w`, `--warn` | warnt bei nicht-POSIX-Konstrukten |
| `-s`, `--standard` | verarbeitet strikt nach POSIX-`bc` |
| `-h`, `--help` | zeigt die Hilfe an |
| `-v`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Rechen-Grundlagen</summary>

- `scale` = Anzahl der Nachkommastellen (Standard `0` → Division wird abgeschnitten).
- Operatoren: `+`, `-`, `*`, `/`, `%` (Rest), `^` (Potenz).
- `ibase` / `obase` = Zahlenbasis für Eingabe / Ausgabe (z. B. Hex/Binär).
- Funktionen mit `-l`: `sqrt(x)` Wurzel, `s(x)` Sinus, `c(x)` Cosinus, `l(x)` nat. Logarithmus, `e(x)` Exponent.

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
echo "2 + 3 * 4" | bc            # 14
echo "scale=4; 22/7" | bc        # 3.1428
echo "sqrt(2)" | bc -l           # 1.41421356237309504880
echo "2^16" | bc                 # 65536
bc <<< "ibase=16; FF"            # 255 (Hexadezimal -> Dezimal)
ergebnis=$(echo "5 * 5" | bc)    # Ergebnis in einer Variablen speichern
```

</details>

>**Hinweis:** Ohne `scale` schneidet die Division auf ganze Zahlen ab (`10/3` → `3`); `bc -l` setzt `scale=20`. Oft nicht vorinstalliert (`sudo apt install bc`). Für reine Ganzzahl-Rechnung genügt die Shell selbst: `$(( 2 + 3 ))`.

---

### cal
>**Funktion:** Kalender anzeigen<br />
>**Syntax:** `cal [optionen] [[<monat>] <jahr>]`<br />
>**Erklärung:** Zeigt einen Kalender im Terminal – standardmäßig den aktuellen Monat mit hervorgehobenem heutigen Tag. Mit Argumenten lässt sich ein bestimmter Monat oder ein ganzes Jahr anzeigen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-y` zeigt das ganze aktuelle Jahr<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-3` zeigt vorigen, aktuellen und nächsten Monat<br />
>**Beispiel:** `cal`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-y`, `--year` | zeigt den Kalender für das ganze (aktuelle) Jahr |
| `-3`, `--three` | zeigt den vorigen, aktuellen und nächsten Monat |
| `-1`, `--one` | zeigt nur den aktuellen Monat (Standard) |
| `-m`, `--monday` | beginnt die Woche am Montag |
| `-s`, `--sunday` | beginnt die Woche am Sonntag |
| `-w`, `--week` | zeigt zusätzlich die Kalenderwochen-Nummern |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
cal          # aktueller Monat
cal 2026     # ganzes Jahr 2026
cal 12 2026  # Dezember 2026
cal -3       # drei Monate um den aktuellen
```

</details>

>**Hinweis:** Die verwandte Variante `ncal` zeigt den Kalender spaltenweise und kann u. a. den Wochentag eines Datums oder den Ostersonntag (`ncal -e`) berechnen. Der Wochenbeginn hängt von der Region ab – mit `-m`/`-s` festlegbar.

---

### date
>**Funktion:** Datum und Uhrzeit anzeigen oder setzen<br />
>**Syntax:** `date [optionen] [+<format>]`<br />
>**Erklärung:** Zeigt das aktuelle Datum und die Uhrzeit an; mit einem Format-String lässt sich die Ausgabe frei gestalten. Als Administrator kann auch die Systemzeit gesetzt werden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`+<format>` legt das Ausgabeformat fest (z. B. `+%Y-%m-%d`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <datum>` zeigt ein anderes Datum statt jetzt an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` nutzt UTC statt der lokalen Zeit<br />
>**Beispiel:** `date +%Y-%m-%d`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `+<format>` | legt das Ausgabeformat fest (Platzhalter siehe unten) |
| `-d <datum>`, `--date=<datum>` | zeigt ein anderes Datum an (auch „menschlich": `"yesterday"`, `"2 days ago"`) |
| `-f <datei>`, `--file=<datei>` | verarbeitet je eine Datumsangabe pro Zeile aus einer Datei (wie `-d` mehrfach) |
| `-u`, `--utc` | nutzt UTC statt der lokalen Zeitzone |
| `-r <datei>`, `--reference=<datei>` | zeigt die letzte Änderungszeit einer Datei |
| `-s <datum>`, `--set=<datum>` | setzt die Systemzeit (root nötig) |
| `-I[<genauigkeit>]`, `--iso-8601` | Ausgabe im ISO-8601-Format |
| `-R`, `--rfc-email` | Ausgabe im RFC-5322-Format (z. B. für E-Mail-Header) |
| `--rfc-3339=<genauigkeit>` | Ausgabe im RFC-3339-Format (`date`, `seconds` oder `ns`) |
| `--debug` | erläutert das geparste Datum und warnt bei fragwürdiger Eingabe |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Häufige Format-Platzhalter</summary>

| Platzhalter | Bedeutung |
|---|---|
| `%Y` | Jahr (vierstellig) |
| `%m` | Monat (01–12) |
| `%d` | Tag (01–31) |
| `%H` / `%M` / `%S` | Stunde / Minute / Sekunde |
| `%A` / `%a` | Wochentag lang / kurz |
| `%B` / `%b` | Monatsname lang / kurz |
| `%j` | Tag des Jahres (001–366) |
| `%s` | Unix-Zeit (Sekunden seit 1970) |
| `%F` | Kurzform für `%Y-%m-%d` |
| `%T` | Kurzform für `%H:%M:%S` |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
date                          # vollständiges Datum und Uhrzeit
date +%Y-%m-%d                # 2026-06-24
date +"%d.%m.%Y %H:%M"        # 24.06.2026 15:30
date -d "2 days ago" +%F      # Datum von vorgestern
date -u                       # aktuelle Zeit in UTC
date +%s                      # Unix-Zeitstempel
tar -cf backup_$(date +%F).tar ordner/   # Dateiname mit aktuellem Datum
```

</details>

>**Hinweis:** Sehr praktisch in Skripten für Zeitstempel in Datei- oder Log-Namen, z. B. `log_$(date +%F).txt`. Das Setzen der Zeit mit `-s` erfordert root-Rechte; auf modernen Systemen übernimmt das meist `timedatectl` bzw. NTP.

---

### dc
>**Funktion:** Rechner in umgekehrter polnischer Notation (RPN)<br />
>**Syntax:** `dc [optionen] [<datei>]`<br />
>**Erklärung:** Ein stapelbasierter Rechner: Zahlen werden zuerst auf einen Stapel gelegt, danach folgt der Operator (umgekehrte polnische Notation). Verwandt mit `bc`, aber ohne die gewohnte Infix-Schreibweise.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-e <ausdruck>` führt einen Ausdruck direkt aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <datei>` führt Befehle aus einer Datei aus<br />
>**Beispiel:** `dc -e "3 4 + p"`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-e <ausdruck>`, `--expression` | führt den angegebenen Ausdruck aus |
| `-f <datei>`, `--file` | liest die Befehle aus einer Datei |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
dc -e "3 4 + p"          # 3 + 4 = 7 (p gibt das Ergebnis aus)
dc -e "5 1 2 + 4 * + p"  # 5 + (1+2)*4 = 17
echo "10 3 / p" | dc     # per Pipe
```

</details>

>**Hinweis:** Die Bedienung ist ungewohnt: erst die Operanden, dann der Operator; `p` gibt den obersten Stapelwert aus. Für „normale" Rechnungen in Infix-Schreibweise (z. B. `3 + 4`) ist `bc` bequemer.

---

### expr
>**Funktion:** Ausdrücke auswerten (Arithmetik und Zeichenketten)<br />
>**Syntax:** `expr <ausdruck>`<br />
>**Erklärung:** Berechnet einfache Ausdrücke und gibt das Ergebnis aus – vor allem Ganzzahl-Arithmetik, aber auch Vergleiche und einige Zeichenketten-Operationen. Häufig in älteren Shell-Skripten.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;Operanden und Operatoren durch Leerzeichen trennen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`*` als Multiplikation maskieren: `\*`<br />
>**Beispiel:** `expr 3 + 4`

<details markdown>
<summary>Operatoren und Funktionen</summary>

| Ausdruck | Wirkung |
|---|---|
| `a + b`, `a - b` | Addition, Subtraktion |
| `a \* b`, `a / b`, `a % b` | Multiplikation, Division, Rest (`*` muss maskiert werden) |
| `a = b`, `a != b`, `a < b`, `a > b` | Vergleiche (Ergebnis `1` oder `0`) |
| `length <string>` | Länge einer Zeichenkette |
| `substr <string> <pos> <länge>` | Teilzeichenkette ab einer Position |
| `index <string> <zeichen>` | Position des ersten Treffers |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
expr 3 + 4            # 7
expr 10 \* 3         # 30 (Stern maskieren!)
expr 17 % 5          # 2
expr length "hallo"  # 5
i=$(expr $i + 1)     # Zähler in einem Skript erhöhen
```

</details>

>**Hinweis:** Leerzeichen um die Operatoren sind Pflicht (`expr 3+4` funktioniert nicht). In modernen Bash-Skripten wird `expr` meist durch die eingebaute Arithmetik `$(( … ))` oder `let` ersetzt.

---

### factor
>**Funktion:** Zahlen in Primfaktoren zerlegen<br />
>**Syntax:** `factor [<zahl>...]`<br />
>**Erklärung:** Gibt zu jeder angegebenen Zahl ihre Primfaktoren aus. Ohne Argumente liest `factor` die Zahlen von der Standardeingabe.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`factor <zahl>...` zerlegt eine oder mehrere Zahlen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;ohne Argument: liest Zahlen von der Eingabe<br />
>**Beispiel:** `factor 84`

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
factor 84          # 84: 2 2 3 7
factor 13          # 13: 13 (Primzahl)
factor 12 100 255  # mehrere Zahlen
seq 1 10 | factor  # eine Zahlenfolge zerlegen
```

</details>

>**Hinweis:** Teil der GNU-Coreutils. Praktisch, um schnell zu prüfen, ob eine Zahl eine Primzahl ist – dann steht nur sie selbst hinter dem Doppelpunkt.

---

### hwclock
>**Funktion:** Hardware-Uhr (RTC) anzeigen und stellen<br />
>**Syntax:** `hwclock [optionen]`<br />
>**Erklärung:** Liest oder stellt die batteriegepufferte Echtzeituhr des Rechners (RTC), die auch im ausgeschalteten Zustand weiterläuft. Beim Start liest das System die Zeit von hier; im Betrieb zählt die separate Systemuhr.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` zeigt die Hardware-Uhr an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-w` schreibt die Systemzeit in die Hardware-Uhr<br />
>**Beispiel:** `sudo hwclock -r`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-r`, `--show` | zeigt die aktuelle Zeit der Hardware-Uhr an |
| `-w`, `--systohc` | stellt die Hardware-Uhr auf die Systemzeit |
| `-s`, `--hctosys` | stellt die Systemzeit auf die Hardware-Uhr |
| `--set --date=<zeit>` | setzt die Hardware-Uhr auf einen bestimmten Wert |
| `--utc` / `--localtime` | legt fest, ob die RTC in UTC oder lokaler Zeit läuft |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo hwclock -r  # Hardware-Uhr anzeigen
sudo hwclock -w  # System- -> Hardware-Uhr
sudo hwclock -s  # Hardware- -> Systemuhr
```

</details>

>**Hinweis:** Braucht `sudo`/root. Auf systemd-Systemen wird die Uhr meist automatisch synchronisiert (NTP); zum Anzeigen/Setzen im Alltag ist `timedatectl` bequemer. „RTC" steht für *Real Time Clock*.

---

### let
>**Funktion:** Arithmetik in der Shell auswerten | Intern (Builtins)<br />
>**Syntax:** `let <ausdruck>...`<br />
>**Erklärung:** Wertet einen arithmetischen Ausdruck aus und weist das Ergebnis meist einer Variablen zu. Anders als `expr` braucht es keine Leerzeichen und kein Maskieren von `*`. Ein Bash-Builtin, nur für Ganzzahlen.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`let "x = 2 + 3"` weist zu (Anführungszeichen bei Leerzeichen)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`let i++` erhöht eine Variable<br />
>**Beispiel:** `let "x = 3 * 4"`

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
let "x = 3 * 4"        # x = 12 (kein Maskieren nötig)
let i++                # i um 1 erhöhen
let "y = (2 + 3) * 4"  # y = 20
echo $x $y
```

</details>

>**Hinweis:** Builtin von Bash. Gleichwertig und oft übersichtlicher ist die Arithmetik-Erweiterung `$(( … ))`, z. B. `x=$(( 3 * 4 ))`. Kommazahlen kann `let` nicht – dafür `bc`.

---

### numfmt
>**Funktion:** Zahlen lesbar formatieren und umrechnen<br />
>**Syntax:** `numfmt [optionen] [<zahl>...]`<br />
>**Erklärung:** Wandelt Zahlen zwischen einfacher Schreibweise und lesbaren Einheiten um – z. B. `1048576` ↔ `1,0M`. Nützlich, um große Byte- oder SI-Werte übersichtlich darzustellen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--to=iec` gibt lesbare Größen aus (K, M, G …)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--from=iec` liest lesbare Größen ein<br />
>**Beispiel:** `numfmt --to=iec 1048576`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `--to=<einheit>` | Ausgabeformat: `si` (1000er) oder `iec` (1024er) |
| `--from=<einheit>` | Eingabeformat: `si`, `iec` oder `auto` |
| `--suffix=<text>` | hängt eine Einheit an (z. B. `B` für Byte) |
| `--padding=<n>` | richtet die Ausgabe auf n Stellen aus |
| `--grouping` | gruppiert Tausender gemäß der Locale |
| `--field=<n>` | formatiert nur ein bestimmtes Feld der Eingabe |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
numfmt --to=iec 1048576  # 1,0M
numfmt --from=iec 1,0M   # 1048576
numfmt --to=si 1500      # 1,5K
```

</details>

>**Hinweis:** Teil der GNU-Coreutils. `iec` nutzt 1024er-Schritte (K, M, G …), `si` 1000er-Schritte (kB, MB …). Praktisch in Pipes, um Roh-Byte-Werte lesbar zu machen.

---

### seq
>**Funktion:** Zahlenfolgen erzeugen<br />
>**Syntax:** `seq [optionen] [<start>] [<schritt>] <ende>`<br />
>**Erklärung:** Gibt eine Folge von Zahlen aus – von `start` bis `ende`, standardmäßig in Einerschritten. Oft in Schleifen und Pipes verwendet.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-w` füllt mit führenden Nullen auf gleiche Breite<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <trenn>` setzt ein Trennzeichen<br />
>**Beispiel:** `seq 1 10`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-w`, `--equal-width` | füllt die Zahlen mit führenden Nullen auf gleiche Breite |
| `-s <text>`, `--separator` | trennt die Werte mit `text` (Standard: Zeilenumbruch) |
| `-f <format>`, `--format` | formatiert die Zahlen im printf-Stil (z. B. `%.2f`) |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
seq 5         # 1 bis 5
seq 1 10      # 1 bis 10
seq 0 2 10    # 0,2,4,6,8,10 (Schrittweite 2)
seq -w 1 10   # 01,02,…,10
seq -s, 1 5   # 1,2,3,4,5
```

</details>

>**Hinweis:** Teil der GNU-Coreutils. In Bash ist für einfache Ganzzahl-Bereiche oft die Klammer-Erweiterung `{1..10}` schneller; `seq` ist flexibler bei Schrittweite und Formatierung (`-f`).

---
