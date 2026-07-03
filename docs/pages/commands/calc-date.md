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
- [Prozesse & Steuerung](process-control.md)
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Rechnen & Datum

### bc
>**Funktion:** Rechner mit beliebiger Genauigkeit<br />
>**Syntax:** `bc [optionen] [<datei>...]`<br />
>**Erklärung:** Wertet mathematische Ausdrücke aus – auch mit Nachkommastellen und sehr großen Zahlen. Im Kern eine kleine Programmiersprache mit Variablen, Schleifen und Funktionen. Liest aus der Eingabe (Pipe/Here-String) oder arbeitet interaktiv.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` lädt die mathematische Bibliothek und setzt `scale=20`<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-q` startet ohne Begrüßungstext (quiet)<br />
>**Beispiel:** `echo "scale=2; 10 / 3" | bc`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--mathlib` | lädt die mathematische Bibliothek (`sqrt`, `s`, `c`, `l`, `e`) und setzt `scale=20` |
| `-q`, `--quiet` | unterdrückt den Begrüßungstext beim Start |
| `-P`, `--no-prompt` | zeigt keine Eingabeaufforderung an |
| `-i`, `--interactive` | erzwingt den interaktiven Modus |
| `-w`, `--warn` | warnt bei nicht-POSIX-Konstrukten |
| `-s`, `--standard` | verarbeitet strikt nach POSIX-`bc` |
| `-h`, `--help` | zeigt die Hilfe an |
| `-v`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Rechen-Grundlagen</summary>

- `scale` = Anzahl der Nachkommastellen (Standard `0` → Division wird abgeschnitten).
- Operatoren: `+`, `-`, `*`, `/`, `%` (Rest), `^` (Potenz).
- `ibase` / `obase` = Zahlenbasis für Eingabe / Ausgabe (z. B. Hex/Binär).
- Funktionen mit `-l`: `sqrt(x)` Wurzel, `s(x)` Sinus, `c(x)` Cosinus, `l(x)` nat. Logarithmus, `e(x)` Exponent.

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
echo "2 + 3 * 4" | bc            # 14
echo "scale=4; 22/7" | bc        # 3.1428
echo "sqrt(2)" | bc -l           # 1.41421356237309504880
echo "2^16" | bc                 # 65536
bc <<< "ibase=16; FF"            # 255 (Hexadezimal -> Dezimal)
ergebnis=$(echo "5 * 5" | bc)    # Ergebnis in einer Variablen speichern
```

</details>

>**Hinweis:** Ohne `scale` schneidet die Division auf ganze Zahlen ab (`10/3` → `3`); `bc -l` setzt `scale=20`. Oft nicht vorinstalliert (`sudo apt install bc`). Für reine Ganzzahl-Rechnung genügt die Shell selbst: `$(( 2 + 3 ))`.

---

### date
>**Funktion:** Datum und Uhrzeit anzeigen oder setzen<br />
>**Syntax:** `date [optionen] [+<format>]`<br />
>**Erklärung:** Zeigt das aktuelle Datum und die Uhrzeit an; mit einem Format-String lässt sich die Ausgabe frei gestalten. Als Administrator kann auch die Systemzeit gesetzt werden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`+<format>` legt das Ausgabeformat fest (z. B. `+%Y-%m-%d`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <datum>` zeigt ein anderes Datum statt jetzt an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` nutzt UTC statt der lokalen Zeit<br />
>**Beispiel:** `date +%Y-%m-%d`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `+<format>` | legt das Ausgabeformat fest (Platzhalter siehe unten) |
| `-d <datum>`, `--date=<datum>` | zeigt ein anderes Datum an (auch „menschlich": `"yesterday"`, `"2 days ago"`) |
| `-f <datei>`, `--file=<datei>` | verarbeitet je eine Datumsangabe pro Zeile aus einer Datei (wie `-d` mehrfach) |
| `-u`, `--utc` | nutzt UTC statt der lokalen Zeitzone |
| `-r <datei>`, `--reference=<datei>` | zeigt die letzte Änderungszeit einer Datei |
| `-s <datum>`, `--set=<datum>` | setzt die Systemzeit (root nötig) |
| `-I[<genauigkeit>]`, `--iso-8601` | Ausgabe im ISO-8601-Format |
| `-R`, `--rfc-email` | Ausgabe im RFC-5322-Format (z. B. für E-Mail-Header) |
| `--rfc-3339=<genauigkeit>` | Ausgabe im RFC-3339-Format (`date`, `seconds` oder `ns`) |
| `--debug` | erläutert das geparste Datum und warnt bei fragwürdiger Eingabe |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Häufige Format-Platzhalter</summary>

| Platzhalter | Bedeutung |
|---|---|
| `%Y` | Jahr (vierstellig) |
| `%m` | Monat (01–12) |
| `%d` | Tag (01–31) |
| `%H` / `%M` / `%S` | Stunde / Minute / Sekunde |
| `%A` / `%a` | Wochentag lang / kurz |
| `%B` / `%b` | Monatsname lang / kurz |
| `%j` | Tag des Jahres (001–366) |
| `%s` | Unix-Zeit (Sekunden seit 1970) |
| `%F` | Kurzform für `%Y-%m-%d` |
| `%T` | Kurzform für `%H:%M:%S` |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
date                          # vollständiges Datum und Uhrzeit
date +%Y-%m-%d                # 2026-06-24
date +"%d.%m.%Y %H:%M"        # 24.06.2026 15:30
date -d "2 days ago" +%F      # Datum von vorgestern
date -u                       # aktuelle Zeit in UTC
date +%s                      # Unix-Zeitstempel
tar -cf backup_$(date +%F).tar ordner/   # Dateiname mit aktuellem Datum
```

</details>

>**Hinweis:** Sehr praktisch in Skripten für Zeitstempel in Datei- oder Log-Namen, z. B. `log_$(date +%F).txt`. Das Setzen der Zeit mit `-s` erfordert root-Rechte; auf modernen Systemen übernimmt das meist `timedatectl` bzw. NTP.

---
