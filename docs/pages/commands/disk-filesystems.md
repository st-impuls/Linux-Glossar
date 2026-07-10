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

# Datenträger & Dateisysteme

**Dateisystem-Familien:** Viele Werkzeuge hier sind auf eine Dateisystem-Familie zugeschnitten. **ext2/3/4** ist auf Debian/Ubuntu üblich (`e2fsck`, `tune2fs`, `dumpe2fs`, `e2label`, `resize2fs`). **XFS** ist die Standard-Dateisystem auf **RHEL, CentOS, Fedora** und deren Ablegern (`xfs_repair`, `xfs_info`, `xfs_growfs`, `xfs_admin`). Allgemeine Befehle wie `mount`/`umount`, `df`, `blkid`, `fdisk`/`parted` und `mkfs` gelten für alle Typen.

### blkid
>**Funktion:** Blockgeräte samt UUID, Typ und Label der Dateisysteme anzeigen<br />
>**Syntax:** `blkid [optionen] [<gerät>]`<br />
>**Erklärung:** Zeigt zu Partitionen und Datenträgern die eindeutige UUID, den Dateisystemtyp und (falls vorhanden) das Label. Diese Angaben braucht man vor allem für stabile Einträge in `/etc/fstab`, die auch dann gültig bleiben, wenn Geräte anders benannt werden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <feld>` zeigt nur ein bestimmtes Feld (z. B. `UUID`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-o <format>` wählt das Ausgabeformat (z. B. `value`)<br />
>**Beispiel:** `blkid /dev/sda1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-s <feld>`, `--match-tag <feld>` | zeigt nur ein Feld (z. B. `UUID`, `TYPE`, `LABEL`) |
| `-o <format>`, `--output <format>` | Ausgabeformat: `full`, `value`, `list`, `export` |
| `-L <label>`, `--label <label>` | gibt das Gerät zu einem Label aus |
| `-U <uuid>`, `--uuid <uuid>` | gibt das Gerät zu einer UUID aus |
| `-p`, `--probe` | umgeht den Cache und untersucht das Gerät direkt |
| `-c <datei>`, `--cache-file <datei>` | nutzt eine andere Cache-Datei (`/dev/null` = ohne) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
blkid                                # Übersicht aller Geräte
blkid /dev/sda1                      # nur eine Partition
blkid -s UUID -o value /dev/sda1     # nur die UUID ausgeben
blkid -L DATEN                       # Gerät zum Label "DATEN"
```

</details>

>**Hinweis:** Meist ohne `sudo` nutzbar, für frische Geräte ggf. mit Root-Rechten. Die UUID aus `blkid` wird typischerweise in `/etc/fstab` und Bootloader-Konfigurationen eingetragen. Dieselben Angaben mit Baumstruktur zeigt `lsblk -f`.

---

### cfdisk
>**Funktion:** Festplatten interaktiv in einem Textmenü partitionieren<br />
>**Syntax:** `cfdisk [optionen] [<gerät>]`<br />
>**Erklärung:** Bietet eine übersichtliche, menügeführte (ncurses-)Oberfläche zum Anlegen, Löschen und Ändern von Partitionen – einsteigerfreundlicher als `fdisk`. Änderungen werden erst nach ausdrücklichem „Write" auf den Datenträger geschrieben.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-z` startet mit leerer Partitionstabelle<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L` erzwingt farbige Darstellung<br />
>**Beispiel:** `sudo cfdisk /dev/sdb`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-z`, `--zero` | beginnt mit einer leeren Tabelle im Speicher (kein Einlesen) |
| `-L [<modus>]`, `--color[=<modus>]` | Farbdarstellung (`auto`, `always`, `never`) |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo cfdisk /dev/sdb        # Gerät im Textmenü partitionieren
sudo cfdisk -z /dev/sdb     # mit leerer Tabelle neu beginnen
```

</details>

>**Hinweis:** **Immer** `sudo` und das richtige Gerät – vorher mit `lsblk` prüfen. Bedient wird über ein Menü (New, Delete, Type, Write, Quit); erst „Write" schreibt die Änderungen. Skriptfähige bzw. mächtigere Alternativen: `fdisk` und `parted`.

---

### df
>**Funktion:** Belegung der Dateisysteme anzeigen<br />
>**Syntax:** `df [optionen] [<datei|mountpoint>...]`<br />
>**Erklärung:** Zeigt den belegten und freien Speicherplatz der eingehängten Dateisysteme (disk free).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` lesbare Größen (KB/MB/GB)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-T` zeigt den Dateisystemtyp an<br />
>**Beispiel:** `df -h`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h`, `--human-readable` | lesbare Größen (z. B. `1G`, `512M`) |
| `-H`, `--si` | wie `-h`, aber Basis 1000 statt 1024 |
| `-T`, `--print-type` | zeigt den Dateisystemtyp an |
| `-i`, `--inodes` | zeigt die Inode-Belegung statt der Bytes |
| `-a`, `--all` | zeigt auch Pseudo-Dateisysteme an |
| `-t <typ>`, `--type=<typ>` | nur Dateisysteme dieses Typs |
| `-x <typ>`, `--exclude-type=<typ>` | schließt einen Typ aus |
| `-B <größe>`, `--block-size=<größe>` | nutzt eine feste Blockgröße (z. B. `-BM`) |
| `-l`, `--local` | beschränkt auf lokale Dateisysteme |
| `-P`, `--portability` | POSIX-kompatibles Ausgabeformat |
| `--total` | fügt eine Zeile mit der Gesamtsumme hinzu |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
df -h          # Belegung aller Dateisysteme, lesbar
df -h /        # nur das Wurzel-Dateisystem
df -hT         # mit Dateisystemtyp
df -i          # Inode-Belegung
df -h /home    # Dateisystem, auf dem /home liegt
```

</details>

>**Hinweis:** `df` zeigt den Platz pro Dateisystem; für den Verbrauch einzelner Verzeichnisse `du` verwenden.

---

### du
>**Funktion:** Speicherverbrauch von Dateien und Verzeichnissen anzeigen<br />
>**Syntax:** `du [optionen] [<pfad>...]`<br />
>**Erklärung:** Berechnet, wie viel Speicherplatz Dateien und Verzeichnisse belegen (disk usage).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` lesbare Größen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` nur die Gesamtsumme je Argument<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <n>` begrenzt die Tiefe der Auflistung<br />
>**Beispiel:** `du -sh /var/log`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h`, `--human-readable` | lesbare Größen |
| `-s`, `--summarize` | nur die Gesamtsumme je Argument |
| `-d <n>`, `--max-depth=<n>` | listet nur n Verzeichnisebenen tief auf |
| `-a`, `--all` | zählt auch einzelne Dateien, nicht nur Verzeichnisse |
| `-c`, `--total` | gibt am Ende eine Gesamtsumme aus |
| `-x`, `--one-file-system` | bleibt auf einem Dateisystem |
| `--exclude=<muster>` | schließt passende Einträge aus |
| `-B <größe>`, `--block-size=<größe>` | nutzt eine feste Blockgröße |
| `--apparent-size` | zählt die tatsächliche Dateigröße statt belegter Blöcke |
| `-L`, `--dereference` | folgt symbolischen Links |
| `-S`, `--separate-dirs` | rechnet Unterverzeichnisse nicht zum übergeordneten dazu |
| `--time` | zeigt zusätzlich den Zeitstempel der letzten Änderung |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
du -sh /var/log            # Gesamtgröße eines Verzeichnisses
du -h --max-depth=1 /var   # Größe je Unterverzeichnis von /var
du -sh *                   # Größe jedes Eintrags im aktuellen Ordner
du -sh * | sort -h         # nach Größe sortiert
du -ah . | sort -h | tail  # die größten Dateien/Ordner zuletzt
```

</details>

>**Hinweis:** `du -sh *` zeigt schnell die größten Verzeichnisse. Mit `| sort -h` lässt sich die Ausgabe nach lesbaren Größen sortieren. Für freien Platz pro Dateisystem `df` verwenden.

---

### dumpe2fs
>**Funktion:** Superblock- und Gruppeninformationen eines ext2/3/4-Dateisystems anzeigen<br />
>**Syntax:** `dumpe2fs [optionen] <gerät>`<br />
>**Erklärung:** Gibt die internen Verwaltungsdaten eines ext-Dateisystems aus – Superblock (UUID, Label, Blockgröße, Features, Zähler) und die Beschreibung der einzelnen Blockgruppen. Nützlich zur Diagnose und um Einstellungen zu prüfen, die man mit `tune2fs` ändert.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` zeigt nur den Superblock (ohne Gruppendetails)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` listet als defekt markierte Blöcke<br />
>**Beispiel:** `sudo dumpe2fs -h /dev/sda1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h` | zeigt nur die Superblock-Informationen (ohne Blockgruppen) |
| `-b` | zeigt die als defekt („bad") reservierten Blöcke |
| `-f` | erzwingt die Anzeige, auch bei unbekannten Features |
| `-x` | zeigt Blocknummern hexadezimal |
| `-o superblock=<n>` | verwendet einen alternativen Superblock |
| `-V` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo dumpe2fs -h /dev/sda1                            # nur Superblock (UUID, Features, Zähler)
sudo dumpe2fs /dev/sda1 | less                        # vollständige Ausgabe durchblättern
sudo dumpe2fs -h /dev/sda1 | grep -i 'mount count'    # Zähler bis zum nächsten fsck
```

</details>

>**Hinweis:** Nur-lesend – ändert nichts; das Dateisystem darf eingehängt sein. Für die **ext**-Familie (e2fsprogs). Zum **Ändern** derselben Parameter dient `tune2fs`, für XFS gibt es `xfs_info`.

---

### e2fsck
>**Funktion:** Ein ext2/3/4-Dateisystem prüfen und reparieren<br />
>**Syntax:** `e2fsck [optionen] <gerät>`<br />
>**Erklärung:** Das eigentliche Prüfprogramm für ext-Dateisysteme, das `fsck` bei diesen Typen im Hintergrund aufruft. Prüft die Konsistenz und behebt Fehler; das Zieldateisystem muss ausgehängt (oder nur lesend eingehängt) sein.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` repariert automatisch, was sicher ist (preen)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt die Prüfung, auch wenn das FS als sauber gilt<br />
>**Beispiel:** `sudo e2fsck -f /dev/sda1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-p` | behebt automatisch unkritische Fehler (preen, ohne Rückfragen) |
| `-y` | beantwortet alle Rückfragen mit „ja" |
| `-n` | beantwortet alles mit „nein" (nur prüfen) |
| `-f` | erzwingt die Prüfung trotz „clean"-Markierung |
| `-c` | sucht mit `badblocks` nach defekten Blöcken |
| `-b <superblock>` | nutzt einen Ersatz-Superblock (z. B. `8193`, `32768`) |
| `-B <größe>` | gibt die Blockgröße vor (bei kaputtem Superblock) |
| `-v` | ausführliche Ausgabe |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo e2fsck -f /dev/sda1        # gründliche Prüfung erzwingen
sudo e2fsck -p /dev/sda1        # automatische, sichere Reparatur
sudo e2fsck -b 32768 /dev/sda1  # Ersatz-Superblock verwenden
```

</details>

>**Hinweis:** **Nie** auf ein eingehängtes, beschreibbares Dateisystem – vorher `umount`. `fsck /dev/sda1` ruft bei ext intern `e2fsck` auf; direkt aufgerufen hat man mehr ext-spezifische Optionen. Ersatz-Superblöcke listet `dumpe2fs` bzw. `mke2fs -n`. Verwandt: `fsck`, `tune2fs` (Prüfintervalle setzen).

---

### e2label
>**Funktion:** Das Label eines ext2/3/4-Dateisystems anzeigen oder setzen<br />
>**Syntax:** `e2label <gerät> [<neues-label>]`<br />
>**Erklärung:** Zeigt ohne weiteres Argument das aktuelle Label eines ext-Dateisystems; mit Angabe eines Namens setzt es ein neues. Das Label kann – wie die UUID – zum Einhängen in `/etc/fstab` verwendet werden (`LABEL=…`).<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`e2label <gerät>` zeigt das aktuelle Label<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`e2label <gerät> <name>` setzt ein neues Label<br />
>**Beispiel:** `sudo e2label /dev/sda1 DATEN`

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
e2label /dev/sda1             # aktuelles Label anzeigen
sudo e2label /dev/sda1 DATEN  # Label auf "DATEN" setzen
```

</details>

>**Hinweis:** Kurzform für `tune2fs -L`. Bei ext-Dateisystemen ist das Label auf 16 Zeichen begrenzt. Zum Einhängen per Label dient `mount LABEL=DATEN …` bzw. ein `LABEL=`-Eintrag in `/etc/fstab`; die zugehörige UUID zeigt `blkid`.

---

### fdisk
>**Funktion:** Partitionstabellen anzeigen und bearbeiten<br />
>**Syntax:** `fdisk [optionen] [<gerät>]`<br />
>**Erklärung:** Zeigt und bearbeitet die Partitionstabellen von Datenträgern (MBR/DOS und GPT). Mit einem Gerät als Argument startet ein interaktiver Modus; mit `-l` werden vorhandene Partitionen nur aufgelistet.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` listet die Partitionstabellen auf<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-x` listet mit zusätzlichen Details auf<br />
>**Beispiel:** `sudo fdisk -l`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--list` | listet die Partitionstabellen (aller oder eines Geräts) auf |
| `-x`, `--list-details` | listet mit zusätzlichen Details auf |
| `-b <größe>`, `--sector-size <größe>` | legt die Sektorgröße fest (512, 1024, 2048, 4096) |
| `-t <typ>`, `--type <typ>` | bearbeitet nur Partitionstabellen dieses Typs (z. B. `gpt`, `dos`) |
| `-u[<einheit>]`, `--units[=<einheit>]` | Einheit für die Anzeige (`cylinders` oder `sectors`) |
| `-w <wann>`, `--wipe <wann>` | löscht Signaturen: `auto`, `always` oder `never` |
| `-s <partition>`, `--getsz` | gibt die Größe einer Partition in Sektoren aus (veraltet) |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo fdisk -l            # alle Partitionstabellen auflisten
sudo fdisk -l /dev/sda   # nur die eines Geräts
sudo fdisk /dev/sdb      # Partitionen interaktiv bearbeiten
```

</details>

>**Hinweis:** Braucht `sudo`. Der interaktive Modus ändert die Partitionstabelle erst beim Speichern mit `w`; mit `q` wird ohne Änderungen beendet. Wichtige Tasten dort: `m` Hilfe, `p` anzeigen, `n` neu, `d` löschen, `t` Typ. Zum reinen Auflisten ist `lsblk` übersichtlicher.

---

### fsck
>**Funktion:** Dateisysteme auf Fehler prüfen und reparieren<br />
>**Syntax:** `fsck [optionen] [<gerät>]`<br />
>**Erklärung:** Prüft die Konsistenz eines Dateisystems und behebt gefundene Fehler. `fsck` ist dabei nur ein Vordergrundprogramm, das je nach Typ das passende Werkzeug aufruft (`fsck.ext4`, `fsck.xfs` …). Das Zieldateisystem muss ausgehängt (oder nur lesend eingehängt) sein.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-y` beantwortet alle Rückfragen automatisch mit „ja"<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-A` prüft alle in `/etc/fstab` eingetragenen Dateisysteme<br />
>**Beispiel:** `sudo fsck /dev/sda1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-y` | beantwortet alle Rückfragen automatisch mit „ja" |
| `-n` | beantwortet alles mit „nein" (nur prüfen, nichts ändern) |
| `-f` | erzwingt die Prüfung, auch wenn das FS als sauber gilt |
| `-A` | prüft alle Dateisysteme aus `/etc/fstab` |
| `-a` | repariert automatisch ohne Rückfragen (Vorsicht) |
| `-r` | interaktiv mit Rückfragen (Standard bei einzelnem Gerät) |
| `-t <typ>` | gibt den Dateisystemtyp an (z. B. `ext4`) |
| `-M` | überspringt eingehängte Dateisysteme |
| `-C` | zeigt einen Fortschrittsbalken (ext-Familie) |
| `-V` | ausführliche Ausgabe |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo fsck /dev/sda1          # Partition prüfen (interaktiv)
sudo fsck -y /dev/sda1       # alle Fehler automatisch beheben
sudo fsck -A -f              # alle fstab-Dateisysteme, Prüfung erzwingen
sudo fsck.ext4 -f /dev/sdb1  # gezielt das ext4-Werkzeug
```

</details>

>**Hinweis:** **Niemals** auf ein eingehängtes, beschreibbares Dateisystem anwenden – das kann Daten zerstören; vorher `umount`. Die Wurzel-Partition prüft man am besten vom Live-System oder im Rettungsmodus. Der Exit-Code fasst das Ergebnis zusammen (`0` = sauber). Verwandt: `mkfs` (erstellen), `blkid` (Typ ermitteln).

---

### mkfs
>**Funktion:** Ein Dateisystem auf einer Partition anlegen (formatieren)<br />
>**Syntax:** `mkfs [optionen] [-t <typ>] <gerät>`<br />
>**Erklärung:** Erstellt ein neues Dateisystem auf einem Datenträger oder einer Partition – das Gegenstück zum „Formatieren". Wie `fsck` ruft `mkfs` je nach Typ das passende Werkzeug auf (`mkfs.ext4`, `mkfs.xfs`, `mkfs.vfat` …). **Alle vorhandenen Daten gehen dabei verloren.**<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t <typ>` legt den Dateisystemtyp fest (z. B. `ext4`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L <label>` vergibt eine Datenträgerbezeichnung (Label)<br />
>**Beispiel:** `sudo mkfs -t ext4 /dev/sdb1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-t <typ>`, `--type <typ>` | Dateisystemtyp (`ext4`, `xfs`, `btrfs`, `vfat` …) |
| `-L <label>` | vergibt ein Label (bei den meisten Typen) |
| `-n <label>` | Label bei `vfat`/`exfat` |
| `-c` | prüft das Gerät vorher auf defekte Blöcke (ext-Familie) |
| `-q` | knappe Ausgabe (quiet) |
| `-V`, `--verbose` | ausführliche Ausgabe |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo mkfs -t ext4 /dev/sdb1          # ext4 anlegen
sudo mkfs.ext4 -L DATEN /dev/sdb1    # ext4 mit Label
sudo mkfs.vfat -F 32 /dev/sdc1       # FAT32 (z. B. für USB-Sticks)
sudo mkfs.xfs /dev/sdb2              # XFS anlegen
```

</details>

>**Hinweis:** **Zerstört alle Daten** auf dem Ziel – Gerät vorher mit `lsblk`/`blkid` genau prüfen und sicher aushängen. Ohne `-t` wird standardmäßig **ext2** (ohne Journaling) angelegt – daher den Typ besser immer explizit angeben (`-t ext4`). Direkt `mkfs.<typ>` aufzurufen bietet oft mehr typspezifische Optionen. Danach mit `blkid` die UUID für `/etc/fstab` auslesen. Für Swap dient stattdessen `mkswap`.

---

### mkswap
>**Funktion:** Einen Auslagerungsbereich (Swap) einrichten<br />
>**Syntax:** `mkswap [optionen] <gerät|datei>`<br />
>**Erklärung:** Bereitet eine Partition oder eine Datei als Auslagerungsspeicher (Swap) vor. Anschließend wird der Bereich mit `swapon` aktiviert. Swap erweitert den Arbeitsspeicher auf den Datenträger.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L <label>` vergibt ein Label<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` prüft vorher auf defekte Blöcke<br />
>**Beispiel:** `sudo mkswap /dev/sdb2`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-L <label>`, `--label <label>` | vergibt ein Label für den Swap-Bereich |
| `-c`, `--check` | prüft das Gerät vorher auf defekte Blöcke |
| `-p <größe>`, `--pagesize <größe>` | legt die Seitengröße fest |
| `-U <uuid>`, `--uuid <uuid>` | setzt eine bestimmte UUID |
| `-f`, `--force` | erzwingt die Einrichtung, auch wenn es riskant erscheint |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo mkswap /dev/sdb2                              # Partition als Swap vorbereiten
sudo swapon /dev/sdb2                              # Swap aktivieren
sudo fallocate -l 2G /swapfile                     # Swap-Datei anlegen …
sudo chmod 600 /swapfile && sudo mkswap /swapfile  # … Rechte setzen und einrichten
```

</details>

>**Hinweis:** Nach `mkswap` den Swap mit `swapon` aktivieren (dauerhaft über einen Eintrag in `/etc/fstab`). Bei einer Swap-**Datei** vorher die Rechte auf `600` setzen. Aktive Swap-Bereiche zeigt `swapon --show` oder `free -h`.

---

### mount
>**Funktion:** Dateisysteme einhängen<br />
>**Syntax:** `mount [optionen] <gerät> <mountpoint>`<br />
>**Erklärung:** Bindet ein Dateisystem (z. B. Festplatte oder USB-Stick) an ein Verzeichnis ein. Ohne Argumente zeigt es die aktuell eingehängten Dateisysteme.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t <typ>` Dateisystemtyp (z. B. `ext4`, `vfat`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-o <optionen>` Einhänge-Optionen (z. B. `ro`, `rw`)<br />
>**Beispiel:** `sudo mount /dev/sdb1 /mnt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-t <typ>`, `--types` | Dateisystemtyp (z. B. `ext4`, `vfat`, `ntfs`) |
| `-o <optionen>` | Einhänge-Optionen, kommagetrennt (`ro`, `rw`, `noexec`, `loop`) |
| `-a`, `--all` | hängt alle Einträge aus `/etc/fstab` ein |
| `-r`, `--read-only` | hängt nur lesend ein |
| `-l` | listet eingehängte Dateisysteme mit Labels |
| `-w`, `--rw` | hängt schreibend ein (Standard) |
| `-B`, `--bind` | hängt ein Verzeichnis an einer zweiten Stelle ein |
| `-L <label>` | hängt das Dateisystem mit diesem Label ein |
| `-U <uuid>` | hängt das Dateisystem mit dieser UUID ein |
| `-v`, `--verbose` | ausführliche Ausgabe |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
mount                              # eingehängte Dateisysteme anzeigen
sudo mount /dev/sdb1 /mnt          # Partition einhängen
sudo mount -t vfat /dev/sdb1 /mnt  # mit Dateisystemtyp
sudo mount -o ro /dev/sdb1 /mnt    # nur lesend einhängen
sudo mount -o loop image.iso /mnt  # ISO-Abbild einhängen
```

</details>

>**Hinweis:** Braucht in der Regel `sudo`. Der Mountpoint (Zielverzeichnis) muss existieren. Dauerhafte Einhängungen werden in `/etc/fstab` eingetragen. Zum Aushängen `umount` verwenden.

---

### parted
>**Funktion:** Festplatten partitionieren (auch GPT) – interaktiv oder per Skript<br />
>**Syntax:** `parted [optionen] [<gerät>] [befehl [argumente...]]`<br />
>**Erklärung:** Mächtiges Werkzeug zum Anlegen, Löschen und Verändern von Partitionen. Anders als das klassische `fdisk` beherrscht es problemlos große Platten und **GPT**, kann Partitionen verschieben/verkleinern und lässt sich auch nichtinteraktiv (`-s`) in Skripten einsetzen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` listet die Partitionen aller Geräte auf<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` Skriptmodus ohne Rückfragen<br />
>**Beispiel:** `sudo parted -l`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--list` | listet die Partitionstabellen aller Geräte auf |
| `-s`, `--script` | Skriptmodus: keine Rückfragen (für Automatisierung) |
| `-m`, `--machine` | maschinenlesbare Ausgabe |
| `-a <typ>`, `--align <typ>` | Ausrichtung (`none`, `cyl`, `min`, `optimal`) |
| `-f`, `--fix` | behebt erkannte Probleme automatisch (Skriptmodus) |
| `-h`, `--help` | zeigt die Hilfe an |
| `-v`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Interne Befehle (interaktiv oder als Argument)</summary>

| Befehl | Wirkung |
|---|---|
| `print` | zeigt die aktuelle Partitionstabelle |
| `mklabel <typ>` | legt eine neue Tabelle an (`gpt`, `msdos`) |
| `mkpart …` | erstellt eine Partition |
| `resizepart <n> <ende>` | ändert die Größe einer Partition |
| `rm <n>` | löscht Partition n |
| `quit` | beendet parted |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo parted -l                                         # alle Geräte auflisten
sudo parted /dev/sdb print                             # Tabelle eines Geräts
sudo parted -s /dev/sdb mklabel gpt                    # GPT-Tabelle (Skript)
sudo parted -s /dev/sdb mkpart primary ext4 1MiB 100%  # eine Partition anlegen
```

</details>

>**Hinweis:** **Vorsicht** – `parted` schreibt viele Änderungen **sofort** auf den Datenträger (nicht erst nach „Write" wie `fdisk`); Gerät vorher mit `lsblk` genau prüfen. Für reine GPT-Arbeit gibt es `gdisk`, einsteigerfreundlich ist `cfdisk`. Ein Dateisystem legt man danach mit `mkfs` an.

---

### partprobe
>**Funktion:** Den Kernel die Partitionstabelle neu einlesen lassen<br />
>**Syntax:** `partprobe [optionen] [<gerät>]`<br />
>**Erklärung:** Weist den Kernel an, die Partitionstabelle eines Datenträgers erneut einzulesen, ohne dass ein Neustart nötig ist. Nützlich, nachdem man mit `fdisk`/`parted` Partitionen geändert hat und der Kernel die neue Aufteilung noch nicht kennt.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` zeigt eine Zusammenfassung der Partitionen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` führt nichts aus, meldet nur (Testlauf)<br />
>**Beispiel:** `sudo partprobe /dev/sdb`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-s`, `--summary` | zeigt eine Zusammenfassung der erkannten Partitionen |
| `-d`, `--dry-run` | ändert nichts, meldet nur (Testlauf) |
| `-h`, `--help` | zeigt die Hilfe an |
| `-v`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo partprobe               # alle Geräte neu einlesen
sudo partprobe /dev/sdb      # nur ein bestimmtes Gerät
sudo partprobe -s /dev/sdb   # mit Zusammenfassung
```

</details>

>**Hinweis:** Braucht `sudo` und funktioniert nur, wenn das Gerät nicht in Benutzung ist (keine eingehängten Partitionen). Gehört zum Paket `parted`. Alternativen: `partx -u <gerät>` oder als letztes Mittel ein Neustart.

---

### resize2fs
>**Funktion:** Ein ext2/3/4-Dateisystem vergrößern oder verkleinern<br />
>**Syntax:** `resize2fs [optionen] <gerät> [<größe>]`<br />
>**Erklärung:** Passt die Größe eines ext-Dateisystems an – meist nachdem die zugrunde liegende Partition (mit `parted`/`fdisk`) oder das LVM-Volume geändert wurde. Ohne Größenangabe wird das Dateisystem auf die volle Größe der Partition ausgedehnt.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-M` verkleinert auf die minimal mögliche Größe<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-P` zeigt nur die minimal mögliche Größe an<br />
>**Beispiel:** `sudo resize2fs /dev/sda1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-p` | zeigt einen Fortschrittsbalken |
| `-M` | verkleinert auf die kleinstmögliche Größe |
| `-f` | erzwingt die Aktion (überspringt einige Sicherheitschecks) |
| `-P` | zeigt nur die minimal mögliche Größe an, ohne zu ändern |
| `<größe>` | Zielgröße mit Einheit, z. B. `20G`, `500M` (ohne = ganze Partition) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo resize2fs /dev/sda1        # auf die volle Partitionsgröße ausdehnen
sudo resize2fs /dev/sda1 20G    # auf 20 GiB setzen
sudo resize2fs -M /dev/sda1     # so klein wie möglich machen
sudo resize2fs -P /dev/sda1     # nur die Mindestgröße anzeigen
```

</details>

>**Hinweis:** **Reihenfolge beachten** – beim **Vergrößern** zuerst die Partition, dann das Dateisystem; beim **Verkleinern** zuerst das Dateisystem, dann die Partition. Vor dem Verkleinern `sudo e2fsck -f` ausführen. Online-**Vergrößern** geht bei ext4 im eingehängten Zustand, **Verkleinern** nur ausgehängt. Nur ext-Familie (XFS lässt sich mit `xfs_growfs` ausschließlich vergrößern).

---

### tune2fs
>**Funktion:** Einstellbare Parameter eines ext2/3/4-Dateisystems anzeigen und ändern<br />
>**Syntax:** `tune2fs [optionen] <gerät>`<br />
>**Erklärung:** Ändert Parameter eines **bestehenden** ext-Dateisystems, ohne es neu anzulegen – etwa Label, UUID, den Anteil reservierter Blöcke oder die Intervalle für automatische Prüfungen. Ergänzt so `mkfs` (erstellen) und `fsck` (prüfen).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zeigt den Inhalt des Superblocks<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L <label>` setzt das Label<br />
>**Beispiel:** `sudo tune2fs -l /dev/sda1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l` | zeigt die Superblock-Parameter (wie `dumpe2fs -h`) |
| `-L <label>` | setzt das Label (Volume-Name) |
| `-U <uuid>` | setzt eine neue UUID (`random` erzeugt eine zufällige) |
| `-m <prozent>` | Anteil der für root reservierten Blöcke (Standard 5 %) |
| `-r <blöcke>` | reservierte Blöcke als absolute Anzahl |
| `-c <n>` | Prüfung nach n Einhängevorgängen (`-c 0` = aus) |
| `-i <intervall>` | Prüfintervall nach Zeit (z. B. `30d`, `0` = aus) |
| `-j` | fügt ein Journal hinzu (macht aus ext2 ein ext3) |
| `-e <verhalten>` | Reaktion auf Fehler (`continue`, `remount-ro`, `panic`) |
| `-O <feature>` | schaltet ein Dateisystem-Feature ein/aus (z. B. `^has_journal`) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo tune2fs -l /dev/sda1          # alle Parameter anzeigen
sudo tune2fs -m 1 /dev/sda1        # reservierte Blöcke auf 1 % senken
sudo tune2fs -L DATEN /dev/sda1    # Label setzen
sudo tune2fs -c 0 -i 0 /dev/sda1   # automatische fsck-Prüfungen abschalten
sudo tune2fs -j /dev/sda1          # Journal ergänzen (ext2 → ext3)
```

</details>

>**Hinweis:** Strukturändernde Optionen (`-O`, `-j`) nur auf **ausgehängten** Dateisystemen anwenden; `-l` liest auch im eingehängten Zustand. Auf Datenpartitionen senkt man die reservierten 5 % oft auf 0–1 % (`-m`). Nur ext-Familie (e2fsprogs). Dieselben Daten nur-lesend zeigt `dumpe2fs`.

---

### umount
>**Funktion:** Dateisysteme aushängen<br />
>**Syntax:** `umount [optionen] <gerät|mountpoint>`<br />
>**Erklärung:** Hängt ein zuvor eingehängtes Dateisystem wieder aus. Erst danach lässt sich z. B. ein USB-Stick sicher entfernen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` verzögertes Aushängen (lazy)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Aushängen<br />
>**Beispiel:** `sudo umount /mnt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--lazy` | hängt aus, sobald das Dateisystem nicht mehr benutzt wird |
| `-f`, `--force` | erzwingt das Aushängen (z. B. bei nicht erreichbarem Netzlaufwerk) |
| `-a`, `--all` | hängt alle Dateisysteme aus `/etc/fstab` aus |
| `-R`, `--recursive` | hängt verschachtelte Mountpoints aus |
| `-r`, `--read-only` | hängt bei Fehler stattdessen nur lesend wieder ein |
| `-v`, `--verbose` | ausführliche Ausgabe |
| `-t <typ>`, `--types <typ>` | beschränkt auf bestimmte Dateisystemtypen |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo umount /mnt        # über den Mountpoint aushängen
sudo umount /dev/sdb1   # über das Gerät aushängen
sudo umount -l /mnt     # verzögert aushängen (wenn noch in Benutzung)
```

</details>

>**Hinweis:** Der Befehl heißt `umount` (ohne „n"!). „target is busy" bedeutet, dass noch ein Prozess das Dateisystem nutzt – mit `lsof +D /mnt` oder `fuser -m /mnt` lässt sich der Verursacher finden.

---

### xfs_admin
>**Funktion:** Parameter eines XFS-Dateisystems anzeigen und ändern (Label, UUID)<br />
>**Syntax:** `xfs_admin [optionen] <gerät>`<br />
>**Erklärung:** Ändert einstellbare Parameter eines bestehenden XFS-Dateisystems – vor allem Label und UUID. Das XFS-Gegenstück zu `tune2fs` bei ext. Für Änderungen muss das Dateisystem ausgehängt sein.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zeigt das aktuelle Label<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L <label>` setzt das Label<br />
>**Beispiel:** `sudo xfs_admin -L DATEN /dev/sdb1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l` | zeigt das aktuelle Label |
| `-L <label>` | setzt das Label (max. 12 Zeichen; `--` entfernt es) |
| `-u` | zeigt die UUID an |
| `-U <uuid>` | setzt eine neue UUID (`generate` erzeugt eine zufällige) |
| `-c 1` | schaltet lazy-counters ein (`-c 0` = aus) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo xfs_admin -l /dev/sdb1           # Label anzeigen
sudo xfs_admin -L DATEN /dev/sdb1     # Label setzen
sudo xfs_admin -u /dev/sdb1           # UUID anzeigen
sudo xfs_admin -U generate /dev/sdb1  # neue zufällige UUID
```

</details>

>**Hinweis:** XFS-Gegenstück zu `tune2fs`. Das Label ist bei XFS auf 12 Zeichen begrenzt. Änderungen nur auf **ausgehängten** Dateisystemen; reines Anzeigen (`-l`/`-u`) geht auch eingehängt. Gehört zum Paket `xfsprogs`.

---

### xfs_growfs
>**Funktion:** Ein XFS-Dateisystem vergrößern<br />
>**Syntax:** `xfs_growfs [optionen] <mountpoint>`<br />
>**Erklärung:** Vergrößert ein XFS-Dateisystem auf die verfügbare Größe der Partition bzw. des Volumes. Anders als bei ext (`resize2fs`) arbeitet `xfs_growfs` mit dem **Mountpoint** und nur im **eingehängten** Zustand – und XFS lässt sich ausschließlich **vergrößern**, nie verkleinern.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` vergrößert den Datenbereich auf das Maximum<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` zeigt nur die Geometrie, ohne zu ändern<br />
>**Beispiel:** `sudo xfs_growfs /daten`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-d`, `--data` | dehnt den Datenbereich auf die maximale Größe aus (Standard) |
| `-D <größe>` | setzt eine bestimmte Zielgröße (in Blöcken) |
| `-n` | zeigt nur die aktuelle Geometrie an, ohne zu ändern |
| `-m <wert>` | ändert den maximalen Inode-Prozentsatz |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo xfs_growfs /daten     # auf die volle Größe ausdehnen
sudo xfs_growfs -n /daten  # nur die Geometrie anzeigen
sudo xfs_growfs /          # Wurzel-Dateisystem vergrößern
```

</details>

>**Hinweis:** Zuerst die zugrunde liegende Partition bzw. das LVM-Volume vergrößern (`parted`, `lvextend`), dann `xfs_growfs`. Nimmt den **Mountpoint** (nicht das Gerät) und läuft im eingehängten Zustand. **Verkleinern ist bei XFS nicht möglich** – dafür neu anlegen und zurückkopieren. ext-Gegenstück: `resize2fs`.

---

### xfs_info
>**Funktion:** Geometrie und Parameter eines XFS-Dateisystems anzeigen<br />
>**Syntax:** `xfs_info <mountpoint|gerät>`<br />
>**Erklärung:** Zeigt den Aufbau eines XFS-Dateisystems – Blockgröße, Anzahl der Allocation Groups, Journal-Größe usw. Das XFS-Gegenstück zu `dumpe2fs -h` bei ext. Praktisch, um vor einem `xfs_growfs` die aktuelle Geometrie zu sehen.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`xfs_info <mountpoint>` zeigt die Struktur des eingehängten FS<br />
>&nbsp;&nbsp;&nbsp;&nbsp;(entspricht `xfs_growfs -n`)<br />
>**Beispiel:** `xfs_info /daten`

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
xfs_info /daten          # Geometrie des eingehängten FS
xfs_info /               # Wurzel-Dateisystem
sudo xfs_info /dev/sdb1  # per Gerät (ausgehängt)
```

</details>

>**Hinweis:** Nur-lesend – ändert nichts. Bevorzugt den **Mountpoint** (bei eingehängtem FS); intern entspricht es `xfs_growfs -n`. XFS-Gegenstück zu `dumpe2fs -h`. Gehört zum Paket `xfsprogs`.

---

### xfs_repair
>**Funktion:** Ein XFS-Dateisystem prüfen und reparieren<br />
>**Syntax:** `xfs_repair [optionen] <gerät>`<br />
>**Erklärung:** Prüft ein XFS-Dateisystem auf Fehler und repariert sie. Bei XFS gibt es **kein** funktionsfähiges `fsck.xfs` – die Konsistenzprüfung und Reparatur läuft ausschließlich über `xfs_repair`. Das Dateisystem muss dazu **ausgehängt** sein.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` prüft nur (no modify), ohne zu reparieren<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L` verwirft das Journal (letztes Mittel, Datenverlust möglich)<br />
>**Beispiel:** `sudo xfs_repair /dev/sdb1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n` | nur prüfen, nichts ändern (dry run) |
| `-L` | setzt das Log/Journal zurück (nur als **letztes Mittel**, riskant) |
| `-d` | erlaubt die Reparatur einer read-only eingehängten Root (gefährlich) |
| `-v` | ausführliche Ausgabe |
| `-m <MB>` | begrenzt den Speicherverbrauch |
| `-l <gerät>` | gibt ein externes Log-Gerät an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo xfs_repair /dev/sdb1                         # prüfen und reparieren
sudo xfs_repair -n /dev/sdb1                      # nur prüfen, nichts ändern
sudo umount /daten && sudo xfs_repair /dev/sdb1   # vorher aushängen
sudo xfs_repair -L /dev/sdb1                      # Journal verwerfen (Notfall!)
```

</details>

>**Hinweis:** Nur auf **ausgehängten** XFS-Dateisystemen. Lässt sich das FS wegen eines beschädigten Journals nicht reparieren, hilft oft, es einmal ein- und wieder auszuhängen (Journal-Replay). `-L` verwirft das Journal und **kann Daten kosten** – wirklich nur im Notfall. XFS-Gegenstück zu `e2fsck`; ein `fsck /dev/sdX` bewirkt bei XFS nichts. Paket `xfsprogs`.

---

