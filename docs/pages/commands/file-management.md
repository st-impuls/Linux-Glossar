[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Navigation & Suche](navigation-search.md)
- [Netzwerk & Download](network-download.md)
- [Paketverwaltung](package-management.md)
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Datei- & Verzeichnisverwaltung

### cp
>**Funktion:** Dateien oder Ordner kopieren | Extern<br />
>**Syntax:** `cp [optionen] <quelle>... <ziel>`<br />
>**Erklärung:** Kopiert Dateien oder Verzeichnisse an einen anderen Ort. Die letzte Angabe ist das Ziel; davor stehen eine oder mehrere Quellen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` kopiert rekursiv (für Ordner mit Inhalt)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor dem Überschreiben nach<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt die kopierten Dateien an<br />
>**Beispiel:** `cp datei.txt /home/user/backup/`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-r`, `-R`, `--recursive` | kopiert rekursiv (ganze Verzeichnisse mit Inhalt) |
| `-i`, `--interactive` | fragt vor dem Überschreiben nach |
| `-n`, `--no-clobber` | überschreibt vorhandene Dateien **nicht** |
| `-f`, `--force` | erzwingt das Kopieren, entfernt störende Zieldateien |
| `-v`, `--verbose` | zeigt jede kopierte Datei an (ausführlich) |
| `-p`, `--preserve` | erhält Attribute: Zeitstempel, Rechte, Eigentümer |
| `-a`, `--archive` | Archivmodus: rekursiv und alle Attribute erhalten (gut für Backups) |
| `-u`, `--update` | kopiert nur, wenn die Quelle neuer ist oder das Ziel fehlt |
| `-l`, `--link` | erstellt Hardlinks statt echter Kopien |
| `-s`, `--symbolic-link` | erstellt symbolische Links statt Kopien |
| `-b`, `--backup` | legt von vorhandenen Zieldateien eine Sicherung an |
| `-t <ziel>`, `--target-directory=<ziel>` | gibt das Zielverzeichnis zuerst an |
| `-T`, `--no-target-directory` | behandelt das Ziel als normale Datei, nicht als Verzeichnis |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
cp -r projekt/ backup/          # ganzen Ordner mit Inhalt kopieren
cp -i datei.txt ziel.txt        # vor Überschreiben nachfragen
cp -av quelle/ /sicherung/      # Backup mit erhaltenen Attributen, ausführlich
cp datei1.txt datei2.txt ziel/  # mehrere Dateien in ein Verzeichnis
cp -u *.txt /home/user/docs/    # nur neuere/fehlende Dateien aktualisieren
```

</details>

>**Hinweis:** Standardmäßig überschreibt `cp` vorhandene Zieldateien ohne Rückfrage. Mit `-i` (nachfragen) oder `-n` (nie überschreiben) lässt sich das verhindern.

---

### mkdir
>**Funktion:** Neues Verzeichnis erstellen | Extern<br />
>**Syntax:** `mkdir [optionen] <verzeichnis>...`<br />
>**Erklärung:** Erstellt einen oder mehrere neue Ordner.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` erstellt bei Bedarf auch übergeordnete Verzeichnisse (verschachtelte Pfade)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jedes erstellte Verzeichnis eine Meldung an<br />
>**Beispiel:** `mkdir -p projekt/quellcode`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-p`, `--parents` | erstellt bei Bedarf übergeordnete Verzeichnisse; meldet keinen Fehler, wenn der Ordner schon existiert |
| `-v`, `--verbose` | zeigt für jedes erstellte Verzeichnis eine Meldung an (ausführlich) |
| `-m <modus>`, `--mode=<modus>` | setzt die Zugriffsrechte des neuen Verzeichnisses direkt (z. B. `-m 755`) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
mkdir berichte                       # ein einzelnes Verzeichnis anlegen
mkdir bilder musik videos            # mehrere Verzeichnisse auf einmal
mkdir -p projekt/quellcode/module    # verschachtelten Pfad in einem Schritt
mkdir -m 700 privat                  # Ordner nur für den Eigentümer zugänglich
```

</details>

>**Hinweis:** Ohne -p bricht mkdir mit einem Fehler ab, wenn ein übergeordneter Ordner fehlt oder das Verzeichnis bereits existiert.

---

### mv
>**Funktion:** Dateien oder Ordner verschieben oder umbenennen | Extern<br />
>**Syntax:** `mv [optionen] <quelle>... <ziel>`<br />
>**Erklärung:** Verschiebt eine Datei bzw. einen Ordner an einen anderen Ort oder benennt sie um. Gibt es nur eine Quelle und das Ziel ist kein Verzeichnis, wird umbenannt; bei mehreren Quellen muss das Ziel ein Verzeichnis sein.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor dem Überschreiben nach<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt die ausgeführte Aktion an<br />
>**Beispiel:** `mv alt.txt neu.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-i`, `--interactive` | fragt vor dem Überschreiben nach |
| `-n`, `--no-clobber` | überschreibt vorhandene Dateien **nicht** |
| `-f`, `--force` | überschreibt ohne Rückfrage (Standardverhalten, unterdrückt `-i`) |
| `-v`, `--verbose` | zeigt jede ausgeführte Aktion an (ausführlich) |
| `-u`, `--update` | verschiebt nur, wenn die Quelle neuer ist oder das Ziel fehlt |
| `-b`, `--backup` | legt von vorhandenen Zieldateien eine Sicherung an |
| `-t <ziel>`, `--target-directory=<ziel>` | gibt das Zielverzeichnis zuerst an |
| `-T`, `--no-target-directory` | behandelt das Ziel als normale Datei, nicht als Verzeichnis |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
mv alt.txt neu.txt              # Datei umbenennen
mv datei.txt /home/user/docs/   # Datei in ein Verzeichnis verschieben
mv ordner/ /sicherung/          # ganzen Ordner verschieben
mv -i *.txt archiv/             # mehrere Dateien verschieben, vorher nachfragen
mv -t archiv/ a.txt b.txt c.txt # Ziel zuerst, dann mehrere Quellen
```

</details>

>**Hinweis:** Wie `cp` überschreibt `mv` vorhandene Zieldateien standardmäßig ohne Rückfrage. Mit `-i` (nachfragen) oder `-n` (nie überschreiben) lässt sich das verhindern. Für Ordner gibt es kein `-r` – `mv` verschiebt Verzeichnisse immer komplett.

---

### rm
>**Funktion:** Dateien oder Verzeichnisse löschen | Extern<br />
>**Syntax:** `rm [optionen] <datei>...`<br />
>**Erklärung:** Löscht Dateien; mit `-r` auch ganze Verzeichnisse samt Inhalt.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` löscht rekursiv (Ordner samt Inhalt)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Löschen ohne Nachfrage<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor jedem Löschen nach<br />
>**Beispiel:** `rm datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-r`, `-R`, `--recursive` | löscht rekursiv (Verzeichnisse samt Inhalt) |
| `-f`, `--force` | erzwingt das Löschen, ignoriert fehlende Dateien, fragt nie nach |
| `-i` | fragt vor **jedem** Löschen nach |
| `-I` | fragt **einmal** nach, bevor mehr als drei Dateien oder rekursiv gelöscht wird (weniger aufdringlich als `-i`) |
| `-d`, `--dir` | löscht leere Verzeichnisse |
| `-v`, `--verbose` | zeigt jede gelöschte Datei an (ausführlich) |
| `--one-file-system` | bleibt bei `-r` innerhalb desselben Dateisystems (überspringt eingehängte) |
| `--preserve-root` | schützt das Wurzelverzeichnis `/` vor versehentlichem Löschen (Standard) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
rm datei.txt                # einzelne Datei löschen
rm -i *.tmp                 # vor jedem Löschen nachfragen
rm -r alter_ordner/         # Verzeichnis mit Inhalt löschen
rm -rf build/               # Verzeichnis ohne Rückfrage erzwingen (Vorsicht!)
rm -d leerer_ordner/        # leeres Verzeichnis löschen
```

</details>

>**Hinweis:** Vorsicht – Gelöschtes landet **nicht** im Papierkorb, sondern ist sofort weg. Besonders `rm -rf` ist gefährlich: prüfe Pfad und Platzhalter, bevor du Enter drückst. Zum Löschen leerer Verzeichnisse ist `rmdir` die sicherere Wahl.

---

### rmdir
>**Funktion:** Leeres Verzeichnis löschen | Extern<br />
>**Syntax:** `rmdir [optionen] <verzeichnis>...`<br />
>**Erklärung:** Löscht ein Verzeichnis, aber nur wenn es **leer** ist. Enthält es noch Dateien oder Unterordner, bricht der Befehl mit einer Fehlermeldung ab.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` entfernt auch übergeordnete Verzeichnisse, sofern diese leer sind<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jedes entfernte Verzeichnis eine Meldung an<br />
>**Beispiel:** `rmdir alter_ordner`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-p`, `--parents` | entfernt auch übergeordnete Verzeichnisse, sofern diese nach dem Löschen leer sind (z. B. `a/b/c` → `a/b` → `a`) |
| `-v`, `--verbose` | zeigt für jedes entfernte Verzeichnis eine Meldung an (ausführlich) |
| `--ignore-fail-on-non-empty` | meldet keinen Fehler, wenn ein Verzeichnis wegen vorhandenen Inhalts nicht gelöscht werden kann |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
rmdir leerer_ordner                 # einzelnes leeres Verzeichnis löschen
rmdir bilder musik videos           # mehrere leere Verzeichnisse auf einmal
rmdir -p projekt/quellcode/module   # verschachtelten, leeren Pfad komplett entfernen
```

</details>

>**Hinweis:** `rmdir` ist die **sichere** Variante zum Aufräumen: Es kann nichts versehentlich mitlöschen, da nur leere Verzeichnisse entfernt werden. Für nicht leere Verzeichnisse `rm -r` verwenden (aber mit Vorsicht).

---

### stat
>**Funktion:** Detaillierte Datei- oder Verzeichnisinformationen anzeigen | Extern<br />
>**Syntax:** `stat [optionen] <datei>...`<br />
>**Erklärung:** Zeigt ausführliche Informationen zu einer Datei oder einem Verzeichnis an: Größe, Rechte, Besitzer, Inode und die Zeitstempel für Zugriff, Änderung und Statusänderung.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <format>` gibt nur bestimmte Felder im eigenen Format aus (z. B. `-c '%s'` = Größe)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L` folgt symbolischen Links und zeigt das Ziel an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t` kompakte Ausgabe in einer Zeile<br />
>**Beispiel:** `stat datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-c <format>`, `--format=<format>` | gibt nur bestimmte Felder im eigenen Format aus (z. B. `%s` = Größe, `%a` = Rechte oktal, `%U` = Besitzer) |
| `--printf=<format>` | wie `-c`, interpretiert aber `\n`, `\t` usw. und hängt keinen Zeilenumbruch an |
| `-f`, `--file-system` | zeigt Informationen zum **Dateisystem** statt zur Datei |
| `-L`, `--dereference` | folgt symbolischen Links und zeigt das Ziel an |
| `-t`, `--terse` | kompakte Ausgabe in einer Zeile (gut für Skripte) |

</details>

<details markdown>
<summary>Häufige Format-Felder (`-c`)</summary>

| Feld | Bedeutung |
|---|---|
| `%n` | Dateiname |
| `%s` | Größe in Bytes |
| `%a` | Zugriffsrechte oktal (z. B. `644`) |
| `%A` | Zugriffsrechte symbolisch (z. B. `-rw-r--r--`) |
| `%U` / `%G` | Besitzer / Gruppe (Name) |
| `%u` / `%g` | Besitzer / Gruppe (ID) |
| `%i` | Inode-Nummer |
| `%x` / `%y` / `%z` | letzter Zugriff / Inhaltsänderung / Statusänderung |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
stat datei.txt              # vollständige Informationen anzeigen
stat -c '%s' datei.txt      # nur die Größe in Bytes
stat -c '%A %U %n' *.txt    # Rechte, Besitzer und Name aller .txt-Dateien
stat -f /home               # Informationen zum Dateisystem von /home
```

</details>

>**Hinweis:** Zeigt mehr Details als `ls -l`, u. a. die drei Zeitstempel: Zugriff (atime), Inhaltsänderung (mtime) und Metadaten-/Statusänderung (ctime). Die Rechte werden sowohl symbolisch als auch oktal angezeigt.

---

### touch
>**Funktion:** Neue Datei erstellen oder Zeitstempel ändern | Extern<br />
>**Syntax:** `touch [optionen] <datei>...`<br />
>**Erklärung:** Erstellt eine leere Datei oder aktualisiert den Zeitstempel einer vorhandenen Datei auf die aktuelle Zeit.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` erstellt keine Datei, wenn sie noch nicht existiert<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t <stempel>` setzt eine bestimmte Zeit ([[CC]YY]MMDDhhmm[.ss])<br />
>**Beispiel:** `touch notizen.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-c`, `--no-create` | erstellt keine neue Datei, ändert nur vorhandene |
| `-a` | ändert nur die Zugriffszeit (atime) |
| `-m` | ändert nur die Änderungszeit (mtime) |
| `-t <stempel>` | setzt eine bestimmte Zeit im Format `[[CC]YY]MMDDhhmm[.ss]`, mehr dazu -> "Weiter Beispiele" |
| `-d <datum>`, `--date=<datum>` | setzt die Zeit aus einer freien Datumsangabe (z. B. `"2 days ago"`, `"2026-01-01 10:00"`) |
| `-r <datei>`, `--reference=<datei>` | übernimmt die Zeitstempel einer anderen Datei |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
touch notizen.txt              # leere Datei erstellen (oder Zeit aktualisieren)
touch a.txt b.txt c.txt        # mehrere Dateien auf einmal anlegen
touch -c vorhanden.txt         # nur aktualisieren, nichts Neues anlegen
touch -d "2026-01-01 10:00" datei.txt   # Zeitstempel auf ein festes Datum setzen
touch -r vorlage.txt neu.txt   # Zeitstempel von vorlage.txt übernehmen
touch -t 202601011000 datei.txt        # 01.01.2026, 10:00:00
touch -t 2606231830.45 datei.txt       # 23.06.2026, 18:30:45  (YY = 26)
touch -t 06231830 datei.txt            # 23.06., 18:30 – aktuelles Jahr
touch -t 12312359.59 jahr.txt          # 31.12., 23:59:59 – aktuelles Jahr

Hinweise zu -t:
Das Jahr darf entfallen (06231830) – dann wird das aktuelle Jahr verwendet. Zweistelliges Jahr: 00–68 → 20xx, 69–99 → 19xx.
Die Sekunden (.ss) sind optional.
-t ändert beide Zeitstempel gleichzeitig (atime und mtime). Soll nur einer geändert werden, zusätzlich -a oder -m angeben.
Wer sich das Format nicht merken möchte, nimmt einfacher -d: touch -d "2026-01-01 10:00" datei.txt (versteht auch „menschliche“ Angaben wie "yesterday" oder "2 hours ago").
```

</details>

>**Hinweis:** `touch` wird oft genutzt, um schnell eine leere Datei anzulegen oder den Zeitstempel einer Datei zu „erneuern“ (z. B. damit Build-Tools sie als geändert erkennen).

---

