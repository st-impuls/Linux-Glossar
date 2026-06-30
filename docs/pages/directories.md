[← Home](../index.md)
- [Abkürzungen / Begriffe](abbreviations.md)
- [Tastenkürzel](shortcuts.md)
- [Befehle](commands/index.md)

# Wichtige Verzeichnisse

## Inhalt

- [Einhängepunkte und Dienstdaten](#einhangepunkte-und-dienstdaten)
- [Konfiguration und Benutzer](#konfiguration-und-benutzer)
- [Programme (ausführbare Dateien)](#programme-ausfuhrbare-dateien)
- [Systemstart und Bibliotheken](#systemstart-und-bibliotheken)
- [Veränderliche und temporäre Daten](#veranderliche-und-temporare-daten)
- [Virtuelle Dateisysteme](#virtuelle-dateisysteme)

## Legende

Jedes Verzeichnis wird nach dem gleichen Schema beschrieben:

- **Zweck** – wofür das Verzeichnis grundsätzlich da ist (eine Zeile).
- **Beschreibung** – ausführlichere Erläuterung von Aufgabe und Hintergrund.
- **Inhalt** – typische Dateien oder Unterverzeichnisse, die man dort findet.
- **Art (FHS)** – Einordnung nach dem *Filesystem Hierarchy Standard* (FHS), der die Verzeichnisstruktur von Linux festlegt:
    - **statisch** – ändert sich im Normalbetrieb nicht, nur bei Installation oder Update.
    - **veränderlich** – Inhalt ändert sich laufend während des Betriebs.
    - **virtuell** – liegt nicht auf der Festplatte, sondern wird vom Kernel im Arbeitsspeicher bereitgestellt.
- **Hinweis** – Besonderheiten, Stolperfallen und verwandte Befehle oder Verzeichnisse.

## Einhängepunkte und Dienstdaten

### /media
>**Zweck:** Automatische Einhängepunkte für Wechseldatenträger<br />
>**Beschreibung:** Hier hängt das System Wechseldatenträger wie USB-Sticks, externe Festplatten, SD-Karten und CDs/DVDs automatisch ein, sobald sie angeschlossen werden. Für jeden Datenträger wird dabei selbsttätig ein eigenes Unterverzeichnis angelegt.<br />
>**Inhalt:** automatisch erzeugte Einhängepunkte, meist `/media/<benutzer>/<label>`<br />
>**Art (FHS):** veränderlich<br />
>**Hinweis:** Die Unterverzeichnisse entstehen beim Einhängen automatisch und verschwinden beim Auswerfen wieder – feste, vordefinierte Unterordner gibt es hier nicht. Zum manuellen Einhängen ist `/mnt` gedacht.

---

### /mnt
>**Zweck:** Temporärer Einhängepunkt für den Administrator<br />
>**Beschreibung:** Ein in der Regel leeres Verzeichnis, das als Stelle zum kurzfristigen, manuellen Einhängen eines Dateisystems dient – etwa um schnell eine Partition, ein Abbild oder eine Netzwerkfreigabe zu prüfen.<br />
>**Inhalt:** im Normalfall leer; dient nur als Einhängepunkt<br />
>**Art (FHS):** veränderlich<br />
>**Hinweis:** Feste Unterverzeichnisse sind nicht vorgesehen – der Administrator legt bei Bedarf selbst welche an (z. B. `/mnt/usb`, `/mnt/backup`). Eingehängt wird mit `mount`, ausgehängt mit `umount`. Wechseldatenträger landen dagegen automatisch unter `/media`.

---

### /srv
>**Zweck:** Daten, die das System über Dienste bereitstellt<br />
>**Beschreibung:** Enthält die Daten, die ein Server nach außen anbietet – also die Inhalte von Diensten wie Web- oder FTP-Servern. Der Sinn ist, diese Nutzdaten an einer klar benannten Stelle zu bündeln, getrennt von Programmen und Konfiguration.<br />
>**Inhalt:** dienstbezogene Datenverzeichnisse, z. B. `/srv/www` (Webseiten), `/srv/ftp` (FTP-Dateien)<br />
>**Art (FHS):** veränderlich<br />
>**Hinweis:** Die genaue Unterteilung ist nicht festgelegt – Namen wie `/srv/www` oder `/srv/ftp` sind nur Konventionen und je nach Distribution unterschiedlich. Viele Systeme legen Webseiten stattdessen unter `/var/www` ab.

---

## Konfiguration und Benutzer

### /etc
>**Zweck:** Systemweite Konfigurationsdateien<br />
>**Beschreibung:** Hier liegen die zentralen Einstellungen des Systems – für Dienste, Benutzerkonten, Netzwerk, Paketquellen und vieles mehr. Es handelt sich ausschließlich um Textdateien, die sich mit jedem Editor lesen und (als root) ändern lassen; Programme oder Bibliotheken gehören ausdrücklich nicht hierher.<br />
>**Inhalt:** wichtige Einzeldateien wie `passwd`, `shadow`, `group`, `fstab`, `hostname`, `hosts`, `sudoers` sowie zahlreiche Unterverzeichnisse (neben den unten genannten z. B. `/etc/init.d/`, `/etc/modprobe.d/`, `/etc/profile.d/`)<br />
>**Art (FHS):** statisch<br />
>**Hinweis:** Änderungen meist mit `sudo`; vor dem Bearbeiten eine Sicherungskopie anlegen. Der Name steht historisch für *editable text config*.

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/etc/apt/` | enthält die **Paketquellen** und Einstellungen der Paketverwaltung APT (`sources.list`, `sources.list.d/`) – Debian/Ubuntu. |
| `/etc/cron.d/` | nimmt **zeitgesteuerte Aufträge** (Cronjobs) auf; daneben `cron.daily/`, `cron.weekly/` usw. für regelmäßige Aufgaben. |
| `/etc/default/` | enthält **Standardwerte** für Dienste und Systemskripte (einfache Variablen-Dateien). |
| `/etc/network/` | enthält die **Netzwerkkonfiguration** im klassischen Schema (`interfaces`) – auf manchen Distributionen. |
| `/etc/skel/` | liefert die **Vorlage** für neue Home-Verzeichnisse: ihr Inhalt wird beim Anlegen eines Benutzers kopiert. |
| `/etc/ssh/` | enthält die **Konfiguration** von SSH-Server und -Client (`sshd_config`, `ssh_config`). |
| `/etc/systemd/` | enthält die **Konfiguration von systemd** – Dienste (Units), Timer und Systemeinstellungen. |

</details>

---

### /home
>**Zweck:** Persönliche Verzeichnisse der Benutzer<br />
>**Beschreibung:** Enthält für jeden normalen Benutzer ein eigenes Unterverzeichnis (z. B. `/home/anna`), in dem dessen persönliche Dateien und individuelle Einstellungen liegen. Beim Anmelden landet der Benutzer in seinem Home-Verzeichnis (in der Shell mit `~` abgekürzt).<br />
>**Inhalt:** ein Unterverzeichnis je Benutzer; darin typische Ordner wie `Dokumente`, `Downloads`, `Bilder` und versteckte Konfiguration (`.config/`, `.ssh/`, `.bashrc`)<br />
>**Art (FHS):** veränderlich<br />
>**Hinweis:** Feste Unterverzeichnisse gibt es nicht – die Namen richten sich nach den Benutzerkonten. Die Vorlage für ein neues Home-Verzeichnis steht in `/etc/skel`. Das Home von root liegt nicht hier, sondern in `/root`.

---

### /root
>**Zweck:** Home-Verzeichnis des Administrators (root)<br />
>**Beschreibung:** Das persönliche Verzeichnis des Superusers root. Es liegt bewusst außerhalb von `/home`, damit es auch dann verfügbar ist, wenn `/home` (etwa als eigene Partition) noch nicht eingehängt ist.<br />
>**Inhalt:** persönliche Dateien und Einstellungen von root; im Normalfall überschaubar<br />
>**Art (FHS):** veränderlich<br />
>**Hinweis:** Nicht mit dem Wurzelverzeichnis `/` verwechseln – `/root` ist nur das Home von root. Zugriff in der Regel nur als root bzw. mit `sudo`.

---

## Programme (ausführbare Dateien)

### /bin
>**Zweck:** Grundlegende Befehle für alle Benutzer<br />
>**Beschreibung:** Enthält die wichtigsten ausführbaren Programme (Binaries), die das System und jeder Benutzer im normalen Betrieb braucht – vor allem grundlegende Shell-Befehle. Jeder Befehl ist dabei eine eigene, eigenständige Programmdatei in diesem Verzeichnis (`ls` ist also die Datei `/bin/ls`). Diese Programme müssen schon früh beim Systemstart und auch im Notfall verfügbar sein (etwa zur Reparatur, wenn `/usr` noch nicht eingebunden ist).<br />
>**Inhalt:** je eine ausführbare Datei pro Befehl, z. B. `ls`, `cp`, `mv`, `rm`, `cat`, `echo`, `bash`<br />
>**Art (FHS):** statisch<br />
>**Hinweis:** Auf modernen Systemen ist `/bin` nur noch ein symbolischer Link auf `/usr/bin` („usr-merge"). Für Administrationsbefehle siehe `/sbin`.

---

### /opt
>**Zweck:** Zusätzliche Software von Drittanbietern<br />
>**Beschreibung:** Nimmt Programme auf, die nicht zum Standardumfang der Distribution gehören und nicht über die normale Paketverwaltung nach `/usr` einsortiert werden – typisch für proprietäre oder von Hand installierte Pakete. Jede Anwendung erhält ihr eigenes Unterverzeichnis, in dem alles Nötige (Programme, Bibliotheken, Daten) gebündelt liegt. „opt" steht für *optional*.<br />
>**Inhalt:** ein Unterverzeichnis je Anwendung, z. B. `/opt/google/chrome`, `/opt/<hersteller>/<programm>`<br />
>**Art (FHS):** statisch<br />
>**Hinweis:** Hält Fremdsoftware sauber vom System getrennt. Ähnlich wie `/usr/local`, das aber eher für selbst kompilierte Programme gedacht ist.

---

### /sbin
>**Zweck:** Grundlegende Befehle zur Systemverwaltung<br />
>**Beschreibung:** Enthält ausführbare Programme für die Administration des Systems (system binaries) – etwa zum Partitionieren, Einhängen von Dateisystemen oder Konfigurieren des Netzwerks. Sie sind in erster Linie für den Administrator (root) gedacht und werden, wie die Befehle in `/bin`, schon früh beim Systemstart benötigt.<br />
>**Inhalt:** je eine ausführbare Datei pro Befehl, z. B. `fdisk`, `mount`, `mkfs`, `ip`, `reboot`<br />
>**Art (FHS):** statisch<br />
>**Hinweis:** Liegt bei normalen Benutzern oft nicht im `PATH`; Aufruf daher meist mit `sudo`. Auf modernen Systemen ein symbolischer Link auf `/usr/sbin` („usr-merge"). Gegenstück für allgemeine Befehle: `/bin`.

---

### /usr
>**Zweck:** Programme und Daten für den normalen Betrieb<br />
>**Beschreibung:** `/usr` ist die „zweite Hierarchie" des Linux-Systems und meist der größte Verzeichnisbaum. Es enthält den Großteil der installierten Software – Programme (`/usr/bin`), Bibliotheken (`/usr/lib`), Header-Dateien (`/usr/include`) und architekturunabhängige Daten (`/usr/share`).<br />
>
>Historisch war `/usr` eine separate Partition, die erst nach dem Booten eingehängt wurde – daher die parallele Struktur zu `/` (mit `bin/`, `sbin/`, `lib/`).<br />
>
>In modernen Systemen ist diese Trennung aufgehoben: `/bin`, `/sbin` und `/lib` sind symbolische Links auf ihre Pendants in `/usr`. Der Bootvorgang funktioniert trotzdem, weil die initramfs alle essenziellen Werkzeuge bereits vor dem Einhängen des Root-Dateisystems bereitstellt.<br />
>
>`/usr/local` ist ein eigener Unterbaum für lokal installierte Software, die nicht vom Paketmanager verwaltet wird.<br />
>**Inhalt:** Unterverzeichnisse wie `bin/`, `sbin/`, `lib/`, `share/`, `local/`, `include/`<br />
>**Art (FHS):** statisch<br />
>**Hinweis:** Wird im Betrieb nur lesend gebraucht. Selbst installierte Software gehört nach `/usr/local`, nicht direkt nach `/usr/bin`.

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/usr/bin` | ist die **Hauptsammlung** der ausführbaren Programme für alle Benutzer – nahezu jeder systemweite Befehl liegt hier und wird über die Paketverwaltung installiert. Dank der Variable `$PATH` lassen sich diese Befehle ohne Pfadangabe aufrufen; eigene Skripte gehören dagegen nicht hierher, sondern nach `/usr/local/bin`. |
| `/usr/include` | enthält die **Header-Dateien** (`.h`) von C/C++. Sie beschreiben Funktionen und Schnittstellen der Bibliotheken und werden beim Kompilieren von Programmen aus dem Quellcode benötigt. |
| `/usr/lib` | enthält die **Bibliotheken** (shared libraries) und internen Hilfsprogramme für die Software in `/usr/bin` und `/usr/sbin`. Auf 64-Bit-Systemen kommen `/usr/lib64` bzw. architekturspezifische Ordner wie `/usr/lib/x86_64-linux-gnu` hinzu. |
| `/usr/local` | ist eine **eigene Hierarchie** (mit `bin/`, `sbin/`, `lib/`, `share/`) für selbst kompilierte oder von Hand installierte Software. Sie wird nicht vom Paketmanager angefasst und bleibt bei System-Updates unberührt. |
| `/usr/sbin` | enthält die ausführbaren Programme zur **Systemverwaltung**, die nicht schon zum Booten gebraucht werden (`/sbin` verweist heute hierher). In erster Linie für root gedacht. |
| `/usr/share` | enthält **architekturunabhängige Daten**, die unabhängig vom Prozessortyp gleich sind – etwa Handbuchseiten (`man/`), Dokumentation (`doc/`), Symbole, Übersetzungen und Anwendungsdaten. |

</details>

---

## Systemstart und Bibliotheken

### /boot
>**Zweck:** Dateien zum Starten des Systems<br />
>**Beschreibung:** Enthält alles, was der Rechner zum Booten braucht, bevor das eigentliche System läuft: den Kernel (`vmlinuz`), die initramfs (ein kleines Start-Dateisystem) und den Bootloader. Erst nachdem diese geladen sind, übernimmt der Kernel und bindet das Wurzeldateisystem ein.<br />
>**Inhalt:** `vmlinuz-<version>` (Kernel), `initrd.img-<version>` (initramfs), `config-<version>`, `System.map-<version>` sowie der Bootloader-Ordner<br />
>**Art (FHS):** statisch<br />
>**Hinweis:** Liegt häufig auf einer eigenen Partition. Hier nichts von Hand löschen – ohne gültigen Kernel und Bootloader startet das System nicht mehr. Die laufende Kernel-Version zeigt `uname -r`.

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/boot/efi/` | ist der **Einhängepunkt der EFI-Partition** auf UEFI-Systemen und enthält die Bootloader im `.efi`-Format. |
| `/boot/grub/` | enthält die **Konfiguration des Bootloaders** GRUB (`grub.cfg`) und dessen Module. |

</details>

---

### /lib
>**Zweck:** Bibliotheken und Kernel-Module für den Systembetrieb<br />
>**Beschreibung:** Enthält die geteilten Bibliotheken (shared libraries), die von den Programmen in `/bin` und `/sbin` benötigt werden, sowie die Module des Kernels. Bibliotheken bündeln Funktionen, die mehrere Programme gemeinsam nutzen, statt sie in jedes Programm einzubauen.<br />
>**Inhalt:** Bibliotheksdateien (`*.so`), Kernel-Module und Firmware; auf 64-Bit-Systemen zusätzlich `/lib64`<br />
>**Art (FHS):** statisch<br />
>**Hinweis:** Auf modernen Systemen ein symbolischer Link auf `/usr/lib` („usr-merge"). Die Bibliotheken werden zur Laufzeit dynamisch geladen; gesteuert wird das über den Dynamic Linker (`ld.so`, `ldconfig`).

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/lib/firmware/` | enthält **Firmware-Dateien**, die der Kernel an bestimmte Hardware (z. B. WLAN, Grafik) lädt. |
| `/lib/modules/` | enthält die **Kernel-Module**, getrennt nach Kernel-Version (`/lib/modules/<version>/`). Damit arbeiten `lsmod`, `modprobe` und `modinfo`. |
| `/lib/systemd/` | enthält **ausführbare Teile und Unit-Dateien** von systemd. |

</details>

---

## Veränderliche und temporäre Daten

### /run
>**Zweck:** Laufzeitdaten seit dem Systemstart<br />
>**Beschreibung:** Nimmt Daten auf, die Prozesse während des Betriebs brauchen und die nur bis zum nächsten Neustart gelten – etwa Prozess-IDs (PID-Dateien), Sockets und Statusinformationen von Diensten. Das Verzeichnis liegt im Arbeitsspeicher (tmpfs) und ist beim Hochfahren leer.<br />
>**Inhalt:** PID-Dateien, Sockets und dienstbezogene Laufzeitverzeichnisse<br />
>**Art (FHS):** veränderlich<br />
>**Hinweis:** Liegt im RAM und wird bei jedem Neustart neu aufgebaut – nichts hier ist dauerhaft. Ersetzt das frühere `/var/run`, das heute meist nur noch ein Verweis auf `/run` ist.

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/run/lock/` | enthält **Lock-Dateien**, mit denen Programme den exklusiven Zugriff auf eine Ressource markieren. |
| `/run/systemd/` | enthält den **Laufzeitzustand von systemd** (aktive Units, Sitzungen). |
| `/run/user/<uid>/` | enthält **Laufzeitdaten je angemeldeter Benutzersitzung** (ein Ordner pro Benutzer-ID). |

</details>

---

### /tmp
>**Zweck:** Temporäre Dateien für alle Programme<br />
>**Beschreibung:** Ein allgemein beschreibbarer Ablageort, in dem Programme und Benutzer kurzlebige Dateien zwischenspeichern. Man sollte sich nicht darauf verlassen, dass die Daten dort erhalten bleiben.<br />
>**Inhalt:** kurzlebige Dateien beliebiger Programme; keine feste Struktur<br />
>**Art (FHS):** veränderlich<br />
>**Hinweis:** Wird regelmäßig geleert – oft bei jedem Neustart, manchmal nach einer bestimmten Zeit. Feste Unterverzeichnisse gibt es nicht; jedes Programm legt bei Bedarf eigene an. Daten, die einen Neustart überstehen sollen, gehören nach `/var/tmp`.

---

### /var
>**Zweck:** Veränderliche Daten, die im Betrieb wachsen<br />
>**Beschreibung:** Sammelt all jene Daten, die sich während des Betriebs laufend ändern und größer werden – im Gegensatz zu den eher statischen Programmen und Konfigurationen. Dazu zählen Protokolle, zwischengespeicherte Dateien, Warteschlangen und Datenbanken von Diensten.<br />
>**Inhalt:** Unterverzeichnisse wie `log/`, `cache/`, `lib/`, `spool/`, `www/`, `tmp/`, `mail/`<br />
>**Art (FHS):** veränderlich<br />
>**Hinweis:** Läuft besonders `/var/log` voll, kann das System Probleme bekommen – den Plattenplatz im Blick behalten (`du -sh /var/*`). Die Logs des Journals zeigt `journalctl`.

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/var/cache/` | enthält **zwischengespeicherte Daten** von Programmen, z. B. von `apt` heruntergeladene Pakete. |
| `/var/lib/` | enthält **veränderliche Zustandsdaten** von Programmen (Datenbanken, Paket- und Dienstdaten). |
| `/var/log/` | enthält die **Protokolldateien** des Systems und der Dienste. |
| `/var/spool/` | enthält **Warteschlangen** für Aufträge: Druck, cron und E-Mail. |
| `/var/tmp/` | enthält **temporäre Dateien**, die im Gegensatz zu `/tmp` einen Neustart überdauern. |
| `/var/www/` | enthält die **Inhalte des Webservers** (auf vielen Systemen). |

</details>

---

## Virtuelle Dateisysteme

### /dev
>**Zweck:** Zugriff auf Geräte über Dateien<br />
>**Beschreibung:** Stellt die Geräte des Rechners als Dateien dar – nach dem Linux-Grundsatz „alles ist eine Datei". Programme sprechen Festplatten, Terminals oder Schnittstellen an, indem sie aus den entsprechenden Dateien lesen oder in sie schreiben. Daneben gibt es nützliche Pseudogeräte ohne echte Hardware.<br />
>**Inhalt:** Gerätedateien wie `sda` (Festplatte), `null`, `zero`, `random` sowie gerätebezogene Unterverzeichnisse<br />
>**Art (FHS):** virtuell<br />
>**Hinweis:** Wird beim Start vom Kernel (devtmpfs) automatisch erzeugt und gepflegt – Einträge nicht von Hand anlegen oder löschen. Welche Datenträger es gibt, zeigen `lsblk` und `fdisk -l`.

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/dev/disk/` | enthält **stabile Verweise auf Datenträger** nach UUID, Label oder Pfad (`by-uuid/`, `by-id/`). |
| `/dev/pts/` | enthält die **Pseudoterminals** – die geöffneten Terminal-Sitzungen. |
| `/dev/shm/` | dient als **gemeinsam genutzter Speicher** (shared memory) in Form von Dateien im RAM. |

</details>

---

### /proc
>**Zweck:** Informationen über Prozesse und Kernel als Dateien<br />
>**Beschreibung:** Ein virtuelles Dateisystem, das der Kernel im Arbeitsspeicher bereitstellt. Jeder laufende Prozess erscheint als Verzeichnis mit seiner PID; daneben liefern zahlreiche Dateien Auskunft über den Zustand des Systems. Viele Standardbefehle lesen ihre Informationen von hier.<br />
>**Inhalt:** ein Verzeichnis je Prozess (`<pid>/`) sowie Infodateien wie `cpuinfo`, `meminfo`, `mounts`, `uptime`<br />
>**Art (FHS):** virtuell<br />
>**Hinweis:** Quelle u. a. für `lscpu`, `free` und `lsmod`. Die Dateien nur lesen; Einstellungen unter `/proc/sys` ändert man besser über `sysctl`.

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/proc/<pid>/` | enthält **je Prozess ein Verzeichnis** mit dessen Zustand, Speicher und geöffneten Dateien. |
| `/proc/sys/` | enthält **einstellbare Kernel-Parameter**, die sich über `sysctl` ändern lassen. |
| `/proc/cpuinfo` | **Datei** mit Details zur CPU – Grundlage für `lscpu`. |
| `/proc/meminfo` | **Datei** mit der Speicherbelegung – Grundlage für `free`. |

</details>

---

### /sys
>**Zweck:** Geräte und Kernel ansehen und steuern<br />
>**Beschreibung:** Ein virtuelles Dateisystem (sysfs), über das der Kernel Informationen zur angeschlossenen Hardware und zu sich selbst bereitstellt. Anders als `/proc` ist es klar nach Geräten und Treibern strukturiert; einige Werte lassen sich auch zur Steuerung schreiben.<br />
>**Inhalt:** nach Geräten, Klassen und Modulen geordnete Unterverzeichnisse mit vielen kleinen Attributdateien<br />
>**Art (FHS):** virtuell<br />
>**Hinweis:** Quelle u. a. für `lsblk`. Wird vom Kernel erzeugt; die meisten Dateien nur lesen, einzelne Werte sind für Fachleute auch schreibbar.

<details markdown>
<summary>Wichtige Unterverzeichnisse</summary>

| Verzeichnis | Zweck |
|---|---|
| `/sys/block/` | enthält die **Blockgeräte** (Festplatten, SSDs) und ihre Eigenschaften. |
| `/sys/class/` | gruppiert **Geräte nach Klassen** (z. B. `net/`, `block/`, `power/`). |
| `/sys/devices/` | bildet den **gesamten Gerätebaum** des Systems ab. |

</details>
