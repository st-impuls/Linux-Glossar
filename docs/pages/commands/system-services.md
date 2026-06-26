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
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [Shell & Skripte](shell-scripting.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# System & Dienste

### df
>**Funktion:** Belegung der Dateisysteme anzeigen<br />
>**Syntax:** `df [optionen] [<datei|mountpoint>...]`<br />
>**Erklärung:** Zeigt den belegten und freien Speicherplatz der eingehängten Dateisysteme (disk free).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` lesbare Größen (KB/MB/GB)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-T` zeigt den Dateisystemtyp an<br />
>**Beispiel:** `df -h`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h`, `--human-readable` | lesbare Größen (z. B. `1G`, `512M`) |
| `-H`, `--si` | wie `-h`, aber Basis 1000 statt 1024 |
| `-T`, `--print-type` | zeigt den Dateisystemtyp an |
| `-i`, `--inodes` | zeigt die Inode-Belegung statt der Bytes |
| `-a`, `--all` | zeigt auch Pseudo-Dateisysteme an |
| `-t <typ>`, `--type=<typ>` | nur Dateisysteme dieses Typs |
| `-x <typ>`, `--exclude-type=<typ>` | schließt einen Typ aus |
| `-B <größe>`, `--block-size=<größe>` | nutzt eine feste Blockgröße (z. B. `-BM`) |
| `-l`, `--local` | beschränkt auf lokale Dateisysteme |
| `-P`, `--portability` | POSIX-kompatibles Ausgabeformat |
| `--total` | fügt eine Zeile mit der Gesamtsumme hinzu |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
df -h          # Belegung aller Dateisysteme, lesbar
df -h /        # nur das Wurzel-Dateisystem
df -hT         # mit Dateisystemtyp
df -i          # Inode-Belegung
df -h /home    # Dateisystem, auf dem /home liegt
```

</details>

>**Hinweis:** `df` zeigt den Platz pro Dateisystem; für den Verbrauch einzelner Verzeichnisse `du` verwenden.

---

### du
>**Funktion:** Speicherverbrauch von Dateien und Verzeichnissen anzeigen<br />
>**Syntax:** `du [optionen] [<pfad>...]`<br />
>**Erklärung:** Berechnet, wie viel Speicherplatz Dateien und Verzeichnisse belegen (disk usage).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` lesbare Größen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` nur die Gesamtsumme je Argument<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-d <n>` begrenzt die Tiefe der Auflistung<br />
>**Beispiel:** `du -sh /var/log`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h`, `--human-readable` | lesbare Größen |
| `-s`, `--summarize` | nur die Gesamtsumme je Argument |
| `-d <n>`, `--max-depth=<n>` | listet nur n Verzeichnisebenen tief auf |
| `-a`, `--all` | zählt auch einzelne Dateien, nicht nur Verzeichnisse |
| `-c`, `--total` | gibt am Ende eine Gesamtsumme aus |
| `-x`, `--one-file-system` | bleibt auf einem Dateisystem |
| `--exclude=<muster>` | schließt passende Einträge aus |
| `-B <größe>`, `--block-size=<größe>` | nutzt eine feste Blockgröße |
| `--apparent-size` | zählt die tatsächliche Dateigröße statt belegter Blöcke |
| `-L`, `--dereference` | folgt symbolischen Links |
| `-S`, `--separate-dirs` | rechnet Unterverzeichnisse nicht zum übergeordneten dazu |
| `--time` | zeigt zusätzlich den Zeitstempel der letzten Änderung |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
du -sh /var/log            # Gesamtgröße eines Verzeichnisses
du -h --max-depth=1 /var   # Größe je Unterverzeichnis von /var
du -sh *                   # Größe jedes Eintrags im aktuellen Ordner
du -sh * | sort -h         # nach Größe sortiert
du -ah . | sort -h | tail  # die größten Dateien/Ordner zuletzt
```

</details>

>**Hinweis:** `du -sh *` zeigt schnell die größten Verzeichnisse. Mit `| sort -h` lässt sich die Ausgabe nach lesbaren Größen sortieren. Für freien Platz pro Dateisystem `df` verwenden.

---

### free
>**Funktion:** Belegung des Arbeitsspeichers anzeigen<br />
>**Syntax:** `free [optionen]`<br />
>**Erklärung:** Zeigt belegten und freien Arbeitsspeicher (RAM) sowie den Auslagerungsspeicher (Swap).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h` lesbare Größen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s <n>` aktualisiert alle n Sekunden<br />
>**Beispiel:** `free -h`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h`, `--human` | lesbare Größen |
| `-m` / `-g` | Ausgabe in Mebibyte / Gibibyte |
| `-s <n>`, `--seconds <n>` | wiederholt die Ausgabe alle n Sekunden |
| `-t`, `--total` | zeigt eine Zeile mit der Gesamtsumme |
| `-w`, `--wide` | breite Ausgabe (trennt buffers und cache) |
| `-b` / `-k` | Ausgabe in Byte / Kibibyte |
| `--si` | nutzt Zehnerpotenzen (KB, MB) statt 1024er-Schritten |
| `-l`, `--lohi` | zeigt hohen und niedrigen Speicher getrennt an |
| `-c <n>`, `--count <n>` | wiederholt die Ausgabe n-mal (mit `-s`) |
| `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
free -h        # Speicherbelegung, lesbar
free -h -s 2   # alle 2 Sekunden aktualisieren (Abbruch mit Ctrl + C)
free -m        # Ausgabe in MiB
free -ht       # mit Gesamtsumme
```

</details>

>**Hinweis:** Die Spalte „available" gibt an, wie viel Speicher tatsächlich noch für Programme verfügbar ist – aussagekräftiger als „free", da Linux ungenutzten RAM als Cache verwendet.

---

### hostnamectl
>**Funktion:** Rechnernamen und Systeminformationen anzeigen/setzen<br />
>**Syntax:** `hostnamectl [optionen] [<unterbefehl>]`<br />
>**Erklärung:** Zeigt den Hostnamen und grundlegende Systemdaten (OS, Kernel, Architektur) und kann den Hostnamen ändern.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;ohne Argument: zeigt alle Informationen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`set-hostname <name>` setzt den Hostnamen<br />
>**Beispiel:** `hostnamectl`

<details markdown>
<summary>Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `status` | zeigt alle Informationen (Standard ohne Argument) |
| `set-hostname <name>` | setzt den (statischen) Hostnamen |
| `set-icon-name <name>` | setzt das Symbol für den Rechner |
| `set-chassis <typ>` | setzt den Gerätetyp (z. B. `laptop`, `server`) |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--static` / `--transient` / `--pretty` | wählt, welcher Hostname gesetzt bzw. gezeigt wird |
| `-H <host>`, `--host=<host>` | führt den Befehl auf einem entfernten Host aus |
| `--no-ask-password` | fragt nicht nach einem Passwort |
| `-h`, `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
hostnamectl                         # alle Informationen anzeigen
hostnamectl status                  # ausführlicher Status
sudo hostnamectl set-hostname pc1   # Hostnamen dauerhaft ändern
```

</details>

>**Hinweis:** Teil von systemd. Den reinen Hostnamen zeigt auch der klassische Befehl `hostname`. Änderungen mit `set-hostname` sind dauerhaft – anders als `hostname <name>`, das nur bis zum Neustart gilt.

---

### journalctl
>**Funktion:** Systemd-Protokoll (Journal) anzeigen<br />
>**Syntax:** `journalctl [optionen]`<br />
>**Erklärung:** Zeigt die Logs, die der systemd-Journaldienst sammelt – vom Systemstart, von Diensten und vom Kernel.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-u <dienst>` nur Logs eines Dienstes<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` folgt dem Log live<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-b` nur Logs des aktuellen Bootvorgangs<br />
>**Beispiel:** `journalctl -u ssh -f`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-u <dienst>`, `--unit=<dienst>` | nur Logs einer bestimmten Unit/eines Dienstes |
| `-f`, `--follow` | folgt dem Log live (wie `tail -f`) |
| `-b [<n>]`, `--boot` | Logs des aktuellen (oder n-ten) Bootvorgangs |
| `-e`, `--pager-end` | springt ans Ende des Logs |
| `-r`, `--reverse` | neueste Einträge zuerst |
| `-n <z>`, `--lines=<z>` | nur die letzten z Zeilen |
| `-p <prio>`, `--priority=<prio>` | filtert nach Priorität (z. B. `err`) |
| `--since`, `--until` | Zeitraum, z. B. `--since "today"` |
| `-k`, `--dmesg` | nur Kernelmeldungen |
| `-x`, `--catalog` | ergänzt Einträge um erklärende Texte |
| `-o <format>`, `--output=<format>` | wählt das Ausgabeformat (z. B. `json`, `short`) |
| `-g <muster>`, `--grep=<muster>` | zeigt nur Einträge, die zum Muster passen |
| `--no-pager` | gibt direkt aus, ohne Pager |
| `--list-boots` | listet die vergangenen Bootvorgänge auf |
| `-h`, `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
journalctl -u ssh                                      # Logs des SSH-Dienstes
journalctl -u ssh -f                                   # live mitlesen
journalctl -b                                          # Logs seit dem letzten Boot
journalctl -p err -b                                   # nur Fehler des aktuellen Boots
journalctl --since "2024-01-01" --until "2024-01-02"   # Zeitraum
journalctl -k                                          # Kernelmeldungen
```

</details>

>**Hinweis:** Braucht oft `sudo`, um alle Logs zu sehen. `journalctl --disk-usage` zeigt den Platzbedarf, `sudo journalctl --vacuum-time=2weeks` räumt alte Logs auf. Logs eines einzelnen Dienstes: `journalctl -u <dienst>`.

---

### mount
>**Funktion:** Dateisysteme einhängen<br />
>**Syntax:** `mount [optionen] <gerät> <mountpoint>`<br />
>**Erklärung:** Bindet ein Dateisystem (z. B. Festplatte oder USB-Stick) an ein Verzeichnis ein. Ohne Argumente zeigt es die aktuell eingehängten Dateisysteme.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t <typ>` Dateisystemtyp (z. B. `ext4`, `vfat`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-o <optionen>` Einhänge-Optionen (z. B. `ro`, `rw`)<br />
>**Beispiel:** `sudo mount /dev/sdb1 /mnt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-t <typ>`, `--types` | Dateisystemtyp (z. B. `ext4`, `vfat`, `ntfs`) |
| `-o <optionen>` | Einhänge-Optionen, kommagetrennt (`ro`, `rw`, `noexec`, `loop`) |
| `-a`, `--all` | hängt alle Einträge aus `/etc/fstab` ein |
| `-r`, `--read-only` | hängt nur lesend ein |
| `-l` | listet eingehängte Dateisysteme mit Labels |
| `-w`, `--rw` | hängt schreibend ein (Standard) |
| `-B`, `--bind` | hängt ein Verzeichnis an einer zweiten Stelle ein |
| `-L <label>` | hängt das Dateisystem mit diesem Label ein |
| `-U <uuid>` | hängt das Dateisystem mit dieser UUID ein |
| `-v`, `--verbose` | ausführliche Ausgabe |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
mount                              # eingehängte Dateisysteme anzeigen
sudo mount /dev/sdb1 /mnt          # Partition einhängen
sudo mount -t vfat /dev/sdb1 /mnt  # mit Dateisystemtyp
sudo mount -o ro /dev/sdb1 /mnt    # nur lesend einhängen
sudo mount -o loop image.iso /mnt  # ISO-Abbild einhängen
```

</details>

>**Hinweis:** Braucht in der Regel `sudo`. Der Mountpoint (Zielverzeichnis) muss existieren. Dauerhafte Einhängungen werden in `/etc/fstab` eingetragen. Zum Aushängen `umount` verwenden.

---

### reboot
>**Funktion:** System neu starten<br />
>**Syntax:** `reboot [optionen]`<br />
>**Erklärung:** Startet den Rechner neu. Entspricht weitgehend `systemctl reboot`.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;ohne Argument: startet sofort neu<br />
>&nbsp;&nbsp;&nbsp;&nbsp;meist mit `sudo` auszuführen<br />
>**Beispiel:** `sudo reboot`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--halt` | hält das System an, statt neu zu starten |
| `-f`, `--force` | erzwingt den Neustart ohne sauberes Herunterfahren |
| `-w`, `--wtmp-only` | schreibt nur den Log-Eintrag, ohne neu zu starten |
| `--poweroff` | schaltet aus, statt neu zu starten |
| `--no-wall` | sendet keine Warnmeldung an angemeldete Benutzer |
| `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo reboot           # sofort neu starten
sudo shutdown -r +5   # Neustart in 5 Minuten
sudo shutdown -r now  # sofortiger Neustart (Alternative)
```

</details>

>**Hinweis:** Braucht `sudo`. Für einen geplanten Neustart eignet sich `shutdown -r +5 "Neustart in 5 Minuten"`. `reboot` entspricht `systemctl reboot`.

---

### service
>**Funktion:** Dienste starten, stoppen und steuern (klassisch)<br />
>**Syntax:** `service <dienst> <aktion>`<br />
>**Erklärung:** Älterer Befehl zum Steuern von Diensten (SysV-Init). Auf systemd-Systemen ein Wrapper, der an `systemctl` weiterreicht.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`service <dienst> start|stop|restart|status`<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`service --status-all` listet alle Dienste<br />
>**Beispiel:** `sudo service ssh restart`

<details markdown>
<summary>Aktionen</summary>

| Aktion | Wirkung |
|---|---|
| `start` | startet den Dienst |
| `stop` | stoppt den Dienst |
| `restart` | startet den Dienst neu |
| `reload` | lädt die Konfiguration neu, ohne zu stoppen |
| `status` | zeigt den Status des Dienstes |
| `--status-all` | listet alle Dienste mit Status |
| `--full-restart` | stoppt und startet den Dienst vollständig neu |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo service ssh start     # Dienst starten
sudo service ssh restart   # Dienst neu starten
service ssh status         # Status anzeigen
service --status-all       # alle Dienste auflisten
```

</details>

>**Hinweis:** Auf modernen Systemen ist `systemctl` der eigentliche Befehl; `service` bleibt aus Kompatibilität bestehen. Anders als `systemctl enable` aktiviert `service` einen Dienst nicht für den Autostart.

---

### shutdown
>**Funktion:** System herunterfahren oder neu starten<br />
>**Syntax:** `shutdown [optionen] [<zeit>] [<nachricht>]`<br />
>**Erklärung:** Fährt das System geordnet herunter oder startet es neu – sofort oder zu einer geplanten Zeit.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-h now` sofort herunterfahren<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` neu starten<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` geplante Abschaltung abbrechen<br />
>**Beispiel:** `sudo shutdown -h now`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h`, `--poweroff` | fährt herunter und schaltet aus |
| `-r`, `--reboot` | startet neu |
| `-c` | bricht eine geplante Abschaltung ab |
| `-k` | sendet nur die Warnmeldung, ohne herunterzufahren |
| `-H`, `--halt` | hält das System an (ohne den Strom abzuschalten) |
| `-P`, `--poweroff` | schaltet aus (wie `-h`) |
| `--no-wall` | sendet keine Warnmeldung |
| `--help` | zeigt die Hilfe an |
| `<zeit>` | `now`, `+<minuten>` oder `hh:mm` |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo shutdown -h now            # sofort herunterfahren
sudo shutdown -r now            # sofort neu starten
sudo shutdown -h +10 "Wartung"  # in 10 Minuten, mit Nachricht an alle
sudo shutdown 22:00             # um 22:00 Uhr herunterfahren
sudo shutdown -c                # geplante Abschaltung abbrechen
```

</details>

>**Hinweis:** Braucht `sudo`. Ohne Zeitangabe plant `shutdown` standardmäßig +1 Minute. Verwandt: `poweroff`, `halt`, `reboot` sowie `systemctl poweroff`/`reboot`.

---

### systemctl
>**Funktion:** Systemd-Dienste und das System steuern<br />
>**Syntax:** `systemctl [optionen] <unterbefehl> [<dienst>]`<br />
>**Erklärung:** Zentrales Werkzeug von systemd zum Starten, Stoppen, Aktivieren und Abfragen von Diensten (Units) sowie zum Steuern des Systemzustands.<br />
>**Unterbefehle:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`start|stop|restart <dienst>` steuert einen Dienst<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`enable|disable <dienst>` (de)aktiviert den Autostart<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`status <dienst>` zeigt den Status<br />
>**Beispiel:** `sudo systemctl restart ssh`

<details markdown>
<summary>Alle Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `start <dienst>` | startet einen Dienst |
| `stop <dienst>` | stoppt einen Dienst |
| `restart <dienst>` | startet einen Dienst neu |
| `reload <dienst>` | lädt die Konfiguration neu |
| `status <dienst>` | zeigt Status und letzte Logzeilen |
| `enable <dienst>` | aktiviert den Autostart beim Booten |
| `disable <dienst>` | deaktiviert den Autostart |
| `enable --now <dienst>` | aktiviert und startet sofort |
| `is-active <dienst>` | prüft, ob ein Dienst läuft |
| `is-enabled <dienst>` | prüft, ob der Autostart aktiv ist |
| `list-units --type=service` | listet aktive Dienste auf |
| `daemon-reload` | lädt geänderte Unit-Dateien neu ein |
| `reload-or-restart <dienst>` | lädt neu oder startet neu, je nach Möglichkeit |
| `mask <dienst>` / `unmask <dienst>` | sperrt bzw. entsperrt einen Dienst vollständig |
| `list-unit-files` | listet alle Unit-Dateien mit ihrem Status |
| `cat <dienst>` | zeigt die Unit-Datei eines Dienstes |
| `reboot` / `poweroff` | startet neu / schaltet aus |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--now` | mit `enable`/`disable`: den Dienst sofort starten/stoppen |
| `--type=<typ>` | filtert nach Unit-Typ (z. B. `service`) |
| `--state=<zustand>` | filtert nach Zustand (z. B. `running`) |
| `--all` | zeigt auch inaktive Units an |
| `--user` | wirkt auf die Benutzer-Instanz von systemd |
| `--no-pager` | gibt direkt aus, ohne Pager |
| `-h`, `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo systemctl start ssh                              # Dienst starten
sudo systemctl enable --now ssh                       # Autostart aktivieren und sofort starten
systemctl status ssh                                  # Status und letzte Logs
systemctl is-enabled ssh                              # Autostart aktiv?
systemctl list-units --type=service --state=running   # laufende Dienste
sudo systemctl daemon-reload                          # nach Änderung an Unit-Dateien
```

</details>

>**Hinweis:** `start`/`stop` wirken sofort, aber nicht über einen Neustart hinaus – für den Autostart zusätzlich `enable`. Die Logs eines Dienstes zeigt `journalctl -u <dienst>`.

---

### timedatectl
>**Funktion:** Datum, Uhrzeit und Zeitzone anzeigen/setzen<br />
>**Syntax:** `timedatectl [optionen] [<unterbefehl>]`<br />
>**Erklärung:** Zeigt und ändert die Systemzeit, die Zeitzone und die automatische Zeitsynchronisation (NTP).<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;ohne Argument: zeigt den aktuellen Status<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`set-timezone <zone>` setzt die Zeitzone<br />
>**Beispiel:** `timedatectl`

<details markdown>
<summary>Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `status` | zeigt Zeit, Zeitzone und NTP-Status (Standard) |
| `set-time "<JJJJ-MM-TT hh:mm:ss>"` | setzt Datum und Uhrzeit manuell |
| `set-timezone <zone>` | setzt die Zeitzone (z. B. `Europe/Berlin`) |
| `list-timezones` | listet alle Zeitzonen auf |
| `set-ntp true\|false` | (de)aktiviert die automatische Zeitsynchronisation |
| `timesync-status` | zeigt den Status der Zeitsynchronisation |
| `show` | zeigt die Eigenschaften maschinenlesbar an |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-H <host>`, `--host=<host>` | führt den Befehl auf einem entfernten Host aus |
| `--no-ask-password` | fragt nicht nach einem Passwort |
| `-h`, `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
timedatectl                                   # aktuellen Status anzeigen
timedatectl list-timezones                    # verfügbare Zeitzonen
sudo timedatectl set-timezone Europe/Berlin   # Zeitzone setzen
sudo timedatectl set-ntp true                 # Zeitsynchronisation aktivieren
```

</details>

>**Hinweis:** Teil von systemd. Bei aktivem NTP (`set-ntp true`) wird die Uhr automatisch synchronisiert; `set-time` ist dann gesperrt. Die reine Uhrzeit zeigt auch `date`.

---

### umount
>**Funktion:** Dateisysteme aushängen<br />
>**Syntax:** `umount [optionen] <gerät|mountpoint>`<br />
>**Erklärung:** Hängt ein zuvor eingehängtes Dateisystem wieder aus. Erst danach lässt sich z. B. ein USB-Stick sicher entfernen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` verzögertes Aushängen (lazy)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f` erzwingt das Aushängen<br />
>**Beispiel:** `sudo umount /mnt`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-l`, `--lazy` | hängt aus, sobald das Dateisystem nicht mehr benutzt wird |
| `-f`, `--force` | erzwingt das Aushängen (z. B. bei nicht erreichbarem Netzlaufwerk) |
| `-a`, `--all` | hängt alle Dateisysteme aus `/etc/fstab` aus |
| `-R`, `--recursive` | hängt verschachtelte Mountpoints aus |
| `-r`, `--read-only` | hängt bei Fehler stattdessen nur lesend wieder ein |
| `-v`, `--verbose` | ausführliche Ausgabe |
| `-t <typ>`, `--types <typ>` | beschränkt auf bestimmte Dateisystemtypen |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo umount /mnt        # über den Mountpoint aushängen
sudo umount /dev/sdb1   # über das Gerät aushängen
sudo umount -l /mnt     # verzögert aushängen (wenn noch in Benutzung)
```

</details>

>**Hinweis:** Der Befehl heißt `umount` (ohne „n"!). „target is busy" bedeutet, dass noch ein Prozess das Dateisystem nutzt – mit `lsof +D /mnt` oder `fuser -m /mnt` lässt sich der Verursacher finden.

---

### uname
>**Funktion:** Systeminformationen anzeigen<br />
>**Syntax:** `uname [optionen]`<br />
>**Erklärung:** Gibt Informationen über das System und den Kernel aus (Kernelname, Version, Architektur).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt alle Informationen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` zeigt die Kernelversion<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m` zeigt die Architektur<br />
>**Beispiel:** `uname -a`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-a`, `--all` | alle Informationen |
| `-s`, `--kernel-name` | Kernelname (Standard, z. B. `Linux`) |
| `-r`, `--kernel-release` | Kernel-Release (Version) |
| `-v`, `--kernel-version` | Kernel-Build-Version |
| `-m`, `--machine` | Maschinen-/Architekturtyp (z. B. `x86_64`) |
| `-n`, `--nodename` | Netzwerk-Hostname |
| `-o`, `--operating-system` | Betriebssystem (z. B. `GNU/Linux`) |
| `-p`, `--processor` | Prozessortyp (oft „unknown") |
| `-i`, `--hardware-platform` | Hardware-Plattform (oft „unknown") |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
uname -a   # alle Informationen
uname -r   # nur die Kernelversion
uname -m   # Architektur (z. B. x86_64)
```

</details>

>**Hinweis:** Für Details zur Distribution (Name, Version) eher `lsb_release -a` oder `cat /etc/os-release` verwenden – `uname` betrifft vor allem den Kernel.

---

### uptime
>**Funktion:** Laufzeit und Systemlast anzeigen<br />
>**Syntax:** `uptime [optionen]`<br />
>**Erklärung:** Zeigt, wie lange das System bereits läuft, wie viele Benutzer angemeldet sind und die durchschnittliche Systemlast (load average).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` Laufzeit in lesbarer Form<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-s` Zeitpunkt des Systemstarts<br />
>**Beispiel:** `uptime`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-p`, `--pretty` | Laufzeit in lesbarer Form (z. B. „up 3 hours, 5 minutes") |
| `-s`, `--since` | Zeitpunkt des letzten Systemstarts |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
uptime      # Laufzeit, Benutzer und Load Average
uptime -p   # nur die Laufzeit, lesbar
uptime -s   # Zeitpunkt des Systemstarts
```

</details>

>**Hinweis:** Die drei Load-Average-Werte stehen für die durchschnittliche Last der letzten 1, 5 und 15 Minuten. Als grobe Faustregel sind Werte um die Anzahl der CPU-Kerne unkritisch.

---
