# Linux-Glossar

Persönliches Glossar zu Linux: Abkürzungen, Befehle und Tastenkürzel.
Als Website gebaut mit [MkDocs Material](https://squidfunk.github.io/mkdocs-material/)
und automatisch über GitHub Pages veröffentlicht.

## Struktur

```
docs/                    Inhalt (Markdown + Bilder) – hier wird geschrieben
  index.md               Startseite
  pages/                 einzelne Glossar-Seiten
  images/                Bilder
mkdocs.yml               Konfiguration der Website
requirements.txt         Python-Abhängigkeit (mkdocs-material)
.github/workflows/       GitHub Action: baut & veröffentlicht bei jedem Push
```

## Lokale Vorschau

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows; Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve                     # http://127.0.0.1:8000
```

## Veröffentlichen über GitHub Pages

Einmalig:

1. Repository auf GitHub anlegen und pushen:
   ```bash
   git remote add origin https://github.com/<NUTZER>/<REPO>.git
   git push -u origin main
   ```
2. Auf GitHub: **Settings → Pages → Build and deployment → Source: GitHub Actions**.

Danach läuft alles automatisch: Bei jedem `git push` baut die Action
(`.github/workflows/deploy.yml`) die Seite und veröffentlicht sie unter
`https://<NUTZER>.github.io/<REPO>/`.

### Alltag

```bash
git add .
git commit -m "Befehl grep ergänzt"
git push
```

Nach ein paar Sekunden ist die Website aktualisiert.
Rohe Markdown-Dateien sind auf GitHub jederzeit über den **Raw**-Button bzw.
`raw.githubusercontent.com` abrufbar.
