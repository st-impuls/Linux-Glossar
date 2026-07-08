[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

## Kategorien
- [Archivierung & Kompression](archiving-compression.md)
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Datenträger & Dateisysteme](disk-filesystems.md)
- [Hilfe & Dokumentation](help-documentation.md)
- [Navigation & Suche](navigation-search.md)
- [Netzwerk & Download](network-download.md)
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Paketverwaltung

**Überblick:** Welches Werkzeug zu welcher Distribution gehört und auf welcher Ebene es arbeitet – *High-Level* (mit Repositories und automatischer Abhängigkeitsauflösung) oder *Low-Level* (einzelne lokale Paketdatei, ohne Abhängigkeiten):

| Distribution | High-Level | Low-Level |
|---|---|---|
| Debian/Ubuntu | `apt` | `dpkg` |
| Fedora/RHEL | `dnf` / `yum` | `rpm` |
| openSUSE | `zypper` | `rpm` |
| Arch | `pacman` | – |

Daneben gibt es distributionsunabhängige Paketformate wie `flatpak` und `snap`.

---

### apt - Advanced Package Tool
>**Funktion:** Pakete installieren, aktualisieren und entfernen (Debian/Ubuntu)<br />
>**Syntax:** `apt [optionen] <unterbefehl> [<paket>...]`<br />
>**Erklärung:** Moderne, einheitliche Oberfläche zur Paketverwaltung auf Debian/Ubuntu. Die Aktionen laufen über Unterbefehle wie `install` oder `update`; schreibende Aktionen brauchen `sudo`.<br />
>**Unterbefehle:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`update` aktualisiert die Paketlisten<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`upgrade` installiert verfügbare Aktualisierungen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`install <paket>` installiert ein Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`remove <paket>` entfernt ein Paket<br />
>**Beispiel:** `sudo apt update && sudo apt upgrade`

<details markdown>
<summary>Alle Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `update` | aktualisiert die Paketlisten aus den Quellen (installiert nichts) |
| `upgrade` | installiert verfügbare Aktualisierungen der installierten Pakete |
| `full-upgrade` | wie `upgrade`, entfernt bei Konflikten auch Pakete |
| `install <paket>` | installiert ein oder mehrere Pakete |
| `reinstall <paket>` | installiert ein Paket erneut |
| `remove <paket>` | entfernt ein Paket (Konfigurationsdateien bleiben) |
| `purge <paket>` | entfernt ein Paket samt Konfigurationsdateien |
| `autoremove` | entfernt nicht mehr benötigte Abhängigkeiten |
| `search <begriff>` | sucht in den Paketquellen nach einem Begriff |
| `show <paket>` | zeigt Informationen zu einem Paket |
| `list --installed` | listet installierte Pakete auf |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-h`, `--help` | zeigt Hilfe zur Verwendung von `apt` an |
| `-v`, `--version` | zeigt die installierte Version von `apt` an |
| `-y`, `--yes`, `--assume-yes` | beantwortet Rückfragen automatisch mit „yes“ |
| `--assume-no` | beantwortet Rückfragen automatisch mit „no“ |
| `-s`, `--simulate` | simuliert den Vorgang, ohne Änderungen am System vorzunehmen |
| `-d`, `--download-only` | lädt Pakete nur herunter, installiert sie aber nicht |
| `-q`, `--quiet` | reduziert die Ausgabe im Terminal |
| `--no-install-recommends` | installiert keine empfohlenen Zusatzpakete |
| `--install-suggests` | installiert auch vorgeschlagene Pakete |
| `--reinstall` | installiert ein bereits installiertes Paket erneut |
| `--only-upgrade` | aktualisiert ein Paket nur, wenn es bereits installiert ist |
| `-f`, `--fix-broken` | versucht, beschädigte Abhängigkeiten zu reparieren |
| `--purge` | entfernt Pakete inklusive Konfigurationsdateien |
| `--autoremove` | entfernt automatisch installierte, nicht mehr benötigte Pakete |
| `--allow-downgrades` | erlaubt das Installieren einer älteren Paketversion |
| `--allow-change-held-packages` | erlaubt Änderungen an zurückgehaltenen Paketen |
| `-o <option>` | setzt eine Konfigurationsoption direkt über die Kommandozeile |
| `-c <datei>` | verwendet eine bestimmte Konfigurationsdatei |
| `-t <release>`, `--target-release` | installiert aus einer bestimmten Veröffentlichung (z. B. Backports) |
| `-V`, `--verbose-versions` | zeigt die vollständigen Versionsnummern an |
| `-m`, `--fix-missing` | ignoriert fehlende Pakete und macht ohne sie weiter |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo apt update                # Paketlisten aktualisieren
sudo apt upgrade               # verfügbare Updates installieren
sudo apt install git           # Paket installieren
sudo apt install -y htop curl  # mehrere Pakete ohne Rückfrage
sudo apt remove firefox        # Paket entfernen
sudo apt purge firefox         # samt Konfigurationsdateien entfernen
sudo apt autoremove            # unnötige Abhängigkeiten aufräumen
apt search bildbearbeitung     # nach Paketen suchen
apt show git                   # Paketdetails anzeigen
```

</details>

>**Hinweis:** Vor dem Installieren immer `sudo apt update` ausführen, damit die Paketlisten aktuell sind. In Skripten ist `apt-get` (stabilere Ausgabe) üblich. `apt` gibt es nur auf Debian-basierten Systemen (Ubuntu, Debian, Mint); andere nutzen `dnf`/`yum` (Fedora/RHEL), `pacman` (Arch) oder `zypper` (openSUSE).

---

### dnf
>**Funktion:** Pakete installieren, aktualisieren und entfernen (Fedora/RHEL)<br />
>**Syntax:** `dnf [optionen] <unterbefehl> [<paket>...]`<br />
>**Erklärung:** Moderne Paketverwaltung der Red-Hat-Familie (Fedora, RHEL, CentOS, Rocky, AlmaLinux). Arbeitet mit `.rpm`-Paketen, löst Abhängigkeiten automatisch auf und läuft über Unterbefehle wie `install` oder `upgrade`. Nachfolger von `yum`.<br />
>**Unterbefehle:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`install <paket>` installiert ein Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`remove <paket>` entfernt ein Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`upgrade` installiert verfügbare Aktualisierungen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`search <begriff>` sucht nach Paketen<br />
>**Beispiel:** `sudo dnf install git`

<details markdown>
<summary>Alle Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `install <paket>` | installiert ein oder mehrere Pakete |
| `remove <paket>` | entfernt ein Paket |
| `upgrade [<paket>]` | aktualisiert alle oder einzelne Pakete |
| `check-update` | prüft auf verfügbare Aktualisierungen (installiert nichts) |
| `search <begriff>` | sucht in den Quellen nach einem Begriff |
| `info <paket>` | zeigt Informationen zu einem Paket |
| `list --installed` | listet installierte Pakete auf |
| `provides <datei>` | findet heraus, welches Paket eine Datei liefert |
| `autoremove` | entfernt nicht mehr benötigte Abhängigkeiten |
| `reinstall <paket>` | installiert ein Paket erneut |
| `downgrade <paket>` | installiert eine ältere Version |
| `groupinstall <gruppe>` | installiert eine Paketgruppe (`grouplist` zeigt alle) |
| `history` | zeigt die Transaktionshistorie (rückgängig mit `history undo`) |
| `repolist` | listet die konfigurierten Paketquellen auf |
| `clean all` | leert den Paket-Cache |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-y`, `--assumeyes` | beantwortet Rückfragen automatisch mit „yes“ |
| `--assumeno` | beantwortet Rückfragen automatisch mit „no“ |
| `-q`, `--quiet` | reduziert die Ausgabe |
| `-v`, `--verbose` | ausführliche Ausgabe |
| `--enablerepo=<repo>` / `--disablerepo=<repo>` | schaltet eine Paketquelle vorübergehend ein/aus |
| `--nogpgcheck` | überspringt die GPG-Signaturprüfung |
| `--best` | bevorzugt immer die neueste verfügbare Version |
| `--allowerasing` | erlaubt das Entfernen von Paketen zur Konfliktlösung |
| `-h`, `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo dnf install git            # Paket installieren
sudo dnf upgrade                # System aktualisieren
sudo dnf remove firefox         # Paket entfernen
dnf search bildbearbeitung      # nach Paketen suchen
dnf info git                    # Paketdetails anzeigen
dnf provides /usr/bin/git       # Paket zu einer Datei finden
sudo dnf history undo last      # letzte Transaktion rückgängig machen
```

</details>

>**Hinweis:** Gibt es auf Fedora, RHEL, CentOS, Rocky und AlmaLinux. Löst – anders als `rpm` – Abhängigkeiten automatisch auf. Nachfolger von `yum`; auf modernen Systemen ist `yum` nur noch ein Verweis auf `dnf`. Das Debian-Gegenstück ist `apt`.

---

### dpkg
>**Funktion:** Lokale `.deb`-Pakete verwalten (Debian/Ubuntu)<br />
>**Syntax:** `dpkg [optionen] <paket|datei>`<br />
>**Erklärung:** Niedrigere Ebene als `apt`: installiert einzelne `.deb`-Dateien und fragt den Paketstatus ab. Löst keine Abhängigkeiten automatisch auf.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i <datei.deb>` installiert ein lokales Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r <paket>` entfernt ein installiertes Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-l` listet installierte Pakete auf<br />
>**Beispiel:** `sudo dpkg -i programm.deb`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-i <datei>`, `--install` | installiert eine lokale `.deb`-Datei |
| `-r <paket>`, `--remove` | entfernt ein Paket (Konfiguration bleibt) |
| `-P <paket>`, `--purge` | entfernt ein Paket samt Konfiguration |
| `-l [<muster>]`, `--list` | listet installierte Pakete auf |
| `-L <paket>`, `--listfiles` | zeigt, welche Dateien zu einem Paket gehören |
| `-s <paket>`, `--status` | zeigt den Status eines Pakets |
| `-S <datei>`, `--search` | findet heraus, zu welchem Paket eine Datei gehört |
| `-I <datei>`, `--info` | zeigt die Informationen (Metadaten) einer `.deb`-Datei |
| `-c <datei>`, `--contents` | listet den Inhalt (die enthaltenen Dateien) einer `.deb`-Datei auf |
| `-x <datei> <verz>`, `--extract` | entpackt die Dateien einer `.deb`-Datei in ein Verzeichnis |
| `--unpack <datei>` | entpackt ein Paket, ohne es zu konfigurieren |
| `--configure <paket>` | konfiguriert ein entpacktes Paket (`-a` = alle ausstehenden) |
| `-C`, `--audit` | sucht nach halb installierten oder defekten Paketen |
| `--get-selections` | zeigt die Paketauswahl (Status) an |
| `--set-selections` | setzt die Paketauswahl aus der Standardeingabe |
| `--print-architecture` | zeigt die Systemarchitektur an (z. B. `amd64`) |
| `--force-<dinge>` | erzwingt eine Aktion trotz Problemen (z. B. `--force-overwrite`, `--force-depends`); alle Modi zeigt `--force-help`. Mit Vorsicht – kann das System beschädigen |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo dpkg -i programm.deb    # lokales Paket installieren
sudo apt install -f          # fehlende Abhängigkeiten nachinstallieren
dpkg -l | grep python        # installierte Pakete durchsuchen
dpkg -L git                  # Dateien des Pakets git anzeigen
dpkg -S /usr/bin/git         # Paket zu einer Datei finden
dpkg -I programm.deb         # Infos (Metadaten) einer .deb-Datei anzeigen
dpkg -c programm.deb         # Inhalt einer .deb-Datei auflisten
```

</details>

>**Hinweis:** `dpkg -i` löst keine Abhängigkeiten auf – fehlen welche, danach `sudo apt install -f` ausführen. Für den Alltag ist `apt` bequemer; `dpkg` ist vor allem für einzelne heruntergeladene `.deb`-Dateien nützlich.

---

### dpkg-reconfigure
>**Funktion:** Ein installiertes Paket neu konfigurieren (Debian/Ubuntu)<br />
>**Syntax:** `dpkg-reconfigure [optionen] <paket>...`<br />
>**Erklärung:** Startet den Konfigurationsdialog (debconf) eines bereits installierten Pakets erneut und wendet die neuen Antworten an – nützlich, um Einstellungen zu ändern, ohne das Paket neu zu installieren.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-f <frontend>` wählt die Oberfläche (z. B. `dialog`, `noninteractive`)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p <priorität>` legt fest, ab welcher Stufe Fragen gestellt werden<br />
>**Beispiel:** `sudo dpkg-reconfigure tzdata`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-f <frontend>`, `--frontend=<frontend>` | wählt die Oberfläche (`dialog`, `readline`, `noninteractive`, `web`) |
| `-p <prio>`, `--priority=<prio>` | stellt erst ab dieser Priorität Fragen (`low`, `medium`, `high`, `critical`) |
| `-a`, `--all` | konfiguriert alle installierten Pakete neu |
| `-u`, `--unseen-only` | zeigt nur bisher nicht gestellte Fragen |
| `--default-priority` | nutzt die Standard-Priorität |
| `-h`, `--help` | zeigt die Hilfe an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo dpkg-reconfigure tzdata                   # Zeitzone neu einstellen
sudo dpkg-reconfigure locales                  # Sprachen/Locales anpassen
sudo dpkg-reconfigure keyboard-configuration   # Tastaturlayout ändern
sudo dpkg-reconfigure -f noninteractive tzdata # ohne Rückfragen anwenden
```

</details>

>**Hinweis:** Braucht `sudo`. Wirkt nur bei Paketen, die überhaupt debconf-Fragen mitbringen (z. B. `tzdata`, `locales`, `keyboard-configuration`). Eigenständiges Werkzeug – kein Unterbefehl von `dpkg`.

---

### flatpak
>**Funktion:** Distributionsunabhängige Anwendungspakete verwalten<br />
>**Syntax:** `flatpak [optionen] <unterbefehl> [<app>...]`<br />
>**Erklärung:** Installiert sandbox-isolierte Anwendungen, die unabhängig von der Distribution funktionieren – meist aus dem Flathub-Repository.<br />
>**Unterbefehle:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`install <app>` installiert eine Anwendung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`uninstall <app>` entfernt eine Anwendung<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`update` aktualisiert installierte Anwendungen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`list` zeigt installierte Anwendungen<br />
>**Beispiel:** `flatpak install flathub org.gimp.GIMP`

<details markdown>
<summary>Alle Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `install <app>` | installiert eine Anwendung (oft `flatpak install flathub <id>`) |
| `uninstall <app>` | entfernt eine Anwendung |
| `update` | aktualisiert alle installierten Anwendungen |
| `list` | listet installierte Anwendungen und Laufzeiten auf |
| `search <begriff>` | sucht nach Anwendungen |
| `run <app>` | startet eine installierte Anwendung |
| `info <app>` | zeigt Informationen zu einer installierten Anwendung |
| `remote-add` | fügt eine Paketquelle hinzu (z. B. Flathub) |
| `remotes` | listet die konfigurierten Paketquellen auf |
| `override <app>` | passt die Berechtigungen einer Anwendung an |
| `repair` | repariert die Flatpak-Installation |
| `history` | zeigt das Protokoll vergangener Aktionen |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--user` | wirkt nur auf die Installation des Benutzers |
| `--system` | wirkt auf die systemweite Installation (Standard, oft mit `sudo`) |
| `-y`, `--assumeyes` | beantwortet Rückfragen automatisch mit „yes“ |
| `-v`, `--verbose` | ausführliche Ausgabe |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
flatpak install flathub org.gimp.GIMP   # GIMP aus Flathub installieren
flatpak list                            # installierte Anwendungen anzeigen
flatpak update                          # alles aktualisieren
flatpak run org.gimp.GIMP               # Anwendung starten
flatpak uninstall org.gimp.GIMP         # Anwendung entfernen
```

</details>

>**Hinweis:** Flatpak ist nicht überall vorinstalliert (`sudo apt install flatpak`) und benötigt meist die Flathub-Quelle: `flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`. Anwendungen laufen isoliert (Sandbox) und sind distributionsunabhängig.

---

### pacman
>**Funktion:** Pakete verwalten (Arch Linux)<br />
>**Syntax:** `pacman <operation> [optionen] [<paket>...]`<br />
>**Erklärung:** Paketverwaltung von Arch Linux und Abkömmlingen (Manjaro, EndeavourOS). Vereint hohe und niedrige Ebene in einem Werkzeug: installiert aus Repositories samt Abhängigkeiten, kann aber auch einzelne lokale Paketdateien einspielen. Gesteuert wird es über große Operations-Buchstaben (`-S`, `-R`, `-Q`, `-U`), die sich mit weiteren Buchstaben kombinieren lassen.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-S <paket>` installiert ein Paket aus den Repositories<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-Syu` aktualisiert das gesamte System<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-R <paket>` entfernt ein Paket<br />
>**Beispiel:** `sudo pacman -Syu`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-S <paket>` | installiert ein Paket aus den Repositories (mit Abhängigkeiten) |
| `-Sy` | aktualisiert die Paketdatenbanken |
| `-Syu` | aktualisiert Datenbanken **und** das gesamte System (Standard-Update) |
| `-Ss <begriff>` | durchsucht die Repositories nach einem Begriff |
| `-Si <paket>` | zeigt Informationen zu einem Paket aus den Repositories |
| `-Sc` | leert den Paket-Cache (`-Scc` entfernt alles) |
| `-U <datei>` | installiert eine lokale Paketdatei (`.pkg.tar.zst`) |
| `-R <paket>` | entfernt ein Paket |
| `-Rs <paket>` | entfernt ein Paket samt nicht mehr benötigter Abhängigkeiten |
| `-Rns <paket>` | entfernt Paket, Abhängigkeiten und Konfigurationsdateien |
| `-Q` | listet installierte Pakete auf |
| `-Qs <begriff>` | durchsucht die installierten Pakete |
| `-Qi <paket>` | zeigt Informationen zu einem installierten Paket |
| `-Ql <paket>` | listet die Dateien eines Pakets auf |
| `-Qo <datei>` | findet heraus, zu welchem Paket eine Datei gehört |
| `--noconfirm` | beantwortet Rückfragen automatisch (wie `-y` bei `apt`) |
| `-h`, `--help` | zeigt die Hilfe an |
| `-V`, `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo pacman -Syu                    # gesamtes System aktualisieren (Standard)
sudo pacman -S git                  # Paket installieren
pacman -Ss editor                   # Repositories durchsuchen
pacman -Qi git                      # Details zu installiertem Paket
pacman -Qo /usr/bin/git             # Paket zu einer Datei finden
sudo pacman -Rs firefox             # Paket samt Abhängigkeiten entfernen
sudo pacman -U ./paket.pkg.tar.zst  # lokale Paketdatei installieren
```

</details>

>**Hinweis:** Arch ist eine Rolling-Release-Distribution; aktualisiert wird immer mit `sudo pacman -Syu` (Teil-Updates wie `-Sy paket` gelten als unsicher). Vereint – anders als bei Debian/Fedora – hohe und niedrige Ebene in einem Befehl; ein separates Low-Level-Werkzeug wie `dpkg`/`rpm` gibt es nicht. Pakete aus dem AUR verwalten Helfer wie `yay`.

---

### rpm
>**Funktion:** Lokale `.rpm`-Pakete verwalten (Fedora/RHEL)<br />
>**Syntax:** `rpm [optionen] <paket|datei>`<br />
>**Erklärung:** Niedrigere Ebene als `dnf`: installiert einzelne `.rpm`-Dateien und fragt den Paketstatus ab. Löst keine Abhängigkeiten automatisch auf.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i <datei.rpm>` installiert ein lokales Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-e <paket>` entfernt ein installiertes Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-q <paket>` fragt ab, ob ein Paket installiert ist<br />
>**Beispiel:** `sudo rpm -ivh programm.rpm`

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-i <datei>`, `--install` | installiert eine lokale `.rpm`-Datei |
| `-U <datei>`, `--upgrade` | installiert neu oder aktualisiert ein Paket |
| `-F <datei>`, `--freshen` | aktualisiert nur, wenn das Paket bereits installiert ist |
| `-e <paket>`, `--erase` | entfernt ein installiertes Paket |
| `-q <paket>`, `--query` | fragt ab, ob/welche Version installiert ist |
| `-qa` | listet alle installierten Pakete auf |
| `-qi <paket>` | zeigt Informationen zu einem installierten Paket |
| `-ql <paket>` | listet die Dateien eines Pakets auf |
| `-qf <datei>` | findet heraus, zu welchem Paket eine Datei gehört |
| `-qp <datei.rpm>` | fragt eine `.rpm`-Datei ab (statt eines installierten Pakets) |
| `-V <paket>`, `--verify` | prüft ein Paket auf nachträgliche Veränderungen |
| `-v` | ausführliche Ausgabe |
| `-h`, `--hash` | zeigt bei der Installation einen Fortschrittsbalken (`#`) |
| `--nodeps` | ignoriert Abhängigkeiten (mit Vorsicht) |
| `--force` | erzwingt die Installation trotz Konflikten |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo rpm -ivh programm.rpm   # lokales Paket installieren (mit Fortschritt)
sudo rpm -Uvh programm.rpm   # installieren oder aktualisieren
rpm -qa                      # alle installierten Pakete auflisten
rpm -qa | grep python        # installierte Pakete durchsuchen
rpm -qi git                  # Details zu einem installierten Paket
rpm -ql git                  # Dateien des Pakets git anzeigen
rpm -qf /usr/bin/git         # Paket zu einer Datei finden
sudo rpm -e git              # Paket entfernen
```

</details>

>**Hinweis:** `rpm -i` löst keine Abhängigkeiten auf – im Alltag daher `dnf` bevorzugen, das `.rpm`-Pakete samt Abhängigkeiten verwaltet. `rpm` ist vor allem für einzelne heruntergeladene `.rpm`-Dateien und für Abfragen (`-q…`) nützlich. Debian-Gegenstück: `dpkg`.

---

### snap
>**Funktion:** Snap-Pakete verwalten (vor allem Ubuntu)<br />
>**Syntax:** `snap [optionen] <unterbefehl> [<paket>...]`<br />
>**Erklärung:** Canonicals Paketformat: in sich geschlossene, automatisch aktualisierte Pakete aus dem Snap Store. Unter Ubuntu vorinstalliert.<br />
>**Unterbefehle:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`install <paket>` installiert ein Snap<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`remove <paket>` entfernt ein Snap<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`refresh` aktualisiert installierte Snaps<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`list` zeigt installierte Snaps<br />
>**Beispiel:** `sudo snap install vlc`

<details markdown>
<summary>Alle Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `install <paket>` | installiert ein Snap |
| `remove <paket>` | entfernt ein Snap |
| `refresh [<paket>]` | aktualisiert ein oder alle Snaps |
| `list` | listet installierte Snaps auf |
| `find <begriff>` | sucht im Snap Store |
| `info <paket>` | zeigt Informationen zu einem Snap |
| `revert <paket>` | setzt ein Snap auf die vorherige Version zurück |
| `enable <paket>` / `disable <paket>` | aktiviert bzw. deaktiviert ein Snap |
| `services` | zeigt die Dienste der Snaps an |
| `logs <dienst>` | zeigt die Protokolle eines Snap-Dienstes |
| `changes` | zeigt laufende und vergangene Änderungen |
| `version` | zeigt die Snap- und Snapd-Versionen an |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `--classic` | installiert im Classic-Modus (ohne strenge Sandbox) |
| `--channel=<kanal>` | wählt den Kanal (z. B. `latest/stable`) |
| `--edge` / `--beta` / `--candidate` / `--stable` | Kurzform für den jeweiligen Kanal |
| `--devmode` | installiert im Entwicklermodus (Sandbox ausgesetzt) |
| `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo snap install vlc    # Snap installieren
snap find editor         # im Store suchen
snap list                # installierte Snaps anzeigen
sudo snap refresh        # alle Snaps aktualisieren
sudo snap remove vlc     # Snap entfernen
```

</details>

>**Hinweis:** Snaps aktualisieren sich automatisch im Hintergrund. Sie sind größer als klassische `.deb`-Pakete, dafür distributionsunabhängig. Unter Ubuntu vorinstalliert; auf anderen Systemen via `sudo apt install snapd`.

---

### yum
>**Funktion:** Pakete verwalten – Vorgänger von `dnf` (RHEL/CentOS)<br />
>**Syntax:** `yum [optionen] <unterbefehl> [<paket>...]`<br />
>**Erklärung:** Ältere Paketverwaltung der Red-Hat-Familie mit `.rpm`-Paketen. Löst Abhängigkeiten auf und funktioniert über Unterbefehle wie `install` oder `update`. Auf modernen Systemen durch `dnf` ersetzt.<br />
>**Unterbefehle:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`install <paket>` installiert ein Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`remove <paket>` entfernt ein Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`update` installiert verfügbare Aktualisierungen<br />
>**Beispiel:** `sudo yum install git`

<details markdown>
<summary>Alle Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `install <paket>` | installiert ein oder mehrere Pakete |
| `remove <paket>` | entfernt ein Paket |
| `update [<paket>]` | aktualisiert alle oder einzelne Pakete |
| `check-update` | prüft auf verfügbare Aktualisierungen |
| `search <begriff>` | sucht in den Quellen nach einem Begriff |
| `info <paket>` | zeigt Informationen zu einem Paket |
| `list installed` | listet installierte Pakete auf |
| `provides <datei>` | findet das Paket zu einer Datei |
| `groupinstall <gruppe>` | installiert eine Paketgruppe (`grouplist` zeigt alle) |
| `history` | zeigt die Transaktionshistorie |
| `clean all` | leert den Paket-Cache |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-y`, `--assumeyes` | beantwortet Rückfragen automatisch mit „yes“ |
| `-q`, `--quiet` | reduziert die Ausgabe |
| `-v`, `--verbose` | ausführliche Ausgabe |
| `--enablerepo=<repo>` / `--disablerepo=<repo>` | schaltet eine Paketquelle ein/aus |
| `--nogpgcheck` | überspringt die GPG-Signaturprüfung |
| `-h`, `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo yum install git       # Paket installieren
sudo yum update            # System aktualisieren
sudo yum remove firefox    # Paket entfernen
yum search editor          # nach Paketen suchen
yum info git               # Paketdetails anzeigen
```

</details>

>**Hinweis:** Weitgehend durch `dnf` abgelöst – auf Fedora und RHEL/CentOS 8+ ist `yum` nur noch ein Verweis auf `dnf`, die Befehle sind identisch. Auf neueren Systemen besser gleich `dnf` verwenden. Debian-Gegenstück: `apt`.

---

### zypper
>**Funktion:** Pakete installieren, aktualisieren und entfernen (openSUSE)<br />
>**Syntax:** `zypper [optionen] <unterbefehl> [<paket>...]`<br />
>**Erklärung:** Paketverwaltung von openSUSE und SUSE Linux Enterprise. Arbeitet mit `.rpm`-Paketen und Repositories, löst Abhängigkeiten automatisch auf und läuft über Unterbefehle wie `install` oder `update`. Viele Unterbefehle haben eine Kurzform (z. B. `in`, `rm`, `up`).<br />
>**Unterbefehle:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`install` (`in`) `<paket>` installiert ein Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`remove` (`rm`) `<paket>` entfernt ein Paket<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`refresh` (`ref`) aktualisiert die Repository-Daten<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`update` (`up`) installiert Aktualisierungen<br />
>**Beispiel:** `sudo zypper install git`

<details markdown>
<summary>Alle Unterbefehle</summary>

| Unterbefehl | Wirkung |
|---|---|
| `install` (`in`) `<paket>` | installiert ein oder mehrere Pakete |
| `remove` (`rm`) `<paket>` | entfernt ein Paket |
| `refresh` (`ref`) | aktualisiert die Metadaten der Repositories (≈ `apt update`) |
| `update` (`up`) | installiert verfügbare Aktualisierungen |
| `dist-upgrade` (`dup`) | Distributions-Upgrade (Rolling Release, z. B. Tumbleweed) |
| `search` (`se`) `<begriff>` | sucht in den Quellen nach einem Begriff |
| `info <paket>` | zeigt Informationen zu einem Paket |
| `list-updates` (`lu`) | zeigt verfügbare Aktualisierungen an |
| `repos` (`lr`) | listet die konfigurierten Repositories auf |
| `addrepo` (`ar`) / `removerepo` (`rr`) | fügt ein Repository hinzu / entfernt es |
| `clean` | leert den Paket-Cache |

</details>

<details markdown>
<summary>Mehr Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n`, `--non-interactive` | beantwortet Rückfragen automatisch (wie `-y` bei `apt`) |
| `-q`, `--quiet` | reduziert die Ausgabe |
| `-v`, `--verbose` | ausführliche Ausgabe |
| `--no-gpg-checks` | überspringt die GPG-Signaturprüfung |
| `-h`, `--help` | zeigt die Hilfe an |
| `--version` | zeigt die Version an |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
sudo zypper refresh          # Repository-Daten aktualisieren
sudo zypper install git      # Paket installieren
sudo zypper remove firefox   # Paket entfernen
sudo zypper update           # Updates installieren
sudo zypper dup              # Distributions-Upgrade (Tumbleweed)
zypper search editor         # nach Paketen suchen
```

</details>

>**Hinweis:** Gehört zu openSUSE/SLE und arbeitet wie `dnf`/`apt` auf hoher Ebene (Repositories + Abhängigkeitsauflösung). Nutzt darunter `.rpm`; das Low-Level-Werkzeug ist dort ebenfalls `rpm`. Viele Unterbefehle haben Kurzformen (`in`, `rm`, `up`, `dup`).

---
