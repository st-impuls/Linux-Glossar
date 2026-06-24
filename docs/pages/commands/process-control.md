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
- [Rechnen & Datum](calc-date.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Prozesse & Steuerung

### htop
>**Funktion:** Prozesse interaktiv anzeigen und verwalten | Extern<br />
>**Syntax:** `htop [optionen]`<br />
>**Erklärung:** Interaktiver, farbiger Prozess-Monitor – komfortablere Alternative zu `top` mit Mausbedienung, Scrollen und Baumansicht.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <z>` Aktualisierungsintervall in Zehntelsekunden<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u <benutzer>` zeigt nur Prozesse dieses Benutzers<br />
>**Beispiel:** `htop`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-d <z>`, `--delay=<z>` | Aktualisierungsintervall in Zehntelsekunden (z. B. `-d 10` = 1 s) |
| `-u <benutzer>`, `--user=<benutzer>` | zeigt nur Prozesse dieses Benutzers |
| `-p <pid>`, `--pid=<pid>` | überwacht nur bestimmte PIDs (kommagetrennt) |
| `-s <spalte>`, `--sort-key=<spalte>` | sortiert nach Spalte (z. B. `PERCENT_CPU`) |
| `-t`, `--tree` | startet in der Baumansicht |
| `-C`, `--no-color` | einfarbige Darstellung |

</details>

<details markdown>
<summary>Interaktive Befehle (Tasten im Programm)</summary>

| Taste | Wirkung |
|---|---|
| `F1` / `h` | Hilfe anzeigen |
| `F3` / `/` | nach Prozess suchen |
| `F4` | Liste filtern |
| `F5` / `t` | Baumansicht umschalten |
| `F6` | Sortierspalte wählen |
| `F9` / `k` | Prozess beenden (Signal senden) |
| `F10` / `q` | beenden |
| `Leertaste` | Prozess markieren |
| `u` | nach Benutzer filtern |

</details>

>**Hinweis:** `htop` ist nicht überall vorinstalliert (`sudo apt install htop`). Gegenüber `top` bietet es u. a. Mausbedienung, horizontales Scrollen und eine übersichtliche Baumdarstellung.

---

### jobs
>**Funktion:** Hintergrund- und angehaltene Jobs der aktuellen Shell anzeigen | Intern (Builtins)<br />
>**Syntax:** `jobs [optionen] [<job>...]`<br />
>**Erklärung:** Listet die Jobs auf, die in der aktuellen Shell im Hintergrund laufen oder angehalten sind.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zeigt zusätzlich die PID an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` zeigt nur laufende Jobs an<br />
>**Beispiel:** `jobs -l`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l` | zeigt zusätzlich die Prozess-ID (PID) an |
| `-p` | zeigt nur die PIDs an |
| `-r` | zeigt nur laufende (running) Jobs |
| `-s` | zeigt nur angehaltene (stopped) Jobs |
| `-n` | zeigt nur Jobs mit geändertem Status seit der letzten Meldung |

</details>

<details markdown>
<summary>Jobs steuern (verwandte Befehle)</summary>

| Befehl | Wirkung |
|---|---|
| `befehl &` | startet einen Befehl direkt im Hintergrund |
| `Ctrl + Z` | hält den laufenden Vordergrundprozess an |
| `bg %<n>` | setzt Job n im Hintergrund fort |
| `fg %<n>` | holt Job n in den Vordergrund |
| `kill %<n>` | beendet Job n |

</details>

>**Hinweis:** Jobs gehören zur jeweiligen Shell-Sitzung. Ein Job wird über `%<n>` angesprochen (z. B. `fg %1`). Mit `&` startet man Befehle gleich im Hintergrund, mit `Ctrl + Z` hält man den Vordergrundprozess an.

---

### kill
>**Funktion:** Signal an einen Prozess senden (meist zum Beenden) | Intern (Builtins)<br />
>**Syntax:** `kill [-<signal>] <pid>...`<br />
>**Erklärung:** Sendet ein Signal an einen Prozess – standardmäßig `TERM` (15), das den Prozess zum geordneten Beenden auffordert.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-9` erzwingt sofortiges Beenden (`KILL`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` listet alle verfügbaren Signale auf<br />
>**Beispiel:** `kill 1234`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-<n>`, `-s <name>`, `--signal <name>` | sendet das angegebene Signal (z. B. `-9` oder `-s KILL`) |
| `-l`, `--list` | listet alle Signalnamen auf |
| `-L`, `--table` | zeigt die Signale als Tabelle mit Nummern |

</details>

<details markdown>
<summary>Häufige Signale</summary>

| Signal | Nr. | Wirkung |
|---|---|---|
| `TERM` | 15 | normales, geordnetes Beenden (Standard); kann vom Programm abgefangen werden |
| `KILL` | 9 | sofortiges Beenden, nicht abfangbar (letztes Mittel) |
| `HUP` | 1 | Terminal getrennt; viele Dienste laden damit ihre Konfiguration neu |
| `INT` | 2 | Unterbrechung wie `Ctrl + C` |
| `STOP` | 19 | hält den Prozess an (nicht abfangbar) |
| `CONT` | 18 | setzt einen angehaltenen Prozess fort |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
kill 1234            # Signal TERM (15) an PID 1234
kill -9 1234         # sofortiges Beenden (KILL)
kill -HUP 1234       # Konfiguration neu laden
kill %1              # Job 1 der aktuellen Shell beenden
kill -l              # alle Signale auflisten
```

</details>

>**Hinweis:** Zuerst `kill <pid>` (TERM) versuchen; nur wenn der Prozess nicht reagiert, `kill -9`. Verwandt: `pkill <name>` und `killall <name>` beenden Prozesse über den Namen statt der PID.

---

### ps
>**Funktion:** Laufende Prozesse anzeigen (Momentaufnahme) | Extern<br />
>**Syntax:** `ps [optionen]`<br />
>**Erklärung:** Zeigt eine Momentaufnahme der laufenden Prozesse. Ohne Optionen nur die Prozesse der aktuellen Shell.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`aux` zeigt alle Prozesse aller Benutzer (BSD-Stil)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-ef` zeigt alle Prozesse mit voller Befehlszeile (UNIX-Stil)<br />
>**Beispiel:** `ps aux`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `a` | Prozesse aller Benutzer (nicht nur die eigenen) |
| `u` | benutzerorientiertes Format (mit CPU-/Speicheranteil) |
| `x` | auch Prozesse ohne zugehöriges Terminal |
| `-e`, `-A` | alle Prozesse |
| `-f` | volles Format (mit PPID und Befehlszeile) |
| `-u <benutzer>` | Prozesse eines bestimmten Benutzers |
| `-p <pid>` | nur bestimmte PIDs |
| `--sort=<spalte>` | sortiert, z. B. `--sort=-%cpu` (absteigend) |
| `-o <felder>` | eigene Spaltenauswahl, z. B. `-o pid,comm,%cpu` |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
ps aux                       # alle Prozesse, BSD-Stil
ps -ef                       # alle Prozesse, UNIX-Stil
ps aux --sort=-%mem | head   # nach Speicher sortiert, größte Verbraucher
ps -u nomas                  # Prozesse des Benutzers nomas
ps -p 1234 -o pid,comm,%cpu  # eigene Spalten für eine bestimmte PID
ps aux | grep firefox        # nach einem Prozessnamen suchen
```

</details>

>**Hinweis:** `ps aux` (BSD-Stil) und `ps -ef` (UNIX-Stil) sind die beiden gängigen Aufrufe. Anders als `top`/`htop` liefert `ps` eine einmalige Momentaufnahme – ideal in Pipes, z. B. `ps aux | grep nginx`.

---

### sleep
>**Funktion:** Eine bestimmte Zeit warten | Extern<br />
>**Syntax:** `sleep <dauer>...`<br />
>**Erklärung:** Pausiert für die angegebene Dauer. Praktisch in Skripten, um zwischen Schritten zu warten.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`sleep <n>` wartet n Sekunden (z. B. `sleep 5`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`sleep <n>m` Minuten, `<n>h` Stunden (nur GNU)<br />
>**Beispiel:** `sleep 10`

<details markdown>
<summary>Zeitangaben</summary>

| Angabe | Bedeutung |
|---|---|
| `sleep 5` | 5 Sekunden (Standardeinheit) |
| `sleep 0.5` | eine halbe Sekunde |
| `sleep 2m` | 2 Minuten (`s`/`m`/`h`/`d`) |
| `sleep 1h` | 1 Stunde |
| `sleep 1.5h` | 90 Minuten |
| `sleep 1m 30s` | mehrere Angaben werden addiert (90 s) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sleep 5                               # 5 Sekunden warten
sleep 2m                              # 2 Minuten warten
echo "Start"; sleep 3; echo "Fertig" # Pause zwischen zwei Befehlen
while true; do date; sleep 60; done   # jede Minute die Uhrzeit ausgeben
```

</details>

>**Hinweis:** Einheiten wie `m`/`h` und mehrere Argumente versteht nur GNU `sleep` (Linux). Auf manchen Systemen sind ausschließlich ganze Sekunden erlaubt.

---

### top
>**Funktion:** Prozesse und Systemlast laufend anzeigen | Extern<br />
>**Syntax:** `top [optionen]`<br />
>**Erklärung:** Zeigt fortlaufend aktualisiert die aktivsten Prozesse sowie die Auslastung von CPU, Speicher und System. Beenden mit `q`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <s>` Aktualisierungsintervall in Sekunden<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u <benutzer>` zeigt nur Prozesse dieses Benutzers<br />
>**Beispiel:** `top`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-d <s>`, `--delay` | Aktualisierungsintervall in Sekunden (z. B. `-d 2`) |
| `-u <benutzer>`, `--user` | zeigt nur Prozesse dieses Benutzers |
| `-p <pid>` | überwacht nur bestimmte PIDs |
| `-n <z>` | beendet sich nach z Aktualisierungen automatisch |
| `-b` | Batch-Modus (für Ausgabe in Dateien/Pipes) |
| `-o <feld>` | sortiert nach Feld (z. B. `-o %MEM`) |

</details>

<details markdown>
<summary>Interaktive Befehle (Tasten im Programm)</summary>

| Taste | Wirkung |
|---|---|
| `q` | beenden |
| `h` | Hilfe anzeigen |
| `k` | Prozess beenden (PID und Signal eingeben) |
| `r` | Priorität ändern (renice) |
| `M` | nach Speicherverbrauch sortieren |
| `P` | nach CPU-Verbrauch sortieren |
| `T` | nach Laufzeit sortieren |
| `u` | nach Benutzer filtern |
| `1` | CPU-Kerne einzeln anzeigen |
| `c` | volle Befehlszeile ein-/ausblenden |

</details>

>**Hinweis:** `top` ist praktisch immer vorinstalliert. Eine komfortablere, farbige Alternative mit Mausbedienung ist `htop`.

---

### watch
>**Funktion:** Einen Befehl wiederholt ausführen und die Ausgabe beobachten | Extern<br />
>**Syntax:** `watch [optionen] <befehl>`<br />
>**Erklärung:** Führt den Befehl regelmäßig (Standard: alle 2 Sekunden) aus und zeigt die jeweils aktuelle Ausgabe im Vollbild. Beenden mit `Ctrl + C`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <s>` Intervall in Sekunden<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` hebt Änderungen gegenüber dem letzten Durchlauf hervor<br />
>**Beispiel:** `watch -n 1 date`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n <s>`, `--interval=<s>` | Intervall in Sekunden (z. B. `-n 0.5`) |
| `-d`, `--differences` | hebt geänderte Stellen hervor |
| `-t`, `--no-title` | blendet die Kopfzeile aus |
| `-g`, `--chgexit` | beendet sich, sobald sich die Ausgabe ändert |
| `-b`, `--beep` | Signalton, wenn der Befehl mit Fehler endet |
| `-x`, `--exec` | übergibt den Befehl ohne Shell-Interpretation |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
watch -n 1 date              # jede Sekunde die Uhrzeit
watch -d free -h             # Speicherbelegung, Änderungen hervorgehoben
watch -n 5 'ls -l /var/log'  # Verzeichnis alle 5 s neu auflisten
watch -g 'ls *.tmp'          # warten, bis sich die Ausgabe ändert
```

</details>

>**Hinweis:** Enthält der Befehl Pipes, Globs oder Variablen, in Anführungszeichen setzen – z. B. `watch 'ps aux | grep nginx'` –, damit `watch` und nicht die aufrufende Shell die ganze Pipe wiederholt.

---
