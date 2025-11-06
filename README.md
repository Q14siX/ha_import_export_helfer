# Import / Export Helfer – Home Assistant Add-on

Ein Add-on, um **einzelne Home‑Assistant‑Elemente gezielt zu exportieren und wieder zu importieren** – ideal zum Migrieren zwischen Instanzen oder für punktuelle Backups (Helfer, Automationen, Skripte, Szenen, Blueprints).

> Repository: https://github.com/Q14siX/ha_import_export_helfer

---

## ✨ Funktionen

- Export einzelner Elemente (z. B. **Helper**, **Automationen**, **Skripte**, **Szenen**, **Blueprints**)
- Erkennung von Konflikten beim Import (z. B. `unique_id`, Namen) und Auswahl, wie damit verfahren wird
- Unterstützung für **.storage** (UI‑Elemente) **und** YAML‑Dateien (z. B. `automations.yaml`, `scripts.yaml`)
- **Ingress‑Web‑UI** für einfache Bedienung (kein Port nötig)

> Typischer Anwendungsfall: Du betreibst eine zweite HA‑Instanz zu Testzwecken und willst Elemente **gezielt** zwischen Instanzen übertragen.

---

## 🧩 Installation (Add-on-Repository)

1. In Home Assistant: **Einstellungen → Add-ons → Add-on‑Store → ⋮ (oben rechts) → Repositories**
2. Repository‑URL eintragen:  
   `https://github.com/Q14siX/ha_import_export_helfer`
3. Das Add-on **„Import / Export Helfer“** auswählen → **Installieren** → **Starten** → **Öffnen**

> Hinweis: Das Repo kann **ein einzelnes Add-on** enthalten. Achte darauf, dass im Repo‑Root eine `repository.json` liegt und das Add-on in einem Unterordner (z. B. `import_export_helfer/`) mit `config.yaml` vorhanden ist.

**Beispielstruktur:**
```
ha_import_export_helfer/
├─ repository.json
├─ README.md                 # diese Datei
└─ import_export_helfer/     # Add-on-Ordner (Slug)
   ├─ config.yaml
   ├─ Dockerfile
   ├─ run.sh
   ├─ README.md              # Add-on-spezifische Details/Changelog
   ├─ logo.png / icon.png
   └─ app/…                  # dein Code (z. B. FastAPI/Flask)
```

**repository.json (Beispiel):**
```json
{
  "name": "Import / Export Helfer (Q14siX)",
  "url": "https://github.com/Q14siX/ha_import_export_helfer"
}
```

---

## ⚙️ Konfiguration / Betrieb

- **Ingress** (empfohlen):  
  `ingress: true` im `config.yaml` des Add-ons.  
  *Kein* `ports:` notwendig. *Kein* `webui:` erforderlich.
- **Statische Dateien** (UI): In der App **relative Pfade** verwenden (z. B. in Flask `url_for(...)`), damit Ingress korrekt rendert.
- **Dateizugriff**: Für Export/Import wird Lese-/Schreibzugriff auf `/config` benötigt.
  - In `config.yaml`: `map: ["config:rw"]`
- **Rollen**: Falls Supervisor‑APIs genutzt werden: `hassio_role: admin`

---

## 🚀 Images (Optional: schnelleres Installieren)

Lässt du `image:` im `config.yaml` **weg**, baut der Supervisor lokal aus dem `Dockerfile`.  
Für schnellere Installation kannst du Multi‑Arch‑Images via **GHCR** bereitstellen und im Add-on setzen:

```yaml
image: "ghcr.io/q14six/import_export_helfer-{arch}"
```

Minimaler GitHub‑Actions‑Workflow (multi‑arch Build bei Git‑Tag):
```yaml
name: Build & Push Add-on
on:
  push:
    tags: ['*']

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - uses: actions/checkout@v4
      - uses: docker/setup-qemu-action@v3
      - uses: docker/setup-buildx-action@v3
      - name: Login GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - name: Build & Push (multi-arch)
        uses: docker/build-push-action@v6
        with:
          context: ./import_export_helfer
          platforms: linux/amd64,linux/arm64,linux/arm/v7
          push: true
          tags: |
            ghcr.io/${{ github.repository_owner }}/import_export_helfer-amd64:${{ github.ref_name }}
            ghcr.io/${{ github.repository_owner }}/import_export_helfer-aarch64:${{ github.ref_name }}
            ghcr.io/${{ github.repository_owner }}/import_export_helfer-armv7:${{ github.ref_name }}
```

> **Wichtig:** Keine lokalen Image‑Namen wie `local/…` verwenden. Entweder `image:` **weglassen** (lokaler Build) oder auf eine Registry (z. B. GHCR) verweisen.

---

## 🧪 Nutzung (kurz)

1. Add-on starten → **Öffnen** (Ingress)
2. **Export**: Elementtyp wählen → einzelne Einträge auswählen → Datei erzeugen.
3. **Import**: Datei hochladen → Konflikte werden angezeigt → gewünschte Aktion wählen → importieren.

---

## 🛠️ Troubleshooting

- **Leeres/teilweises UI im Ingress** → Prüfe, dass alle Web‑Assets **relative Pfade** nutzen.
- **„pull access denied for local/…“** → Entferne `image:` aus `config.yaml` **oder** verweise auf eine echte Registry (siehe oben).
- **Rechteprobleme beim Schreiben** → `map: ["config:rw"]` im Add-on prüfen.
- Logs ansehen: **Add-ons → Import / Export Helfer → Protokoll**.

---

## 📦 Entwicklung lokal

- Änderungen im Add-on‑Ordner committen und Add-on in HA neu starten.
- Für Test‑Builds ohne Registry `image:` weglassen.
- SemVer für `version` im `config.yaml` nutzen (z. B. `1.0.0`). Release‑Tag sollte zum Wert passen.

---

## 📄 Lizenz

Dieses Repository steht unter der **MIT-Lizenz**. Siehe [`LICENSE`](./LICENSE).
