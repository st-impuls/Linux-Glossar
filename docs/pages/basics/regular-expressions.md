[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Befehle](../commands/index.md)
- [Wichtige Verzeichnisse](../directories.md)

## Grundlagen
- [Globbing & Platzhalter](globbing.md)
- [Streams & Umleitungen](streams-redirects.md)
- [Verkettung & Hintergrund](command-chaining.md)

[← Zurück zur Übersicht](index.md)

# Reguläre Ausdrücke

Ein **regulärer Ausdruck** (regular expression, kurz *Regex*) ist ein Suchmuster für **Text**. Statt nach einer festen Zeichenkette zu suchen, beschreibt man, *wie* der gesuchte Text aussieht – etwa „eine Zeile, die mit `root` beginnt". Werkzeuge wie `grep`, `sed` und `awk` bauen darauf auf; auch `less`, `vim` und viele Konfigurationsdateien sprechen dieselbe Sprache.

> **Zum Ausprobieren: [regex101.com](https://regex101.com/)**
>
> Dort lassen sich Muster interaktiv testen: Die Seite hebt jeden Treffer farbig hervor, erklärt den Ausdruck Zeichen für Zeichen und bietet eine Kurzreferenz sowie einen Ersetzungs-Modus.
>
> **Beachten:** regex101 arbeitet mit Perl-artigen Dialekten (Voreinstellung PCRE2). Das entspricht `grep -P`. Für `grep` und `sed` in der Grundform (BRE) gelten andere Backslash-Regeln – siehe Abschnitt *BRE, ERE, PCRE*.

## Gewöhnliche Zeichen (Literale)

Die meisten Zeichen stehen für sich selbst. Das Muster `cat` findet jede Zeile, in der ein `c` steht, direkt gefolgt von einem `a` und einem `t` – auch mitten im Wort, etwa in `concatenate`.

## Der Punkt `.` – ein beliebiges Zeichen

```
r.t   → findet "rat", "rot", "rut", "r7t"   (genau EIN beliebiges Zeichen zwischen r und t)
```

`.` steht für **genau ein** Zeichen, gleich welches – nur den Zeilenumbruch trifft er nicht.

## Anker `^` und `$`

Anker binden das Muster an eine **Position** in der Zeile:

```
^abc   → Zeile BEGINNT mit "abc"
abc$   → Zeile ENDET auf "abc"
^abc$  → Zeile besteht NUR aus "abc"
^$     → leere Zeile
```

Anker verbrauchen selbst kein Zeichen, sie bezeichnen nur eine Stelle. Deshalb passt `^$` auf eine Zeile ohne jeden Inhalt.

## Zeichenklassen `[…]`

Eckige Klammern stehen für **ein** Zeichen aus der angegebenen Menge:

```
[aeiou]    → ein beliebiger Vokal
[0-9]      → eine Ziffer (Bereich)
[a-z]      → ein Kleinbuchstabe
[A-Za-z]   → ein Buchstabe (beliebige Schreibweise)
[^0-9]     → ein Zeichen, das KEINE Ziffer ist   (^ innerhalb der Klammern = Verneinung!)
```

**Hinweis:** `^` hat zwei verschiedene Bedeutungen – **außerhalb** der Klammern den Zeilenanfang, **innerhalb** von `[^…]` die Verneinung.

## POSIX-Zeichenklassen `[[:…:]]`

Benannte Klassen sind gut lesbar und in Prüfungen häufig:

```
[[:digit:]]   = [0-9]        Ziffern
[[:alpha:]]   = [A-Za-z]     Buchstaben
[[:alnum:]]   = Buchstaben und Ziffern
[[:upper:]]   = [A-Z]        Großbuchstaben
[[:lower:]]   = [a-z]        Kleinbuchstaben
[[:space:]]   = Leerzeichen, Tabulator, Zeilenumbruch
```

Auf die **doppelten** Klammern achten: das äußere `[…]` ist die Zeichenklasse, das innere `[:…:]` der Name der POSIX-Klasse. Anders als `[a-z]` hängen die benannten Klassen nicht von der eingestellten Sprachumgebung (Locale) ab.

## Quantoren (Wiederholung)

Quantoren geben an, wie oft das **vorige** Element wiederholt wird:

```
*      → 0 oder mehr Mal
+      → 1 oder mehr Mal
?      → 0 oder 1 Mal (optional)
{n}    → genau n Mal
{n,}   → n oder mehr Mal
{n,m}  → n bis m Mal
```

**Hinweis:** Unverändert funktioniert überall nur `*`. In `grep` und `sed` (Grundform, BRE) brauchen `+`, `?` und `{…}` einen Backslash – `\+`, `\?`, `\{2,4\}` –, sonst gelten sie als gewöhnliche Zeichen. Warum das so ist, erklärt der nächste Abschnitt.

## Gruppierung, Alternative und Rückwärtsverweise

Runde Klammern `(…)` fassen mehrere Zeichen zu **einem Block** zusammen. Damit lassen sich Quantoren und die Alternative auf eine ganze Zeichenfolge anwenden – und der Block merkt sich, was er getroffen hat.

### Ein Quantor für die ganze Gruppe

Ohne Klammern bezieht sich ein Quantor nur auf das **unmittelbar vorangehende** Zeichen. Klammern erweitern seine Reichweite auf alles, was in ihnen steht:

```
ab+     → ein a, dann ein oder mehr b     : ab, abb, abbb
(ab)+   → ein oder mehr Mal die Folge ab  : ab, abab, ababab
```

### Alternative `|` – „entweder … oder"

Der senkrechte Strich trennt Alternativen. Klammern grenzen ein, **wie weit** die Alternative reicht:

```
gr(a|e)y   → "gray" oder "grey"
gra|ey     → "gra" ODER "ey"   – ohne Klammern gilt der Strich für den GESAMTEN Ausdruck
```

### Rückwärtsverweise: `\1`, `\2` …

Was eine Gruppe trifft, wird gespeichert und ist über ihre Nummer abrufbar: die erste Klammer als `\1`, die zweite als `\2` und so fort. Steht ein solcher Verweis **im selben Muster**, sucht er eine Wiederholung des zuvor Getroffenen:

```bash
grep -E '(.)\1'    datei    # zwei gleiche Zeichen nebeneinander: "ll", "ee", "oo"
grep -E '(\w+) \1' datei    # ein doppeltes Wort: "the the"
```

### Gruppen in Ersetzungen (`sed`)

Beim Suchen-und-Ersetzen ist das Gedächtnis der Klammern besonders nützlich: Im Ersetzungstext fügt `\1`, `\2` … das jeweils Getroffene wieder ein. So lassen sich Teile umstellen:

```bash
echo 'Mustermann, Max' | sed -E 's/(.+), (.+)/\2 \1/'
# Max Mustermann        \1 = "Mustermann", \2 = "Max", vertauscht
```

**Hinweis:** In ERE (`grep -E`, `sed -E`) sind `(`, `)` und `|` sofort Metazeichen. In BRE (einfaches `grep`, `sed`) muss man sie einschalten: `\(`, `\)`, `\|`. Der Rückwärtsverweis `\1` schreibt sich dagegen in **beiden** Dialekten mit Backslash. Näheres im Abschnitt *BRE, ERE, PCRE*.

## BRE, ERE, PCRE – welche Variante wann

Reguläre Ausdrücke gibt es in mehreren Dialekten. Sie unterscheiden sich vor allem darin, **welche Zeichen als Metazeichen gelten**.

### Grundform: BRE

In den *Basic Regular Expressions* sind nur `.`, `*`, `[…]`, `^` und `$` Metazeichen; alle übrigen Zeichen stehen für sich selbst. Der Backslash wirkt hier **umgekehrt** zur Gewohnheit: er nimmt die Sonderbedeutung nicht weg, sondern **schaltet sie ein**.

```bash
grep '3+b'  datei    # findet die Zeichenfolge 3+b – das Plus ist wörtlich gemeint
grep '3\+b' datei    # findet 3b, 33b, 3333b – eine oder mehr Ziffern 3
```

### Erweiterte Form: ERE

In den *Extended Regular Expressions* sind `+`, `?`, `{…}`, `(…)` und die Alternative von sich aus Metazeichen. Wer sie wörtlich meint, stellt einen Backslash voran.

```bash
grep -E '3+b'  datei    # findet 3b, 33b, 3333b
grep -E '3\+b' datei    # findet die Zeichenfolge 3+b
```

### Gegenüberstellung

| Bedeutung | BRE (`grep`, `sed`) | ERE (`grep -E`, `sed -E`, `awk`) |
|---|---|---|
| eins oder mehr | `3\+b` | `3+b` |
| null oder eins | `ab\?c` | `ab?c` |
| n- bis m-mal | `a\{2,3\}` | `a{2,3}` |
| Gruppierung | `\(ab\)` | `(ab)` |
| Alternative | <code>rot\&#124;blau</code> | <code>rot&#124;blau</code> |
| Pluszeichen wörtlich | `3+b` | `3\+b` |

Die letzte Zeile zeigt die Umkehrung: In BRE ist `\+` der Quantor und `+` das Zeichen – in ERE genau andersherum.

### Welches Werkzeug spricht welchen Dialekt?

| Werkzeug | Dialekt |
|---|---|
| `grep` | BRE |
| `grep -E` (früher `egrep`) | ERE |
| `grep -P` | PCRE |
| `sed` | BRE |
| `sed -E` (auch `sed -r`) | ERE |
| `awk` | ERE |

### PCRE

Mit `grep -P` schaltet man auf *Perl Compatible Regular Expressions* um. Dieser Dialekt kennt zusätzliche Kurzformen:

```bash
grep -P '\d{3}' datei    # \d = Ziffer, entspricht [[:digit:]]
grep -P '\w+'   datei    # \w = Buchstabe, Ziffer oder Unterstrich
grep -P '\s'    datei    # \s = Leerraum (Leerzeichen, Tabulator …)
```

PCRE verhält sich wie ERE: `+`, `?`, `{…}`, `(…)` sind von sich aus Metazeichen, ein Backslash macht sie wörtlich.

**Hinweis:** `-P` steht nicht überall zur Verfügung – `grep` muss mit PCRE-Unterstützung übersetzt sein. Für portable Skripte bleibt man besser bei BRE oder ERE.

### Faustregeln

- Genügen `.`, `*`, `[…]`, `^` und `$`, so **reicht BRE** – also das einfache `grep 'muster'`.
- Sobald `+`, `?`, `{…}`, Gruppierung oder Alternative gebraucht werden: **`grep -E` nehmen**. Das spart Backslashes und bleibt lesbar – man vergleiche `grep '\(ab\)\+'` mit `grep -E '(ab)+'`.
- `\+` und `\?` sind **GNU-Erweiterungen** und gehören nicht zum POSIX-Standard; auf fremden Unix-Systemen (BSD, Solaris) können sie fehlen. `\{n,m\}` dagegen ist standardisiert.
- Portabel und überall gültig lässt sich „eins oder mehr" auch mit `*` ausdrücken:

```bash
grep '33*b' datei    # eine 3, danach beliebig viele weitere 3
```

## Gierige und faule Quantoren

Quantoren sind von Haus aus **gierig** (greedy): Sie nehmen so viele Zeichen wie möglich. Der Treffer ist deshalb nicht der erstmögliche, sondern der **längstmögliche**.

### Wann Gier überhaupt auffällt

Das gewöhnliche `grep` filtert **ganze Zeilen** – ob das Muster kurz oder lang zutrifft, ändert an der Ausgabe nichts. Sichtbar wird die Gier erst dort, wo der Treffer selbst gebraucht wird: bei `grep -o`, bei Ersetzungen mit `sed 's/…/…/'` und bei Rückwärtsverweisen (`\1`).

### Wie die Suche abläuft

Der Suchalgorithmus ist einfacher, als man vermutet:

1. Für **jede Position** in der Zeile: versuche, das Muster ab dort zu treffen.
2. Kein Treffer? Eine Position weiter, von vorn.

Interessant wird es innerhalb eines Versuchs. Wir suchen mit dem Muster `".+"` – Anführungszeichen, dann ein oder mehr beliebige Zeichen, dann wieder ein Anführungszeichen:

```bash
echo 'user="root" shell="/bin/bash" ok' | grep -oE '".+"'
```

**Schritt 1.** Das erste Musterzeichen ist `"`. An Position 1 steht ein `u` – kein Treffer. Die Suche rückt vor, bis sie das Anführungszeichen findet:

```
user="root" shell="/bin/bash" ok
     ^
     Muster: "   gefunden
```

**Schritt 2.** Es folgt `.` – „ein beliebiges Zeichen außer Zeilenumbruch". Das `r` passt:

```
user="root" shell="/bin/bash" ok
     "^
      Muster: .   passt auf r
```

**Schritt 3.** Wegen des Quantors `+` wiederholt sich der Punkt. Der Suchalgorithmus nimmt Zeichen um Zeichen dazu. Bis wohin? Jedes Zeichen passt auf den Punkt – er hört also erst **am Zeilenende** auf:

```
user="root" shell="/bin/bash" ok
     "~~~~~~~~~~~~~~~~~~~~~~~~~~
      .+ hat alles bis zum Zeilenende genommen
```

**Schritt 4.** Jetzt ist `.+` fertig, und das letzte Musterzeichen ist an der Reihe: `"`. Aber die Zeile ist zu Ende, da steht nichts mehr. Der Suchalgorithmus erkennt, dass er zu viel genommen hat, und **gibt ein Zeichen zurück** (*Backtracking*):

```
user="root" shell="/bin/bash" ok
     "~~~~~~~~~~~~~~~~~~~~~~~~~
                               ^ hier müsste " stehen, es steht aber k
```

**Schritt 5.** Kein Treffer, also noch ein Zeichen zurück – und noch eines:

```
user="root" shell="/bin/bash" ok
     "~~~~~~~~~~~~~~~~~~~~~~~~
                              ^ hier steht o
user="root" shell="/bin/bash" ok
     "~~~~~~~~~~~~~~~~~~~~~~~
                             ^ hier steht ein Leerzeichen
```

**Schritt 6.** Beim nächsten Rückschritt passt es: an dieser Stelle steht tatsächlich ein `"`.

```
user="root" shell="/bin/bash" ok
     "~~~~~~~~~~~~~~~~~~~~~~"
     └────── Treffer ───────┘
```

Gefunden wird also `"root" shell="/bin/bash"` – ein einziger, viel zu langer Treffer. Nicht das, was man erwartet hat, aber genau das, was „gierig" bedeutet: **Der Quantor nimmt zuerst alles und gibt nur so viel zurück, wie nötig ist, damit der Rest des Musters noch passt.**

### Die Lösung: negierte Zeichenklasse

Statt „beliebige Zeichen" sagt man „beliebige Zeichen **außer** dem Begrenzer". Dann kann der Quantor gar nicht erst zu weit greifen:

```bash
echo 'user="root" shell="/bin/bash" ok' | grep -o '"[^"]*"'
```

`"[^"]*"` heißt: Anführungszeichen, dann beliebig viele Zeichen **die keine Anführungszeichen sind**, dann wieder ein Anführungszeichen. Dieser Weg funktioniert in **jedem** Dialekt – auch im einfachen BRE – und kommt ohne Backtracking aus. Dasselbe Vorgehen bei HTML-Auszeichnungen:

```bash
echo '<b>fett</b> und <i>kursiv</i>' | sed -E 's/<.*>/X/'      # X
echo '<b>fett</b> und <i>kursiv</i>' | sed -E 's/<[^>]*>/X/g'  # XfettX und XkursivX
```

Im ersten Fall beginnt `<.*>` beim allerersten `<` der Zeile. Der Punkt trifft **jedes** Zeichen – auch `>` und `<` –, also frisst `.*` sämtliche Auszeichnungen und den Text dazwischen und hört erst beim **letzten** `>` der Zeile auf. Ein einziger Treffer über die ganze Zeile, ersetzt durch ein `X`.

Im zweiten Fall darf `[^>]*` das Zeichen `>` **nicht** überspringen. Der Treffer endet damit zwangsläufig am **nächsten** `>` – das ist genau eine Auszeichnung. Das angehängte `g` ist dabei keine Option von `sed`, sondern ein **Flag des `s`-Befehls**: Es wiederholt die Ersetzung für **jedes** Vorkommen in der Zeile, nicht nur für das erste. Deshalb bleiben `fett` und `kursiv` stehen. Im ersten Beispiel wäre `g` sinnlos – der gierige Treffer hat die Zeile ohnehin schon vollständig verschluckt. Mehr dazu bei [sed](../commands/text-processing.md#sed).

### Faule Quantoren – nur in PCRE

Perl-artige Dialekte kennen den **faulen** (lazy, non-greedy) Modus: ein `?` **hinter** dem Quantor lässt ihn so wenig Zeichen wie möglich nehmen. Der Quantor nimmt zunächst gar nichts und fügt erst dann Zeichen hinzu, wenn der Rest des Musters sonst nicht passt – das genaue Gegenteil zu Schritt 3.

```bash
echo 'user="root" shell="/bin/bash" ok' | grep -oP '".+?"'
# "root"
# "/bin/bash"
```

| Gierig | Faul | Bedeutung |
|---|---|---|
| `*` | `*?` | null oder mehr |
| `+` | `+?` | eins oder mehr |
| `?` | `??` | null oder eins |
| `{n,m}` | `{n,m}?` | n bis m Mal |

**Achtung:** In BRE und ERE gibt es **keine** faulen Quantoren. Dort wird das `?` als eigener Quantor auf das Vorherige gelesen – das Muster bleibt gierig und meldet **keinen Fehler**:

```bash
echo 'user="root" shell="/bin/bash" ok' | grep -oE '".+?"'
# "root" shell="/bin/bash"     unverändert gierig
```

Das gilt ebenso für `sed -E` und `awk`. Wer `.*?` aus Perl, Python oder JavaScript gewohnt ist, erzeugt hier also stillschweigend ein falsches Ergebnis. In `grep`, `sed` und `awk` gilt: **negierte Zeichenklasse statt faulem Quantor.**

### Gier gezielt einsetzen

Richtig verstanden ist die Gier ein Werkzeug. `.*` läuft bis zum **letzten** möglichen Vorkommen – damit lässt sich bequem am hintersten Trenner schneiden:

```bash
echo 'eins,zwei,drei' | sed 's/.*,//'    # drei   – alles bis zum LETZTEN Komma weg
echo 'eins,zwei,drei' | sed 's/,.*//'    # eins   – ab dem ERSTEN Komma alles weg
```

**Hinweis:** POSIX-Werkzeuge liefern grundsätzlich den **längsten** Treffer an der frühesten Fundstelle (*leftmost-longest*). PCRE nimmt dagegen die **erste passende** Alternative:

```bash
echo 'abcd' | grep -oE 'ab|abcd'    # abcd
echo 'abcd' | grep -oP 'ab|abcd'    # ab
```

## Regex ist nicht Globbing

Der häufigste Anfängerfehler: dieselben Zeichen bedeuten in beiden Welten etwas **anderes**.

| | Globbing | Regulärer Ausdruck |
|---|---|---|
| Wer wertet aus? | die **Shell**, *vor* dem Start des Befehls | der **Befehl** selbst (`grep`, `sed`, `awk`) |
| Angewendet auf | **Dateinamen** | den **Inhalt** von Textzeilen |
| `*` | beliebig viele beliebige Zeichen | **null oder mehr** vom Zeichen **davor** |
| `?` | genau ein beliebiges Zeichen | vom Zeichen davor: null oder einmal (nur ERE) |
| `.` | ein gewöhnlicher Punkt | **ein beliebiges** Zeichen |
| Beispiel | `ls *.txt` | `grep 'ab*c'` |

**Merke:** In `ls *.txt` heißt `*` „irgendetwas". In `grep 'ab*c'` heißt `*` „das `b` davor darf beliebig oft vorkommen, auch gar nicht" – es passt auf `ac`, `abc`, `abbbc`. Siehe auch [Globbing & Platzhalter](globbing.md).

## Muster immer in Anführungszeichen

Ein Regex enthält oft Zeichen, die auch die Shell für sich beansprucht (`*`, `?`, `$`, `[`, `\`). Damit das Muster **unverändert** beim Befehl ankommt, gehört es in **einfache** Anführungszeichen:

```bash
grep '^root' /etc/passwd    # richtig: die Shell fasst das Muster nicht an
grep ^root /etc/passwd      # geht oft gut, ist aber Glückssache
grep "$HOME" datei          # Vorsicht: doppelte Quotes ersetzen Variablen
```
