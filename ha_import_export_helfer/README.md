[![Version](https://img.shields.io/github/v/release/Q14siX/ha_import_export_helfer)](https://github.com/Q14siX/ha_import_export_helfer/releases) [![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE) ![Languages](https://img.shields.io/badge/languages-1-blue.svg) ![Status](https://img.shields.io/badge/status-stable-brightgreen.svg) ![Downloads](https://img.shields.io/github/downloads/Q14siX/ha_import_export_helfer/total)

-----

# Import / Export Helfer (Home Assistant Add-on)

**Exportiere und importiere gezielt einzelne Home‑Assistant‑Elemente** – perfekt zum Migrieren zwischen Instanzen oder für selektive Backups.
Unterstützt sowohl **.storage** (UI‑Objekte) als auch **YAML‑Dateien** (z. B. `automations.yaml`, `scripts.yaml`).

-----

## ✨ Funktionen

  - Export einzelner Elemente (Helfer, Automationen, Skripte, Szenen, Blueprints)
  - Konflikterkennung beim Import (z. B. `unique_id`, Name) mit Auswahl der Strategie
  - **Vollständig responsive Benutzeroberfläche** – optimiert für Desktop und Mobilgeräte.
  - Ingress‑Web‑UI – keine Portfreigabe nötig
  - Zugriff auf `/config` zum Lesen/Schreiben der relevanten Dateien

-----

## 📸 Screenshots

Die Benutzeroberfläche ist vollständig responsiv und passt sich an alle Bildschirmgrößen an.
### Desktop-Ansicht

| Exportieren | Importieren |
| :---: | :---: |
| ![DesktopExportieren](images/desktop_export.png) | ![DesktopImportieren](images/desktop_import.png) |

### Mobile-Ansicht

| Exportieren | Importieren |
| :---: | :---: |
| ![MobilExportieren](images/mobil_export.png) | ![MobilImportieren](images/mobil_import.png) |

-----

## 🧩 Installation

Dieses Add‑on wird über das Add‑on‑Repository bereitgestellt:

1.  **Einstellungen → Add‑ons → Add‑on‑Store → ⋮ → Repositories**
2.  Repository‑URL hinzufügen: `https://github.com/Q14siX/ha_import_export_helfer`
3.  „**Import / Export Helfer**“ installieren, starten und **Öffnen** (Ingress).

> Das Repository enthält die Datei `repository.json` im Root und dieses Add‑on im Ordner `import_export_helfer/`.

-----

## ⚙️ Laufzeit & Konfiguration

  - **Ingress** (empfohlen): `ingress: true` im `config.yaml` des Add‑ons.
      - *Kein* `ports:` nötig. *Kein* `webui:` erforderlich.
      - Achte in der App auf **relative Pfade** (z. B. Flask `url_for(...)`), sonst fehlen Assets im Ingress.
  - **Dateizugriff**: `map: ["config:rw"]` – Export/Import benötigt Schreibrechte auf `/config`.
  - **Rollen**: Falls Supervisor‑APIs genutzt werden, `hassio_role: admin` setzen.
  - **Images**: Wenn `image:` weggelassen wird, baut der Supervisor lokal aus dem `Dockerfile`.
      - Optional schneller: Vorbau über GHCR, z. B. `image: "ghcr.io/q14six/import_export_helfer-{arch}"`.

-----

## 🚀 Nutzung (Kurzablauf)

1.  Add‑on starten → **Öffnen** (Ingress).
2.  **Export**: Elementtyp wählen → Einträge auswählen → Datei erzeugen.
3.  **Import**: Datei hochladen → Konflikte prüfen → gewünschte Aktion wählen → importieren.

-----

## 🛠️ Troubleshooting

  - **UI leer/Assets fehlen (Ingress)** → in der Web‑App ausschließlich **relative Pfade** verwenden.
  - **„pull access denied for local/…“** → kein lokaler Imagename. `image:` entfernen **oder** auf Registry (GHCR/Docker Hub) verweisen.
  - **Schreibrechte** → `map: ["config:rw"]` prüfen.
  - **Logs** → Add‑on öffnen → **Protokoll**.

-----

## 🧪 Entwicklung

  - Änderungen committen → Add‑on neu starten.
  - Versionspflege in `config.yaml` (`version: "x.y.z"`). Release‑Tag sollte dazu passen.

-----

## 📄 Lizenz

Dieses Add‑on steht unter der **MIT-Lizenz**.
Siehe die Datei [`LICENSE`](https://www.google.com/search?q=../LICENSE) im Repository‑Root.

-----

## 📨 Support / Issues

Issues und Feature‑Requests bitte hier erstellen:  
[https://github.com/Q14siX/ha\_import\_export\_helfer/issues](https://github.com/Q14siX/ha_import_export_helfer/issues)
