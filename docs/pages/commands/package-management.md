[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Navigation & Suche](navigation-search.md)
- [Netzwerk & Download](network-download.md)
- [Prozesse & Steuerung](process-control.md)
- [Rechnen & Datum](calc-date.md)
- [Shell & Skripte](shell-scripting.md)
- [System & Dienste](system-services.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Paketverwaltung

### apt
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
| `--unpack <datei>` | entpackt ein Paket, ohne es zu konfigurieren |
| `--configure <paket>` | konfiguriert ein entpacktes Paket (`-a` = alle ausstehenden) |
| `-C`, `--audit` | sucht nach halb installierten oder defekten Paketen |
| `--get-selections` | zeigt die Paketauswahl (Status) an |
| `--set-selections` | setzt die Paketauswahl aus der Standardeingabe |
| `--print-architecture` | zeigt die Systemarchitektur an (z. B. `amd64`) |
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
```

</details>

>**Hinweis:** `dpkg -i` löst keine Abhängigkeiten auf – fehlen welche, danach `sudo apt install -f` ausführen. Für den Alltag ist `apt` bequemer; `dpkg` ist vor allem für einzelne heruntergeladene `.deb`-Dateien nützlich.

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
