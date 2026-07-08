[← Home](../../index.md)
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
- [Rechnen & Datum](calc-date.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Shell & Skripte

### alias
>**Funktion:** Abkürzungen für Befehle anlegen | Intern (Builtins)<br />
>**Syntax:** `alias [<name>='<befehl>']`<br />
>**Erklärung:** Legt eine Abkürzung (Alias) für einen längeren Befehl an. Ohne Argumente werden alle definierten Aliase angezeigt.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`alias` listet alle Aliase auf<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`alias <name>='<befehl>'` legt einen Alias an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`alias -p` gibt alle Aliase in wiederverwendbarer Form aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`unalias <name>` entfernt einen Alias<br />
>**Beispiel:** `alias ll='ls -la'`

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
alias                 # alle Aliase anzeigen
alias ll='ls -la'     # Abkürzung anlegen
alias gs='git status'
alias ..='cd ..'
unalias ll            # Alias wieder entfernen
```

</details>

>**Hinweis:** Ein im Terminal angelegter Alias gilt nur für die aktuelle Sitzung. Dauerhaft werden Aliase in `~/.bashrc` (oder `~/.bash_aliases`) eingetragen. Den eigentlichen Befehl hinter einem Alias umgeht man mit vorangestelltem Backslash, z. B. `\ls`.

---

### echo
>**Funktion:** Text ausgeben | Intern (Builtins)<br />
>**Syntax:** `echo [optionen] [<text>...]`<br />
>**Erklärung:** Gibt den übergebenen Text oder den Wert einer Variablen im Terminal aus.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` gibt keinen abschließenden Zeilenumbruch aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-e` interpretiert Escape-Sequenzen wie `\n` (Zeilenumbruch) oder `\t` (Tabulator)<br />
>**Beispiel:** `echo "Hallo Welt"`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n` | gibt keinen abschließenden Zeilenumbruch aus |
| `-e` | interpretiert Escape-Sequenzen (`\n` Umbruch, `\t` Tabulator, `\\` Backslash) |
| `-E` | interpretiert keine Escape-Sequenzen (Standard) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
echo "Hallo Welt"               # einfache Textausgabe
echo $HOME                      # Wert einer Variablen ausgeben
echo -e "Zeile1\nZeile2"        # mit interpretiertem Zeilenumbruch
echo "Hallo" > datei.txt        # Ausgabe in eine Datei schreiben
echo "Mehr" >> datei.txt        # an eine bestehende Datei anhängen
echo "hallo" | tr 'a-z' 'A-Z'   # Ausgabe an einen anderen Befehl weitergeben
```

</details>

>**Hinweis:** Variablen werden mit `$` ausgegeben, z. B. `echo $HOME`. Mit `>` lässt sich die Ausgabe in eine Datei umleiten (schreiben); `>>` hängt an eine bestehende Datei an. Oft in Pipes oder Skripten verwendet.

---

### env
>**Funktion:** Umgebung anzeigen oder Befehl mit geänderter Umgebung starten<br />
>**Syntax:** `env [optionen] [<name>=<wert>...] [<befehl>]`<br />
>**Erklärung:** Ohne Argumente zeigt es alle Umgebungsvariablen. Mit Zuweisungen startet es einen Befehl mit zusätzlich oder geändert gesetzten Variablen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` startet mit leerer Umgebung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u <name>` entfernt eine Variable für den Aufruf<br />
>**Beispiel:** `env`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-i`, `--ignore-environment` | startet mit komplett leerer Umgebung |
| `-u <name>`, `--unset=<name>` | entfernt die Variable für diesen Aufruf |
| `-C <verz>`, `--chdir=<verz>` | wechselt vor dem Start ins angegebene Verzeichnis |
| `-0`, `--null` | trennt die Ausgabe mit dem Nullbyte statt mit Zeilenumbruch |
| `-S <text>`, `--split-string=<text>` | zerlegt einen String in mehrere Argumente (für die Shebang-Zeile) |
| `-v`, `--debug` | zeigt jeden Verarbeitungsschritt an |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
env                  # alle Umgebungsvariablen anzeigen
env | grep PATH      # eine bestimmte Variable suchen
env LANG=C date      # date mit geänderter Sprache starten
env -i bash          # Shell mit leerer Umgebung starten
```

</details>

>**Hinweis:** Häufig in der Shebang-Zeile von Skripten: `#!/usr/bin/env python3` findet den Interpreter über den `PATH`, statt ihn fest zu verdrahten. Nur die Variablen zeigt auch `printenv`.

---

### export
>**Funktion:** Variablen an Unterprozesse vererben (Umgebungsvariablen) | Intern (Builtins)<br />
>**Syntax:** `export [<name>[=<wert>]]`<br />
>**Erklärung:** Markiert eine Shell-Variable als Umgebungsvariable, sodass sie auch von gestarteten Programmen und Unter-Shells gesehen wird.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`export <name>=<wert>` setzt und exportiert in einem Schritt<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`export <name>` exportiert eine bestehende Variable<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`export -p` listet alle exportierten Variablen<br />
>**Beispiel:** `export PATH="$PATH:/opt/bin"`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-p` | listet alle exportierten Variablen auf |
| `-n <name>` | hebt den Export auf (Variable bleibt lokal in der Shell) |
| `-f <name>` | exportiert eine Funktion statt einer Variablen |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
export EDITOR=nano             # Standardeditor setzen und vererben
export PATH="$PATH:$HOME/bin"  # eigenes Verzeichnis an PATH anhängen
name="max"; export name        # bestehende Variable nachträglich exportieren
export -p                      # alle exportierten Variablen anzeigen
```

</details>

>**Hinweis:** Ohne `export` ist eine Variable nur in der aktuellen Shell sichtbar, nicht in gestarteten Programmen. Dauerhaft gehören solche Zeilen in `~/.bashrc` oder `~/.profile`.

---

### history
>**Funktion:** Befehlsverlauf anzeigen und verwalten | Intern (Builtins)<br />
>**Syntax:** `history [optionen] [<anzahl>]`<br />
>**Erklärung:** Zeigt die zuletzt eingegebenen Befehle mit Nummer. Mit `!<nummer>` lässt sich ein Befehl erneut ausführen.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`history <n>` zeigt die letzten n Befehle<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` löscht den Verlauf der Sitzung<br />
>**Beispiel:** `history 20`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-c` | löscht den gesamten Verlauf der aktuellen Sitzung |
| `-d <n>` | löscht den Eintrag mit der Nummer n |
| `-w` | schreibt den Verlauf in die History-Datei |
| `-a` | hängt die neuen Zeilen an die History-Datei an |
| `-r` | liest die History-Datei und hängt sie an den Verlauf an |
| `-n` | liest nur die noch nicht gelesenen Zeilen der History-Datei |
| `-p <text>` | führt die History-Expansion aus, ohne etwas zu speichern |
| `-s <text>` | fügt die Argumente als einen Eintrag in den Verlauf ein |

</details>

<details markdown>
<summary>Verlauf erneut ausführen (Kürzel)</summary>

| Eingabe | Wirkung |
|---|---|
| `!!` | wiederholt den letzten Befehl |
| `!<n>` | führt den Befehl mit der Nummer n erneut aus |
| `!<text>` | letzter Befehl, der mit „text" begann |
| `Ctrl + R` | den Verlauf interaktiv rückwärts durchsuchen |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
history       # gesamten Verlauf anzeigen
history 20    # die letzten 20 Befehle
!!            # letzten Befehl wiederholen
!42           # Befehl Nummer 42 erneut ausführen
sudo !!       # letzten Befehl mit sudo wiederholen
history -c    # Verlauf der Sitzung löschen
```

</details>

>**Hinweis:** Der Verlauf wird in `~/.bash_history` gespeichert. Mit `Ctrl + R` lässt er sich rückwärts durchsuchen, mit `Alt + .` fügt man das letzte Argument des vorherigen Befehls ein.

---

### printf
>**Funktion:** Formatierte Textausgabe | Intern (Builtins)<br />
>**Syntax:** `printf <format> [<argument>...]`<br />
>**Erklärung:** Gibt Text nach einer Formatvorlage aus – genauer steuerbar als `echo`, z. B. für Zahlen und Spalten. Anders als `echo` wird kein Zeilenumbruch automatisch angehängt.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`%s` Zeichenkette, `%d` ganze Zahl, `%f` Kommazahl<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`\n` Zeilenumbruch, `\t` Tabulator<br />
>**Beispiel:** `printf "%s ist %d Jahre alt\n" Max 30`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-v <variable>` | schreibt die Ausgabe in eine Variable statt auf die Standardausgabe |

</details>

<details markdown>
<summary>Format-Platzhalter</summary>

| Platzhalter | Bedeutung |
|---|---|
| `%s` | Zeichenkette |
| `%d` / `%i` | ganze Zahl |
| `%f` | Kommazahl (z. B. `%.2f` = 2 Nachkommastellen) |
| `%x` | Hexadezimalzahl |
| `%%` | ein wörtliches Prozentzeichen |
| `\n` / `\t` | Zeilenumbruch / Tabulator |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
printf "Hallo %s\n" Welt          # Hallo Welt
printf "%s ist %d\n" Alter 30
printf "%.2f\n" 3.14159           # 3.14
printf "%-10s|%-10s\n" Name Wert  # linksbündige Spalten
printf "%d\n" {1..3}              # Format wird je Argument wiederholt
```

</details>

>**Hinweis:** Im Gegensatz zu `echo` hängt `printf` keinen Zeilenumbruch an – `\n` muss man selbst angeben. Dafür ist die Ausgabe systemübergreifend einheitlich, weshalb `printf` in Skripten oft `echo` vorgezogen wird.

---

### read
>**Funktion:** Eingabe in Variablen einlesen | Intern (Builtins)<br />
>**Syntax:** `read [optionen] [<variable>...]`<br />
>**Erklärung:** Liest eine Zeile von der Standardeingabe und speichert sie in einer oder mehreren Variablen. Grundbaustein für interaktive Skripte.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p <text>` zeigt einen Eingabeprompt an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` versteckt die Eingabe (z. B. Passwörter)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t <n>` Zeitlimit in Sekunden<br />
>**Beispiel:** `read -p "Name: " name`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-p <text>` | zeigt vor der Eingabe einen Prompt an |
| `-s` | stille Eingabe ohne Echo (z. B. für Passwörter) |
| `-t <n>` | bricht nach n Sekunden ab |
| `-n <z>` | liest nur z Zeichen (ohne Enter) |
| `-a <array>` | liest die Wörter in ein Array |
| `-r` | interpretiert `\` nicht als Escape (empfohlen) |
| `-d <zeichen>` | liest bis zum angegebenen Zeichen statt bis zum Zeilenende |
| `-e` | nutzt die Readline-Bearbeitung (Pfeiltasten, Vervollständigung) |
| `-i <text>` | setzt einen Vorgabetext (nur zusammen mit `-e`) |
| `-N <z>` | liest genau z Zeichen, ohne an Leerzeichen zu trennen |
| `-u <fd>` | liest aus dem angegebenen Dateideskriptor statt von der Tastatur |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
read -p "Name: " name                 # mit Eingabeaufforderung
echo "Hallo $name"
read -s -p "Passwort: " pass; echo    # versteckte Eingabe
read -t 5 antwort                     # 5 Sekunden Zeit
read vorname nachname                 # zwei Wörter in zwei Variablen
while read -r zeile; do echo "$zeile"; done < datei.txt   # Datei zeilenweise lesen
```

</details>

>**Hinweis:** In Schleifen liest `while read -r zeile; do …; done < datei` eine Datei Zeile für Zeile. Die Option `-r` sollte fast immer dabei sein, damit Backslashes nicht falsch interpretiert werden.

---

### set
>**Funktion:** Shell-Optionen und Positionsparameter setzen | Intern (Builtins)<br />
>**Syntax:** `set [optionen] [<argumente>...]`<br />
>**Erklärung:** Steuert das Verhalten der Shell (Optionen) und setzt die Positionsparameter (`$1`, `$2`, …). Ohne Argumente zeigt es alle Variablen und Funktionen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-e` bricht das Skript beim ersten Fehler ab<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` behandelt undefinierte Variablen als Fehler<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-x` zeigt jeden ausgeführten Befehl an (Debug)<br />
>**Beispiel:** `set -e`

<details markdown>
<summary>Häufige Optionen</summary>

| Option | Wirkung |
|---|---|
| `-e` (`-o errexit`) | bricht das Skript beim ersten fehlgeschlagenen Befehl ab |
| `-u` (`-o nounset`) | unbekannte Variablen lösen einen Fehler aus |
| `-x` (`-o xtrace`) | gibt jeden Befehl vor der Ausführung aus (Debugging) |
| `-o pipefail` | eine Pipe gilt als fehlgeschlagen, sobald ein Teil fehlschlägt |
| `-a` (`-o allexport`) | exportiert automatisch alle neu gesetzten Variablen |
| `-f` (`-o noglob`) | schaltet die Dateinamen-Expansion (Globbing) ab |
| `-C` (`-o noclobber`) | verhindert das Überschreiben von Dateien mit `>` |
| `-n` (`-o noexec`) | liest die Befehle nur, ohne sie auszuführen (Syntaxprüfung) |
| `-v` (`-o verbose`) | gibt die Eingabezeilen beim Lesen aus |
| `+e`, `+x` … | schaltet die jeweilige Option wieder aus |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
set -e                # bei Fehler abbrechen
set -x                # Befehle mitprotokollieren (Debug)
set +x                # Debug wieder ausschalten
set -euo pipefail     # robuste Grundeinstellung am Skriptanfang
set -- eins zwei drei # $1=eins, $2=zwei, $3=drei
```

</details>

>**Hinweis:** `set -euo pipefail` am Anfang eines Skripts gilt als sichere Grundeinstellung (Abbruch bei Fehlern, keine undefinierten Variablen). Mit vorangestelltem `+` wird eine Option wieder ausgeschaltet.

---

### source
>**Funktion:** Ein Skript in der aktuellen Shell ausführen | Intern (Builtins)<br />
>**Syntax:** `source <datei> [<argumente>...]` &nbsp;(kurz: `. <datei>`)<br />
>**Erklärung:** Führt die Befehle einer Datei in der aktuellen Shell aus – nicht in einer Unter-Shell. Dadurch bleiben gesetzte Variablen und Funktionen danach erhalten.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`source <datei>` oder gleichbedeutend `. <datei>`<br />
>**Beispiel:** `source ~/.bashrc`

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
source ~/.bashrc            # geänderte Konfiguration neu einlesen
. ~/.bashrc                 # gleichbedeutende Kurzform
source venv/bin/activate    # virtuelle Python-Umgebung aktivieren
source funktionen.sh        # Funktionen in die aktuelle Shell laden
```

</details>

>**Hinweis:** Der Unterschied zu `./skript.sh`: Letzteres startet eine **Unter-Shell**, deren Variablen danach verschwinden. `source` führt im aktuellen Prozess aus – darum übernimmt man so z. B. `~/.bashrc`-Änderungen ohne neues Terminal. `.` und `source` sind identisch.

---

### test
>**Funktion:** Bedingungen prüfen (für Skripte) | Intern (Builtins)<br />
>**Syntax:** `test <ausdruck>` &nbsp;(gleichbedeutend: `[ <ausdruck> ]`)<br />
>**Erklärung:** Wertet einen Ausdruck aus und liefert einen Erfolgs- (wahr) oder Fehlerstatus (falsch) – die Grundlage für `if` und Schleifen. Die Schreibweise `[ … ]` ist dasselbe wie `test …`.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <datei>` Datei existiert<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <verz>` Verzeichnis existiert<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-z <text>` Zeichenkette ist leer<br />
>**Beispiel:** `[ -f datei.txt ] && echo "vorhanden"`

<details markdown>
<summary>Operatoren</summary>

| Ausdruck | Wahr, wenn … |
|---|---|
| `-f <datei>` | Datei existiert und eine reguläre Datei ist |
| `-d <verz>` | Verzeichnis existiert |
| `-e <pfad>` | Pfad existiert (Datei oder Verzeichnis) |
| `-z <text>` | Zeichenkette leer ist |
| `-n <text>` | Zeichenkette nicht leer ist |
| `<a> = <b>` / `<a> != <b>` | Zeichenketten (un)gleich |
| `<a> -eq <b>` | Zahlen gleich (auch `-ne`, `-lt`, `-le`, `-gt`, `-ge`) |
| `-r`/`-w`/`-x <datei>` | Datei ist les-/schreib-/ausführbar |
| `-s <datei>` | Datei existiert und ist nicht leer |
| `-L <pfad>` | Pfad ist ein symbolischer Link |
| `<a> -nt <b>` / `<a> -ot <b>` | Datei a ist neuer / älter als b |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
[ -f datei.txt ] && echo "vorhanden"
[ -d /tmp ] && echo "Verzeichnis da"
if [ "$x" -gt 10 ]; then echo "groß"; fi
[ -z "$var" ] && echo "leer"
test 5 -lt 9 && echo "kleiner"
```

</details>

>**Hinweis:** Innerhalb von `[ ]` müssen um die Klammern und Operatoren Leerzeichen stehen. Variablen am besten in Anführungszeichen setzen (`[ "$x" = "y" ]`), um Fehler bei leeren Werten zu vermeiden. In Bash ist die modernere Form `[[ … ]]` oft robuster.

---

### trap
>**Funktion:** Auf Signale und Ereignisse reagieren | Intern (Builtins)<br />
>**Syntax:** `trap '<befehl>' <signal|ereignis>...`<br />
>**Erklärung:** Legt fest, welcher Befehl ausgeführt wird, wenn ein bestimmtes Signal oder Ereignis eintritt. In Skripten vor allem zum Aufräumen beim Beenden oder Abbrechen.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`trap '<befehl>' EXIT` führt den Befehl beim Skriptende aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`trap - <signal>` setzt auf das Standardverhalten zurück<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`trap -p` zeigt die gesetzten Handler<br />
>**Beispiel:** `trap 'rm -f "$tmp"' EXIT`

<details markdown>
<summary>Signale & Ereignisse</summary>

| Name | Wann |
|---|---|
| `EXIT` | beim Beenden des Skripts (auch bei Fehlern) – ideal zum Aufräumen |
| `INT` | Unterbrechung durch `Ctrl + C` |
| `TERM` | Aufforderung zum Beenden (z. B. von `kill`) |
| `ERR` | nach einem Befehl, der mit Fehler endet |
| `HUP` | das Terminal wurde getrennt |

</details>

<details markdown>
<summary>Besondere Schreibweisen</summary>

| Aufruf | Wirkung |
|---|---|
| `trap '<befehl>' <signal>` | führt den Befehl beim Signal aus |
| `trap '' <signal>` | ignoriert das Signal (leerer Befehl) |
| `trap - <signal>` | stellt das Standardverhalten wieder her |
| `trap -p` | listet die aktuell gesetzten Handler auf |
| `trap -l` | listet alle Signalnamen mit Nummern auf |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
tmp=$(mktemp)
trap 'rm -f "$tmp"' EXIT               # Temp-Datei beim Beenden löschen
trap 'echo Abbruch; exit 1' INT TERM   # auf Ctrl + C reagieren
trap '' INT                            # Ctrl + C vorübergehend ignorieren
trap - INT                             # wieder auf Standard zurücksetzen
trap -p                                # gesetzte Handler anzeigen
```

</details>

>**Hinweis:** `trap '<befehl>' EXIT` ist der zuverlässigste Weg, temporäre Dateien aufzuräumen, da es bei jedem Beenden greift – auch zusammen mit `set -e`. Die Signalnamen sind dieselben wie bei `kill` (siehe `kill -l`).

---

### type
>**Funktion:** Art eines Befehls anzeigen | Intern (Builtins)<br />
>**Syntax:** `type [optionen] <name>...`<br />
>**Erklärung:** Zeigt, um was für eine Art Befehl es sich handelt – Builtin, Alias, Funktion oder externes Programm (mit Pfad).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt alle Fundstellen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t` zeigt nur die Art als Stichwort<br />
>**Beispiel:** `type ls`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-a`, `--all` | zeigt alle passenden Fundstellen (Alias, Builtin, Datei) |
| `-t` | gibt nur die Art aus (`alias`, `builtin`, `function`, `file`) |
| `-p` | gibt nur den Pfad eines externen Programms aus |
| `-P` | sucht ausschließlich im `PATH`, auch wenn ein Builtin/Alias existiert |
| `-f` | ignoriert Shell-Funktionen bei der Suche |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
type ls       # zeigt z. B. einen Alias oder /usr/bin/ls
type -t cd    # builtin
type -a echo  # alle Varianten (Builtin und /usr/bin/echo)
type -p git   # /usr/bin/git
```

</details>

>**Hinweis:** Ähnlich wie `which`, aber `type` kennt auch Aliase, Funktionen und Builtins – nicht nur externe Programme im `PATH`.

---

### unalias
>**Funktion:** Einen Alias wieder entfernen | Intern (Builtins)<br />
>**Syntax:** `unalias [optionen] <name>...`<br />
>**Erklärung:** Löscht einen zuvor mit `alias` angelegten Kurzbefehl. Betrifft nur die aktuelle Shell-Sitzung.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`unalias <name>` entfernt einen Alias<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`unalias -a` entfernt alle Aliase<br />
>**Beispiel:** `unalias ll`

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
unalias ll   # den Alias ll entfernen
unalias -a   # alle Aliase der Sitzung entfernen
alias        # verbleibende Aliase anzeigen
```

</details>

>**Hinweis:** Wirkt nur in der laufenden Sitzung. Dauerhaft angelegte Aliase (z. B. in `~/.bashrc`) kommen bei der nächsten Sitzung zurück – dort die entsprechende `alias`-Zeile löschen. Gegenstück zu `alias`.

---

### unset
>**Funktion:** Variablen oder Funktionen entfernen | Intern (Builtins)<br />
>**Syntax:** `unset [optionen] <name>...`<br />
>**Erklärung:** Löscht eine Shell-Variable oder Funktion, sodass sie danach nicht mehr definiert ist.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v <name>` entfernt eine Variable (Standard)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <name>` entfernt eine Funktion<br />
>**Beispiel:** `unset name`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-v <name>` | entfernt eine Variable (Standard) |
| `-f <name>` | entfernt eine Funktion |
| `-n <name>` | entfernt einen Namensverweis (nameref), nicht dessen Ziel |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
name="max"
unset name              # Variable löschen
echo "$name"            # gibt jetzt nichts mehr aus
unset -f meine_funktion # eine Funktion entfernen
```

</details>

>**Hinweis:** `unset` entfernt die Variable ganz; eine leere Zuweisung (`name=`) lässt sie dagegen als leere Variable bestehen. Aus der Umgebung allein (ohne Löschen) nimmt man eine Variable mit `export -n`.

---
