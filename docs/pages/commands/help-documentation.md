[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
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

# Hilfe & Dokumentation

### apropos
>**Funktion:** Man-Seiten nach Stichwort durchsuchen<br />
>**Syntax:** `apropos [optionen] <stichwort>...`<br />
>**Erklärung:** Durchsucht die Kurzbeschreibungen aller Handbuchseiten nach einem Stichwort und listet passende Befehle auf. Nützlich, wenn man weiß, *was* man tun will, aber nicht, *welcher* Befehl dafür zuständig ist. Gleichbedeutend mit `man -k`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-e` sucht nach exakten Wörtern<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <abschnitt>` beschränkt auf einen Handbuchabschnitt<br />
>**Beispiel:** `apropos "copy files"`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-e`, `--exact` | findet nur exakte Übereinstimmungen |
| `-w`, `--wildcard` | erlaubt Platzhalter (`*`, `?`) im Suchbegriff |
| `-r`, `--regex` | interpretiert den Suchbegriff als regulären Ausdruck (Standard) |
| `-a`, `--and` | zeigt nur Einträge, die auf **alle** Stichwörter passen |
| `-s <liste>`, `--sections=<liste>` | beschränkt die Suche auf bestimmte Handbuchabschnitte |
| `-l`, `--long` | bricht die Ausgabe nicht auf die Terminalbreite ab |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
apropos editor              # Befehle rund um "editor"
apropos -s 1 copy           # nur Abschnitt 1 (Benutzerbefehle)
apropos -a network manager  # passt auf beide Wörter
man -k passwd               # gleichbedeutend zu apropos
```

</details>

>**Hinweis:** Greift auf dieselbe Datenbank zu wie `whatis` und `man -k`. Ist die Datenbank leer oder veraltet, baut `sudo mandb` sie neu auf. Zeigt nur, *welcher* Befehl passt – die volle Beschreibung dann mit `man <befehl>`.

---

### help | Intern (Builtins)
>**Funktion:** Hilfe zu eingebauten Shell-Befehlen anzeigen<br />
>**Syntax:** `help [optionen] [<muster>]`<br />
>**Erklärung:** Zeigt die eingebaute Hilfe der Shell zu ihren Builtins (z. B. `cd`, `export`, `for`). Für diese gibt es keine eigenen Man-Seiten, weil sie Teil der Shell selbst sind. Ohne Argument listet `help` alle Builtins auf.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt nur eine Kurzbeschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` zeigt die Hilfe im Man-Seiten-Format<br />
>**Beispiel:** `help cd`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-d` | zeigt zu jedem Treffer nur eine kurze Beschreibung |
| `-m` | zeigt die Hilfe im Format einer Handbuchseite |
| `-s` | zeigt nur die Syntax (Kurzform der Verwendung) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
help            # alle Builtins auflisten
help cd         # Hilfe zum Builtin cd
help -s export  # nur die Syntax von export
help for        # Hilfe zur for-Schleife
```

</details>

>**Hinweis:** Funktioniert nur für **eingebaute** Shell-Befehle; für externe Programme (`ls`, `grep` …) stattdessen `man` oder `<befehl> --help`. `help` ist selbst ein Bash-Builtin und in anderen Shells (z. B. zsh) nicht identisch.

---

### info
>**Funktion:** GNU-Dokumentation im Info-Format anzeigen<br />
>**Syntax:** `info [optionen] [<thema>]`<br />
>**Erklärung:** Öffnet die ausführliche GNU-Dokumentation, die oft umfangreicher ist als die Man-Seite. Info-Seiten sind wie Hypertext in verlinkte Knoten (Nodes) gegliedert, durch die man navigieren kann. Ohne Argument öffnet sich das Verzeichnis aller Themen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <datei>` öffnet eine bestimmte Info-Datei<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-k <stichwort>` sucht in allen Info-Handbüchern<br />
>**Beispiel:** `info coreutils`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-f <datei>`, `--file=<datei>` | öffnet eine bestimmte Info-Datei |
| `-n <knoten>`, `--node=<knoten>` | springt direkt zu einem Knoten (Node) |
| `-k <wort>`, `--apropos=<wort>` | sucht ein Stichwort in allen Info-Handbüchern |
| `-o <datei>`, `--output=<datei>` | schreibt den Text in eine Datei, statt ihn anzuzeigen |
| `--subnodes` | gibt auch untergeordnete Knoten mit aus |
| `-h`, `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
info                            # Verzeichnis aller Info-Themen
info coreutils                  # Handbuch zu den GNU-Coreutils
info bash                       # ausführliches Bash-Handbuch
info coreutils 'ls invocation'  # direkt zum Abschnitt über ls
```

</details>

>**Hinweis:** Navigation im Info-Reader: `Leertaste` blättert, `n`/`p` nächster/voriger Knoten, `u` eine Ebene höher, `q` beendet. Fehlt eine Info-Seite, greift `info` auf die Man-Seite zurück. Kürzer und meist ausreichend ist `man <befehl>`.

---

### man
>**Funktion:** Handbuchseite (Manual) eines Befehls anzeigen<br />
>**Syntax:** `man [optionen] [<abschnitt>] <name>`<br />
>**Erklärung:** Zeigt die Handbuchseite zu einem Befehl, einer Datei oder einem Systemaufruf – das zentrale Nachschlagewerk unter Linux. Die Seiten sind in Abschnitte gegliedert (1 = Benutzerbefehle, 5 = Konfigurationsdateien, 8 = Systemverwaltung usw.).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-k <stichwort>` durchsucht alle Kurzbeschreibungen (wie `apropos`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <name>` zeigt die Kurzbeschreibung (wie `whatis`)<br />
>**Beispiel:** `man ls`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-k <muster>`, `--apropos` | durchsucht die Kurzbeschreibungen (entspricht `apropos`) |
| `-f <name>`, `--whatis` | zeigt die einzeilige Beschreibung (entspricht `whatis`) |
| `-a`, `--all` | zeigt nacheinander alle passenden Seiten, nicht nur die erste |
| `-w`, `--where` | zeigt den Pfad der Handbuchdatei, statt sie zu öffnen |
| `-K <text>`, `--global-apropos` | durchsucht den **gesamten** Text aller Seiten |
| `<abschnitt>` | wählt einen bestimmten Abschnitt (z. B. `man 5 passwd`) |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
man ls            # Handbuch zum Befehl ls
man 5 passwd      # Datei /etc/passwd (Abschnitt 5)
man -k directory  # Seiten rund um "directory" suchen
man -f printf     # Kurzbeschreibung anzeigen
man man           # das Handbuch von man selbst
```

</details>

>**Hinweis:** Navigation mit `Pfeiltasten`/`Leertaste`, Suche mit `/muster`, beenden mit `q` (nutzt den Pager, meist `less`). Denselben Namen kann es in mehreren Abschnitten geben – dann die Nummer angeben (`man 5 passwd`). Kurzbeschreibung: `whatis`, Stichwortsuche: `apropos`.

---

### tldr
>**Funktion:** Knappe Praxisbeispiele zu einem Befehl anzeigen<br />
>**Syntax:** `tldr [optionen] <befehl>`<br />
>**Erklärung:** Zeigt statt der vollständigen Man-Seite eine kurze, von der Community gepflegte Sammlung typischer Anwendungsbeispiele. Ideal, um schnell die gebräuchlichsten Aufrufe eines Befehls zu sehen, ohne die lange Dokumentation zu lesen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` aktualisiert den lokalen Seiten-Cache<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p <plattform>` wählt die Plattform (z. B. `linux`)<br />
>**Beispiel:** `tldr tar`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-u`, `--update` | aktualisiert den lokalen Zwischenspeicher der tldr-Seiten |
| `-p <plattform>`, `--platform <plattform>` | wählt die Plattform (`linux`, `osx`, `windows` …) |
| `-l`, `--list` | listet alle verfügbaren Seiten auf |
| `-h`, `--help` | zeigt die Hilfe an |
| `-v`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
tldr tar         # typische tar-Aufrufe
tldr git commit  # Beispiele zu einem Unterbefehl
tldr -u          # Seiten-Cache aktualisieren
tldr -l          # alle verfügbaren Seiten auflisten
```

</details>

>**Hinweis:** **Keine Standardsoftware** – muss nachinstalliert werden (z. B. `sudo apt install tldr` oder über `npm install -g tldr`). Ergänzt `man`, ersetzt es aber nicht: `tldr` zeigt Beispiele, `man` die vollständige Referenz.

---

### whatis
>**Funktion:** Einzeilige Beschreibung eines Befehls anzeigen<br />
>**Syntax:** `whatis [optionen] <name>...`<br />
>**Erklärung:** Zeigt die Kurzbeschreibung (die `NAME`-Zeile der Handbuchseite) eines Befehls – gerade genug, um zu wissen, wofür er da ist. Gleichbedeutend mit `man -f`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <abschnitt>` beschränkt auf einen Handbuchabschnitt<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-w` erlaubt Platzhalter im Namen<br />
>**Beispiel:** `whatis ls`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-s <liste>`, `--sections=<liste>` | beschränkt die Suche auf bestimmte Handbuchabschnitte |
| `-w`, `--wildcard` | erlaubt Platzhalter (`*`, `?`) im Namen |
| `-r`, `--regex` | interpretiert den Namen als regulären Ausdruck |
| `-l`, `--long` | bricht die Ausgabe nicht auf die Terminalbreite ab |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
whatis ls        # Kurzbeschreibung von ls
whatis ls cp mv  # mehrere Befehle auf einmal
whatis -w "loc*" # mit Platzhalter
man -f passwd    # gleichbedeutend zu whatis
```

</details>

>**Hinweis:** Nutzt dieselbe Datenbank wie `apropos`/`man -k`. Während `whatis` nach dem **Befehlsnamen** sucht, durchsucht `apropos` die **Beschreibungen**. Datenbank neu aufbauen mit `sudo mandb`.

---
