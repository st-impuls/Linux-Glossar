[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Befehle](../commands/index.md)
- [Wichtige Verzeichnisse](../directories.md)

## Grundlagen
- [Globbing & Platzhalter](globbing.md)
- [Verkettung & Hintergrund](command-chaining.md)

[← Zurück zur Übersicht](index.md)

# Streams & Umleitungen

Streams (Datenströme) und Umleitungen (Redirections) sind grundlegende Mechanismen für Ein- und Ausgabe und für den Datenaustausch zwischen Prozessen. Mit ihnen lässt sich steuern, woher ein Befehl seine Eingabe bezieht und wohin seine Ausgabe geht: ins Terminal, in eine Datei, ins Nichts – oder direkt in den nächsten Befehl.

## Was ist ein Stream?

Ein **Stream** ist eine Abstraktion: eine fortlaufende Folge von **Bytes**, die gelesen oder geschrieben wird. Das Betriebssystem bietet damit eine einheitliche Schnittstelle – gleichgültig, ob die Daten von der Tastatur kommen, aus einer Datei, von einem anderen Programm oder aus dem Netzwerk. Für das Programm sieht alles gleich aus: es liest Bytes oder es schreibt Bytes.

Angesprochen wird ein Stream über eine Nummer, den **Dateideskriptor** (file descriptor, FD). Beim Start eines Prozesses sind drei davon bereits geöffnet: `0`, `1` und `2`.

Drei Eigenschaften prägen das ganze Thema:

- Ein Stream ist **gerichtet**: man liest daraus **oder** schreibt hinein, nie beides zugleich.
- Ein Stream ist **unstrukturiert** – nur Bytes, üblicherweise Text mit Zeilenumbrüchen. Genau deshalb lassen sich kleine Werkzeuge wie `grep`, `sort` oder `wc` beliebig aneinanderreihen (Unix-Philosophie).
- **Wohin** ein Stream zeigt, entscheidet nicht das Programm, sondern seine Umgebung.

## Die drei Standard-Streams

Jeder Prozess hat drei offene Kanäle. Der Schlüssel zum ganzen Thema sind ihre **Nummern**, die *Dateideskriptoren* (engl. file descriptor, FD):

| FD | Name | Deutsch | Standardziel | Zweck |
|---|---|---|---|---|
| `0` | stdin | Standardeingabe | Tastatur | Eingabe |
| `1` | stdout | Standardausgabe | Bildschirm | normale Ausgabe |
| `2` | stderr | Standardfehler | Bildschirm | Fehlermeldungen |

**Warum das wichtig ist:** `stdout` und `stderr` landen beide auf dem Bildschirm – optisch sind sie also nicht zu unterscheiden. Umgeleitet werden sie aber **getrennt**. Genau darauf zielen die meisten Prüfungsfragen.

```bash
ls /etc /gibtsnicht > liste.txt   # nur die Trefferliste landet in liste.txt,
                                  # die Fehlermeldung bleibt auf dem Bildschirm
```

## Was ist eine Umleitung?

Eine **Umleitung** ist ein Mechanismus der Shell, der Quelle oder Ziel eines Streams austauscht: statt von der Tastatur aus einer Datei lesen, statt auf den Bildschirm in eine Datei schreiben – oder den Stream direkt an einen anderen Prozess weitergeben (Pipe).

Der Schlüssel zum Verständnis:

**Die Shell richtet die Umleitung ein, *bevor* der Befehl startet.** Der Befehl selbst merkt davon nichts – er schreibt unverändert nach FD `1`.

Daraus folgen zwei praktische Konsequenzen:

- Ein Programm muss nichts über Dateien wissen, um seine Ausgabe in einer Datei ablegen zu können.
- Eine Umleitung läuft immer mit den Rechten der **aufrufenden Shell**, nicht mit denen des Befehls – daher scheitert `sudo befehl > /etc/datei` (siehe *Stolperfallen*).

## Umleitungsoperatoren

| Operator | Wirkung |
|---|---|
| `befehl > datei` | stdout → Datei, vorhandene Datei wird **überschrieben** (Kurzform für `1>`) |
| `befehl >> datei` | stdout → Datei, **anhängen** |
| `befehl 2> datei` | stderr → Datei (überschreiben) |
| `befehl 2>> datei` | stderr → Datei (anhängen) |
| `befehl < datei` | stdin ← Datei, statt von der Tastatur |
| `befehl << EOF` | Here-Document: mehrzeiliger Text als stdin |
| `befehl <<< "text"` | Here-String: eine Zeichenkette als stdin |
| <code>befehl1 &#124; befehl2</code> | stdout von `befehl1` → stdin von `befehl2` (Pipe) |
| `befehl &> datei` | stdout **und** stderr → Datei (Bash; überschreiben) |
| `befehl &>> datei` | stdout **und** stderr → Datei (anhängen) |

**Merkhilfe:** Beide leiten dieselbe Ausgabe (stdout) weiter, nur an ein anderes Ziel – `>` schreibt sie **in eine Datei**, `|` reicht sie **an einen Befehl** weiter.

Beispiele: `echo "Hallo" > notiz.txt` · `ls -l >> liste.txt` · `sort < namen.txt`

**Hinweis:** Mit `set -o noclobber` verhindert die Shell, dass `>` eine bestehende Datei überschreibt. Erzwingen lässt es sich dann mit `>|`.

## stdout und stderr zusammenführen (`2>&1`)

`2>&1` bedeutet: *„leite FD 2 dorthin, wo FD 1 **gerade** hinzeigt"*. Das kaufmännische Und (`&`) sagt der Shell, dass `1` ein **Deskriptor** ist und keine Datei.

```bash
befehl 2>&1     # stderr geht dorthin, wo stdout hinzeigt
befehl 2>1      # FALSCH: schreibt stderr in eine Datei namens "1"
```

### Die Reihenfolge entscheidet

Die Shell arbeitet die Umleitungen **von links nach rechts** ab. `2>&1` kopiert das *aktuelle* Ziel von FD 1 – es ist keine dauerhafte Verknüpfung:

```bash
befehl > datei 2>&1   # 1) stdout → datei  2) stderr → dorthin, wo 1 zeigt (= datei)
                      #    Ergebnis: beides in der Datei
befehl 2>&1 > datei   # 1) stderr → dorthin, wo 1 zeigt (= Bildschirm!)
                      # 2) stdout → datei
                      #    Ergebnis: stderr auf dem Bildschirm, stdout in der Datei
```

Kurzform für den ersten Fall (nur Bash): `befehl &> datei`

## Ausgabe verwerfen: `/dev/null`

`/dev/null` ist ein besonderes Gerät, das alles Geschriebene verschluckt – der „Mülleimer" des Systems.

```bash
befehl > /dev/null          # normale Ausgabe verwerfen, Fehler bleiben sichtbar
befehl 2> /dev/null         # Fehler verwerfen, normale Ausgabe bleibt sichtbar
befehl > /dev/null 2>&1     # alles verwerfen (portabel)
befehl &> /dev/null         # alles verwerfen (Bash-Kurzform)
```

## Pipes

Eine Pipe (`|`) verbindet die **stdout** des einen Befehls mit der **stdin** des nächsten. `stderr` geht dabei **nicht** durch die Pipe, sondern weiterhin auf den Bildschirm:

```bash
cat datei.txt | grep "fehler"    # nur stdout wandert weiter
befehl 2>&1 | less               # auch Fehler durch die Pipe schicken
befehl |& less                   # Bash-Kurzform für "2>&1 |"
```

### Ausgabe gleichzeitig sehen und speichern: `tee`

Eine Umleitung mit `>` schreibt in die Datei, aber nichts erscheint mehr am Bildschirm. [`tee`](../commands/text-processing.md#tee) macht beides – es ist ein „T-Stück" in der Leitung:

```bash
befehl | tee log.txt             # Ausgabe am Bildschirm UND in log.txt
befehl | tee -a log.txt          # an log.txt anhängen
befehl 2>&1 | tee log.txt        # inklusive Fehlermeldungen
echo "text" | sudo tee /etc/datei   # in eine Datei schreiben, die root gehört
```

Der letzte Fall ist ein häufiger Kniff: bei `sudo befehl > /etc/datei` läuft nur `befehl` als root – die **Umleitung** führt die eigene Shell aus und scheitert an den Rechten.

## Eingabe umleiten

### Aus einer Datei (`<`)

```bash
sort < namen.txt          # liest die Eingabe aus der Datei
wc -l < datei.txt         # gibt nur die Zahl aus, ohne den Dateinamen
```

### Here-Document (`<<`)

Mit einem Here-Document wird einem Befehl ein mehrzeiliger Text direkt als Eingabe (stdin) übergeben – bis zu einer frei wählbaren Schlusszeile (üblich: `EOF`):

- `befehl <<EOF … EOF` – im Text werden Variablen (`$name`), Befehlssubstitution (`$(…)`) und Backticks **ersetzt**.
- `befehl <<'EOF' … EOF` – der Text wird **wörtlich** übernommen, nichts wird ersetzt (gleichbedeutend: `<<"EOF"`, `<<\EOF`).
- `befehl <<-EOF … EOF` – führende **Tabs** der Zeilen werden entfernt, sodass die Schlusszeile eingerückt sein darf.

Die Schlusszeile darf nur den Begrenzer enthalten und muss am Zeilenanfang stehen (ohne vorangestellte Leerzeichen). Bei `<<-EOF` darf die Schlusszeile mit Tabs eingerückt sein, aber nicht mit Leerzeichen.

Beispiel – Text in eine Datei schreiben, ohne dass `$WERT` ersetzt wird:

```sh
cat <<'EOF' > config.txt
key = $WERT
EOF
```

### Here-String (`<<<`)

Übergibt eine einzelne Zeichenkette als stdin – die kurze Schwester des Here-Documents:

```bash
grep "fehler" <<< "$meldung"     # Variable durchsuchen, ohne Datei
bc <<< "3 * 7"                   # Rechnung an bc übergeben
tr 'a-z' 'A-Z' <<< "hallo"       # ergibt HALLO
```

## Wichtige Nuancen

**Umleitungen werden vererbt.** Sie gelten für den Prozess, in dem sie gesetzt wurden, und für alle seine Kindprozesse: Dateideskriptoren werden beim Start eines Kindprozesses übernommen.

**`stdout` wird gepuffert, `stderr` nicht.** Das Puffern übernimmt die C-Bibliothek, nicht die Shell:

- am **Terminal** puffert `stdout` zeilenweise – jede fertige Zeile erscheint sofort;
- in eine **Datei oder Pipe** puffert `stdout` blockweise – die Ausgabe erscheint erst, wenn einige Kilobyte zusammengekommen sind.

`stderr` ist **ungepuffert**: Fehlermeldungen erscheinen sofort und gehen selbst dann nicht verloren, wenn das Programm abstürzt. Praktische Folge: in `./skript.sh > alles.log 2>&1` können Fehlerzeilen scheinbar an der falschen Stelle stehen. Zeilenweises Puffern erzwingt `stdbuf -oL befehl`.

**Die Streams gibt es auch als Dateien.** `/dev/stdin`, `/dev/stdout` und `/dev/stderr` (Verweise auf `/proc/self/fd/0`, `1`, `2`) sprechen die Streams wie gewöhnliche Dateien an. Das hilft bei Befehlen, die zwingend einen Dateinamen erwarten:

```bash
grep "fehler" /dev/stdin < protokoll.log   # stdin wie eine Datei behandeln
echo "hallo" | tee /dev/stderr | wc -c     # Zwischenstand sichtbar machen
```

**Es gibt mehr als drei Deskriptoren.** Ein Prozess kann weitere öffnen (`3`, `4`, …), etwa mit `exec 3> protokoll.log`. Welche ein laufender Prozess offen hat, zeigt `ls -l /proc/<pid>/fd`.

## Stolperfallen

| Schreibweise | Was wirklich passiert |
|---|---|
| `befehl 2>1` | schreibt stderr in eine **Datei namens `1`** – gemeint war `2>&1` |
| `befehl 2>&1 > datei` | stderr auf den **Bildschirm**, nur stdout in die Datei |
| `sudo befehl > /etc/datei` | die **Umleitung** läuft ohne root-Rechte → „Permission denied"; stattdessen <code>&#124; sudo tee</code> |
| `befehl > datei` | `stderr` bleibt sichtbar – Fehler landen **nicht** in der Datei |
| <code>befehl &#124; grep x</code> | `stderr` umgeht die Pipe – ggf. <code>2&gt;&amp;1 &#124;</code> verwenden |
| `befehl > datei` (zweimal) | die Datei wird jedes Mal **geleert**; zum Anhängen `>>` |

## Praxisbeispiele

```bash
# Ausgabe und Fehler getrennt protokollieren
./skript.sh > ausgabe.log 2> fehler.log

# Alles zusammen in eine Datei, nichts am Bildschirm
./skript.sh > alles.log 2>&1

# Alles am Bildschirm und zugleich ins Protokoll
./skript.sh 2>&1 | tee alles.log

# Nur Fehler sehen, normale Ausgabe wegwerfen
./skript.sh > /dev/null

# Befehl völlig stumm im Hintergrund
./skript.sh &> /dev/null &

# Nur die Fehlermeldungen durchsuchen
./skript.sh 2>&1 >/dev/null | grep -i "error"
```

## Warum das wichtig ist

Streams und Umleitungen sind der Kitt der Unix-Philosophie: viele kleine Programme, die jeweils **eine** Sache gut können, werden über Byte-Ströme zu beliebig komplexen Verarbeitungsketten verbunden. Wer die drei Deskriptoren kennt und die Reihenfolge der Umleitungen verstanden hat, kann Ausgaben gezielt protokollieren, Fehler von Nutzdaten trennen, Rauschen verwerfen und Befehle zu Pipelines verketten.
