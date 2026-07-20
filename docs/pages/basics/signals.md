[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Befehle](../commands/index.md)
- [Wichtige Verzeichnisse](../directories.md)

## Grundlagen
- [Globbing & Platzhalter](globbing.md)
- [Reguläre Ausdrücke](regular-expressions.md)
- [Streams & Umleitungen](streams-redirects.md)
- [Verkettung & Hintergrund](command-chaining.md)

[← Zurück zur Übersicht](index.md)

# Signale

Ein **Signal** ist eine kurze Nachricht des Kernels an einen Prozess: „beende dich", „halte an", „lies deine Konfiguration neu". Es überträgt keine Daten, nur die Art der Nachricht selbst. Jeder laufende Prozess kann Signale empfangen – über Tastenkürzel, über Befehle wie `kill` oder vom Kernel selbst.

## Was mit einem Signal geschieht

Trifft ein Signal ein, hat der Prozess drei Möglichkeiten:

- **Standardaktion ausführen** – meist beenden, bei manchen Signalen anhalten oder ignorieren
- **Abfangen** (*Signal-Handler*) – eigene Reaktion, z. B. Konfiguration neu laden und weiterlaufen
- **Ignorieren** – das Signal wirkungslos verfallen lassen

Zwei Signale sind davon ausgenommen: `SIGKILL` (9) und `SIGSTOP` (19) lassen sich **weder abfangen noch ignorieren**. Der Kernel führt sie immer aus – deshalb sind sie das letzte Mittel gegen einen hängenden Prozess.

## Die wichtigsten Signale im Alltag

| Signal | Nr. | Bedeutung | Abfangbar |
|---|---|---|---|
| `SIGHUP` | 1 | Terminal getrennt; Dienste laden damit ihre Konfiguration neu | ja |
| `SIGINT` | 2 | Unterbrechung von der Tastatur (`Ctrl + C`) | ja |
| `SIGQUIT` | 3 | Abbruch von der Tastatur (`Ctrl + \`), mit Core-Dump | ja |
| `SIGKILL` | 9 | sofortiges Beenden – letztes Mittel | **nein** |
| `SIGTERM` | 15 | geordnetes Beenden; Vorgabe von `kill` | ja |
| `SIGCONT` | 18 | angehaltenen Prozess fortsetzen | ja |
| `SIGSTOP` | 19 | Prozess anhalten | **nein** |
| `SIGTSTP` | 20 | Anhalten von der Tastatur (`Ctrl + Z`) | ja |

## SIGTERM oder SIGKILL?

`SIGTERM` **bittet** den Prozess, sich zu beenden. Er darf vorher aufräumen: Dateien schließen, Puffer schreiben, Sperren freigeben. Deshalb ist es die Vorgabe von `kill`.

`SIGKILL` **beendet** ihn ohne Rückfrage. Der Prozess erfährt nichts davon und kann nicht aufräumen – halb geschriebene Dateien und liegengebliebene Sperrdateien sind die Folge.

```bash
kill 1234        # SIGTERM – zuerst immer das
kill -9 1234     # SIGKILL – nur wenn der Prozess nicht reagiert
```

## SIGSTOP oder SIGTSTP?

Beide halten einen Prozess an, aber:

| | `SIGSTOP` (19) | `SIGTSTP` (20) |
|---|---|---|
| Auslöser | nur per `kill` | Tastatur `Ctrl + Z` |
| Abfangbar | nein | ja |

Ein Programm kann `SIGTSTP` also abfangen und sich weigern, sich per `Ctrl + Z` anhalten zu lassen – `SIGSTOP` dagegen wirkt immer. Fortgesetzt werden beide mit `SIGCONT` bzw. mit `fg`/`bg`.

## Alle Standardsignale

| Signal | Nr. | Bedeutung | Standardaktion |
|---|---|---|---|
| `SIGHUP` | 1 | Terminal getrennt / Konfiguration neu laden | Beenden |
| `SIGINT` | 2 | Unterbrechung (`Ctrl + C`) | Beenden |
| `SIGQUIT` | 3 | Abbruch (`Ctrl + \`) | Beenden + Core-Dump |
| `SIGILL` | 4 | ungültiger Maschinenbefehl | Beenden + Core-Dump |
| `SIGTRAP` | 5 | Haltepunkt für den Debugger | Beenden + Core-Dump |
| `SIGABRT` | 6 | Programm hat sich selbst abgebrochen (`abort()`) | Beenden + Core-Dump |
| `SIGBUS` | 7 | Busfehler, fehlerhafter Speicherzugriff | Beenden + Core-Dump |
| `SIGFPE` | 8 | Rechenfehler, z. B. Division durch null | Beenden + Core-Dump |
| `SIGKILL` | 9 | sofortiges Beenden, nicht abfangbar | Beenden |
| `SIGUSR1` | 10 | frei verwendbar durch das Programm | Beenden |
| `SIGSEGV` | 11 | Speicherzugriffsfehler (*segmentation fault*) | Beenden + Core-Dump |
| `SIGUSR2` | 12 | frei verwendbar durch das Programm | Beenden |
| `SIGPIPE` | 13 | Schreiben in eine Pipe, die niemand mehr liest | Beenden |
| `SIGALRM` | 14 | Wecker von `alarm()` abgelaufen | Beenden |
| `SIGTERM` | 15 | geordnetes Beenden (Vorgabe von `kill`) | Beenden |
| `SIGSTKFLT` | 16 | Stapelfehler des Coprozessors (unbenutzt) | Beenden |
| `SIGCHLD` | 17 | Kindprozess beendet oder angehalten | Ignorieren |
| `SIGCONT` | 18 | angehaltenen Prozess fortsetzen | Fortsetzen |
| `SIGSTOP` | 19 | anhalten, nicht abfangbar | Anhalten |
| `SIGTSTP` | 20 | anhalten von der Tastatur (`Ctrl + Z`) | Anhalten |
| `SIGTTIN` | 21 | Hintergrundprozess liest vom Terminal | Anhalten |
| `SIGTTOU` | 22 | Hintergrundprozess schreibt auf das Terminal | Anhalten |
| `SIGURG` | 23 | dringende Daten an einem Socket | Ignorieren |
| `SIGXCPU` | 24 | Rechenzeitgrenze überschritten | Beenden + Core-Dump |
| `SIGXFSZ` | 25 | Dateigrößengrenze überschritten | Beenden + Core-Dump |
| `SIGVTALRM` | 26 | virtueller Wecker abgelaufen | Beenden |
| `SIGPROF` | 27 | Profiling-Zeitgeber abgelaufen | Beenden |
| `SIGWINCH` | 28 | Fenstergröße des Terminals geändert | Ignorieren |
| `SIGIO` | 29 | Ein-/Ausgabe jetzt möglich (auch `SIGPOLL`) | Beenden |
| `SIGPWR` | 30 | Stromausfall gemeldet | Beenden |
| `SIGSYS` | 31 | ungültiger Systemaufruf | Beenden + Core-Dump |

**Hinweis:** Die Nummern 32 bis 64 sind **Echtzeitsignale** (`SIGRTMIN` bis `SIGRTMAX`). Sie haben keine feste Bedeutung, werden in der Reihenfolge ihres Eintreffens zugestellt und dienen der Verständigung zwischen eigens dafür geschriebenen Programmen. In der Systemverwaltung kommen sie praktisch nicht vor.

## Signale senden

```bash
kill 1234                # SIGTERM an PID 1234
kill -9 1234             # per Nummer
kill -KILL 1234          # per Name, ohne SIG-Vorsilbe
kill -SIGKILL 1234       # per Name, mit Vorsilbe – gleichwertig
killall firefox          # an alle Prozesse dieses Namens
pkill -u max firefox     # nach Name und Benutzer ausgewählt
kill -l                  # alle Signalnamen auflisten
```

Name und Nummer sind gleichwertig, ebenso mit und ohne `SIG`: `-9`, `-KILL` und `-SIGKILL` bewirken dasselbe.

Von der Tastatur aus erreichbar sind drei Signale:

| Tastenkürzel | Signal |
|---|---|
| `Ctrl + C` | `SIGINT` (2) |
| `Ctrl + \` | `SIGQUIT` (3) |
| `Ctrl + Z` | `SIGTSTP` (20) |

## Signale im Exit-Code

Wird ein Prozess durch ein Signal beendet, meldet die Shell den Exit-Code **128 + Signalnummer**:

```bash
sleep 100
# in einem anderen Terminal: kill -9 <pid>
echo $?     # 137  =  128 + 9   (SIGKILL)
```

### Warum ausgerechnet 128?

Der Exit-Code in `$?` ist nur **ein Byte** groß, also eine Zahl von 0 bis 255. Alles darüber wird abgeschnitten:

```bash
bash -c 'exit 300'; echo $?    # 44   (300 - 256)
```

Zu melden sind aber **zwei verschiedene Dinge**. Der Kernel unterscheidet beim Ende eines Prozesses genau:

- der Prozess hat sich **selbst** beendet – mit diesem Rückgabewert
- der Prozess wurde **durch ein Signal** beendet – durch dieses Signal

Ein Programm, das `9` zurückgibt, und ein Programm, das `SIGKILL` erwischt hat, sind zweierlei. Beides muss aber in dieselbe eine Zahl passen. Die Lösung: den Wertebereich des Bytes **halbieren**.

| Bereich | Bedeutung | Wert |
|---|---|---|
| 0 – 127 | Prozess hat sich selbst beendet | der Rückgabewert selbst |
| 128 – 255 | Prozess wurde durch ein Signal beendet | 128 + Signalnummer |

Die 128 ist dabei nicht willkürlich gewählt: Sie ist `2^7`, also das **oberste Bit** eines Bytes. 128 zu addieren heißt schlicht, dieses Bit zu setzen – die untere Hälfte des Bereichs steht für „selbst beendet", die obere für „abgeschossen". Und da es nur 31 Standardsignale gibt, bleibt `128 + n` immer unter 255.

### Die geläufigen Werte

| Signal | Nr. | Exit-Code |
|---|---|---|
| `SIGINT` | 2 | 130 |
| `SIGQUIT` | 3 | 131 |
| `SIGKILL` | 9 | 137 |
| `SIGSEGV` | 11 | 139 |
| `SIGTERM` | 15 | 143 |

`137` in einer Protokolldatei heißt also „durch `SIGKILL` beendet" – ein häufiger Anblick, wenn ein Dienst zu viel Arbeitsspeicher belegt hat und vom Kernel abgeräumt wurde. `143` bedeutet entsprechend ein reguläres `SIGTERM`, etwa beim Herunterfahren.

### Die Grenzen des Verfahrens

Aus `$?` allein lassen sich beide Fälle **nicht** sicher unterscheiden – ein Programm darf sich auch absichtlich mit 137 beenden:

```bash
bash -c 'exit 137'; echo $?    # 137 – nicht von SIGKILL zu unterscheiden
```

Die Shell selbst kennt den Unterschied, denn vom Kernel bekommt sie die vollständige Statusinformation; in `$?` ist er aber verloren. Für eigene Skripte gilt deshalb: **Rückgabewerte unter 126 wählen.** Ab 128 beginnt der Bereich der Signale, und 126 und 127 sind bei den Shells ebenfalls vergeben:

| Wert | Bedeutung |
|---|---|
| 126 | Befehl gefunden, aber nicht ausführbar |
| 127 | Befehl nicht gefunden |

## Die Nummern sind nicht überall gleich

Die hier genannten Nummern gelten für Linux auf **x86, x86-64 und ARM** – also für praktisch alle gängigen Systeme. Auf anderen Architekturen (Alpha, MIPS, SPARC) weichen einzelne Nummern ab; `SIGSTOP` ist dort etwa 17 statt 19. Verlässlich sind deshalb immer die **Namen**, nicht die Nummern:

```bash
kill -TERM 1234     # überall gleichbedeutend
kill -15 1234       # nur dort, wo TERM die 15 ist
```

Welche Nummern das eigene System verwendet, zeigt `kill -l`.
