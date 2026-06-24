[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Navigation & Suche](navigation-search.md)
- [Netzwerk & Download](network-download.md)
- [Paketverwaltung](package-management.md)
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [System & Dienste](system-services.md)

[← Zurück zur Übersicht](index.md)

# Textbearbeitung & Filter

### cut
>**Funktion:** Spalten oder Felder ausschneiden | Extern<br />
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

### grep
>**Funktion:** Nach Text suchen | Extern<br />
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

### sort
>**Funktion:** Zeilen sortieren | Extern<br />
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

### tee
>**Funktion:** Eingabe gleichzeitig auf den Bildschirm und in Datei(en) schreiben | Extern<br />
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
>**Funktion:** Zeichen ersetzen oder löschen | Extern<br />
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
>**Funktion:** Aufeinanderfolgende doppelte Zeilen entfernen | Extern<br />
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
>**Funktion:** Zeilen, Wörter und Zeichen zählen | Extern<br />
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
>**Funktion:** Eingabe in Argumente für einen anderen Befehl umwandeln | Extern<br />
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