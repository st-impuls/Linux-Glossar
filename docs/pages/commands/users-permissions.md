[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

## Kategorien
- [Archivierung & Kompression](archiving-compression.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Datenträger & Dateisysteme](disk-filesystems.md)
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

# Benutzer & Rechte

### adduser
>**Funktion:** Benutzer anlegen (Debian/Ubuntu)<br />
>**Syntax:** `adduser [optionen] <benutzer> [<gruppe>]`<br />
>**Erklärung:** Komfortables, interaktives Werkzeug zum Anlegen eines Benutzers oder zum Hinzufügen zu einer Gruppe; ein Perl-Skript auf Basis von `useradd`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--ingroup <gruppe>` legt die primäre Gruppe des neuen Benutzers fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--home <pfad>` legt das Home-Verzeichnis fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--system` legt einen Systembenutzer ohne Login an<br />
>**Beispiel:** `sudo adduser user`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--ingroup <gruppe>` | legt die primäre Gruppe des neuen Benutzers fest |
| `--home <pfad>` | legt das Home-Verzeichnis fest |
| `--shell <shell>` | legt die Login-Shell fest (z. B. `/bin/bash`) |
| `--system` | legt einen Systembenutzer ohne Login an |
| `--disabled-password` | legt das Konto ohne Passwort an |
| `--uid <n>` | legt eine feste Benutzer-ID fest |
| `--gid <n>` | legt die primäre Gruppen-ID fest |
| `--gecos <text>` | setzt den vollständigen Namen (GECOS-Feld) |
| `--disabled-login` | legt das Konto an, verhindert aber jede Anmeldung |
| `--no-create-home` | legt kein Home-Verzeichnis an |
| `--quiet` | unterdrückt informative Meldungen |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo adduser max                  # interaktiv neuen Benutzer anlegen
sudo adduser max sudo             # bestehenden Benutzer zur Gruppe sudo hinzufügen
sudo adduser --system backupbot   # Systembenutzer anlegen
```

</details>

>**Hinweis:** Nur auf Debian-basierten Systemen verfügbar. Fragt Passwort und Benutzerdaten interaktiv ab und legt das Home-Verzeichnis automatisch an. Mit zwei Argumenten (`adduser user gruppe`) wird der Benutzer nur zur Gruppe hinzugefügt. Gegenstück: `deluser`; Low-Level-Alternative: `useradd`.

---

### chgrp
>**Funktion:** Gruppe einer Datei ändern<br />
>**Syntax:** `chgrp [optionen] <gruppe> <datei>...`<br />
>**Erklärung:** Ändert die Gruppenzugehörigkeit einer Datei oder eines Verzeichnisses (change group).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R` ändert die Gruppe rekursiv für alle Unterordner und Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jede Datei die vorgenommene Änderung an<br />
>**Beispiel:** `sudo chgrp entwickler projekt/`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-R`, `--recursive` | ändert die Gruppe rekursiv für alle Unterordner und Dateien |
| `-v`, `--verbose` | zeigt für jede Datei die vorgenommene Änderung an |
| `-c`, `--changes` | meldet nur tatsächlich vorgenommene Änderungen |
| `--reference=<datei>` | übernimmt die Gruppe von einer anderen Datei |
| `-f`, `--silent` | unterdrückt die meisten Fehlermeldungen |
| `-h`, `--no-dereference` | wirkt auf den Link selbst, nicht auf das Ziel |
| `-H` / `-L` / `-P` | steuert mit `-R`, ob symbolischen Links gefolgt wird |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo chgrp entwickler projekt/            # Gruppe einer Datei/eines Ordners setzen
sudo chgrp -R entwickler projekt/         # rekursiv für den ganzen Ordner
chgrp --reference=vorlage.txt datei.txt   # Gruppe von vorlage.txt übernehmen
```

</details>

>**Hinweis:** Verwandt mit `chown`, das auch `besitzer:gruppe` setzen kann. Meist mit `sudo` nötig.

---

### chmod
>**Funktion:** Zugriffsrechte ändern<br />
>**Syntax:** `chmod [optionen] <modus> <datei>...`<br />
>**Erklärung:** Ändert die Berechtigungen (Lesen, Schreiben, Ausführen) einer Datei oder eines Ordners.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R` ändert die Rechte rekursiv für alle Unterordner und Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jede Datei die vorgenommene Änderung an<br />
>**Beispiel:** `chmod 755 script.sh`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-R`, `--recursive` | ändert die Rechte rekursiv für alle Unterordner und Dateien |
| `-v`, `--verbose` | zeigt für jede Datei die vorgenommene Änderung an |
| `-c`, `--changes` | meldet nur tatsächlich vorgenommene Änderungen |
| `--reference=<datei>` | übernimmt die Rechte von einer anderen Datei |
| `-f`, `--silent` | unterdrückt die meisten Fehlermeldungen |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Rechte als Zahl (oktal)</summary>

Die Rechte setzen sich pro Stelle aus der Summe zusammen: `4` = lesen `(r)`, `2` = schreiben `(w)`, `1` = ausführen `(x)`. Drei Stellen stehen für **Besitzer**, **Gruppe** und **Andere**.

| Zahl | Recht |
|---|---|
| `7` | `rwx` – lesen, schreiben, ausführen |
| `6` | `rw-` – lesen, schreiben |
| `5` | `r-x` – lesen, ausführen |
| `4` | `r--` – nur lesen |
| `0` | `---` – keine Rechte |

Beispiele: `755` = Besitzer alles, Gruppe/Andere lesen+ausführen · `644` = Besitzer lesen+schreiben, Rest nur lesen · `600` = nur Besitzer lesen+schreiben.

</details>

<details markdown>
<summary>Symbolische Schreibweise</summary>

Wer (`u` Besitzer, `g` Gruppe, `o` Andere, `a` alle) + Operator (`+` hinzufügen, `-` entfernen, `=` exakt setzen) + Recht (`r`, `w`, `x`).

```bash
chmod u+x script.sh        # Ausführrecht für den Besitzer hinzufügen
chmod go-w datei.txt       # Schreibrecht für Gruppe und Andere entfernen
chmod a+r datei.txt        # Leserecht für alle hinzufügen
chmod u=rw,go=r datei.txt  # Rechte exakt setzen
```

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
chmod 755 script.sh        # ausführbares Skript
chmod 644 datei.txt        # normale Datei (Besitzer schreibt, Rest liest)
chmod +x script.sh         # Ausführrecht für alle hinzufügen
chmod -R 750 ordner/       # rekursiv für einen Ordner
```

</details>

>**Hinweis:** Rechte als Zahl: `4` = lesen, `2` = schreiben, `1` = ausführen. `755` = Besitzer alles (7), Gruppe und andere lesen + ausführen (5).

---

### chown
>**Funktion:** Besitzer ändern<br />
>**Syntax:** `chown [optionen] <besitzer>[:<gruppe>] <datei>...`<br />
>**Erklärung:** Ändert den Eigentümer und optional die Gruppe einer Datei oder eines Ordners.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R` ändert den Besitzer rekursiv für alle Unterordner und Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jede Datei die vorgenommene Änderung an<br />
>**Beispiel:** `chown user:user datei.txt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-R`, `--recursive` | ändert den Besitzer rekursiv für alle Unterordner und Dateien |
| `-v`, `--verbose` | zeigt für jede Datei die vorgenommene Änderung an |
| `-c`, `--changes` | meldet nur tatsächlich vorgenommene Änderungen |
| `--reference=<datei>` | übernimmt Besitzer und Gruppe von einer anderen Datei |
| `-f`, `--silent` | unterdrückt die meisten Fehlermeldungen |
| `-h`, `--no-dereference` | wirkt auf den Link selbst, nicht auf das Ziel |
| `-H` / `-L` / `-P` | steuert mit `-R`, ob symbolischen Links gefolgt wird |
| `--from=<aktuell>` | ändert nur, wenn der aktuelle Besitzer passt |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo chown user datei.txt          # nur den Besitzer ändern
sudo chown user:gruppe datei.txt   # Besitzer und Gruppe setzen
sudo chown :gruppe datei.txt       # nur die Gruppe ändern (wie chgrp)
sudo chown -R user:user ordner/    # rekursiv für einen Ordner
```

</details>

>**Hinweis:** Die Angabe `besitzer:gruppe` setzt beides; nur `besitzer` lässt die Gruppe unverändert. Meist mit `sudo` nötig.

---

### chpasswd
>**Funktion:** Passwörter mehrerer Benutzer im Stapel ändern<br />
>**Syntax:** `chpasswd [optionen]`<br />
>**Erklärung:** Liest Zeilen der Form `benutzer:passwort` von der Standardeingabe und setzt die Passwörter ohne Rückfrage. Gedacht für Skripte und das Anlegen oder Aktualisieren vieler Konten auf einmal.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-e` die Passwörter liegen bereits verschlüsselt vor<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <methode>` wählt das Verschlüsselungsverfahren<br />
>**Beispiel:** `echo 'anna:geheim123' | sudo chpasswd`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-e`, `--encrypted` | die übergebenen Passwörter liegen bereits verschlüsselt vor |
| `-c <methode>`, `--crypt-method <methode>` | Verschlüsselungsverfahren (`SHA512`, `SHA256`, `MD5`, `DES`, `NONE`) |
| `-m`, `--md5` | nutzt MD5 für unverschlüsselt übergebene Passwörter |
| `-s <runden>`, `--sha-rounds <runden>` | Anzahl der Runden für SHA256/SHA512 |
| `-R <verz>`, `--root <verz>` | wendet die Änderungen in einem alternativen Wurzelverzeichnis an |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
echo 'anna:geheim123' | sudo chpasswd         # ein Passwort setzen
sudo chpasswd < passwoerter.txt               # viele auf einmal (benutzer:passwort je Zeile)
printf 'anna:pw1\nben:pw2\n' | sudo chpasswd  # mehrere Benutzer
echo 'anna:$6$xyz...' | sudo chpasswd -e      # bereits verschlüsseltes Passwort
```

</details>

>**Hinweis:** Braucht `sudo`/root. Klartext-Passwörter in der Eingabe sind unsicher (Shell-History, Prozessliste) – besser eine Datei mit strengen Rechten oder bereits verschlüsselte Hashes (`-e`) verwenden. Für die interaktive Änderung eines einzelnen Passworts `passwd` nutzen.

---

### deluser
>**Funktion:** Benutzer löschen (Debian/Ubuntu)<br />
>**Syntax:** `deluser [optionen] <benutzer> [<gruppe>]`<br />
>**Erklärung:** Komfortables Werkzeug zum Entfernen eines Benutzers oder zum Entfernen aus einer Gruppe; ein Perl-Skript auf Basis von `userdel`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--remove-home` löscht das Home-Verzeichnis des Benutzers<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--remove-all-files` löscht alle Dateien des Benutzers im System<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--backup` legt vor dem Löschen ein Backup der Dateien an<br />
>**Beispiel:** `sudo deluser user sudo`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--remove-home` | löscht das Home-Verzeichnis des Benutzers |
| `--remove-all-files` | löscht alle Dateien des Benutzers im System |
| `--backup` | legt vor dem Löschen ein Backup der Dateien an |
| `--backup-to <pfad>` | legt das Backup in ein bestimmtes Verzeichnis |
| `--system` | entfernt nur, wenn es sich um einen Systembenutzer handelt |
| `--quiet` | unterdrückt informative Meldungen |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo deluser max                   # Benutzer entfernen (Home bleibt erhalten)
sudo deluser --remove-home max     # Benutzer samt Home-Verzeichnis löschen
sudo deluser max sudo              # nur aus der Gruppe sudo entfernen
```

</details>

>**Hinweis:** Nur auf Debian-basierten Systemen verfügbar. Mit zwei Argumenten (`deluser user gruppe`) wird der Benutzer nur aus der Gruppe entfernt. Gegenstück: `adduser`.

---

### groupadd
>**Funktion:** Gruppe anlegen<br />
>**Syntax:** `groupadd [optionen] <gruppe>`<br />
>**Erklärung:** Legt eine neue Benutzergruppe im System an (Low-Level-Werkzeug).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-g` legt die Gruppen-ID (GID) fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` legt eine Systemgruppe an<br />
>**Beispiel:** `sudo groupadd entwickler`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-g <gid>`, `--gid <gid>` | legt die Gruppen-ID (GID) fest |
| `-r`, `--system` | legt eine Systemgruppe an |
| `-f`, `--force` | bricht nicht mit Fehler ab, wenn die Gruppe bereits existiert |
| `-o`, `--non-unique` | erlaubt eine nicht eindeutige (doppelte) GID |
| `-p <hash>`, `--password <hash>` | setzt ein (verschlüsseltes) Gruppenpasswort |
| `-K <schl=wert>`, `--key <schl=wert>` | überschreibt einen Wert aus `/etc/login.defs` |
| `-R <pfad>`, `--root <pfad>` | wendet Änderungen in einem anderen Wurzelverzeichnis an |
| `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo groupadd entwickler        # neue Gruppe anlegen
sudo groupadd -g 1500 entwickler # mit fester GID
sudo groupadd -r systemdienst   # Systemgruppe anlegen
```

</details>

>**Hinweis:** Meist mit `sudo` nötig. Auf Debian/Ubuntu ist `addgroup` die komfortablere Alternative.

---

### groupdel
>**Funktion:** Gruppe löschen<br />
>**Syntax:** `groupdel [optionen] <gruppe>`<br />
>**Erklärung:** Entfernt eine bestehende Benutzergruppe aus dem System.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Löschen, auch wenn es die primäre Gruppe eines Benutzers ist (force)<br />
>**Beispiel:** `sudo groupdel entwickler`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-f`, `--force` | erzwingt das Löschen, auch wenn es die primäre Gruppe eines Benutzers ist |
| `-R <pfad>`, `--root <pfad>` | wendet Änderungen in einem anderen Wurzelverzeichnis an |
| `--help` | zeigt die Hilfe an |

</details>

>**Hinweis:** Meist mit `sudo` nötig. Die primäre Gruppe eines bestehenden Benutzers lässt sich normalerweise nicht löschen. Gegenstück: `groupadd`.

---

### groups
>**Funktion:** Gruppenzugehörigkeit anzeigen<br />
>**Syntax:** `groups [<benutzer>...]`<br />
>**Erklärung:** Zeigt alle Gruppen an, denen der aktuelle oder ein angegebener Benutzer angehört.<br />
>**Beispiel:** `groups user`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

>**Hinweis:** Ohne Argument werden die Gruppen des aktuell angemeldeten Benutzers angezeigt. Detailliertere Angaben liefert `id`.

---

### id
>**Funktion:** Benutzer- und Gruppen-IDs anzeigen<br />
>**Syntax:** `id [optionen] [<benutzer>]`<br />
>**Erklärung:** Zeigt die UID, GID und die Gruppenzugehörigkeit des aktuellen oder eines angegebenen Benutzers an.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` zeigt nur die Benutzer-ID (UID) an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-g` zeigt nur die primäre Gruppen-ID (GID) an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` zeigt Namen statt Nummern an (mit `-u`/`-g`/`-G`)<br />
>**Beispiel:** `id user`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-u`, `--user` | zeigt nur die Benutzer-ID (UID) an |
| `-g`, `--group` | zeigt nur die primäre Gruppen-ID (GID) an |
| `-G`, `--groups` | zeigt alle Gruppen-IDs an |
| `-n`, `--name` | zeigt Namen statt Nummern an (mit `-u`/`-g`/`-G`) |
| `-r`, `--real` | zeigt die reale statt der effektiven ID an |
| `-z`, `--zero` | trennt die Ausgaben mit dem Nullbyte |
| `-Z`, `--context` | zeigt nur den SELinux-Sicherheitskontext |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
id                  # eigene UID, GID und Gruppen
id user             # Angaben zu einem bestimmten Benutzer
id -un              # nur der eigene Benutzername (wie whoami)
id -nG              # alle Gruppennamen des Benutzers
```

</details>

>**Hinweis:** Ohne Argument werden die Angaben zum aktuell angemeldeten Benutzer ausgegeben.

---

### logname
>**Funktion:** Anmeldenamen des Benutzers anzeigen<br />
>**Syntax:** `logname`<br />
>**Erklärung:** Zeigt den Namen an, unter dem sich der Benutzer ursprünglich angemeldet hat (ermittelt aus der Anmeldedatenbank des Systems).<br />
>**Beispiel:** `logname`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

>**Hinweis:** Anders als `whoami` zeigt `logname` immer den **ursprünglichen** Anmeldenamen – nach `su` oder `sudo` bleibt es also der echte Anmeldebenutzer, während `whoami` den aktuell aktiven Benutzer (z. B. `root`) ausgibt.

---

### passwd
>**Funktion:** Passwort ändern<br />
>**Syntax:** `passwd [optionen] [<benutzer>]`<br />
>**Erklärung:** Ändert das Passwort des eigenen Benutzers oder (als Administrator) eines anderen Benutzers.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` sperrt ein Benutzerkonto (lock)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` entsperrt ein Benutzerkonto (unlock)<br />
>**Beispiel:** `passwd`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--lock` | sperrt ein Benutzerkonto |
| `-u`, `--unlock` | entsperrt ein Benutzerkonto |
| `-d`, `--delete` | entfernt das Passwort (Konto ohne Passwort) |
| `-e`, `--expire` | erzwingt eine Passwortänderung bei der nächsten Anmeldung |
| `-S`, `--status` | zeigt den Status des Kontos an |
| `-a`, `--all` | mit `-S`: Status aller Konten anzeigen |
| `-n <tage>`, `--mindays` | Mindestzeit zwischen Passwortänderungen |
| `-x <tage>`, `--maxdays` | maximale Gültigkeit des Passworts |
| `-w <tage>`, `--warndays` | Vorwarnzeit vor dem Ablauf |
| `-k`, `--keep-tokens` | ändert nur bereits abgelaufene Passwörter |
| `-q`, `--quiet` | stiller Modus |
| `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
passwd                  # eigenes Passwort ändern
sudo passwd max         # Passwort von max ändern
sudo passwd -l max      # Konto sperren
sudo passwd -S max      # Status des Kontos anzeigen
```

</details>

>**Hinweis:** Ohne Angabe wird das eigene Passwort geändert. Für andere Benutzer, z. B. `sudo passwd max`, sind Administratorrechte nötig.

---

### su
>**Funktion:** Benutzer wechseln<br />
>**Syntax:** `su [optionen] [<benutzer>]`<br />
>**Erklärung:** Startet eine Shell als anderer Benutzer, standardmäßig als `root` (substitute user).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-` (bzw. `-l`) startet eine Login-Shell mit der Umgebung des Zielbenutzers<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` führt einen einzelnen Befehl als Zielbenutzer aus<br />
>**Beispiel:** `su - user`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-`, `-l`, `--login` | startet eine Login-Shell mit der Umgebung des Zielbenutzers |
| `-c <befehl>`, `--command=<befehl>` | führt einen einzelnen Befehl als Zielbenutzer aus |
| `-s <shell>`, `--shell=<shell>` | nutzt eine bestimmte Shell |
| `-m`, `-p`, `--preserve-environment` | behält die bisherige Umgebung bei |
| `-g <gruppe>`, `--group=<gruppe>` | legt die primäre Gruppe fest (nur als root) |
| `-w <liste>`, `--whitelist-environment=<liste>` | behält bestimmte Variablen bei |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Unterschied: <code>su</code> und <code>su -</code></summary>

Der Bindestrich entscheidet, ob die **Umgebung** des Zielbenutzers geladen wird:

| Aufruf | Wirkung |
|---|---|
| `su` (bzw. `su <benutzer>`) | wechselt nur den Benutzer, behält aber die bisherige Umgebung: aktuelles Verzeichnis, `PATH` und Variablen der alten Sitzung bleiben erhalten |
| `su -` (bzw. `su - <benutzer>`) | startet eine vollständige **Login-Shell**: lädt die Profildateien des Zielbenutzers (`~/.bash_profile`, `~/.profile`), setzt `HOME`, `PATH` und `SHELL` neu und wechselt in dessen Home-Verzeichnis |

Praktisch heißt das: Nach `su` ohne Bindestrich fehlen oft Administrationspfade wie `/usr/sbin`, sodass manche Befehle „command not found" melden. `su -` liefert dieselbe Umgebung, als hätte sich der Zielbenutzer direkt angemeldet – darum ist `su -` (für root) meist die richtige Wahl.

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
su - user                  # Login-Shell als user starten
su                         # zu root wechseln (im eigenen Umfeld)
su -c "apt update" root    # einen einzelnen Befehl als root ausführen
```

</details>

>**Hinweis:** Es wird das Passwort des Zielbenutzers abgefragt (anders als bei `sudo`). Mit `exit` kehrt man zur vorherigen Sitzung zurück. Für root meist `su -` verwenden, damit die vollständige Umgebung (inkl. Administrationspfade) geladen wird.

---

### sudo
>**Funktion:** Befehl mit Administratorrechten ausführen<br />
>**Syntax:** `sudo [optionen] <befehl>`<br />
>**Erklärung:** Führt einen einzelnen Befehl mit den Rechten eines anderen Benutzers aus, standardmäßig als `root` (superuser do).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` führt den Befehl als angegebener Benutzer statt als root aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` startet eine Login-Shell als Zielbenutzer<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zeigt die erlaubten Befehle des aktuellen Benutzers an<br />
>**Beispiel:** `sudo apt update`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-u <benutzer>`, `--user=<benutzer>` | führt den Befehl als angegebener Benutzer statt als root aus |
| `-i`, `--login` | startet eine Login-Shell als Zielbenutzer |
| `-s`, `--shell` | startet eine Shell als Zielbenutzer |
| `-l`, `--list` | zeigt die erlaubten Befehle des aktuellen Benutzers an |
| `-k`, `--reset-timestamp` | verlangt beim nächsten Aufruf wieder das Passwort |
| `-E`, `--preserve-env` | übergibt die aktuelle Umgebung an den Befehl |
| `-n`, `--non-interactive` | fragt nicht nach (scheitert, statt zu warten) |
| `-v`, `--validate` | erneuert das Zeitlimit, ohne einen Befehl auszuführen |
| `-b`, `--background` | führt den Befehl im Hintergrund aus |
| `-e`, `--edit` | bearbeitet eine Datei mit erhöhten Rechten (sudoedit) |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo apt update             # Befehl als root ausführen
sudo -u www-data whoami     # Befehl als anderer Benutzer ausführen
sudo -i                     # Root-Login-Shell starten
sudo -l                     # eigene erlaubte Befehle anzeigen
```

</details>

>**Hinweis:** Es wird das eigene Passwort abgefragt; der Benutzer muss über sudoers berechtigt sein, oft über die Gruppe `sudo` oder `wheel`.

---

### umask
>**Funktion:** Standard-Rechtemaske für neue Dateien und Verzeichnisse festlegen | Intern (Builtins)<br />
>**Syntax:** `umask [optionen] [<maske>]`<br />
>**Erklärung:** Legt fest, welche Rechte neu erstellten Dateien und Verzeichnissen **entzogen** werden. Die Maske wird von den Grundrechten abgezogen (Dateien starten bei `666`, Verzeichnisse bei `777`). So erklärt sich, warum eine neue Datei üblicherweise `644` und ein Verzeichnis `755` erhält. Ohne Argument wird die aktuelle Maske angezeigt.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-S` zeigt bzw. setzt die Maske in symbolischer Form<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` gibt sie in einer wieder einlesbaren Form aus<br />
>**Beispiel:** `umask 022`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| (ohne) | zeigt die aktuelle Maske als Oktalzahl an |
| `<maske>` | setzt die Maske (oktal, z. B. `022`, oder symbolisch `u=rwx,g=rx,o=`) |
| `-S` | zeigt/setzt die Maske symbolisch (`u=rwx,g=rx,o=rx`) |
| `-p` | Ausgabe in einer Form, die sich als Befehl wieder einlesen lässt |

</details>

<details markdown>
<summary>Wirkung gängiger Masken</summary>

| Maske | Neue Datei | Neues Verzeichnis | Bedeutung |
|---|---|---|---|
| `022` | `644` | `755` | Standard; andere dürfen lesen |
| `027` | `640` | `750` | Gruppe liest, andere nichts |
| `077` | `600` | `700` | nur der Eigentümer |
| `002` | `664` | `775` | Gruppe darf schreiben (Team-Ordner) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
umask                 # aktuelle Maske anzeigen (z. B. 0022)
umask -S              # symbolisch anzeigen (u=rwx,g=rx,o=rx)
umask 077             # neue Dateien nur für den Eigentümer
umask u=rwx,g=rx,o=   # dasselbe symbolisch gesetzt
```

</details>

>**Hinweis:** `umask` **entzieht** Rechte, während `chmod` sie an bestehenden Objekten setzt. Dateien bekommen nie das Ausführungsrecht per Maske – es wird nur von den Grundrechten `666` abgezogen. Die Maske gilt nur für die aktuelle Shell und ihre Kindprozesse; dauerhaft trägt man sie in `~/.bashrc` oder `/etc/profile` ein. Als Shell-Builtin ohne eigene Man-Seite – Hilfe über `help umask`.

---

### useradd
>**Funktion:** Benutzer anlegen<br />
>**Syntax:** `useradd [optionen] <benutzername>`<br />
>**Erklärung:** Legt ein neues Benutzerkonto an (Low-Level-Werkzeug).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` erstellt das Home-Verzeichnis des Benutzers<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` legt die Login-Shell fest (z. B. `/bin/bash`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-G` fügt den Benutzer zusätzlichen Gruppen hinzu<br />
>**Beispiel:** `sudo useradd -m -s /bin/bash user`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-m`, `--create-home` | erstellt das Home-Verzeichnis des Benutzers |
| `-M`, `--no-create-home` | legt kein Home-Verzeichnis an |
| `-d <pfad>`, `--home-dir <pfad>` | legt den Pfad des Home-Verzeichnisses fest |
| `-b <pfad>`, `--base-dir <pfad>` | Basisverzeichnis für das Home (Standard `/home`) |
| `-k <pfad>`, `--skel <pfad>` | Vorlagenverzeichnis für das Home (Standard `/etc/skel`) |
| `-s <shell>`, `--shell <shell>` | legt die Login-Shell fest (z. B. `/bin/bash`) |
| `-G <gruppen>`, `--groups <gruppen>` | fügt den Benutzer zusätzlichen Gruppen hinzu |
| `-g <gruppe>`, `--gid <gruppe>` | legt die primäre Gruppe fest |
| `-N`, `--no-user-group` | legt keine gleichnamige Gruppe für den Benutzer an |
| `-U`, `--user-group` | legt eine gleichnamige Gruppe an (Standard auf vielen Systemen) |
| `-u <uid>`, `--uid <uid>` | legt die Benutzer-ID fest |
| `-o`, `--non-unique` | erlaubt eine nicht eindeutige (doppelte) UID |
| `-c <text>`, `--comment <text>` | Kommentar bzw. vollständiger Name (GECOS) |
| `-e <JJJJ-MM-TT>`, `--expiredate` | Ablaufdatum des Kontos |
| `-f <n>`, `--inactive <n>` | Tage nach Passwortablauf bis zur Sperre (`-1` = aus) |
| `-p <hash>`, `--password <hash>` | setzt das Passwort als **verschlüsselten** Hash (nicht im Klartext) |
| `-r`, `--system` | legt einen Systembenutzer an |
| `-D`, `--defaults` | zeigt/ändert die Standardwerte (`/etc/default/useradd`) |
| `-K <schl=wert>`, `--key <schl=wert>` | überschreibt einen Wert aus `/etc/login.defs` |
| `-l`, `--no-log-init` | trägt den Benutzer nicht in lastlog/faillog ein |
| `-R <pfad>`, `--root <pfad>` | wendet Änderungen in einem anderen Wurzelverzeichnis an (chroot) |
| `-Z <kontext>`, `--selinux-user` | ordnet einen SELinux-Benutzer zu |
| `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo useradd -m -s /bin/bash max    # Benutzer mit Home und Bash anlegen
sudo useradd -m -G sudo,dev max     # zusätzlich zu Gruppen hinzufügen
sudo useradd -r -s /usr/sbin/nologin dienst  # Dienstkonto ohne Login-Shell
sudo useradd -r -s /bin/false dienst2        # Alternative: Login ebenfalls gesperrt
sudo passwd max                     # danach das Passwort setzen
```

</details>

>**Hinweis:** Meist mit `sudo` nötig. Das Passwort wird separat mit `passwd` gesetzt. Auf Debian/Ubuntu ist `adduser` die komfortablere, interaktive Alternative.

---

### userdel
>**Funktion:** Benutzer löschen<br />
>**Syntax:** `userdel [optionen] <benutzername>`<br />
>**Erklärung:** Entfernt ein Benutzerkonto aus dem System (Low-Level-Werkzeug).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` löscht zusätzlich das Home-Verzeichnis und den Mail-Spool (remove)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Löschen, auch wenn der Benutzer noch angemeldet ist (force)<br />
>**Beispiel:** `sudo userdel -r user`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-r`, `--remove` | löscht zusätzlich das Home-Verzeichnis und den Mail-Spool |
| `-f`, `--force` | erzwingt das Löschen, auch wenn der Benutzer noch angemeldet ist |
| `-R <pfad>`, `--root <pfad>` | wendet Änderungen in einem anderen Wurzelverzeichnis an (chroot) |
| `-Z`, `--selinux-user` | entfernt die SELinux-Benutzerzuordnung |
| `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo userdel max        # Konto löschen, Home-Verzeichnis bleibt
sudo userdel -r max     # Konto samt Home-Verzeichnis löschen
```

</details>

>**Hinweis:** Meist mit `sudo` nötig. Ohne `-r` bleibt das Home-Verzeichnis erhalten.

---

### usermod
>**Funktion:** Bestehendes Benutzerkonto ändern<br />
>**Syntax:** `usermod [optionen] <benutzername>`<br />
>**Erklärung:** Ändert die Eigenschaften eines vorhandenen Kontos – Name, Home-Verzeichnis, Shell, Gruppen oder Sperrstatus.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-aG <gruppe>` fügt den Benutzer zu einer zusätzlichen Gruppe hinzu (append)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <shell>` legt die Login-Shell fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L` / `-U` sperrt / entsperrt das Konto<br />
>**Beispiel:** `sudo usermod -aG sudo max`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l <neu>`, `--login <neu>` | ändert den Anmeldenamen (login) |
| `-d <pfad>`, `--home <pfad>` | setzt ein neues Home-Verzeichnis (mit `-m` Inhalt verschieben) |
| `-m`, `--move-home` | verschiebt den Inhalt des alten Home-Verzeichnisses (nur mit `-d`) |
| `-s <shell>`, `--shell <shell>` | legt die Login-Shell fest (z. B. `/bin/bash`) |
| `-g <gruppe>`, `--gid <gruppe>` | ändert die primäre Gruppe |
| `-G <gruppen>`, `--groups <gruppen>` | setzt die sekundären Gruppen (ersetzt bestehende!) |
| `-a`, `--append` | nur mit `-G`: Gruppen hinzufügen, statt sie zu ersetzen |
| `-L`, `--lock` | sperrt das Konto (Passwort-Login deaktiviert) |
| `-U`, `--unlock` | entsperrt das Konto |
| `-e <JJJJ-MM-TT>`, `--expiredate` | setzt das Ablaufdatum des Kontos |
| `-c <text>`, `--comment <text>` | ändert den Kommentar bzw. vollständigen Namen (GECOS) |
| `-u <uid>`, `--uid <uid>` | ändert die Benutzer-ID (UID) |
| `-o`, `--non-unique` | erlaubt eine nicht eindeutige (doppelte) UID (nur mit `-u`) |
| `-f <n>`, `--inactive <n>` | Tage nach Passwortablauf bis zur Sperre (`-1` = aus) |
| `-p <hash>`, `--password <hash>` | setzt das Passwort als **verschlüsselten** Hash (nicht im Klartext) |
| `-R <pfad>`, `--root <pfad>` | wendet Änderungen in einem anderen Wurzelverzeichnis an (chroot) |
| `-Z <kontext>`, `--selinux-user` | ändert die SELinux-Benutzerzuordnung |
| `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo usermod -aG sudo max            # Benutzer der Gruppe sudo hinzufügen
sudo usermod -aG docker,www-data max  # mehreren Gruppen hinzufügen
sudo usermod -s /bin/zsh max         # Login-Shell ändern
sudo usermod -l maxime max           # Benutzer umbenennen (max -> maxime)
sudo usermod -d /home/maxime -m maxime  # Home-Verzeichnis verschieben
sudo usermod -L max                  # Konto sperren
```

</details>

>**Hinweis:** Beim Hinzufügen zu Gruppen immer `-aG` zusammen verwenden – `-G` allein **ersetzt** alle sekundären Gruppen und entfernt so versehentlich bestehende. Änderungen wirken erst bei der nächsten Anmeldung; eine neue Gruppenzugehörigkeit greift in der laufenden Sitzung erst nach erneutem Login (oder `newgrp <gruppe>`).

---

### w
>**Funktion:** Angemeldete Benutzer und ihre Aktivität anzeigen<br />
>**Syntax:** `w [optionen] [<benutzer>]`<br />
>**Erklärung:** Zeigt, welche Benutzer angemeldet sind und welchen Prozess sie gerade ausführen, samt System-Auslastung.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` Kurzformat ohne Anmeldezeit und CPU-Angaben (short)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` blendet die Kopfzeile aus (no header)<br />
>**Beispiel:** `w`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-s`, `--short` | Kurzformat ohne Anmeldezeit und CPU-Angaben |
| `-h`, `--no-header` | blendet die Kopfzeile aus |
| `-i`, `--ip-addr` | zeigt die IP-Adresse statt des Hostnamens an |
| `-f`, `--from` | zeigt bzw. verbirgt das Feld „von wo" (Remote-Host) |
| `-o`, `--old-style` | altes Ausgabeformat |
| `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

>**Hinweis:** Ausführlicher als `who`; `whoami` zeigt dagegen nur den eigenen Benutzernamen.

---

### who
>**Funktion:** Angemeldete Benutzer anzeigen<br />
>**Syntax:** `who [optionen]`<br />
>**Erklärung:** Zeigt an, welche Benutzer aktuell am System angemeldet sind, samt Terminal und Anmeldezeit.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt alle verfügbaren Informationen an (all)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` zeigt den Zeitpunkt des letzten Systemstarts an (boot)<br />
>**Beispiel:** `who`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-a`, `--all` | zeigt alle verfügbaren Informationen an |
| `-b`, `--boot` | zeigt den Zeitpunkt des letzten Systemstarts an |
| `-r`, `--runlevel` | zeigt das aktuelle Runlevel an |
| `-H`, `--heading` | zeigt eine Kopfzeile mit Spaltentiteln an |
| `-q`, `--count` | zählt die angemeldeten Benutzer |
| `-m` | nur Angaben zum eigenen Terminal |
| `-l`, `--login` | zeigt die System-Login-Prozesse |
| `-T`, `--mesg` | zeigt an, ob Nachrichten erlaubt sind (`+`/`-`) |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

>**Hinweis:** `whoami` zeigt nur den eigenen Benutzernamen; `w` zeigt zusätzlich, was die Benutzer gerade tun.

---

### whoami
>**Funktion:** Eigenen Benutzernamen anzeigen<br />
>**Syntax:** `whoami`<br />
>**Erklärung:** Zeigt den effektiven Benutzernamen an, unter dem der aktuelle Prozess läuft.<br />
>**Beispiel:** `whoami`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

>**Hinweis:** Entspricht `id -un`. Nach `sudo -i` bzw. `su` zeigt es den aktuell aktiven Benutzer (z. B. `root`).

---