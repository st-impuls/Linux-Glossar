[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Netzwerk & Download](network-download.md)
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
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-L` | folgt symbolischen Links, zeigt den logischen Pfad (Standard) |
| `-P` | nutzt den physischen Pfad und löst symbolische Links auf |
| `-e` | gibt mit `-P` einen Fehlerstatus zurück, wenn das Arbeitsverzeichnis nicht ermittelt werden kann |

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
>**Funktion:** Dateien und Ordner suchen | Extern<br />
>**Syntax:** `find [<startverzeichnis>] [optionen]`<br />
>**Erklärung:** Durchsucht ein Verzeichnis (und dessen Unterordner) live nach Dateien anhand von Name, Typ, Größe und weiteren Kriterien.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-name <muster>` sucht nach dem Dateinamen (z. B. `-name "*.txt"`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-type f` sucht nur Dateien, `-type d` nur Verzeichnisse<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-size <n>` sucht nach der Größe (z. B. `+10M` = größer als 10 MB)<br />
>**Beispiel:** `find /home -name "*.txt"`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-name <muster>` | sucht nach dem Dateinamen (z. B. `"*.txt"`) |
| `-iname <muster>` | wie `-name`, ignoriert Groß-/Kleinschreibung |
| `-type f` / `-type d` | sucht nur Dateien (`f`) bzw. nur Verzeichnisse (`d`) |
| `-size <n>` | sucht nach Größe (`+10M` größer, `-1k` kleiner) |
| `-mtime <n>` | nach Änderungsdatum in Tagen (`-7` = letzte 7 Tage) |
| `-mmin <n>` | nach Änderungsdatum in Minuten |
| `-user <name>` | Dateien eines bestimmten Besitzers |
| `-perm <modus>` | Dateien mit bestimmten Rechten |
| `-maxdepth <n>` | begrenzt die Suchtiefe auf n Ebenen |
| `-empty` | findet leere Dateien und Verzeichnisse |
| `-delete` | löscht die gefundenen Dateien |
| `-exec <befehl> {} \;` | führt für jeden Treffer einen Befehl aus (`{}` = Trefferpfad) |

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
>**Funktion:** Dateien schnell über den Namen finden | Extern<br />
>**Syntax:** `locate [optionen] <muster>...`<br />
>**Erklärung:** Sucht Dateien anhand des Namens in einer zuvor angelegten Datenbank und ist dadurch deutlich schneller als `find`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` zeigt nur die Anzahl der Treffer an<br />
>**Beispiel:** `locate datei.txt`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-i`, `--ignore-case` | ignoriert Groß- und Kleinschreibung |
| `-c`, `--count` | zeigt nur die Anzahl der Treffer an |
| `-l <n>`, `--limit <n>` | begrenzt die Ausgabe auf n Treffer |
| `-b`, `--basename` | sucht nur im Dateinamen, nicht im ganzen Pfad |
| `-e`, `--existing` | zeigt nur Dateien, die derzeit wirklich existieren |
| `-r <regex>`, `--regexp <regex>` | sucht mit einem regulären Ausdruck |
| `-S`, `--statistics` | zeigt Statistiken zur Datenbank an |

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
>**Funktion:** Dateien und Ordner anzeigen | Extern<br />
>**Syntax:** `ls [optionen] [<datei|verzeichnis>...]`<br />
>**Erklärung:** Listet den Inhalt eines Verzeichnisses auf.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` Langformat mit Details (Rechte, Besitzer, Größe, Datum)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt auch versteckte Dateien (beginnend mit `.`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t` sortiert nach Änderungszeit, neueste zuerst<br />
>**Beispiel:** `ls -la`

<details markdown>
<summary>Alle Optionen</summary>

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
| `--color=auto` | hebt Dateitypen farblich hervor |

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
>**Funktion:** Verzeichnisstruktur als Baum anzeigen | Extern<br />
>**Syntax:** `tree [optionen] [<verzeichnis>]`<br />
>**Erklärung:** Zeigt Verzeichnisse und Dateien in einer übersichtlichen Baumstruktur an.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L <n>` begrenzt die Anzeige auf n Ebenen Tiefe (z. B. `-L 2`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt nur Verzeichnisse, keine Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt auch versteckte Dateien (beginnend mit `.`)<br />
>**Beispiel:** `tree -L 2`

<details markdown>
<summary>Alle Optionen</summary>

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

### whereis
>**Funktion:** Pfad zu Programm, Manpage oder Quellcode finden | Extern<br />
>**Syntax:** `whereis [optionen] <name>...`<br />
>**Erklärung:** Zeigt an, wo sich die Programmdatei, das Handbuch und ggf. die Quelldateien eines Befehls befinden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` sucht nur nach der ausführbaren Datei (binary)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` sucht nur nach der Manpage<br />
>**Beispiel:** `whereis ls`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-b` | sucht nur nach der ausführbaren Datei (binary) |
| `-m` | sucht nur nach der Manpage (manual) |
| `-s` | sucht nur nach den Quelldateien (sources) |
| `-u` | zeigt nur Einträge mit ungewöhnlicher Anzahl Treffer |
| `-B <verz>` | begrenzt die Suche nach Binaries auf bestimmte Verzeichnisse |
| `-M <verz>` | begrenzt die Suche nach Manpages auf bestimmte Verzeichnisse |

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
