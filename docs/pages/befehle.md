[← Zurück zur Übersicht](../index.md)

# Befehle

**Aufbau eines Befehls**

Ein Linux-Befehl ist in der Regel nach folgendem Schema aufgebaut:

`kommando [optionen] <argumente>`

- **kommando** – der Befehl selbst, also das Programm oder die eingebaute Funktion, die ausgeführt wird (z. B. `ls`, `cp`).
- **optionen** – Schalter, die festlegen, *wie* der Befehl arbeitet (z. B. `-l`, `-r`). Meist optional.
- **argumente** – das Objekt, *womit* der Befehl arbeitet, z. B. eine Datei oder ein Verzeichnis.

Schreibweise der Platzhalter in diesem Glossar:

- `<…>` – Platzhalter: hier etwas Eigenes einsetzen (z. B. einen Dateinamen).
- `[…]` – optional: kann angegeben werden, muss aber nicht.
- `…` – wiederholbar: die Angabe kann mehrfach vorkommen.

## Navigation & Suchen

### cd
>**Funktion:** Verzeichnis wechseln | Intern (Builtins)<br />
>**Syntax:** `cd [<verzeichnis>]`<br />
>**Erklärung:** Wechselt in ein anderes Verzeichnis (change directory).<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd ..` wechselt in das übergeordnete Verzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd ~` wechselt in das Home-Verzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd -` wechselt in das zuletzt genutzte Verzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd /` wechselt in das Wurzelverzeichnis (root)<br />
>**Beispiel:** `cd /home/user`

### ls
>**Funktion:** Dateien und Ordner anzeigen | Extern<br />
>**Syntax:** `ls [optionen] [<datei|verzeichnis>...]`<br />
>**Erklärung:** Listet den Inhalt eines Verzeichnisses auf.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` Langformat mit Details (Rechte, Besitzer, Größe, Datum)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt auch versteckte Dateien (beginnend mit `.`)<br />
>**Beispiel:** `ls -la`

### pwd
>**Funktion:** Aktuelles Verzeichnis anzeigen | Intern (Builtins)<br />
>**Syntax:** `pwd [optionen]`<br />
>**Erklärung:** Zeigt den vollständigen Pfad des aktuellen Arbeitsverzeichnisses an (print working directory).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-P` zeigt den physischen Pfad und löst symbolische Links auf<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L` zeigt den logischen Pfad (Standard)<br />
>**Beispiel:** `pwd`

### tree
>**Funktion:** Verzeichnisstruktur als Baum anzeigen | Extern<br />
>**Syntax:** `tree [optionen] [<verzeichnis>]`<br />
>**Erklärung:** Zeigt Verzeichnisse und Dateien in einer übersichtlichen Baumstruktur an.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L <n>` begrenzt die Anzeige auf n Ebenen Tiefe (z. B. `-L 2` = zwei Ebenen)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt nur Verzeichnisse, keine Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt auch versteckte Dateien (beginnend mit `.`)<br />
>**Beispiel:** `tree -L 2`<br />
>**Hinweis:** `tree` ist oft nicht vorinstalliert und muss ggf. nachinstalliert werden, z. B. mit `sudo apt install tree`.

### whereis
>**Funktion:** Pfad zu Programm, Manpage oder Quellcode finden | Extern<br />
>**Syntax:** `whereis [optionen] <name>...`<br />
>**Erklärung:** Zeigt an, wo sich die Programmdatei, das Handbuch und ggf. die Quelldateien eines Befehls befinden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` sucht nur nach der ausführbaren Datei (binary)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` sucht nur nach der Manpage<br />
>**Beispiel:** `whereis ls`<br />
>**Hinweis:** Verwandt sind `which` (zeigt nur den Pfad zum Programm) und `find` (sucht beliebige Dateien).

### locate
>**Funktion:** Dateien schnell über den Namen finden | Extern<br />
>**Syntax:** `locate [optionen] <muster>...`<br />
>**Erklärung:** Sucht Dateien anhand des Namens in einer zuvor angelegten Datenbank und ist dadurch deutlich schneller als `find`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` zeigt nur die Anzahl der Treffer an<br />
>**Beispiel:** `locate datei.txt`<br />
>**Hinweis:** Die Datenbank wird mit `sudo updatedb` aktualisiert; neue Dateien werden sonst nicht gefunden. `locate` ist oft nicht vorinstalliert (Paket `mlocate` bzw. `plocate`).

### find
>**Funktion:** Dateien und Ordner suchen | Extern<br />
>**Syntax:** `find [<startverzeichnis>] [optionen]`<br />
>**Erklärung:** Durchsucht ein Verzeichnis (und dessen Unterordner) live nach Dateien anhand von Name, Typ, Größe und weiteren Kriterien.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-name <muster>` sucht nach dem Dateinamen (z. B. `-name "*.txt"`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-iname <muster>` wie `-name`, ignoriert aber Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-type f` sucht nur Dateien, `-type d` nur Verzeichnisse<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-size <n>` sucht nach der Größe (z. B. `+10M` = größer als 10 MB)<br />
>**Beispiel:** `find /home -name "*.txt"`<br />
>**Hinweis:** Das erste Argument ist das Startverzeichnis (`.` für das aktuelle). Anders als `locate` sucht `find` immer aktuell, ist aber langsamer.

## Dateien & Verzeichnisse verwalten

### mkdir
>**Funktion:** Neues Verzeichnis erstellen | Extern<br />
>**Syntax:** `mkdir [optionen] <verzeichnis>...`<br />
>**Erklärung:** Erstellt einen oder mehrere neue Ordner.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` erstellt bei Bedarf auch übergeordnete Verzeichnisse (verschachtelte Pfade)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jedes erstellte Verzeichnis eine Meldung an<br />
>**Beispiel:** `mkdir -p projekt/quellcode`

### rmdir
>**Funktion:** Leeres Verzeichnis löschen | Extern<br />
>**Syntax:** `rmdir [optionen] <verzeichnis>...`<br />
>**Erklärung:** Löscht ein Verzeichnis, aber nur wenn es leer ist.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` entfernt auch übergeordnete Verzeichnisse, sofern diese leer sind<br />
>**Beispiel:** `rmdir alter_ordner`<br />
>**Hinweis:** Für nicht leere Verzeichnisse `rm -r` verwenden.

### cp
>**Funktion:** Dateien oder Ordner kopieren | Extern<br />
>**Syntax:** `cp [optionen] <quelle>... <ziel>`<br />
>**Erklärung:** Kopiert Dateien oder Verzeichnisse an einen anderen Ort.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` kopiert rekursiv (für Ordner mit Inhalt)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor dem Überschreiben nach<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt die kopierten Dateien an<br />
>**Beispiel:** `cp datei.txt /home/user/backup/`

### mv
>**Funktion:** Dateien oder Ordner verschieben oder umbenennen | Extern<br />
>**Syntax:** `mv [optionen] <quelle>... <ziel>`<br />
>**Erklärung:** Verschiebt eine Datei bzw. einen Ordner oder benennt sie um.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor dem Überschreiben nach<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt die ausgeführte Aktion an<br />
>**Beispiel:** `mv alt.txt neu.txt`

### rm
>**Funktion:** Dateien löschen | Extern<br />
>**Syntax:** `rm [optionen] <datei>...`<br />
>**Erklärung:** Löscht Dateien oder mit Optionen auch ganze Verzeichnisse.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` löscht rekursiv (Ordner samt Inhalt)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Löschen ohne Nachfrage<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor jedem Löschen nach<br />
>**Beispiel:** `rm datei.txt`<br />
>**Hinweis:** Vorsicht – Gelöschtes landet nicht im Papierkorb. Besonders `rm -rf` ist gefährlich.

### touch
>**Funktion:** Neue Datei erstellen oder Zeitstempel ändern | Extern<br />
>**Syntax:** `touch [optionen] <datei>...`<br />
>**Erklärung:** Erstellt eine leere Datei oder aktualisiert den Zeitstempel einer vorhandenen Datei.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` erstellt keine Datei, wenn sie noch nicht existiert<br />
>**Beispiel:** `touch notizen.txt`

## Dateiinhalt anzeigen

### cat
>**Funktion:** Dateiinhalt anzeigen | Extern<br />
>**Syntax:** `cat [optionen] [<datei>...]`<br />
>**Erklärung:** Gibt den gesamten Inhalt einer Datei direkt im Terminal aus (concatenate).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` nummeriert alle Zeilen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` nummeriert nur nicht leere Zeilen<br />
>**Beispiel:** `cat datei.txt`<br />
>**Hinweis:** Mehrere Dateien lassen sich aneinanderhängen, z. B. `cat teil1.txt teil2.txt`.

### less
>**Funktion:** Datei seitenweise anzeigen | Extern<br />
>**Syntax:** `less [optionen] <datei>...`<br />
>**Erklärung:** Öffnet eine Datei im Lesemodus; man kann vor- und zurückblättern. Beenden mit `q`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-N` zeigt Zeilennummern an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`+F` folgt der Datei live (wie `tail -f`)<br />
>**Beispiel:** `less /var/log/syslog`<br />
>**Hinweis:** Mit `/suchbegriff` lässt sich im Text suchen.

### more
>**Funktion:** Datei seitenweise anzeigen (nur vorwärts) | Extern<br />
>**Syntax:** `more [optionen] <datei>...`<br />
>**Erklärung:** Zeigt eine Datei schrittweise an. Im Gegensatz zu `less` kann nur vorwärts geblättert werden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt eine Bedienhilfe am unteren Rand an<br />
>**Beispiel:** `more datei.txt`<br />
>**Hinweis:** `less` ist moderner und meist die bessere Wahl.

### head
>**Funktion:** Anfang einer Datei anzeigen | Extern<br />
>**Syntax:** `head [optionen] [<datei>...]`<br />
>**Erklärung:** Gibt standardmäßig die ersten 10 Zeilen einer Datei aus.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <z>` zeigt die ersten z Zeilen an (z. B. `-n 20`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <n>` zeigt die ersten n Bytes an<br />
>**Beispiel:** `head -n 20 datei.txt`

### tail
>**Funktion:** Ende einer Datei anzeigen | Extern<br />
>**Syntax:** `tail [optionen] [<datei>...]`<br />
>**Erklärung:** Gibt standardmäßig die letzten 10 Zeilen einer Datei aus.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <z>` zeigt die letzten z Zeilen an (z. B. `-n 20`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` folgt der Datei live und zeigt neue Zeilen sofort an<br />
>**Beispiel:** `tail -f /var/log/syslog`<br />
>**Hinweis:** `-f` ist praktisch, um Logdateien in Echtzeit mitzulesen.

## Text verarbeiten & filtern

### grep
>**Funktion:** Nach Text suchen | Extern<br />
>**Syntax:** `grep [optionen] <muster> [<datei>...]`<br />
>**Erklärung:** Durchsucht Dateien oder Ausgaben nach Zeilen, die zu einem Muster passen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` sucht rekursiv in Verzeichnissen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` zeigt die Zeilennummern der Treffer an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt nur Zeilen, die NICHT passen<br />
>**Beispiel:** `grep -i "fehler" logfile.txt`<br />
>**Hinweis:** Oft mit einer Pipe kombiniert, z. B. `ps aux | grep firefox`.

### cut
>**Funktion:** Spalten oder Felder ausschneiden | Extern<br />
>**Syntax:** `cut [optionen] [<datei>...]`<br />
>**Erklärung:** Schneidet aus jeder Zeile bestimmte Teile heraus, z. B. einzelne Spalten.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <z>` legt das Trennzeichen fest (z. B. `-d ":"`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <n>` wählt das n-te Feld aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <n>` wählt bestimmte Zeichen-Positionen aus<br />
>**Beispiel:** `cut -d ":" -f 1 /etc/passwd`

### sort
>**Funktion:** Zeilen sortieren | Extern<br />
>**Syntax:** `sort [optionen] [<datei>...]`<br />
>**Erklärung:** Sortiert die Zeilen einer Datei oder Eingabe, standardmäßig alphabetisch.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` sortiert numerisch statt alphabetisch<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` kehrt die Reihenfolge um (absteigend)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` entfernt doppelte Zeilen<br />
>**Beispiel:** `sort -n zahlen.txt`

### tr
>**Funktion:** Zeichen ersetzen oder löschen | Extern<br />
>**Syntax:** `tr [optionen] <satz1> [<satz2>]`<br />
>**Erklärung:** Wandelt oder entfernt einzelne Zeichen aus einem Eingabestrom (translate).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` löscht die angegebenen Zeichen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` fasst wiederholte Zeichen zu einem zusammen<br />
>**Beispiel:** `echo "hallo" | tr 'a-z' 'A-Z'`<br />
>**Hinweis:** `tr` liest nur aus der Eingabe (Pipe), nicht direkt aus einer Datei.

### wc
>**Funktion:** Zeilen, Wörter und Zeichen zählen | Extern<br />
>**Syntax:** `wc [optionen] [<datei>...]`<br />
>**Erklärung:** Zählt standardmäßig Zeilen, Wörter und Bytes einer Datei oder Eingabe (word count).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zählt nur die Zeilen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-w` zählt nur die Wörter<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` zählt nur die Bytes<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` zählt nur die Zeichen (bei UTF-8 nicht identisch mit den Bytes)<br />
>**Beispiel:** `wc -l datei.txt`

## Rechte & Benutzer

### chgrp
>**Funktion:** Gruppe einer Datei ändern | Extern<br />
>**Syntax:** `chgrp [optionen] <gruppe> <datei>...`<br />
>**Erklärung:** Ändert die Gruppenzugehörigkeit einer Datei oder eines Verzeichnisses (change group).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R` ändert die Gruppe rekursiv für alle Unterordner und Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jede Datei die vorgenommene Änderung an<br />
>**Beispiel:** `sudo chgrp entwickler projekt/`<br />
>**Hinweis:** Verwandt mit `chown`, das auch `besitzer:gruppe` setzen kann. Meist mit `sudo` nötig.

### chmod
>**Funktion:** Zugriffsrechte ändern | Extern<br />
>**Syntax:** `chmod [optionen] <modus> <datei>...`<br />
>**Erklärung:** Ändert die Berechtigungen (Lesen, Schreiben, Ausführen) einer Datei oder eines Ordners.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R` ändert die Rechte rekursiv für alle Unterordner und Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jede Datei die vorgenommene Änderung an<br />
>**Beispiel:** `chmod 755 script.sh`<br />
>**Hinweis:** Rechte als Zahl: `4` = lesen, `2` = schreiben, `1` = ausführen. `755` = Besitzer alles (7), Gruppe und andere lesen + ausführen (5).

### chown
>**Funktion:** Besitzer ändern | Extern<br />
>**Syntax:** `chown [optionen] <besitzer>[:<gruppe>] <datei>...`<br />
>**Erklärung:** Ändert den Eigentümer und optional die Gruppe einer Datei oder eines Ordners.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R` ändert den Besitzer rekursiv für alle Unterordner und Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jede Datei die vorgenommene Änderung an<br />
>**Beispiel:** `chown user:user datei.txt`<br />
>**Hinweis:** Die Angabe `besitzer:gruppe` setzt beides; nur `besitzer` lässt die Gruppe unverändert. Meist mit `sudo` nötig.

### groupadd
>**Funktion:** Gruppe anlegen | Extern<br />
>**Syntax:** `groupadd [optionen] <gruppe>`<br />
>**Erklärung:** Legt eine neue Benutzergruppe im System an (Low-Level-Werkzeug).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-g` legt die Gruppen-ID (GID) fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` legt eine Systemgruppe an<br />
>**Beispiel:** `sudo groupadd entwickler`<br />
>**Hinweis:** Meist mit `sudo` nötig. Auf Debian/Ubuntu ist `addgroup` die komfortablere Alternative.

### groupdel
>**Funktion:** Gruppe löschen | Extern<br />
>**Syntax:** `groupdel [optionen] <gruppe>`<br />
>**Erklärung:** Entfernt eine bestehende Benutzergruppe aus dem System.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Löschen, auch wenn es die primäre Gruppe eines Benutzers ist (force)<br />
>**Beispiel:** `sudo groupdel entwickler`<br />
>**Hinweis:** Meist mit `sudo` nötig. Die primäre Gruppe eines bestehenden Benutzers lässt sich normalerweise nicht löschen. Gegenstück: `groupadd`.

### groups
>**Funktion:** Gruppenzugehörigkeit anzeigen | Extern<br />
>**Syntax:** `groups [<benutzer>...]`<br />
>**Erklärung:** Zeigt alle Gruppen an, denen der aktuelle oder ein angegebener Benutzer angehört.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;(keine gebräuchlichen Optionen; erwartet optional einen Benutzernamen)<br />
>**Beispiel:** `groups user`<br />
>**Hinweis:** Ohne Argument werden die Gruppen des aktuell angemeldeten Benutzers angezeigt. Detailliertere Angaben liefert `id`.

### id
>**Funktion:** Benutzer- und Gruppen-IDs anzeigen | Extern<br />
>**Syntax:** `id [optionen] [<benutzer>]`<br />
>**Erklärung:** Zeigt die UID, GID und die Gruppenzugehörigkeit des aktuellen oder eines angegebenen Benutzers an.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` zeigt nur die Benutzer-ID (UID) an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-g` zeigt nur die primäre Gruppen-ID (GID) an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` zeigt Namen statt Nummern an (mit `-u`/`-g`/`-G`)<br />
>**Beispiel:** `id user`<br />
>**Hinweis:** Ohne Argument werden die Angaben zum aktuell angemeldeten Benutzer ausgegeben.

### passwd
>**Funktion:** Passwort ändern | Extern<br />
>**Syntax:** `passwd [optionen] [<benutzer>]`<br />
>**Erklärung:** Ändert das Passwort des eigenen Benutzers oder (als Administrator) eines anderen Benutzers.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` sperrt ein Benutzerkonto (lock)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` entsperrt ein Benutzerkonto (unlock)<br />
>**Beispiel:** `passwd`<br />
>**Hinweis:** Ohne Angabe wird das eigene Passwort geändert. Für andere Benutzer, z. B. `sudo passwd max`, sind Administratorrechte nötig.

### su
>**Funktion:** Benutzer wechseln | Extern<br />
>**Syntax:** `su [optionen] [<benutzer>]`<br />
>**Erklärung:** Startet eine Shell als anderer Benutzer, standardmäßig als `root` (substitute user).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-` (bzw. `-l`) startet eine Login-Shell mit der Umgebung des Zielbenutzers<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` führt einen einzelnen Befehl als Zielbenutzer aus<br />
>**Beispiel:** `su - user`<br />
>**Hinweis:** Es wird das Passwort des Zielbenutzers abgefragt (anders als bei `sudo`). Mit `exit` kehrt man zur vorherigen Sitzung zurück.

### sudo
>**Funktion:** Befehl mit Administratorrechten ausführen | Extern<br />
>**Syntax:** `sudo [optionen] <befehl>`<br />
>**Erklärung:** Führt einen einzelnen Befehl mit den Rechten eines anderen Benutzers aus, standardmäßig als `root` (superuser do).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` führt den Befehl als angegebener Benutzer statt als root aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` startet eine Login-Shell als Zielbenutzer<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zeigt die erlaubten Befehle des aktuellen Benutzers an<br />
>**Beispiel:** `sudo apt update`<br />
>**Hinweis:** Es wird das eigene Passwort abgefragt; der Benutzer muss in der Gruppe `sudo` bzw. `wheel` sein.

### useradd
>**Funktion:** Benutzer anlegen | Extern<br />
>**Syntax:** `useradd [optionen] <benutzername>`<br />
>**Erklärung:** Legt ein neues Benutzerkonto an (Low-Level-Werkzeug).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` erstellt das Home-Verzeichnis des Benutzers<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` legt die Login-Shell fest (z. B. `/bin/bash`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-G` fügt den Benutzer zusätzlichen Gruppen hinzu<br />
>**Beispiel:** `sudo useradd -m -s /bin/bash user`<br />
>**Hinweis:** Meist mit `sudo` nötig. Das Passwort wird separat mit `passwd` gesetzt. Auf Debian/Ubuntu ist `adduser` die komfortablere, interaktive Alternative.

### adduser
>**Funktion:** Benutzer anlegen (Debian/Ubuntu) | Extern<br />
>**Syntax:** `adduser [optionen] <benutzer> [<gruppe>]`<br />
>**Erklärung:** Komfortables, interaktives Werkzeug zum Anlegen eines Benutzers oder zum Hinzufügen zu einer Gruppe; ein Perl-Skript auf Basis von `useradd`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--ingroup` legt die primäre Gruppe des neuen Benutzers fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--home` legt das Home-Verzeichnis fest<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--system` legt einen Systembenutzer ohne Login an<br />
>**Beispiel:** `sudo adduser user`<br />
>**Hinweis:** Nur auf Debian-basierten Systemen verfügbar. Fragt Passwort und Benutzerdaten interaktiv ab und legt das Home-Verzeichnis automatisch an. Mit zwei Argumenten (`adduser user gruppe`) wird der Benutzer nur zur Gruppe hinzugefügt. Gegenstück: `deluser`; Low-Level-Alternative: `useradd`.

### userdel
>**Funktion:** Benutzer löschen | Extern<br />
>**Syntax:** `userdel [optionen] <benutzername>`<br />
>**Erklärung:** Entfernt ein Benutzerkonto aus dem System (Low-Level-Werkzeug).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` löscht zusätzlich das Home-Verzeichnis und den Mail-Spool (remove)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Löschen, auch wenn der Benutzer noch angemeldet ist (force)<br />
>**Beispiel:** `sudo userdel -r user`<br />
>**Hinweis:** Meist mit `sudo` nötig. Ohne `-r` bleibt das Home-Verzeichnis erhalten.

### deluser
>**Funktion:** Benutzer löschen (Debian/Ubuntu) | Extern<br />
>**Syntax:** `deluser [optionen] <benutzer> [<gruppe>]`<br />
>**Erklärung:** Komfortables Werkzeug zum Entfernen eines Benutzers oder zum Entfernen aus einer Gruppe; ein Perl-Skript auf Basis von `userdel`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--remove-home` löscht das Home-Verzeichnis des Benutzers<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--remove-all-files` löscht alle Dateien des Benutzers im System<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`--backup` legt vor dem Löschen ein Backup der Dateien an<br />
>**Beispiel:** `sudo deluser user sudo`<br />
>**Hinweis:** Nur auf Debian-basierten Systemen verfügbar. Mit zwei Argumenten (`deluser user gruppe`) wird der Benutzer nur aus der Gruppe entfernt. Gegenstück: `adduser`.

### w
>**Funktion:** Angemeldete Benutzer und ihre Aktivität anzeigen | Extern<br />
>**Syntax:** `w [optionen] [<benutzer>]`<br />
>**Erklärung:** Zeigt, welche Benutzer angemeldet sind und welchen Prozess sie gerade ausführen, samt System-Auslastung.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` Kurzformat ohne Anmeldezeit und CPU-Angaben (short)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` blendet die Kopfzeile aus (no header)<br />
>**Beispiel:** `w`<br />
>**Hinweis:** Ausführlicher als `who`; `whoami` zeigt dagegen nur den eigenen Benutzernamen.

### who
>**Funktion:** Angemeldete Benutzer anzeigen | Extern<br />
>**Syntax:** `who [optionen]`<br />
>**Erklärung:** Zeigt an, welche Benutzer aktuell am System angemeldet sind, samt Terminal und Anmeldezeit.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt alle verfügbaren Informationen an (all)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` zeigt den Zeitpunkt des letzten Systemstarts an (boot)<br />
>**Beispiel:** `who`<br />
>**Hinweis:** `whoami` zeigt nur den eigenen Benutzernamen; `w` zeigt zusätzlich, was die Benutzer gerade tun.

### whoami
>**Funktion:** Eigenen Benutzernamen anzeigen | Extern<br />
>**Syntax:** `whoami [optionen]`<br />
>**Erklärung:** Gibt den Anmeldenamen des aktuell angemeldeten Benutzers aus (who am i).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;(keine gebräuchlichen Optionen)<br />
>**Beispiel:** `whoami`<br />
>**Hinweis:** Entspricht `id -un`. Nach `sudo -i` bzw. `su` zeigt es den aktuell aktiven Benutzer (z. B. `root`).