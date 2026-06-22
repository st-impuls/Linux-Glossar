[← Zurück zur Übersicht](../index.md)

# Befehle

## Navigation

### cd
>**Funktion:** Verzeichnis wechseln<br />
>**Erklärung:** Wechselt in ein anderes Verzeichnis (change directory).<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd ..` wechselt in das übergeordnete Verzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd ~` wechselt in das Home-Verzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd -` wechselt in das zuletzt genutzte Verzeichnis<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`cd /` wechselt in das Wurzelverzeichnis (root)<br />
>**Beispiel:** `cd /home/user`

### ls
>**Funktion:** Dateien und Ordner anzeigen<br />
>**Erklärung:** Listet den Inhalt eines Verzeichnisses auf.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` Langformat mit Details (Rechte, Besitzer, Größe, Datum)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt auch versteckte Dateien (beginnend mit `.`)<br />
>**Beispiel:** `ls -la`

### pwd
>**Funktion:** Aktuelles Verzeichnis anzeigen<br />
>**Erklärung:** Zeigt den vollständigen Pfad des aktuellen Arbeitsverzeichnisses an (print working directory).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-P` zeigt den physischen Pfad und löst symbolische Links auf<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L` zeigt den logischen Pfad (Standard)<br />
>**Beispiel:** `pwd`

### tree
>**Funktion:** Verzeichnisstruktur als Baum anzeigen<br />
>**Erklärung:** Zeigt Verzeichnisse und Dateien in einer übersichtlichen Baumstruktur an.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L <n>` begrenzt die Anzeige auf n Ebenen Tiefe (z. B. `-L 2` = zwei Ebenen)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt nur Verzeichnisse, keine Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt auch versteckte Dateien (beginnend mit `.`)<br />
>**Beispiel:** `tree -L 2`<br />
>**Hinweis:** `tree` ist oft nicht vorinstalliert und muss ggf. nachinstalliert werden, z. B. mit `sudo apt install tree`.

## Dateien & Verzeichnisse verwalten

### mkdir
>**Funktion:** Neues Verzeichnis erstellen<br />
>**Erklärung:** Erstellt einen oder mehrere neue Ordner.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` erstellt bei Bedarf auch übergeordnete Verzeichnisse (verschachtelte Pfade)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jedes erstellte Verzeichnis eine Meldung an<br />
>**Beispiel:** `mkdir -p projekt/quellcode`

### rmdir
>**Funktion:** Leeres Verzeichnis löschen<br />
>**Erklärung:** Löscht ein Verzeichnis, aber nur wenn es leer ist.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` entfernt auch übergeordnete Verzeichnisse, sofern diese leer sind<br />
>**Beispiel:** `rmdir alter_ordner`<br />
>**Hinweis:** Für nicht leere Verzeichnisse `rm -r` verwenden.

### cp
>**Funktion:** Dateien oder Ordner kopieren<br />
>**Erklärung:** Kopiert Dateien oder Verzeichnisse an einen anderen Ort.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` kopiert rekursiv (für Ordner mit Inhalt)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor dem Überschreiben nach<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt die kopierten Dateien an<br />
>**Beispiel:** `cp datei.txt /home/user/backup/`

### mv
>**Funktion:** Dateien oder Ordner verschieben oder umbenennen<br />
>**Erklärung:** Verschiebt eine Datei bzw. einen Ordner oder benennt sie um.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor dem Überschreiben nach<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt die ausgeführte Aktion an<br />
>**Beispiel:** `mv alt.txt neu.txt`

### rm
>**Funktion:** Dateien löschen<br />
>**Erklärung:** Löscht Dateien oder mit Optionen auch ganze Verzeichnisse.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` löscht rekursiv (Ordner samt Inhalt)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Löschen ohne Nachfrage<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` fragt vor jedem Löschen nach<br />
>**Beispiel:** `rm datei.txt`<br />
>**Hinweis:** Vorsicht – Gelöschtes landet nicht im Papierkorb. Besonders `rm -rf` ist gefährlich.

### touch
>**Funktion:** Neue Datei erstellen oder Zeitstempel ändern<br />
>**Erklärung:** Erstellt eine leere Datei oder aktualisiert den Zeitstempel einer vorhandenen Datei.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` erstellt keine Datei, wenn sie noch nicht existiert<br />
>**Beispiel:** `touch notizen.txt`

## Dateiinhalt anzeigen

### cat
>**Funktion:** Dateiinhalt anzeigen<br />
>**Erklärung:** Gibt den gesamten Inhalt einer Datei direkt im Terminal aus (concatenate).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` nummeriert alle Zeilen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` nummeriert nur nicht leere Zeilen<br />
>**Beispiel:** `cat datei.txt`<br />
>**Hinweis:** Mehrere Dateien lassen sich aneinanderhängen, z. B. `cat teil1.txt teil2.txt`.

### less
>**Funktion:** Datei seitenweise anzeigen<br />
>**Erklärung:** Öffnet eine Datei im Lesemodus; man kann vor- und zurückblättern. Beenden mit `q`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-N` zeigt Zeilennummern an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`+F` folgt der Datei live (wie `tail -f`)<br />
>**Beispiel:** `less /var/log/syslog`<br />
>**Hinweis:** Mit `/suchbegriff` lässt sich im Text suchen.

### more
>**Funktion:** Datei seitenweise anzeigen (nur vorwärts)<br />
>**Erklärung:** Zeigt eine Datei schrittweise an. Im Gegensatz zu `less` kann nur vorwärts geblättert werden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` zeigt eine Bedienhilfe am unteren Rand an<br />
>**Beispiel:** `more datei.txt`<br />
>**Hinweis:** `less` ist moderner und meist die bessere Wahl.

### head
>**Funktion:** Anfang einer Datei anzeigen<br />
>**Erklärung:** Gibt standardmäßig die ersten 10 Zeilen einer Datei aus.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <z>` zeigt die ersten z Zeilen an (z. B. `-n 20`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <n>` zeigt die ersten n Bytes an<br />
>**Beispiel:** `head -n 20 datei.txt`

### tail
>**Funktion:** Ende einer Datei anzeigen<br />
>**Erklärung:** Gibt standardmäßig die letzten 10 Zeilen einer Datei aus.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n <z>` zeigt die letzten z Zeilen an (z. B. `-n 20`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` folgt der Datei live und zeigt neue Zeilen sofort an<br />
>**Beispiel:** `tail -f /var/log/syslog`<br />
>**Hinweis:** `-f` ist praktisch, um Logdateien in Echtzeit mitzulesen.

## Text verarbeiten & filtern

### grep
>**Funktion:** Nach Text suchen<br />
>**Erklärung:** Durchsucht Dateien oder Ausgaben nach Zeilen, die zu einem Muster passen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` sucht rekursiv in Verzeichnissen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` zeigt die Zeilennummern der Treffer an<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt nur Zeilen, die NICHT passen<br />
>**Beispiel:** `grep -i "fehler" logfile.txt`<br />
>**Hinweis:** Oft mit einer Pipe kombiniert, z. B. `ps aux | grep firefox`.

### cut
>**Funktion:** Spalten oder Felder ausschneiden<br />
>**Erklärung:** Schneidet aus jeder Zeile bestimmte Teile heraus, z. B. einzelne Spalten.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <z>` legt das Trennzeichen fest (z. B. `-d ":"`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <n>` wählt das n-te Feld aus<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <n>` wählt bestimmte Zeichen-Positionen aus<br />
>**Beispiel:** `cut -d ":" -f 1 /etc/passwd`

### sort
>**Funktion:** Zeilen sortieren<br />
>**Erklärung:** Sortiert die Zeilen einer Datei oder Eingabe, standardmäßig alphabetisch.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` sortiert numerisch statt alphabetisch<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` kehrt die Reihenfolge um (absteigend)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` entfernt doppelte Zeilen<br />
>**Beispiel:** `sort -n zahlen.txt`

### tr
>**Funktion:** Zeichen ersetzen oder löschen<br />
>**Erklärung:** Wandelt oder entfernt einzelne Zeichen aus einem Eingabestrom (translate).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d` löscht die angegebenen Zeichen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` fasst wiederholte Zeichen zu einem zusammen<br />
>**Beispiel:** `echo "hallo" | tr 'a-z' 'A-Z'`<br />
>**Hinweis:** `tr` liest nur aus der Eingabe (Pipe), nicht direkt aus einer Datei.

### wc
>**Funktion:** Zeilen, Wörter und Zeichen zählen<br />
>**Erklärung:** Zählt standardmäßig Zeilen, Wörter und Bytes einer Datei oder Eingabe (word count).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` zählt nur die Zeilen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-w` zählt nur die Wörter<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` zählt nur die Bytes<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` zählt nur die Zeichen (bei UTF-8 nicht identisch mit den Bytes)<br />
>**Beispiel:** `wc -l datei.txt`

## Suchen & Lokalisieren

### whereis
>**Funktion:** Pfad zu Programm, Manpage oder Quellcode finden<br />
>**Erklärung:** Zeigt an, wo sich die Programmdatei, das Handbuch und ggf. die Quelldateien eines Befehls befinden.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` sucht nur nach der ausführbaren Datei (binary)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` sucht nur nach der Manpage<br />
>**Beispiel:** `whereis ls`<br />
>**Hinweis:** Verwandt sind `which` (zeigt nur den Pfad zum Programm) und `find` (sucht beliebige Dateien).

### locate
>**Funktion:** Dateien schnell über den Namen finden<br />
>**Erklärung:** Sucht Dateien anhand des Namens in einer zuvor angelegten Datenbank und ist dadurch deutlich schneller als `find`.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i` ignoriert Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` zeigt nur die Anzahl der Treffer an<br />
>**Beispiel:** `locate datei.txt`<br />
>**Hinweis:** Die Datenbank wird mit `sudo updatedb` aktualisiert; neue Dateien werden sonst nicht gefunden. `locate` ist oft nicht vorinstalliert (Paket `mlocate` bzw. `plocate`).

### find
>**Funktion:** Dateien und Ordner suchen<br />
>**Erklärung:** Durchsucht ein Verzeichnis (und dessen Unterordner) live nach Dateien anhand von Name, Typ, Größe und weiteren Kriterien.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-name <muster>` sucht nach dem Dateinamen (z. B. `-name "*.txt"`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-iname <muster>` wie `-name`, ignoriert aber Groß- und Kleinschreibung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-type f` sucht nur Dateien, `-type d` nur Verzeichnisse<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-size <n>` sucht nach der Größe (z. B. `+10M` = größer als 10 MB)<br />
>**Beispiel:** `find /home -name "*.txt"`<br />
>**Hinweis:** Das erste Argument ist das Startverzeichnis (`.` für das aktuelle). Anders als `locate` sucht `find` immer aktuell, ist aber langsamer.

## Rechte & Benutzer

### chmod
>**Funktion:** Zugriffsrechte ändern<br />
>**Erklärung:** Ändert die Berechtigungen (Lesen, Schreiben, Ausführen) einer Datei oder eines Ordners.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R` ändert die Rechte rekursiv für alle Unterordner und Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jede Datei die vorgenommene Änderung an<br />
>**Beispiel:** `chmod 755 script.sh`<br />
>**Hinweis:** Rechte als Zahl: `4` = lesen, `2` = schreiben, `1` = ausführen. `755` = Besitzer alles (7), Gruppe und andere lesen + ausführen (5).

### chown
>**Funktion:** Besitzer ändern<br />
>**Erklärung:** Ändert den Eigentümer und optional die Gruppe einer Datei oder eines Ordners.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R` ändert den Besitzer rekursiv für alle Unterordner und Dateien<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-v` zeigt für jede Datei die vorgenommene Änderung an<br />
>**Beispiel:** `chown user:user datei.txt`<br />
>**Hinweis:** Die Angabe `besitzer:gruppe` setzt beides; nur `besitzer` lässt die Gruppe unverändert. Meist mit `sudo` nötig.

### passwd
>**Funktion:** Passwort ändern<br />
>**Erklärung:** Ändert das Passwort des eigenen Benutzers oder (als Administrator) eines anderen Benutzers.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` sperrt ein Benutzerkonto (lock)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u` entsperrt ein Benutzerkonto (unlock)<br />
>**Beispiel:** `passwd`<br />
>**Hinweis:** Ohne Angabe wird das eigene Passwort geändert. Für andere Benutzer, z. B. `sudo passwd max`, sind Administratorrechte nötig.