[← Home](../index.md)
- [Grundlagen](basics/index.md)
- [Tastenkürzel](shortcuts.md)
- [Befehle](commands/index.md)
- [Wichtige Verzeichnisse](directories.md)

## Begriffe

### GNU - GNU’s Not Unix.
>`GNU` ist ein freies Softwareprojekt, das viele wichtige Werkzeuge für Linux bereitstellt, zum Beispiel Shell-Programme, Compiler und Systemwerkzeuge.

### CLI - Command Line Interface.
>`CLI` ist eine textbasierte Benutzeroberfläche, über die der Benutzer Befehle direkt in einer Konsole oder in einem Terminal eingibt.

### init - initialization.
>`init` ist der erste Prozess, der nach dem Start des Linux-Systems ausgeführt wird und weitere Dienste startet.

### systemd - „system" + Daemon-Suffix `-d`.
>`systemd` ist ein Init-System und Dienstemanager in vielen Linux-Distributionen, der Prozesse und Systemdienste verwaltet. Der Name folgt der Unix-Konvention, Daemons mit `-d` zu benennen; die verbreitete Auflösung „System Daemon" ist inoffiziell. Geschrieben wird er stets klein.

### udev - Userspace-Geräteverwaltung (von „userspace /dev").
>`udev` ist der Geräte-Manager des Linux-Kernels: Er erstellt und entfernt die Gerätedateien in `/dev` dynamisch beim An- und Abstecken von Hardware und vergibt stabile Namen (z. B. unter `/dev/disk/by-uuid/`). Er läuft als Dienst `systemd-udevd`; die Regeln liegen in `/etc/udev/rules.d/`. Gesteuert wird er mit dem Befehl `udevadm`.

### cron - von griechisch „chronos" (Zeit).
>`cron` ist ein Dienst in Linux, mit dem Befehle oder Skripte zu festgelegten Zeiten automatisch ausgeführt werden. Der Name ist keine Abkürzung, sondern leitet sich vom griechischen Wort *chronos* (Zeit) ab.

### ELF - Executable and Linkable Format.
>`ELF` ist ein Dateiformat für ausführbare Programme, Bibliotheken und Objektdateien unter Linux.

### POSIX - Portable Operating System Interface.
>`POSIX` ist ein Standard, der festlegt, wie sich Unix-ähnliche Betriebssysteme und Programme verhalten sollen.

### FHS - Filesystem Hierarchy Standard.
>`FHS` ist ein Standard, der die Struktur von Verzeichnissen in Linux-Systemen beschreibt, zum Beispiel `/etc`, `/home` oder `/var`.

### SSH - Secure Shell.
>`SSH` ist ein Protokoll, mit dem man sich sicher per Netzwerk mit einem Linux-System verbinden kann.

### Bash - Bourne Again Shell.
>`Bash` ist eine weit verbreitete Kommandozeilen-Shell unter Linux, mit der Befehle eingegeben und Skripte ausgeführt werden.

### TTY - Teletypewriter.
>`TTY` ist eine textbasierte Konsole in Linux, über die Befehle ohne grafische Oberfläche eingegeben werden können.

### X11 - X Window System Version 11.
>`X11` ist ein Fenstersystem für grafische Linux-Oberflächen, das die Darstellung und Eingabe verwaltet.

### Wayland - benannt nach der Stadt Wayland (Massachusetts).
>`Wayland` ist ein modernes Protokoll für grafische Oberflächen unter Linux, das als Nachfolger von X11 gilt. Der Name ist keine Abkürzung, sondern geht auf die Heimatstadt des Entwicklers zurück.

### KDE - ursprünglich K Desktop Environment.
>`KDE` ist eine grafische Desktop-Umgebung für Linux mit vielen Programmen und umfangreichen Einstellungsmöglichkeiten. `KDE` bezeichnet heute vor allem die Community und das Software-Ökosystem. Die moderne Desktop-Umgebung heißt KDE Plasma.

### GNOME - GNU Network Object Model Environment.
>`GNOME` ist eine weit verbreitete grafische Desktop-Umgebung für Linux mit einer modernen und übersichtlichen Benutzeroberfläche.