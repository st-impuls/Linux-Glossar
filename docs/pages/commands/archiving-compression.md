[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

## Kategorien
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

[← Zurück zur Übersicht](index.md)

# Archivierung & Kompression

### bunzip2
>**Funktion:** Mit bzip2 komprimierte Dateien (`.bz2`) wieder entpacken<br />
>**Syntax:** `bunzip2 [optionen] <datei.bz2>...`<br />
>**Erklärung:** Stellt eine mit `bzip2` komprimierte Datei wieder her. Standardmäßig wird die `.bz2`-Datei durch die entpackte Fassung ersetzt. Gleichbedeutend mit `bzip2 -d`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-k` behält die komprimierte Datei zusätzlich<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` schreibt das Ergebnis nach stdout, statt die Datei zu ersetzen<br />
>**Beispiel:** `bunzip2 archiv.bz2`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-k`, `--keep` | behält die Eingabedatei (löscht die `.bz2`-Datei nicht) |
| `-c`, `--stdout` | schreibt nach stdout, Eingabedatei bleibt erhalten |
| `-f`, `--force` | überschreibt vorhandene Zieldateien und erzwingt das Entpacken |
| `-t`, `--test` | prüft die Integrität, ohne zu entpacken |
| `-v`, `--verbose` | zeigt Meldungen und Fortschritt an |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
bunzip2 datei.bz2            # entpackt zu "datei", .bz2 wird entfernt
bunzip2 -k datei.bz2         # entpackt und behält datei.bz2
bunzip2 -c datei.bz2 > out   # nach out schreiben, Original bleibt
bunzip2 -t datei.bz2         # nur die Integrität prüfen
```

</details>

>**Hinweis:** `bunzip2` ist gleichbedeutend mit `bzip2 -d`. Ein ganzes Verzeichnis steckt meist in einem `tar`-Archiv – dann mit `tar -xjf archiv.tar.bz2` entpacken. Inhalt einer `.bz2`-Datei ansehen, ohne zu entpacken: `bzcat`.

---

### bzip2
>**Funktion:** Dateien nach dem bzip2-Verfahren komprimieren (`.bz2`)<br />
>**Syntax:** `bzip2 [optionen] <datei>...`<br />
>**Erklärung:** Komprimiert einzelne Dateien mit dem Burrows-Wheeler-Verfahren – meist stärker als `gzip`, dafür langsamer. Wie `gzip` ersetzt es standardmäßig die Originaldatei durch die `.bz2`-Fassung und bearbeitet nur einzelne Dateien.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-k` behält die Originaldatei zusätzlich<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-9` stärkste Kompression (`-1` am schnellsten)<br />
>**Beispiel:** `bzip2 messwerte.csv`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-k`, `--keep` | behält die Originaldatei (löscht sie nicht) |
| `-d`, `--decompress` | entpackt (entspricht `bunzip2`) |
| `-c`, `--stdout` | schreibt nach stdout, Original bleibt erhalten |
| `-z`, `--compress` | komprimiert (Standardaktion) |
| `-1` … `-9` | Kompressionsstufe (Standard `-9`; `-1` schneller) |
| `-f`, `--force` | überschreibt vorhandene Dateien, erzwingt |
| `-t`, `--test` | prüft die Integrität einer `.bz2`-Datei |
| `-v`, `--verbose` | zeigt die Kompressionsrate an |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
bzip2 datei.txt                       # erzeugt datei.txt.bz2, Original wird ersetzt
bzip2 -k -9 datei.txt                 # stärkste Kompression, Original behalten
bzip2 -d datei.txt.bz2                # entpacken (wie bunzip2)
tar -cjf sicherung.tar.bz2 ordner/    # Ordner packen und bzip2-komprimieren
```

</details>

>**Hinweis:** Wie `gzip` fasst `bzip2` keine Verzeichnisse zusammen – dafür `tar` (kombiniert: `tar -cjf …`). Die Standardstufe ist bereits `-9`. Noch stärker komprimiert oft `xz`, deutlich schneller ist `zstd`.

---

### cpio
>**Funktion:** Dateien anhand einer Namensliste archivieren und wiederherstellen<br />
>**Syntax:** `cpio {-o|-i|-p} [optionen] < <namensliste>`<br />
>**Erklärung:** Älteres Archivprogramm, das die Dateinamen über die Standardeingabe erhält (meist aus `find` oder `ls`). Es kennt drei Modi: **-o** (copy-out, Archiv erstellen), **-i** (copy-in, entpacken) und **-p** (copy-pass, direkt in ein Verzeichnis kopieren). Begegnet einem u. a. bei `initramfs` und im Inneren von RPM-Paketen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-o` erstellt ein Archiv (copy-out)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` entpackt ein Archiv (copy-in)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` legt beim Entpacken fehlende Verzeichnisse an<br />
>**Beispiel:** `find . -print | cpio -o > archiv.cpio`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-o`, `--create` | erstellt ein Archiv aus der Namensliste (copy-out) |
| `-i`, `--extract` | entpackt Dateien aus einem Archiv (copy-in) |
| `-p`, `--pass-through` | kopiert Dateien direkt in ein Zielverzeichnis (copy-pass) |
| `-d`, `--make-directories` | legt benötigte Verzeichnisse automatisch an |
| `-v`, `--verbose` | zeigt die verarbeiteten Dateien an |
| `-t`, `--list` | listet den Inhalt eines Archivs auf |
| `-u`, `--unconditional` | überschreibt auch neuere vorhandene Dateien |
| `-m`, `--preserve-modification-time` | behält die ursprüngliche Änderungszeit |
| `-H <format>`, `--format=<format>` | Archivformat (z. B. `newc` für initramfs) |
| `--no-absolute-filenames` | entpackt relativ, nie in absolute Pfade |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
find . -print | cpio -o > archiv.cpio        # aktuelles Verzeichnis archivieren
cpio -id < archiv.cpio                        # entpacken, Verzeichnisse anlegen
cpio -it < archiv.cpio                        # Inhalt nur auflisten
find src -print | cpio -pdm ziel/             # src nach ziel/ kopieren
find . | cpio -o -H newc | gzip > initrd.img  # initramfs-typisches Format
```

</details>

>**Hinweis:** `cpio` liest die Dateinamen **von stdin** – daher fast immer mit `find … | cpio` kombiniert. Für den Alltag ist `tar` bequemer; `cpio` begegnet einem vor allem bei `initramfs` und beim Auspacken von RPMs (`rpm2cpio paket.rpm | cpio -idmv`).

---

### gunzip
>**Funktion:** Mit gzip komprimierte Dateien (`.gz`) wieder entpacken<br />
>**Syntax:** `gunzip [optionen] <datei.gz>...`<br />
>**Erklärung:** Stellt eine mit `gzip` komprimierte Datei wieder her. Standardmäßig wird die `.gz`-Datei durch die entpackte Fassung ersetzt und die Endung entfernt. Gleichbedeutend mit `gzip -d`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-k` behält die komprimierte Datei zusätzlich<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` schreibt das Ergebnis nach stdout, statt die Datei zu ersetzen<br />
>**Beispiel:** `gunzip archiv.gz`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-k`, `--keep` | behält die Eingabedatei (löscht die `.gz`-Datei nicht) |
| `-c`, `--stdout` | schreibt nach stdout, Eingabedatei bleibt erhalten |
| `-f`, `--force` | überschreibt vorhandene Zieldateien und erzwingt das Entpacken |
| `-r`, `--recursive` | entpackt Verzeichnisse rekursiv |
| `-t`, `--test` | prüft die Integrität, ohne zu entpacken |
| `-l`, `--list` | zeigt Namen und Größe des Inhalts an |
| `-v`, `--verbose` | zeigt Namen und Kompressionsrate an |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
gunzip datei.gz            # entpackt zu "datei", .gz wird entfernt
gunzip -k datei.gz         # entpackt und behält datei.gz
gunzip -c datei.gz > out   # nach out schreiben, Original bleibt
gunzip -t datei.gz         # nur die Integrität prüfen
```

</details>

>**Hinweis:** `gunzip` ist gleichbedeutend mit `gzip -d`. Ein ganzes Verzeichnis steckt meist in einem `tar`-Archiv – dann mit `tar -xzf archiv.tar.gz` entpacken. Inhalt einer `.gz`-Datei ansehen, ohne zu entpacken: `zcat`.

---

### gzip
>**Funktion:** Dateien nach dem gzip-Verfahren komprimieren (`.gz`)<br />
>**Syntax:** `gzip [optionen] <datei>...`<br />
>**Erklärung:** Komprimiert einzelne Dateien mit dem verbreiteten DEFLATE-Verfahren. Standardmäßig wird die Originaldatei durch die komprimierte `.gz`-Fassung ersetzt. `gzip` bearbeitet immer nur einzelne Dateien – zum Zusammenfassen mehrerer Dateien dient `tar`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-k` behält die Originaldatei zusätzlich<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-9` stärkste Kompression (`-1` am schnellsten)<br />
>**Beispiel:** `gzip protokoll.log`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-k`, `--keep` | behält die Originaldatei (löscht sie nicht) |
| `-d`, `--decompress` | entpackt (entspricht `gunzip`) |
| `-c`, `--stdout` | schreibt nach stdout, Original bleibt erhalten |
| `-r`, `--recursive` | komprimiert Dateien in Verzeichnissen rekursiv |
| `-1` … `-9` | Kompressionsstufe: `-1` schnell, `-9` stark (Standard `-6`) |
| `--fast` / `--best` | gleichbedeutend mit `-1` bzw. `-9` |
| `-f`, `--force` | überschreibt vorhandene Dateien, erzwingt Kompression |
| `-l`, `--list` | zeigt Kompressionsrate und Größen an |
| `-t`, `--test` | prüft die Integrität einer `.gz`-Datei |
| `-v`, `--verbose` | zeigt die Kompressionsrate an |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
gzip datei.txt                        # erzeugt datei.txt.gz, Original wird ersetzt
gzip -k -9 datei.txt                  # stärkste Kompression, Original behalten
gzip -d datei.txt.gz                  # entpacken (wie gunzip)
tar -czf sicherung.tar.gz ordner/     # Ordner packen und gzip-komprimieren
```

</details>

>**Hinweis:** `gzip` fasst keine Verzeichnisse in ein Archiv zusammen – dafür `tar` (oft kombiniert: `tar -czf …`). Stärker, aber langsamer komprimieren `xz` und `bzip2`; modern und sehr schnell ist `zstd`.

---

### tar
>**Funktion:** Mehrere Dateien und Verzeichnisse zu einem Archiv zusammenfassen und wieder entpacken<br />
>**Syntax:** `tar [optionen] -f <archiv> [<datei>...]`<br />
>**Erklärung:** „tape archiver" – fasst ganze Verzeichnisbäume in eine einzige Archivdatei (`.tar`) zusammen und stellt sie wieder her. `tar` selbst komprimiert nicht; mit `-z`/`-j`/`-J` wird zusätzlich `gzip`/`bzip2`/`xz` angewendet (`.tar.gz` usw.). Merkhilfe: **c**reate, e**x**tract, lis**t**.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` erstellt ein Archiv, `-x` entpackt, `-t` listet den Inhalt<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <archiv>` gibt die Archivdatei an (Name direkt dahinter)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt die verarbeiteten Dateien an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-z`/`-j`/`-J` zusätzlich mit gzip/bzip2/xz (de)komprimieren<br />
>**Beispiel:** `tar -czf sicherung.tar.gz projekt/`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-c`, `--create` | erstellt ein neues Archiv |
| `-x`, `--extract` | entpackt ein Archiv |
| `-t`, `--list` | listet den Inhalt, ohne zu entpacken |
| `-f <archiv>`, `--file=<archiv>` | Name der Archivdatei (`-` = stdin/stdout) |
| `-v`, `--verbose` | zeigt die verarbeiteten Dateien an |
| `-z`, `--gzip` | zusätzlich mit gzip (de)komprimieren (`.tar.gz`) |
| `-j`, `--bzip2` | zusätzlich mit bzip2 (`.tar.bz2`) |
| `-J`, `--xz` | zusätzlich mit xz (`.tar.xz`) |
| `-C <verz>`, `--directory=<verz>` | wechselt vor dem Ver-/Entpacken in dieses Verzeichnis |
| `-r`, `--append` | hängt Dateien an ein bestehendes (unkomprimiertes) Archiv an |
| `-u`, `--update` | fügt nur neuere Versionen vorhandener Dateien hinzu |
| `--exclude=<muster>` | schließt passende Dateien vom Archiv aus |
| `--strip-components=<n>` | entfernt beim Entpacken die obersten `n` Pfadebenen |
| `-p`, `--preserve-permissions` | erhält die ursprünglichen Zugriffsrechte |
| `-k`, `--keep-old-files` | überschreibt beim Entpacken keine vorhandenen Dateien |
| `-A`, `--concatenate` | hängt den Inhalt anderer tar-Archive an ein Archiv an |
| `-d`, `--compare` (`--diff`) | vergleicht den Archivinhalt mit den Dateien im Dateisystem |
| `-W`, `--verify` | prüft das Archiv direkt nach dem Schreiben |
| `--remove-files` | löscht die Originaldateien nach dem Hinzufügen ins Archiv |
| `--xattrs` | sichert bzw. überträgt erweiterte Dateiattribute (xattrs) |
| `--numeric-owner` | speichert Benutzer und Gruppe als Zahl statt als Name |
| `--owner=<name>` / `--group=<name>` | setzt Eigentümer bzw. Gruppe im Archiv |
| `-a`, `--auto-compress` | wählt das Kompressionsprogramm anhand der Dateiendung |
| `--totals` | zeigt am Ende die verarbeitete Gesamtgröße an |
| `-M`, `--multi-volume` | verteilt das Archiv auf mehrere Datenträger/Dateien (Mehrbandarchiv) |
| `--wildcards` | deutet Platzhalter (`*`, `?`) in Dateimustern beim Ver-/Entpacken aus |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
tar -czf sicherung.tar.gz projekt/            # Ordner als gzip-Archiv packen
tar -xzf sicherung.tar.gz                     # gzip-Archiv entpacken
tar -tzf sicherung.tar.gz                     # Inhalt anzeigen, ohne zu entpacken
tar -xzf archiv.tar.gz -C /ziel/              # in ein bestimmtes Verzeichnis entpacken
tar -cJf log.tar.xz *.log --exclude='*.tmp'   # xz-komprimiert, *.tmp ausschließen
```

</details>

>**Hinweis:** Bei `-f` muss der Archivname **direkt** folgen. Am Kürzel erkennt man die Kompression: `.tar.gz`/`.tgz` → `-z`, `.tar.bz2` → `-j`, `.tar.xz` → `-J`. Neuere `tar`-Versionen erkennen die Kompression beim Entpacken oft automatisch (`-a`/`--auto-compress`), das ausgeschriebene Kürzel ist aber eindeutiger.

---

### unzip
>**Funktion:** ZIP-Archive (`.zip`) entpacken<br />
>**Syntax:** `unzip [optionen] <archiv.zip> [<datei>...] [-d <verz>]`<br />
>**Erklärung:** Entpackt Dateien aus einem `.zip`-Archiv – dem verbreiteten, auch unter Windows üblichen Format. Ohne weitere Angabe wird der gesamte Inhalt in das aktuelle Verzeichnis entpackt. Einzelne Dateien lassen sich gezielt herauslösen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <verz>` entpackt in ein bestimmtes Zielverzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` listet den Inhalt, ohne zu entpacken<br />
>**Beispiel:** `unzip daten.zip -d /ziel/`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l` | listet den Inhalt des Archivs auf (ohne zu entpacken) |
| `-d <verz>` | entpackt in das angegebene Zielverzeichnis |
| `-o` | überschreibt vorhandene Dateien ohne Nachfrage |
| `-n` | überschreibt **keine** vorhandenen Dateien |
| `-p` | schreibt den Inhalt nach stdout (ohne Meldungen) |
| `-j` | ignoriert die Verzeichnisstruktur, entpackt alles „flach" |
| `-q` | unterdrückt Meldungen (quiet) |
| `-t` | prüft die Integrität des Archivs |
| `-x <muster>` | schließt passende Dateien aus |
| `-P <passwort>` | Passwort für verschlüsselte Archive (unsicher, im Klartext sichtbar) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
unzip daten.zip               # alles ins aktuelle Verzeichnis
unzip daten.zip -d /ziel/     # in ein bestimmtes Verzeichnis
unzip -l daten.zip            # Inhalt auflisten
unzip daten.zip bild.png      # nur eine Datei herauslösen
unzip -o daten.zip            # vorhandene Dateien überschreiben
```

</details>

>**Hinweis:** `unzip` gehört nicht immer zur Grundinstallation und muss ggf. nachinstalliert werden (`sudo apt install unzip`). Zum Erstellen von ZIP-Archiven dient `zip`. Für `.tar.gz` u. Ä. ist stattdessen `tar` zuständig.

---

### xz
>**Funktion:** Dateien mit dem xz-Verfahren (LZMA2) besonders stark komprimieren (`.xz`)<br />
>**Syntax:** `xz [optionen] <datei>...`<br />
>**Erklärung:** Komprimiert Dateien meist stärker als `gzip` und `bzip2`, dafür langsamer. Wie `gzip` ersetzt es standardmäßig die Originaldatei durch die `.xz`-Fassung. Häufig in Kombination mit `tar` (`.tar.xz`), u. a. für Quellcode-Pakete und Kernel-Archive.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-k` behält die Originaldatei zusätzlich<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` entpackt (entspricht `unxz`)<br />
>**Beispiel:** `xz -k daten.img`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-k`, `--keep` | behält die Originaldatei |
| `-d`, `--decompress` | entpackt (entspricht `unxz`) |
| `-c`, `--stdout` | schreibt nach stdout, Original bleibt erhalten |
| `-z`, `--compress` | komprimiert (Standardaktion) |
| `-0` … `-9` | Kompressionsstufe (Standard `-6`; `-9` sehr stark) |
| `-e`, `--extreme` | verstärkt die gewählte Stufe (langsamer) |
| `-T <n>`, `--threads=<n>` | nutzt `n` Threads (`0` = alle CPU-Kerne) |
| `-f`, `--force` | überschreibt vorhandene Dateien, erzwingt |
| `-l`, `--list` | zeigt Informationen zur `.xz`-Datei an |
| `-t`, `--test` | prüft die Integrität |
| `-v`, `--verbose` | zeigt Fortschritt und Kompressionsrate an |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
xz datei.iso                 # erzeugt datei.iso.xz, Original wird ersetzt
xz -k -9 datei.iso           # stärkste Kompression, Original behalten
xz -d datei.iso.xz           # entpacken (wie unxz)
xz -T0 grosse.datei          # alle CPU-Kerne nutzen
tar -cJf quelle.tar.xz src/  # Ordner packen und xz-komprimieren
```

</details>

>**Hinweis:** `xz -d` entspricht `unxz`, `xz -l` zeigt Details zur Datei. Hohe Stufen (`-9 -e`) brauchen deutlich mehr Zeit und Arbeitsspeicher. Wie `gzip` verarbeitet `xz` nur einzelne Dateien; ganze Verzeichnisse zuerst mit `tar` bündeln. Sehr schnelle Alternative bei guter Kompression: `zstd`.

---

### zcat
>**Funktion:** Inhalt gzip-komprimierter Dateien anzeigen, ohne sie zu entpacken<br />
>**Syntax:** `zcat [optionen] <datei.gz>...`<br />
>**Erklärung:** Gibt den entpackten Inhalt einer `.gz`-Datei direkt auf der Standardausgabe aus – wie `cat`, nur für komprimierte Dateien. Die `.gz`-Datei selbst bleibt dabei unverändert. Gleichbedeutend mit `gunzip -c`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` gibt auch unkomprimierte Dateien unverändert aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zeigt Kompressionsinfos statt des Inhalts<br />
>**Beispiel:** `zcat protokoll.log.gz`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-f`, `--force` | gibt auch nicht komprimierte oder verkettete Eingaben aus |
| `-l`, `--list` | zeigt Größen und Kompressionsrate statt des Inhalts |
| `-r`, `--recursive` | durchläuft Verzeichnisse rekursiv |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
zcat datei.gz                          # Inhalt anzeigen, .gz bleibt erhalten
zcat protokoll.log.gz | grep Fehler    # in einer .gz-Datei suchen
zcat -l archiv.gz                      # Kompressionsinfos anzeigen
zcat a.gz b.gz > alles.txt             # mehrere .gz zusammenführen
```

</details>

>**Hinweis:** `zcat` entspricht `gunzip -c`. Verwandte Helfer, die ohne vorheriges Entpacken arbeiten: `zless`/`zmore` (seitenweise), `zgrep` (suchen); für `.bz2` gibt es `bzcat`/`bzgrep`, für `.xz` `xzcat`.

---

### zip
>**Funktion:** Dateien und Verzeichnisse in einem ZIP-Archiv zusammenfassen und komprimieren (`.zip`)<br />
>**Syntax:** `zip [optionen] <archiv.zip> <datei>...`<br />
>**Erklärung:** Erstellt ein `.zip`-Archiv – anders als `tar` fasst `zip` das Zusammenpacken **und** das Komprimieren in einem Schritt zusammen und ist zudem gut mit Windows und macOS kompatibel. Zum Entpacken dient `unzip`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` bezieht Verzeichnisse rekursiv ein<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-e` verschlüsselt das Archiv mit Passwort<br />
>**Beispiel:** `zip -r projekt.zip projekt/`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-r`, `--recurse-paths` | nimmt Verzeichnisse mitsamt Inhalt rekursiv auf |
| `-e`, `--encrypt` | verschlüsselt das Archiv (Passwortabfrage) |
| `-0` … `-9` | Kompressionsstufe (`-0` nur speichern, `-9` stark) |
| `-u`, `--update` | fügt nur neue oder geänderte Dateien hinzu |
| `-d`, `--delete` | löscht Dateien aus einem bestehenden Archiv |
| `-m`, `--move` | verschiebt: löscht die Originale nach dem Packen |
| `-x <muster>`, `--exclude` | schließt passende Dateien aus |
| `-j`, `--junk-paths` | speichert ohne Verzeichnispfade („flach") |
| `-q`, `--quiet` | unterdrückt Meldungen |
| `-v`, `--verbose` | zeigt ausführliche Meldungen |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
zip archiv.zip a.txt b.txt         # zwei Dateien archivieren
zip -r projekt.zip projekt/        # Ordner rekursiv packen
zip -e geheim.zip datei.txt        # mit Passwort verschlüsseln
zip -r sicherung.zip . -x '*.tmp'  # alles außer *.tmp
zip -u archiv.zip neu.txt          # Datei hinzufügen/aktualisieren
```

</details>

>**Hinweis:** `zip` ist oft nicht vorinstalliert (`sudo apt install zip`). Verzeichnisse werden nur mit `-r` vollständig einbezogen. Die ZIP-Verschlüsselung (`-e`) gilt als schwach – für sensible Daten besser `gpg`. Entpackt wird mit `unzip`.

---

### zstd
>**Funktion:** Dateien mit Zstandard komprimieren – sehr schnell bei guter Kompression (`.zst`)<br />
>**Syntax:** `zstd [optionen] <datei>...`<br />
>**Erklärung:** Modernes Kompressionsverfahren (von Meta/Facebook), das hohe Geschwindigkeit mit guter Kompressionsrate verbindet. Standardmäßig bleibt die Originaldatei erhalten und es entsteht zusätzlich eine `.zst`-Datei. Zunehmend verbreitet, u. a. als Standard bei Arch Linux und in Dateisystemen wie Btrfs.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` entpackt (entspricht `unzstd`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-19` sehr starke Kompression (Bereich `-1`…`-19`, mit `--ultra` bis `-22`)<br />
>**Beispiel:** `zstd -19 daten.tar`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-d`, `--decompress` | entpackt (entspricht `unzstd`) |
| `-k`, `--keep` | behält die Eingabedatei (Standardverhalten) |
| `--rm` | löscht die Eingabedatei nach erfolgreichem Verarbeiten |
| `-c`, `--stdout` | schreibt nach stdout |
| `-1` … `-19` | Kompressionsstufe (Standard `-3`) |
| `--ultra -22` | schaltet die höchsten Stufen bis `-22` frei |
| `-T <n>`, `--threads=<n>` | nutzt `n` Threads (`0` = alle CPU-Kerne) |
| `--long` | größeres Suchfenster für bessere Kompression |
| `-f`, `--force` | überschreibt vorhandene Dateien |
| `-t`, `--test` | prüft die Integrität |
| `-v`, `--verbose` | zeigt ausführliche Meldungen an |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
zstd datei.img                             # erzeugt datei.img.zst, Original bleibt
zstd -19 --rm datei.img                    # stark komprimieren und Original löschen
zstd -d datei.img.zst                      # entpacken (wie unzstd)
zstd -T0 grosse.datei                      # alle CPU-Kerne nutzen
tar --zstd -cf sicherung.tar.zst ordner/   # tar mit zstd kombinieren
```

</details>

>**Hinweis:** Anders als `gzip`/`xz` **behält** `zstd` die Originaldatei standardmäßig; zum Ersetzen `--rm`. Entpacken auch mit `unzstd` oder `zstdcat` (Ausgabe wie `cat`). Mit `tar` über `--zstd` kombinierbar (`.tar.zst`). Gute Wahl, wenn Geschwindigkeit zählt.

---
