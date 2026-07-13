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
- [Netzwerk & Download](network-download.md)
- [Paketverwaltung](package-management.md)
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Navigation & Suche

### cd
>**Funktion:** Verzeichnis wechseln | Intern (Builtins)<br />
>**Syntax:** `cd [optionen] [<verzeichnis>]`<br />
>**Erklärung:** Wechselt in ein anderes Verzeichnis (change directory). Ohne Angabe wechselt `cd` ins Home-Verzeichnis.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd ..` wechselt in das übergeordnete Verzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd ~` wechselt in das Home-Verzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd -` wechselt in das zuletzt genutzte Verzeichnis<br />
>**Beispiel:** `cd /home/user`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-L` | folgt symbolischen Links, zeigt den logischen Pfad (Standard) |
| `-P` | nutzt den physischen Pfad und löst symbolische Links auf |
| `-e` | gibt mit `-P` einen Fehlerstatus zurück, wenn das Arbeitsverzeichnis nicht ermittelt werden kann |
| `-@` | stellt eine Datei mit erweiterten Attributen als Verzeichnis dar (selten unterstützt) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
cd                      # ins Home-Verzeichnis wechseln (wie `cd ~`)
cd ..                   # eine Ebene nach oben
cd ../..                # zwei Ebenen nach oben
cd -                    # zurück ins zuletzt genutzte Verzeichnis
cd /var/log             # absoluter Pfad
cd projekt/quellcode    # relativer Pfad
```

</details>

>**Hinweis:** `cd` ist ein Shell-Builtin – es muss die Shell selbst sein, die das Verzeichnis wechselt (ein externes Programm könnte das Arbeitsverzeichnis der Shell nicht ändern).

---

### find
>**Funktion:** Dateien und Ordner suchen<br />
>**Syntax:** `find [<startverzeichnis>] [optionen]`<br />
>**Erklärung:** Durchsucht ein Verzeichnis (und dessen Unterordner) live nach Dateien anhand von Name, Typ, Größe und weiteren Kriterien.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-name <muster>` sucht nach dem Dateinamen (z. B. `-name "*.txt"`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-type f` sucht nur Dateien, `-type d` nur Verzeichnisse<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-size <n><einheit>` sucht nach der Größe (z. B. `+10M` = größer als 10 MiB)<br />
>**Beispiel:** `find /home -name "*.txt"`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-name <muster>` | sucht nach dem Dateinamen (z. B. `"*.txt"`) |
| `-iname <muster>` | wie `-name`, ignoriert Groß-/Kleinschreibung |
| `-type f` / `-type d` | sucht nur Dateien (`f`) bzw. nur Verzeichnisse (`d`) |
| `-size <n><einheit>` | sucht nach Größe (`+10M` größer, `-1k` kleiner; Einheiten siehe unten) |
| `-mtime <n>` | nach Änderungsdatum in Tagen (`-7` = letzte 7 Tage) |
| `-mmin <n>` | nach Änderungsdatum in Minuten |
| `-user <name>` | Dateien eines bestimmten Besitzers |
| `-group <name>` | Dateien einer bestimmten Gruppe |
| `-uid <n>` | wie `-user`, aber nach der numerischen Benutzer-ID |
| `-gid <n>` | wie `-group`, aber nach der numerischen Gruppen-ID |
| `-nouser` | Dateien, deren UID zu keinem Benutzer gehört (verwaiste Dateien) |
| `-nogroup` | Dateien, deren GID zu keiner Gruppe gehört |
| `-perm <modus>` | Dateien mit bestimmten Rechten |
| `-maxdepth <n>` | begrenzt die Suchtiefe auf n Ebenen |
| `-mindepth <n>` | beginnt erst ab Tiefe n zu durchsuchen |
| `-path <muster>` | sucht im gesamten Pfad statt nur im Namen (`-ipath` ohne Groß-/Kleinschreibung) |
| `-regex <muster>` | sucht im gesamten Pfad per regulärem Ausdruck |
| `-newer <datei>` | findet Dateien, die neuer sind als die Vergleichsdatei |
| `-empty` | findet leere Dateien und Verzeichnisse |
| `-delete` | löscht die gefundenen Dateien |
| `-print0` | trennt die Treffer mit dem Nullbyte (gut für `xargs -0`) |
| `-ls` | zeigt die Treffer im `ls -l`-Format an |
| `-exec <befehl> {} \;` | führt für jeden Treffer einen Befehl aus (`{}` = Trefferpfad) |
| `-exec <befehl> {} +` | wie `\;`, übergibt aber mehrere Treffer gebündelt an einen Aufruf |
| `! <test>`, `-not <test>` | negiert einen Test |
| `<test1> -o <test2>` | ODER-Verknüpfung (`-a` = UND, ist Standard) |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Größenangaben bei `-size`</summary>

| Einheit | Bedeutung |
|---|---|
| `c` | Bytes |
| `k` | Kibibyte (1024 Bytes) |
| `M` | Mebibyte (1024 KiB) |
| `G` | Gibibyte (1024 MiB) |
| `b` | Blöcke zu 512 Bytes – **Vorgabe, wenn keine Einheit angegeben ist** |
| `w` | Wörter zu 2 Bytes |

Das Vorzeichen legt die Richtung fest: `+10M` = größer als 10 MiB, `-10M` = kleiner, `10M` = genau 10 MiB.

**Achtung – `find` rundet auf:** Die Dateigröße wird durch die Einheit geteilt und das Ergebnis **auf eine ganze Zahl aufgerundet**. Verglichen wird erst diese Zahl, nicht die Bytezahl. Bei großen Einheiten führt das zu überraschenden Treffern:

| Datei | Bytes | in GiB | aufgerundet |
|---|---|---|---|
| leere Datei | 0 | 0 | **0** |
| winzig.bin | 100 | 0,00000009 | **1** |
| genau1G.bin | 1073741824 | 1,0 | **1** |
| eins_mehr.bin | 1073741825 | 1,0000000009 | **2** |

```
find . -size 1G     findet genau1G.bin UND winzig.bin (100 Bytes!)
find . -size -1G    findet nur die leere Datei
find . -size 2G     findet eins_mehr.bin (1 GiB plus ein einziges Byte)
find . -size +1G    findet eins_mehr.bin – wie erwartet
```

`-size 1G` bedeutet also nicht „ein Gigabyte groß", sondern „aufgerundet eine Einheit" – das trifft alles von 1 Byte bis 1 GiB. Und `-size -1G` wirkt wie `-empty`, denn „weniger als eine aufgerundete Einheit" ist nur die Null.

**Faustregel:** Bei `M` und `G` ist allein die Form mit `+` verlässlich. Genaue Grenzen und „kleiner als" rechnet man in Bytes:

```bash
find . -size +1G              # gut: wirklich größer als 1 GiB
find . -size -1073741824c     # gut: wirklich kleiner als 1 GiB
find . -size -1G              # Falle: findet nur leere Dateien
```

Der Grund ist historisch: Ursprünglich zählte `-size` nur in Blöcken zu 512 Bytes, wo das Aufrunden sinnvoll ist – eine Datei belegt ohnehin ganze Blöcke. Die Suffixe `k`, `M` und `G` kamen später dazu, das Aufrunden blieb.

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
find . -name "*.txt"                  # alle .txt ab dem aktuellen Verzeichnis
find /home -iname "readme*"           # ohne Groß-/Kleinschreibung
find . -type d -name "test"           # nur Verzeichnisse namens "test"
find . -size +100M                    # Dateien größer als 100 MB
find . -mtime -7                      # in den letzten 7 Tagen geändert
find . -name "*.tmp" -delete          # gefundene .tmp-Dateien löschen
find . -name "*.log" -exec rm {} \;   # für jeden Treffer rm ausführen
```

</details>

>**Hinweis:** Das erste Argument ist das Startverzeichnis (`.` für das aktuelle). Anders als `locate` sucht `find` immer aktuell, ist aber langsamer.

---

### locate
>**Funktion:** Dateien schnell über den Namen finden<br />
>**Syntax:** `locate [optionen] <muster>...`<br />
>**Erklärung:** Sucht Dateien anhand des Namens in einer zuvor angelegten Datenbank und ist dadurch deutlich schneller als `find`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` zeigt nur die Anzahl der Treffer an<br />
>**Beispiel:** `locate datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-i`, `--ignore-case` | ignoriert Groß- und Kleinschreibung |
| `-c`, `--count` | zeigt nur die Anzahl der Treffer an |
| `-l <n>`, `--limit <n>` | begrenzt die Ausgabe auf n Treffer |
| `-b`, `--basename` | sucht nur im Dateinamen, nicht im ganzen Pfad |
| `-e`, `--existing` | zeigt nur Dateien, die derzeit wirklich existieren |
| `-r <regex>`, `--regexp <regex>` | sucht mit einem regulären Ausdruck (Basic Regex) |
| `--regex` | interpretiert die Muster als erweiterte reguläre Ausdrücke |
| `-S`, `--statistics` | zeigt Statistiken zur Datenbank an |
| `-A`, `--all` | zeigt nur Treffer, die auf **alle** Muster passen |
| `-w`, `--wholename` | sucht im ganzen Pfad (Standard, Gegenstück zu `-b`) |
| `-0`, `--null` | trennt die Treffer mit dem Nullbyte (gut für `xargs -0`) |
| `-d <pfad>`, `--database <pfad>` | nutzt eine andere Datenbankdatei |
| `-L`, `--follow` | folgt bei `-e` symbolischen Links (Standard) |
| `-P`, `--nofollow` | folgt bei `-e` symbolischen Links nicht |
| `-q`, `--quiet` | unterdrückt Fehlermeldungen |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
locate datei.txt            # alle Pfade mit "datei.txt"
locate -i readme            # ohne Beachtung der Groß-/Kleinschreibung
locate -c "*.log"           # nur die Anzahl der Treffer
locate -b "\bash"           # nur Treffer im Dateinamen
```

</details>

>**Hinweis:** Die Datenbank wird mit `sudo updatedb` aktualisiert; neue Dateien werden sonst nicht gefunden. `locate` ist oft nicht vorinstalliert (Paket `mlocate` bzw. `plocate`).

---

### ls
>**Funktion:** Dateien und Ordner anzeigen<br />
>**Syntax:** `ls [optionen] [<datei|verzeichnis>...]`<br />
>**Erklärung:** Listet den Inhalt eines Verzeichnisses auf.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` Langformat mit Details (Rechte, Besitzer, Größe, Datum)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt auch versteckte Dateien (beginnend mit `.`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t` sortiert nach Änderungszeit, neueste zuerst<br />
>**Beispiel:** `ls -la`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l` | Langformat mit Details (Rechte, Besitzer, Größe, Datum) |
| `-a`, `--all` | zeigt auch versteckte Dateien (beginnend mit `.`) |
| `-A`, `--almost-all` | wie `-a`, aber ohne `.` und `..` |
| `-h`, `--human-readable` | Größen lesbar anzeigen (K, M, G) – zusammen mit `-l` |
| `-t` | sortiert nach Änderungszeit, neueste zuerst |
| `-S` | sortiert nach Größe, größte zuerst |
| `-r`, `--reverse` | kehrt die Sortierreihenfolge um |
| `-R`, `--recursive` | listet auch alle Unterverzeichnisse auf |
| `-d`, `--directory` | zeigt das Verzeichnis selbst statt seines Inhalts |
| `-1` | ein Eintrag pro Zeile |
| `-i`, `--inode` | zeigt die Inode-Nummer an |
| `-F`, `--classify` | hängt Typkennzeichen an (`/` Ordner, `*` ausführbar, `@` Link) |
| `-p` | hängt nur an Verzeichnisse ein `/` an |
| `-o` | wie `-l`, aber ohne die Gruppenspalte |
| `-g` | wie `-l`, aber ohne die Besitzerspalte |
| `-Q`, `--quote-name` | setzt Dateinamen in Anführungszeichen |
| `--group-directories-first` | listet Verzeichnisse vor Dateien |
| `--time-style=<stil>` | legt das Format der Zeitangabe fest (z. B. `long-iso`, `full-iso`) |
| `--color=auto` | hebt Dateitypen farblich hervor |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
ls                      # einfache Liste des aktuellen Verzeichnisses
ls -l                   # Langformat mit Details
ls -la                  # Langformat inkl. versteckter Dateien
ls -lh                  # Langformat mit lesbaren Größen
ls -ltr                 # nach Zeit sortiert, neueste zuletzt
ls -lS                  # nach Größe sortiert, größte zuerst
ls -d */                # nur Verzeichnisse anzeigen
```

</details>

>**Hinweis:** Optionen lassen sich kombinieren, z. B. `ls -ltr` – Langformat, nach Zeit sortiert, die neuesten zuletzt (`-r` kehrt die Reihenfolge von `-t` um).

---

### pwd
>**Funktion:** Aktuelles Verzeichnis anzeigen | Intern (Builtins)<br />
>**Syntax:** `pwd [optionen]`<br />
>**Erklärung:** Zeigt den vollständigen Pfad des aktuellen Arbeitsverzeichnisses an (print working directory).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-P` zeigt den physischen Pfad und löst symbolische Links auf<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L` zeigt den logischen Pfad (Standard)<br />
>**Beispiel:** `pwd`

>**Hinweis:** Stehst du über einen symbolischen Link in einem Verzeichnis, zeigt `pwd -L` den Link-Pfad, `pwd -P` den echten Zielpfad.

---

### tree
>**Funktion:** Verzeichnisstruktur als Baum anzeigen<br />
>**Syntax:** `tree [optionen] [<verzeichnis>]`<br />
>**Erklärung:** Zeigt Verzeichnisse und Dateien in einer übersichtlichen Baumstruktur an.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L <n>` begrenzt die Anzeige auf n Ebenen Tiefe (z. B. `-L 2`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt nur Verzeichnisse, keine Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt auch versteckte Dateien (beginnend mit `.`)<br />
>**Beispiel:** `tree -L 2`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-L <n>` | begrenzt die Anzeige auf n Ebenen Tiefe |
| `-d` | zeigt nur Verzeichnisse, keine Dateien |
| `-a` | zeigt auch versteckte Dateien (beginnend mit `.`) |
| `-f` | zeigt den vollständigen Pfad vor jedem Eintrag |
| `-h` | zeigt Dateigrößen lesbar an (K, M, G) |
| `-s` | zeigt die Größe jeder Datei in Bytes |
| `-I <muster>` | blendet Einträge aus, die zum Muster passen (z. B. `-I "node_modules"`) |
| `-P <muster>` | zeigt nur Einträge, die zum Muster passen |
| `-C` | erzwingt farbige Ausgabe |
| `--du` | summiert die Größen je Verzeichnis auf |
| `-p` | zeigt die Zugriffsrechte vor jedem Eintrag |
| `-u` | zeigt den Besitzer jedes Eintrags |
| `-g` | zeigt die Gruppe jedes Eintrags |
| `-D` | zeigt das Datum der letzten Änderung |
| `--dirsfirst` | listet Verzeichnisse vor Dateien |
| `-x` | bleibt innerhalb desselben Dateisystems |
| `-J` | gibt die Struktur im JSON-Format aus |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
tree                        # ganzen Baum des aktuellen Verzeichnisses
tree -L 2                   # nur zwei Ebenen tief
tree -d                     # nur Verzeichnisse
tree -a                     # inkl. versteckter Dateien
tree -I "node_modules"      # bestimmten Ordner ausblenden
tree -h /var/log            # mit lesbaren Dateigrößen
```

</details>

>**Hinweis:** `tree` ist oft nicht vorinstalliert und muss ggf. nachinstalliert werden, z. B. mit `sudo apt install tree`.

---

### updatedb
>**Funktion:** Datenbank für `locate` aktualisieren<br />
>**Syntax:** `updatedb [optionen]`<br />
>**Erklärung:** Durchsucht das Dateisystem und baut die Datenbank neu auf, aus der `locate` seine Treffer liest. Erst danach findet `locate` neu angelegte Dateien.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt die indexierten Dateien an (verbose)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-o <datei>` schreibt die Datenbank in eine andere Datei (output)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-U <pfad>` indexiert nur unterhalb des angegebenen Pfades<br />
>**Beispiel:** `sudo updatedb`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-v`, `--verbose` | zeigt die indexierten Dateien an |
| `-o <datei>`, `--output <datei>` | schreibt die Datenbank in eine andere Datei |
| `-U <pfad>`, `--database-root <pfad>` | indexiert nur unterhalb des angegebenen Pfades |
| `-e <pfade>`, `--prunepaths <pfade>` | schließt bestimmte Pfade von der Indexierung aus |
| `-f <fs>`, `--add-prunefs <fs>` | schließt zusätzliche Dateisystemtypen von der Indexierung aus |
| `-n <namen>`, `--add-prunenames <namen>` | schließt zusätzliche Verzeichnisnamen aus |
| `-l <0 oder 1>`, `--require-visibility` | berücksichtigt bei der Ausgabe die Dateirechte |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo updatedb                       # ganze Datenbank neu aufbauen
sudo updatedb -v                    # dabei die indexierten Dateien anzeigen
sudo updatedb -U /home/user -o ~/meine.db  # nur ein Verzeichnis in eigene DB
```

</details>

>**Hinweis:** Erfordert meist `sudo`. Welche Pfade dauerhaft ausgeschlossen werden, steht in `/etc/updatedb.conf` (`PRUNEPATHS`, `PRUNEFS`). Auf vielen Systemen läuft `updatedb` automatisch per `cron`/Timer, sodass ein manueller Aufruf nur nötig ist, wenn `locate` etwas Neues nicht findet. Gehört zum selben Paket wie `locate` (`mlocate` bzw. `plocate`).

---

### whereis
>**Funktion:** Pfad zu Programm, Manpage oder Quellcode finden<br />
>**Syntax:** `whereis [optionen] <name>...`<br />
>**Erklärung:** Zeigt an, wo sich die Programmdatei, das Handbuch und ggf. die Quelldateien eines Befehls befinden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` sucht nur nach der ausführbaren Datei (binary)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` sucht nur nach der Manpage<br />
>**Beispiel:** `whereis ls`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-b` | sucht nur nach der ausführbaren Datei (binary) |
| `-m` | sucht nur nach der Manpage (manual) |
| `-s` | sucht nur nach den Quelldateien (sources) |
| `-u` | zeigt nur Einträge mit ungewöhnlicher Anzahl Treffer |
| `-B <verz>` | begrenzt die Suche nach Binaries auf bestimmte Verzeichnisse |
| `-M <verz>` | begrenzt die Suche nach Manpages auf bestimmte Verzeichnisse |
| `-S <verz>` | begrenzt die Suche nach Quelldateien auf bestimmte Verzeichnisse |
| `-f` | beendet die Verzeichnisliste von `-B`/`-M`/`-S` (danach folgen die Namen) |
| `-l` | zeigt die tatsächlich durchsuchten Verzeichnisse an |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
whereis ls              # Programm, Manpage und Quellen von ls
whereis -b grep         # nur den Pfad zur ausführbaren Datei
whereis -m passwd       # nur die Manpage(s)
```

</details>

>**Hinweis:** Verwandt sind `which` (zeigt nur den Pfad zum Programm) und `find` (sucht beliebige Dateien). `whereis` durchsucht nur feste Standardverzeichnisse und ist daher sehr schnell.

---

### which
>**Funktion:** Pfad zur ausführbaren Datei eines Befehls anzeigen<br />
>**Syntax:** `which [optionen] <name>...`<br />
>**Erklärung:** Durchsucht die Verzeichnisse aus `PATH` und gibt aus, welche Programmdatei beim Aufruf des Befehls tatsächlich ausgeführt würde.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt alle Treffer in `PATH`, nicht nur den ersten (all)<br />
>**Beispiel:** `which python3`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-a`, `--all` | zeigt alle Treffer in `PATH`, nicht nur den ersten |
| `-i`, `--read-alias` | liest Aliase von der Standardeingabe und berücksichtigt sie |
| `--skip-alias` | ignoriert Aliase (auch bei `--read-alias`) |
| `--read-functions` | liest Shell-Funktionen von der Standardeingabe |
| `--skip-functions` | ignoriert Shell-Funktionen |
| `--skip-dot` | überspringt `PATH`-Einträge, die mit `.` beginnen |
| `--skip-tilde` | überspringt `PATH`-Einträge, die mit `~` beginnen |
| `--show-dot` | zeigt `./`, wenn das Programm im Arbeitsverzeichnis liegt |
| `--show-tilde` | kürzt das Home-Verzeichnis zu `~` |
| `--tty-only` | wertet die `--skip-*`/`--show-*`-Optionen nur aus, wenn die Ausgabe ein Terminal ist |
| `--help` | zeigt die Hilfe an |
| `--version`, `-v`, `-V` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
which ls                # Pfad zum zuerst gefundenen Programm
which -a python3        # alle python3 in PATH (z. B. /usr/bin und /usr/local/bin)
which git || echo fehlt # im Skript prüfen, ob ein Programm vorhanden ist
```

</details>

>**Hinweis:** `which` kennt nur externe Programme in `PATH` – Aliase, Funktionen und Shell-Builtins erkennt es nicht. Dafür ist das Builtin `type` zuverlässiger (es zeigt auch Aliase und Builtins an). `whereis` findet zusätzlich Manpage und Quellcode.

---
