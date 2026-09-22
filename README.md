# Mystic Archive

A 100% static, mythic-dark Minecraft relic price grimoire — only HTML, Tailwind CSS, vanilla JavaScript, JSON and PNG textures. No backend, no build step, no npm.

## Site

Everything lives in `docs/` (GitHub Pages → Deploy from branch → `/docs`):

- `docs/index.html` — the price archive: search, sort, schools (categories), rarity auras, compact worth (`240k`, `1.5M`) with exact values on hover
- `docs/db.json` — the database (items with prices, categories, stack sizes, texture paths)
- `docs/data/worth.json` — price source
- `docs/textures/` — vanilla item/block/entity/map textures with clean names, e.g. `textures/item/diamond.png`

## Run locally

Any static file server works — no npm needed:

```bash
cd docs
python -m http.server 8000
```

Then open http://localhost:8000.
Note: opening the HTML file directly via `file://` won't load `db.json` (browsers block `fetch` there) — use http.

## Data

Prices live in `docs/db.json` (schema: `{ version, exportedAt, categories, items[] }`, each item `{ id, name, minecraftId, category, pricePerItem, stackSize, icon }`).
To update prices, edit `db.json` directly or regenerate it from `docs/data/worth.json` keeping the same schema and site-root-relative `textures/…` icon paths.

## Price calculations

| Unit | Formula | Display |
|------|---------|---------|
| Per Item | `pricePerItem` | `240k` for 240000 (exact value on hover) |
| Half Stack | `pricePerItem × floor(stackSize / 2)` | compact `k` / `M` / `B` |
| Full Stack | `pricePerItem × stackSize` | compact `k` / `M` / `B` |

Rarity aura by worth: Common (<100) · Rare (≥100) · Epic (≥1k) · Legendary (≥10k) · Mythic (≥100k).
