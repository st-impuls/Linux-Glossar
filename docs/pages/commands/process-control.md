[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Hilfe & Dokumentation](help-documentation.md)
- [Navigation & Suche](navigation-search.md)
- [Netzwerk & Download](network-download.md)
- [Paketverwaltung](package-management.md)
- [Rechnen & Datum](calc-date.md)
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Prozesse & Steuerung

### at
>**Funktion:** Einen Befehl einmalig zu einer bestimmten Zeit ausführen<br />
>**Syntax:** `at [optionen] <zeit>`<br />
>**Erklärung:** Plant die einmalige Ausführung von Befehlen zu einem späteren Zeitpunkt. Nach dem Aufruf werden die auszuführenden Befehle über die Standardeingabe eingegeben (Abschluss mit `Ctrl + D`). Anders als `cron` läuft der Auftrag nur ein einziges Mal.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <datei>` liest die Befehle aus einer Datei<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` listet anstehende Aufträge (wie `atq`)<br />
>**Beispiel:** `at 22:00`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-f <datei>`, `--file` | liest die auszuführenden Befehle aus einer Datei |
| `-l` | listet die anstehenden Aufträge auf (entspricht `atq`) |
| `-d <id>`, `-r <id>` | löscht einen Auftrag (entspricht `atrm`) |
| `-m` | schickt nach der Ausführung eine E-Mail, auch ohne Ausgabe |
| `-c <id>` | zeigt den Inhalt eines Auftrags an |
| `-v` | zeigt die geplante Ausführungszeit an |

</details>

<details markdown>
<summary>Zeitangaben</summary>

| Beispiel | Bedeutung |
|---|---|
| `at 22:00` | heute um 22 Uhr |
| `at now + 5 minutes` | in 5 Minuten |
| `at 8am tomorrow` | morgen früh um 8 |
| `at 2pm + 3 days` | in 3 Tagen um 14 Uhr |
| `at midnight` | um Mitternacht |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
echo "backup.sh" | at 22:00   # Befehl einzeilig einplanen
at now + 1 hour < skript.sh   # Befehle aus einer Datei
atq                           # anstehende Aufträge anzeigen
atrm 3                        # Auftrag Nr. 3 löschen
```

</details>

>**Hinweis:** Braucht den laufenden Dienst `atd` und ist nicht überall vorinstalliert (`sudo apt install at`). Für **wiederkehrende** Aufgaben ist `cron`/`crontab` gedacht. Verwandte Befehle: `atq` (Liste), `atrm` (löschen).

---

### crontab
>**Funktion:** Wiederkehrende Aufgaben (Cronjobs) verwalten<br />
>**Syntax:** `crontab [optionen]`<br />
>**Erklärung:** Bearbeitet die persönliche Cron-Tabelle, in der wiederkehrende Aufträge stehen – der Dienst `cron` führt sie zu den angegebenen Zeiten automatisch aus. Jede Zeile beschreibt Zeitpunkt und Befehl.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-e` öffnet die Cron-Tabelle zum Bearbeiten<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zeigt die aktuellen Einträge<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` löscht die gesamte Tabelle<br />
>**Beispiel:** `crontab -e`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-e`, `--edit` | öffnet die eigene Cron-Tabelle im Editor |
| `-l`, `--list` | zeigt die aktuellen Einträge an |
| `-r`, `--remove` | löscht die gesamte Cron-Tabelle |
| `-i` | fragt vor dem Löschen (`-r`) nach |
| `-u <benutzer>` | bearbeitet die Tabelle eines anderen Benutzers (als root) |

</details>

<details markdown>
<summary>Aufbau einer Zeile</summary>

Fünf Zeitfelder, danach der Befehl:

```
* * * * * <befehl>
| | | | |
| | | | +-- Wochentag (0–7, 0 und 7 = Sonntag)
| | | +---- Monat (1–12)
| | +------ Tag des Monats (1–31)
| +-------- Stunde (0–23)
+---------- Minute (0–59)
```

Beispiele: `0 3 * * *` = täglich um 3 Uhr · `*/15 * * * *` = alle 15 Minuten · `0 9 * * 1` = montags um 9 Uhr.

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
crontab -e                  # eigene Cronjobs bearbeiten
crontab -l                  # eigene Cronjobs anzeigen
sudo crontab -l -u www-data # Tabelle eines anderen Benutzers
0 2 * * * /pfad/backup.sh    # (in der Tabelle) täglich 2 Uhr Backup
```

</details>

>**Hinweis:** Betrifft die **persönliche** Tabelle des aufrufenden Benutzers. Systemweite Aufträge stehen in `/etc/crontab` und `/etc/cron.d/`. Der Name „cron" leitet sich vom griechischen *chronos* (Zeit) ab. Für einmalige Aufträge dient `at`.

---

### htop
>**Funktion:** Prozesse interaktiv anzeigen und verwalten<br />
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
| `-F <text>`, `--filter=<text>` | startet mit einem voreingestellten Filter |
| `-H`, `--highlight-changes` | hebt geänderte Werte hervor |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

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
| `-x <befehl>` | ersetzt Jobnummern im Befehl durch PIDs und führt ihn aus |

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
>**Funktion:** Laufende Prozesse anzeigen (Momentaufnahme)<br />
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
| `-l` | langes Format (Priorität, Status, PPID u. a.) |
| `-H` | zeigt die Prozesshierarchie durch Einrückung |
| `--forest` | zeigt die Prozesshierarchie als ASCII-Baum |
| `-C <name>` | wählt Prozesse nach Befehlsnamen |
| `--no-headers` | unterdrückt die Kopfzeile |
| `--help <abschnitt>` | zeigt die Hilfe an (z. B. `--help all`) |
| `-V`, `--version` | zeigt die Version an |

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
>**Funktion:** Eine bestimmte Zeit warten<br />
>**Syntax:** `sleep <dauer>...`<br />
>**Erklärung:** Pausiert für die angegebene Dauer. Praktisch in Skripten, um zwischen Schritten zu warten.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`sleep <n>` wartet n Sekunden (z. B. `sleep 5`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`sleep <n>m` Minuten, `<n>h` Stunden (nur GNU)<br />
>**Beispiel:** `sleep 10`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

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

### time
>**Funktion:** Laufzeit eines Befehls messen<br />
>**Syntax:** `time <befehl> [argumente...]`<br />
>**Erklärung:** Führt einen Befehl aus und meldet anschließend, wie lange er gebraucht hat – aufgeschlüsselt in reale Zeit (`real`), CPU-Zeit im Benutzermodus (`user`) und im Kernel (`sys`).<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`time <befehl>` misst die Ausführungszeit<br />
>&nbsp;&nbsp;&nbsp;&nbsp;externes `/usr/bin/time -v` zeigt zusätzlich den Ressourcenverbrauch<br />
>**Beispiel:** `time sleep 2`

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
time sleep 2             # ~2 s real
time ls -R /             # Dauer eines Befehls
/usr/bin/time -v gzip f  # ausführlich: Speicher, CPU u. a. (externes time)
```

</details>

>**Hinweis:** In Bash ist `time` ein **Schlüsselwort** (kein normaler Befehl) – deshalb kann es auch ganze Pipelines messen (`time cmd1 | cmd2`). Daneben gibt es das Programm `/usr/bin/time` (Paket `time`) mit anderer Ausgabe und der Option `-v` für Details. Kein `sudo` nötig.

---

### timeout
>**Funktion:** Einen Befehl mit einem Zeitlimit ausführen<br />
>**Syntax:** `timeout [optionen] <dauer> <befehl> [argumente...]`<br />
>**Erklärung:** Startet einen Befehl und bricht ihn ab, falls er nicht innerhalb der angegebenen Zeit fertig wird. Praktisch, um hängende oder endlos laufende Programme automatisch zu beenden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <signal>` legt das Abbruch-Signal fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-k <dauer>` schickt nach weiterer Zeit `KILL` hinterher<br />
>**Beispiel:** `timeout 5 ping example.com`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-s <signal>`, `--signal=<signal>` | legt das Signal zum Abbruch fest (Standard `TERM`) |
| `-k <dauer>`, `--kill-after=<dauer>` | schickt nach weiterer Zeit `KILL`, falls der Befehl nicht endet |
| `--preserve-status` | gibt den Exit-Code des Befehls zurück statt `124` bei Abbruch |
| `--foreground` | erlaubt dem Befehl den Zugriff auf das Terminal (interaktiv) |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
timeout 5 ping example.com   # nach 5 s abbrechen
timeout 10s ./skript.sh      # mit Einheit (s/m/h/d)
timeout -s KILL 3 ./haenger  # hart mit KILL beenden
timeout -k 5 30 ./job        # nach 30 s TERM, 5 s später KILL
```

</details>

>**Hinweis:** Die Dauer akzeptiert Einheiten `s`, `m`, `h`, `d` (ohne Angabe: Sekunden). Wird der Befehl abgebrochen, liefert `timeout` den Exit-Code `124`. Teil der GNU-Coreutils. Zum wiederholten Beobachten eines Befehls siehe `watch`.

---

### top
>**Funktion:** Prozesse und Systemlast laufend anzeigen<br />
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
| `-H` | zeigt einzelne Threads statt nur Prozesse |
| `-c` | zeigt die volle Befehlszeile |
| `-w [<breite>]` | legt die Ausgabebreite fest (vor allem im Batch-Modus) |
| `-h`, `--help` | zeigt die Hilfe an |
| `-v`, `--version` | zeigt die Version an |

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
>**Funktion:** Einen Befehl wiederholt ausführen und die Ausgabe beobachten<br />
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
| `-p`, `--precise` | versucht, das Intervall exakt einzuhalten |
| `-e`, `--errexit` | hält an, sobald der Befehl mit Fehler endet |
| `-c`, `--color` | interpretiert ANSI-Farb- und Stilcodes |
| `-h`, `--help` | zeigt die Hilfe an |
| `-v`, `--version` | zeigt die Version an |

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
