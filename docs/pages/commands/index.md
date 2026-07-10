[← Home](../../index.md)
- [Grundlagen](../basics/index.md)
- [Abkürzungen / Begriffe](../abbreviations.md)
- [Tastenkürzel](../shortcuts.md)
- [Wichtige Verzeichnisse](../directories.md)

# Befehle

## Kategorien
- [Archivierung & Kompression](archiving-compression.md)
- [Benutzer & Rechte](users-permissions.md)
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

## Aufbau eines Befehls

Ein Linux-Befehl ist in der Regel nach folgendem Schema aufgebaut:

`kommando [optionen] <argumente>`

- **kommando** – der Befehl selbst, also das Programm oder die eingebaute Funktion, die ausgeführt wird (z. B. `ls`, `cp`).
- **optionen** – Schalter, die festlegen, *wie* der Befehl arbeitet (z. B. `-l`, `-r`). Meist optional.
- **argumente** – das Objekt, *womit* der Befehl arbeitet, z. B. eine Datei oder ein Verzeichnis.

Schreibweise der Platzhalter in diesem Glossar:

- `<…>` – Platzhalter: hier etwas Eigenes einsetzen (z. B. einen Dateinamen).
- `[…]` – optional: kann angegeben werden, muss aber nicht.
- `…` – wiederholbar: die Angabe kann mehrfach vorkommen.

## Weiterführende Grundlagen

Wie die Shell Befehle vor der Ausführung umformt, ihre Ein- und Ausgabe umleitet und mehrere Befehle verknüpft, steht ausführlich in der Rubrik [Grundlagen](../basics/index.md):

- [Globbing & Platzhalter](../basics/globbing.md) – `*`, `?`, `[…]` in Datei- und Verzeichnisnamen
- [Streams & Umleitungen](../basics/streams-redirects.md) – `>`, `>>`, `<`, `2>`, Pipes und Here-Document
- [Verkettung & Hintergrund](../basics/command-chaining.md) – `;`, `&&`, `||`, `&` und `$_`
