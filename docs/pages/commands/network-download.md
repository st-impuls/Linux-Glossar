[← Home](../../index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)

## Kategorien
- [Benutzer & Rechte](users-permissions.md)
- [Datei- & Verzeichnisverwaltung](file-management.md)
- [Dateiinhalt anzeigen](file-content.md)
- [Navigation & Suche](navigation-search.md)
- [Textbearbeitung & Filter](text-processing.md)

[← Zurück zur Übersicht](index.md)

# Netzwerk & Download

### curl
>**Funktion:** Daten von oder zu einem Server übertragen | Extern<br />
>**Syntax:** `curl [optionen] <url>`<br />
>**Erklärung:** Ruft Daten über eine URL ab oder sendet sie; unterstützt viele Protokolle (HTTP, HTTPS, FTP u. a.). Standardmäßig wird die Ausgabe im Terminal angezeigt.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-O` speichert die Datei unter ihrem ursprünglichen Namen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-o <datei>` speichert die Ausgabe unter einem eigenen Namen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-L` folgt Weiterleitungen (redirects)<br />
>**Beispiel:** `curl -O https://example.com/datei.zip`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-O`, `--remote-name` | speichert die Datei unter ihrem ursprünglichen Namen |
| `-o <datei>`, `--output <datei>` | speichert die Ausgabe unter einem eigenen Namen |
| `-L`, `--location` | folgt Weiterleitungen (redirects) |
| `-I`, `--head` | zeigt nur die HTTP-Kopfzeilen (header) an |
| `-s`, `--silent` | unterdrückt Fortschritts- und Fehlerausgabe |
| `-S`, `--show-error` | zeigt Fehler trotz `-s` an (oft als `-sS` kombiniert) |
| `-f`, `--fail` | gibt bei HTTP-Fehlern (z. B. 404) keinen Inhalt aus und meldet Fehler |
| `-C -`, `--continue-at -` | setzt einen abgebrochenen Download fort |
| `-H <kopfzeile>`, `--header <kopfzeile>` | sendet eine eigene HTTP-Kopfzeile |
| `-X <methode>`, `--request <methode>` | wählt die HTTP-Methode (GET, POST, PUT …) |
| `-d <daten>`, `--data <daten>` | sendet Daten im Rumpf (impliziert POST) |
| `-u <user:pass>`, `--user <user:pass>` | sendet Zugangsdaten zur Authentifizierung |
| `-k`, `--insecure` | akzeptiert ungültige TLS-Zertifikate (mit Vorsicht) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
curl https://example.com                       # Inhalt im Terminal anzeigen
curl -O https://example.com/datei.zip          # unter Originalnamen speichern
curl -o seite.html https://example.com         # unter eigenem Namen speichern
curl -L https://kurz.url/abc                    # Weiterleitungen folgen
curl -I https://example.com                     # nur die HTTP-Header
curl -s https://api.example.com | grep "name"   # API-Ausgabe weiterverarbeiten
curl -X POST -d "name=max" https://example.com/api   # POST mit Daten senden
```

</details>

>**Hinweis:** Ohne `-o`/`-O` wird der Inhalt ins Terminal geschrieben – gut für APIs, oft mit Pipe kombiniert (z. B. `curl -s <url> | grep …`). Eine einfachere Alternative speziell zum Herunterladen ist `wget`.

---

### dig
>**Funktion:** DNS-Informationen abfragen | Extern<br />
>**Syntax:** `dig [optionen] [@<server>] <name> [<typ>]`<br />
>**Erklärung:** Fragt DNS-Server nach Einträgen zu einem Domainnamen ab (Domain Information Groper) und zeigt ausführliche, technische Antworten.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`+short` zeigt nur die knappe Antwort (z. B. nur die IP)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t <typ>` fragt einen bestimmten Eintragstyp ab (A, MX, NS …)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-x <ip>` Reverse-Lookup: IP → Hostname<br />
>**Beispiel:** `dig example.com`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `+short` | nur die knappe Antwort (z. B. nur die IP) |
| `+noall +answer` | zeigt nur den Answer-Abschnitt der Antwort |
| `-t <typ>` | Eintragstyp abfragen (A, AAAA, MX, NS, TXT, CNAME) |
| `-x <ip>` | Reverse-Lookup: IP → Hostname |
| `@<server>` | einen bestimmten DNS-Server fragen (z. B. `@8.8.8.8`) |
| `+trace` | die Auflösung Schritt für Schritt ab den Root-Servern verfolgen |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
dig example.com                 # vollständige A-Abfrage
dig +short example.com          # nur die IP-Adresse
dig -t MX example.com           # Mailserver-Einträge (MX)
dig -x 8.8.8.8                  # Reverse-Lookup zu einer IP
dig @1.1.1.1 example.com        # über einen bestimmten DNS-Server fragen
```

</details>

>**Hinweis:** `dig` gehört zum Paket `dnsutils` (Debian/Ubuntu) bzw. `bind-utils` und ist ausführlicher als `host` oder `nslookup`.

---

### host
>**Funktion:** DNS-Namen einfach auflösen | Extern<br />
>**Syntax:** `host [optionen] <name> [<server>]`<br />
>**Erklärung:** Einfaches Werkzeug für DNS-Abfragen; liefert knappe, leicht lesbare Antworten.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-t <typ>` fragt einen bestimmten Eintragstyp ab (A, MX, NS …)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-a` zeigt alle verfügbaren Einträge<br />
>**Beispiel:** `host example.com`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-t <typ>` | bestimmten Eintragstyp abfragen (A, MX, NS, TXT …) |
| `-a` | alle verfügbaren Einträge anzeigen (entspricht `-t ANY -v`) |
| `-v` | ausführliche Ausgabe |
| `-4` / `-6` | nur IPv4 bzw. nur IPv6 verwenden |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
host example.com            # IP-Adresse(n) ermitteln
host -t MX example.com      # Mailserver-Einträge
host 8.8.8.8               # Reverse-Lookup (Hostname zur IP)
host -a example.com         # alle Einträge anzeigen
```

</details>

>**Hinweis:** Einfacher und kürzer als `dig`. Ein Reverse-Lookup gelingt direkt mit `host <ip>`.

---

### ip
>**Funktion:** Netzwerk-Adressen, -Routen und -Geräte verwalten | Extern<br />
>**Syntax:** `ip [optionen] <objekt> [befehl]`<br />
>**Erklärung:** Zentrales, modernes Werkzeug zum Anzeigen und Konfigurieren von Netzwerkschnittstellen, IP-Adressen und Routen. Ersetzt das ältere `ifconfig`.<br />
>**Verwendung:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`ip a` zeigt alle Adressen und Schnittstellen (address)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`ip r` zeigt die Routing-Tabelle (route)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`ip link` zeigt die Netzwerkgeräte<br />
>**Beispiel:** `ip a`

<details markdown>
<summary>Alle Unterbefehle</summary>

| Befehl | Wirkung |
|---|---|
| `ip a` (`ip address`) | zeigt die IP-Adressen aller Schnittstellen |
| `ip r` (`ip route`) | zeigt die Routing-Tabelle |
| `ip link` | zeigt die Netzwerkgeräte (Status, MAC-Adresse) |
| `ip neigh` | zeigt die ARP-/Nachbar-Tabelle |
| `ip -s link` | zeigt Statistiken (Pakete, Fehler) |
| `ip addr add <ip>/<maske> dev <gerät>` | weist einer Schnittstelle eine Adresse zu |
| `ip link set <gerät> up` / `down` | schaltet eine Schnittstelle ein bzw. aus |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
ip a                                  # alle Adressen und Schnittstellen
ip r                                  # Routing-Tabelle (u. a. das Gateway)
ip link                               # Geräte mit Status und MAC-Adresse
ip -s link                            # mit Paket-Statistiken
sudo ip link set eth0 up              # Schnittstelle eth0 aktivieren
```

</details>

>**Hinweis:** `ip` gehört zum Paket `iproute2`. Konfigurationsänderungen brauchen meist `sudo` und gelten nur bis zum Neustart; dauerhafte Einstellungen erfolgen über die Netzwerkkonfiguration der Distribution.

---

### nslookup
>**Funktion:** DNS-Namen auflösen (auch interaktiv) | Extern<br />
>**Syntax:** `nslookup [optionen] <name> [<server>]`<br />
>**Erklärung:** Klassisches Werkzeug für DNS-Abfragen mit optionalem interaktivem Modus; auch unter Windows verfügbar.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-type=<typ>` fragt einen bestimmten Eintragstyp ab (A, MX, NS …)<br />
>**Beispiel:** `nslookup example.com`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-type=<typ>` | Eintragstyp abfragen (A, MX, NS, TXT …) |
| `-query=<typ>` | gleichbedeutend mit `-type` |
| `<name> <server>` | die Abfrage über einen bestimmten DNS-Server stellen |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
nslookup example.com            # IP-Adresse ermitteln
nslookup -type=MX example.com   # Mailserver-Einträge
nslookup example.com 8.8.8.8    # über einen bestimmten DNS-Server
nslookup                        # interaktiver Modus (Beenden mit `exit`)
```

</details>

>**Hinweis:** Gilt als veraltet – `dig` wird bevorzugt. Vorteil: auch unter Windows vorhanden. Ohne Argument startet der interaktive Modus.

---

### ping
>**Funktion:** Erreichbarkeit eines Hosts prüfen | Extern<br />
>**Syntax:** `ping [optionen] <host>`<br />
>**Erklärung:** Sendet ICMP-Pakete an einen Host und misst, ob und wie schnell er antwortet.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c <n>` sendet nur n Pakete und beendet sich dann<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i <sek>` legt die Wartezeit zwischen den Paketen fest<br />
>**Beispiel:** `ping -c 4 example.com`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-c <n>` | sendet nur n Pakete und beendet sich dann |
| `-i <sek>` | Wartezeit zwischen den Paketen |
| `-s <bytes>` | Größe der gesendeten Pakete |
| `-W <sek>` | Timeout pro Antwort |
| `-4` / `-6` | IPv4 bzw. IPv6 erzwingen |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
ping example.com            # endlos pingen (Abbruch mit Ctrl + C)
ping -c 4 example.com       # genau 4 Pakete senden
ping -c 4 -i 0.5 8.8.8.8    # 4 Pakete im Abstand von 0,5 s
```

</details>

>**Hinweis:** Unter Linux pingt `ping` ohne `-c` endlos; Abbruch mit `Ctrl + C`. Manche Hosts blockieren ICMP und antworten trotz Erreichbarkeit nicht.

---

### scp
>**Funktion:** Dateien sicher zwischen Rechnern kopieren | Extern<br />
>**Syntax:** `scp [optionen] <quelle> <ziel>`<br />
>**Erklärung:** Kopiert Dateien über eine verschlüsselte SSH-Verbindung zwischen lokalem und entferntem Rechner (secure copy).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-r` kopiert Verzeichnisse rekursiv<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-P <port>` abweichender SSH-Port (Großbuchstabe!)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p` erhält Zeitstempel und Rechte<br />
>**Beispiel:** `scp datei.txt user@host:/home/user/`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-r` | kopiert Verzeichnisse rekursiv |
| `-P <port>` | abweichender SSH-Port (**Großbuchstabe**, anders als bei `ssh`!) |
| `-p` | erhält Zeitstempel und Rechte |
| `-C` | komprimiert die Übertragung |
| `-i <datei>` | nutzt einen bestimmten privaten Schlüssel |
| `-q` | unterdrückt die Fortschrittsanzeige |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
scp datei.txt user@host:/pfad/      # lokal → entfernt
scp user@host:/pfad/datei.txt .     # entfernt → lokal
scp -r ordner/ user@host:/pfad/     # ganzes Verzeichnis kopieren
scp -P 2222 datei.txt user@host:/p  # über einen anderen Port
```

</details>

>**Hinweis:** Adressformat ist `user@host:/pfad`. Achtung: der Port ist `-P` (groß), bei `ssh` dagegen `-p` (klein). Modernere Alternativen sind `rsync` (überträgt nur Änderungen) und `sftp`.

---

### ssh
>**Funktion:** Sicher auf einem entfernten Rechner anmelden | Extern<br />
>**Syntax:** `ssh [optionen] [<benutzer>@]<host> [befehl]`<br />
>**Erklärung:** Baut eine verschlüsselte Verbindung zu einem entfernten Rechner auf und öffnet dort eine Shell oder führt einen Befehl aus (secure shell).<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-p <port>` abweichender Port (Kleinbuchstabe!)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-i <datei>` nutzt einen bestimmten privaten Schlüssel<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-X` aktiviert X11-Weiterleitung (grafische Programme)<br />
>**Beispiel:** `ssh user@host`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-p <port>` | abweichender Port (**Kleinbuchstabe**) |
| `-i <datei>` | privater Schlüssel (Identity-Datei) |
| `-X` | X11-Weiterleitung für grafische Programme |
| `-L <lport>:<host>:<rport>` | lokales Port-Forwarding (Tunnel) |
| `-N` | führt keinen Befehl aus (nur für Tunnel) |
| `-v` | ausführliche Ausgabe zur Fehlersuche |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
ssh user@host                       # interaktive Anmeldung
ssh -p 2222 user@host               # über einen anderen Port
ssh user@host "ls -l /var"          # einen einzelnen Befehl ausführen
ssh -i ~/.ssh/id_ed25519 user@host  # bestimmten Schlüssel nutzen
```

</details>

>**Hinweis:** Eine schlüsselbasierte Anmeldung (`ssh-keygen`, `ssh-copy-id`) ist sicherer und bequemer als Passwörter. `ssh` ist auch die Grundlage für `scp` und `sftp`.

---

### traceroute
>**Funktion:** Route der Pakete zu einem Host anzeigen | Extern<br />
>**Syntax:** `traceroute [optionen] <host>`<br />
>**Erklärung:** Zeigt den Weg (die einzelnen Zwischenstationen, „Hops"), den Pakete bis zum Zielhost nehmen, samt Antwortzeiten.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-n` zeigt IPs statt Hostnamen (keine DNS-Auflösung, schneller)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-m <n>` legt die maximale Anzahl an Hops fest<br />
>**Beispiel:** `traceroute example.com`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-n` | zeigt IPs statt Hostnamen (keine DNS-Auflösung, schneller) |
| `-m <n>` | maximale Anzahl an Hops (Standard 30) |
| `-q <n>` | Anzahl der Testpakete pro Hop |
| `-w <sek>` | Wartezeit auf eine Antwort |
| `-I` | nutzt ICMP statt UDP |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
traceroute example.com          # Route mit Namensauflösung
traceroute -n example.com       # schneller, nur IP-Adressen
traceroute -m 15 example.com    # höchstens 15 Hops verfolgen
```

</details>

>**Hinweis:** Oft nicht vorinstalliert (Paket `traceroute`). Ein `*` bei einem Hop bedeutet „keine Antwort" (häufig eine Firewall). Verwandt ist `mtr`, das `ping` und `traceroute` fortlaufend kombiniert.

---

### wget
>**Funktion:** Dateien aus dem Netz herunterladen | Extern<br />
>**Syntax:** `wget [optionen] <url>...`<br />
>**Erklärung:** Lädt Dateien über HTTP, HTTPS oder FTP herunter und speichert sie standardmäßig unter ihrem ursprünglichen Namen. Funktioniert auch im Hintergrund und bei instabiler Verbindung.<br />
>**Optionen:**<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-O <datei>` speichert unter einem eigenen Namen<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-c` setzt einen abgebrochenen Download fort (continue)<br />
>&nbsp;&nbsp;&nbsp;&nbsp;`-P <verz>` legt das Zielverzeichnis fest<br />
>**Beispiel:** `wget https://example.com/datei.zip`

<details markdown>
<summary>Alle Optionen</summary>

| Option | Wirkung |
|---|---|
| `-O <datei>`, `--output-document=<datei>` | speichert unter einem eigenen Namen (`-O -` = Ausgabe ins Terminal) |
| `-c`, `--continue` | setzt einen abgebrochenen Download fort |
| `-P <verz>`, `--directory-prefix=<verz>` | legt das Zielverzeichnis fest |
| `-q`, `--quiet` | unterdrückt jede Ausgabe |
| `-b`, `--background` | lädt im Hintergrund weiter (Protokoll in `wget-log`) |
| `-i <datei>`, `--input-file=<datei>` | lädt alle URLs aus einer Datei |
| `-nc`, `--no-clobber` | überschreibt vorhandene Dateien nicht |
| `--limit-rate=<rate>` | begrenzt die Bandbreite (z. B. `--limit-rate=500k`) |
| `-r`, `--recursive` | lädt rekursiv (folgt Links, ganze Verzeichnisse/Seiten) |
| `-np`, `--no-parent` | folgt beim rekursiven Laden nicht in übergeordnete Verzeichnisse |
| `-U <agent>`, `--user-agent=<agent>` | sendet eine eigene User-Agent-Kennung |
| `--no-check-certificate` | ignoriert ungültige TLS-Zertifikate (mit Vorsicht) |

</details>

<details markdown>
<summary>Weitere Beispiele</summary>

```bash
wget https://example.com/datei.zip          # unter Originalnamen speichern
wget -O archiv.zip https://example.com/d     # unter eigenem Namen speichern
wget -c https://example.com/gross.iso        # abgebrochenen Download fortsetzen
wget -P ~/downloads https://example.com/x    # in ein bestimmtes Verzeichnis
wget -i urls.txt                             # alle URLs aus einer Datei laden
wget --limit-rate=500k https://example.com/x # Download-Tempo begrenzen
wget -r -np https://example.com/ordner/      # ganzen Ordner rekursiv spiegeln
```

</details>

>**Hinweis:** `wget` ist auf reines Herunterladen ausgelegt und kann von Haus aus rekursiv ganze Seiten spiegeln und Downloads fortsetzen. Für APIs, eigene HTTP-Methoden und das Senden von Daten ist `curl` die flexiblere Wahl.

---