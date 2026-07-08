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

### disown
>**Funktion:** Hintergrundjobs von der aktuellen Shell abkoppeln | Intern (Builtins)<br />
>**Syntax:** `disown [optionen] [<job>...]`<br />
>**Erklärung:** Entfernt einen laufenden Hintergrundjob aus der Job-Liste der Shell, sodass er beim Schließen der Shell kein `HUP`-Signal erhält und weiterläuft. Anders als `nohup` wird `disown` auf einen **bereits gestarteten** Job angewendet.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` schützt den Job vor `HUP`, belässt ihn aber in der Liste<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` betrifft alle Jobs<br />
>**Beispiel:** `disown -h %1`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h` | schützt den Job vor `HUP`, belässt ihn aber in der Job-Liste |
| `-a` | wendet die Aktion auf alle Jobs an |
| `-r` | betrifft nur laufende (running) Jobs |
| `%<n>` | entfernt den angegebenen Job aus der Liste (ohne Option) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
./langlaeufer.sh &   # Job im Hintergrund starten
disown -h %1         # Job 1 vor HUP schützen (bleibt gelistet)
disown %1            # Job 1 ganz aus der Liste nehmen
disown -a            # alle Jobs abkoppeln
```

</details>

>**Hinweis:** Wirkt auf **schon laufende** Jobs; um ein Programm von vornherein abgekoppelt zu starten, dient `nohup`. Nach `disown` erscheint der Prozess nicht mehr in `jobs`, läuft aber weiter (per `ps`/`pgrep` auffindbar). Ist ein Bash-Builtin.

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
| `disown %<n>` | koppelt Job n von der Shell ab (kein `HUP` beim Beenden) |
| `nohup befehl &` | startet abgekoppelt, überlebt das Schließen des Terminals |

</details>

>**Hinweis:** Jobs gehören zur jeweiligen Shell-Sitzung. Ein Job wird über `%<n>` angesprochen (z. B. `fg %1`). Mit `&` startet man Befehle gleich im Hintergrund, mit `Ctrl + Z` hält man den Vordergrundprozess an. Soll ein Job das Schließen des Terminals überstehen, dienen `nohup` (beim Start) und `disown` (nachträglich).

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

### killall
>**Funktion:** Prozesse anhand ihres Namens beenden<br />
>**Syntax:** `killall [optionen] [-<signal>] <name>...`<br />
>**Erklärung:** Sendet ein Signal (standardmäßig `TERM`) an **alle** Prozesse mit dem angegebenen Namen – anders als `kill`, das eine PID erwartet. Praktisch, um mehrere gleichnamige Prozesse auf einmal zu beenden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor jedem Beenden nach<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-9` erzwingt sofortiges Beenden (`KILL`)<br />
>**Beispiel:** `killall firefox`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-<signal>`, `-s <signal>`, `--signal <signal>` | sendet das angegebene Signal (z. B. `-9`) |
| `-i`, `--interactive` | fragt vor jedem Beenden nach |
| `-I`, `--ignore-case` | ignoriert Groß-/Kleinschreibung im Namen |
| `-r`, `--regexp` | interpretiert den Namen als regulären Ausdruck |
| `-u <benutzer>`, `--user <benutzer>` | nur Prozesse dieses Benutzers |
| `-w`, `--wait` | wartet, bis die Prozesse wirklich beendet sind |
| `-v`, `--verbose` | meldet, ob das Signal erfolgreich gesendet wurde |
| `-e`, `--exact` | verlangt exakte Übereinstimmung bei langen Namen |
| `-g`, `--process-group` | beendet die ganze Prozessgruppe |
| `-l`, `--list` | listet alle bekannten Signalnamen auf |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
killall firefox            # alle firefox-Prozesse (TERM)
killall -9 firefox         # hart beenden (KILL)
killall -i nginx           # vor jedem Beenden nachfragen
killall -u nomas chrome    # chrome-Prozesse des Benutzers nomas
```

</details>

>**Hinweis:** Beendet **alle** passenden Prozesse – den Namen also genau prüfen. Gehört zum Paket `psmisc`. Achtung: Auf manchen Unix-Systemen (z. B. Solaris) beendet `killall` sämtliche Prozesse des Systems – unter Linux nur die namentlich genannten. Verwandt: `pkill` (mehr Auswahlkriterien), `kill` (per PID).

---

### nice
>**Funktion:** Ein Programm mit veränderter Scheduling-Priorität (Niceness) starten<br />
>**Syntax:** `nice [-n <wert>] <befehl> [argumente...]`<br />
>**Erklärung:** Startet einen Befehl mit angepasster „Niceness" – einer geänderten Priorität für den CPU-Scheduler. Der Wert reicht von −20 (höchste Priorität) bis 19 (niedrigste); je „netter" (höher) der Wert, desto mehr Rechenzeit überlässt der Prozess anderen. Ohne Angabe gilt 10.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <wert>` setzt die Niceness (−20 … 19)<br />
>**Beispiel:** `nice -n 10 ./rechenjob.sh`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n <wert>`, `--adjustment=<wert>` | startet mit dieser Niceness (−20 … 19; Standard 10) |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
nice ./job.sh                           # startet mit Niceness 10 (Standard)
nice -n 19 ./backup.sh                  # sehr niedrige Priorität, stört kaum
sudo nice -n -5 ./wichtig.sh            # höhere Priorität (nur als root)
nice -n 15 tar -czf sich.tar.gz /daten  # Backup schonend nebenher
```

</details>

>**Hinweis:** Nur **root** darf negative Werte (höhere Priorität) vergeben; normale Nutzer können die Priorität lediglich senken. Ohne `-n` gilt der Standardwert 10. Die aktuelle Niceness zeigt die Spalte `NI` in `top`/`htop` oder `ps -l`. Um die Priorität eines **bereits laufenden** Prozesses zu ändern, dient `renice`.

---

### nohup
>**Funktion:** Ein Programm starten, das das Schließen des Terminals überlebt<br />
>**Syntax:** `nohup <befehl> [argumente...] [&]`<br />
>**Erklärung:** Startet einen Befehl so, dass er das Signal `HUP` („hang up", beim Schließen des Terminals oder Abmelden) ignoriert und weiterläuft. Die Ausgabe wird, sofern nicht umgeleitet, in die Datei `nohup.out` geschrieben. Meist zusammen mit `&`, um zugleich in den Hintergrund zu gehen.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`nohup <befehl> &` startet abgekoppelt und im Hintergrund<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`> datei 2>&1` leitet die Ausgabe gezielt in eine Datei<br />
>**Beispiel:** `nohup ./langlaeufer.sh &`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
nohup ./backup.sh &               # im Hintergrund, überlebt den Logout
nohup ./job.sh > job.log 2>&1 &   # Ausgabe gezielt in eine Datei
nohup python server.py &          # Server weiterlaufen lassen
```

</details>

>**Hinweis:** Ohne Umleitung landet die Ausgabe in `nohup.out` (im aktuellen Verzeichnis oder `$HOME`). `nohup` koppelt nur vom `HUP`-Signal ab, bringt aber selbst **nicht** in den Hintergrund – dafür zusätzlich `&`. Um einen **bereits laufenden** Job abzukoppeln, dient `disown`. Für dauerhafte Dienste eignen sich eher `systemd` oder `tmux`/`screen`.

---

### pgrep
>**Funktion:** Prozess-IDs anhand von Namen oder Merkmalen finden<br />
>**Syntax:** `pgrep [optionen] <muster>`<br />
>**Erklärung:** Durchsucht die laufenden Prozesse und gibt die PIDs aus, die zum Muster passen – im Grunde `ps` und `grep` in einem Befehl. Das Ergebnis lässt sich direkt an `kill`/`renice` weiterreichen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zeigt zusätzlich den Prozessnamen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` sucht in der vollständigen Befehlszeile<br />
>**Beispiel:** `pgrep -l ssh`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--list-name` | gibt neben der PID auch den Prozessnamen aus |
| `-a`, `--list-full` | gibt PID samt vollständiger Befehlszeile aus |
| `-f`, `--full` | vergleicht das Muster mit der ganzen Befehlszeile |
| `-x`, `--exact` | nur exakt passende Namen (keine Teiltreffer) |
| `-u <benutzer>`, `--euid <benutzer>` | nur Prozesse dieses (effektiven) Benutzers |
| `-n`, `--newest` | nur der neueste passende Prozess |
| `-o`, `--oldest` | nur der älteste passende Prozess |
| `-c`, `--count` | gibt nur die Anzahl der Treffer aus |
| `-i`, `--ignore-case` | ignoriert Groß-/Kleinschreibung |
| `-d <trenner>`, `--delimiter <trenner>` | Trennzeichen zwischen den PIDs (Standard: Zeilenumbruch) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
pgrep sshd                   # PIDs aller sshd-Prozesse
pgrep -l ssh                 # PIDs mit Namen
pgrep -af nginx              # PIDs mit voller Befehlszeile
pgrep -u nomas firefox       # firefox-Prozesse von nomas
kill $(pgrep -f meinskript)  # gefundene Prozesse beenden
```

</details>

>**Hinweis:** Ohne Treffer ist der Exit-Code ungleich 0 – praktisch in Skripten zur Prüfung, ob ein Dienst läuft. Standardmäßig zählt nur der **Prozessname** (bis 15 Zeichen); die ganze Befehlszeile durchsucht `-f`. Zum direkten Beenden statt Auflisten: `pkill`. Eine einzelne PID liefert auch `pidof`.

---

### pidof
>**Funktion:** Die Prozess-IDs eines laufenden Programms ermitteln<br />
>**Syntax:** `pidof [optionen] <programm>...`<br />
>**Erklärung:** Gibt die PIDs aller Instanzen eines Programms aus, das über seinen genauen Namen angegeben wird. Ähnlich wie `pgrep`, aber auf den Programmnamen ausgerichtet und häufig in Init-Skripten verwendet.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` gibt nur eine einzige PID zurück<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-x` berücksichtigt auch Skripte<br />
>**Beispiel:** `pidof sshd`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-s`, `--single-shot` | gibt nur eine einzige PID zurück |
| `-x` | findet auch Shell-/Skript-Prozesse mit diesem Namen |
| `-o <pid>`, `--omit-pid <pid>` | schließt bestimmte PIDs aus (mehrfach möglich) |
| `-c` | nur Prozesse mit demselben Root-Verzeichnis (root nötig) |
| `-q` | keine Ausgabe, nur Exit-Code (quiet) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
pidof sshd                 # alle PIDs von sshd
pidof -s nginx             # nur eine PID
pidof -x backup.sh         # PID eines laufenden Skripts
kill $(pidof firefox)      # alle firefox-Instanzen beenden
```

</details>

>**Hinweis:** Erwartet den **genauen** Programmnamen (keine Teilmuster wie bei `pgrep`; entspricht in etwa `pgrep -x`). Gehört zum Paket `procps`. Wird oft in System-/Init-Skripten genutzt, um zu prüfen, ob ein Dienst läuft.

---

### pkill
>**Funktion:** Prozesse anhand von Namen oder Merkmalen beenden<br />
>**Syntax:** `pkill [optionen] [-<signal>] <muster>`<br />
>**Erklärung:** Sendet ein Signal (standardmäßig `TERM`) an alle Prozesse, die zum Muster passen – dieselbe Auswahllogik wie `pgrep`, nur wird nicht aufgelistet, sondern signalisiert. Bequemer als `kill` mit vorher ermittelter PID.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-9` erzwingt sofortiges Beenden (`KILL`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` vergleicht das Muster mit der ganzen Befehlszeile<br />
>**Beispiel:** `pkill firefox`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-<signal>`, `--signal <signal>` | sendet das angegebene Signal (z. B. `-9`) |
| `-f`, `--full` | vergleicht das Muster mit der ganzen Befehlszeile |
| `-x`, `--exact` | nur exakt passende Namen |
| `-u <benutzer>`, `--euid <benutzer>` | nur Prozesse dieses Benutzers |
| `-n`, `--newest` | nur den neuesten passenden Prozess |
| `-o`, `--oldest` | nur den ältesten passenden Prozess |
| `-i`, `--ignore-case` | ignoriert Groß-/Kleinschreibung |
| `-c`, `--count` | gibt die Anzahl der signalisierten Prozesse aus |
| `-e`, `--echo` | zeigt an, welche Prozesse beendet wurden |
| `-g <pgrp>`, `--pgroup <pgrp>` | wählt Prozesse nach Prozessgruppe |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
pkill firefox                    # alle firefox-Prozesse (TERM)
pkill -9 firefox                 # hart beenden (KILL)
pkill -f "python meinskript.py"  # nach voller Befehlszeile
pkill -u nomas                   # alle Prozesse des Benutzers nomas
```

</details>

>**Hinweis:** Teilt sich die Auswahloptionen mit `pgrep` – erst mit `pgrep` prüfen, welche Prozesse betroffen sind, dann `pkill`. Standardmäßig zählt nur der Prozessname; die ganze Befehlszeile durchsucht `-f`. Verwandt: `killall` (per Name), `kill` (per PID).

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

### renice
>**Funktion:** Die Scheduling-Priorität (Niceness) laufender Prozesse ändern<br />
>**Syntax:** `renice [-n] <wert> {-p <pid> | -u <benutzer> | -g <gruppe>}...`<br />
>**Erklärung:** Ändert die „Niceness" eines oder mehrerer **bereits laufender** Prozesse – anders als `nice`, das sie beim Start festlegt. Der Wert reicht wieder von −20 (höchste Priorität) bis 19 (niedrigste). Prozesse lassen sich per PID, Benutzer oder Gruppe auswählen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p <pid>` wählt Prozesse anhand ihrer PID (Standard)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u <benutzer>` wählt alle Prozesse eines Benutzers<br />
>**Beispiel:** `renice -n 5 -p 1234`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n <wert>`, `--priority <wert>` | setzt die absolute Niceness (−20 … 19) |
| `--relative <wert>` | ändert die Niceness relativ zum aktuellen Wert |
| `-p <pid>`, `--pid <pid>` | wählt Prozesse nach PID (Standardmodus) |
| `-u <name>`, `--user <name>` | wählt alle Prozesse eines Benutzers |
| `-g <name>`, `--pgrp <name>` | wählt alle Prozesse einer Prozessgruppe |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
renice -n 5 -p 1234                     # PID 1234 auf Niceness 5 setzen
renice -n 10 -u nomas                   # alle Prozesse des Benutzers nomas
sudo renice -n -5 -p 1234               # höhere Priorität (nur als root)
renice -n 19 -p $(pgrep -d' ' ffmpeg)   # alle ffmpeg-Prozesse drosseln
```

</details>

>**Hinweis:** Negative Werte (höhere Priorität) und das Ändern **fremder** Prozesse setzen root-Rechte voraus; die eigene Priorität kann man nur senken. Die aktuelle Niceness steht in der Spalte `NI` von `top`/`htop`/`ps -l`; in `top`/`htop` lässt sie sich mit der Taste `r` interaktiv ändern. Beim Programmstart legt `nice` die Priorität fest.

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
