# Mystic Item DB

A 100% static Minecraft item price database — only HTML, Tailwind CSS, vanilla JavaScript, JSON and PNG textures. No backend, no build step, no npm.

## Pages

The whole site lives in `docs/` (ready for GitHub Pages → Settings → Pages → Deploy from branch → `/docs`):

| Page | Path | What it does |
|------|------|--------------|
| Price List | `docs/index.html` | Read-only showcase, reads `db.json` |
| Admin | `docs/admin/index.html` | Add/edit/delete items, import/export, reload prices from `worth.json` |
| Component Builder | `docs/builder/index.html` | Build item packs in the browser (localStorage) |

## Run locally

Any static file server works — no npm needed:

```bash
cd docs
python -m http.server 8000
```

Then open http://localhost:8000 (admin at `/admin/`, builder at `/builder/`).
Note: opening the HTML files directly via `file://` won't load `db.json` (browsers block `fetch` there) — use http.

## Data & workflow

- `docs/db.json` — the published database (1187 items with prices, categories, stack sizes, texture paths).
- `docs/data/worth.json` — price source; the admin panel can re-import it in the browser (merge or replace).
- `docs/textures/` — vanilla item/block/entity/map textures with clean names, e.g. `textures/item/diamond.png`.
- Admin edits live in the browser (localStorage). To publish: **Export DB** → replace `docs/db.json` with the download → commit & push.
- Custom icons are stored as base64 (`iconData`); vanilla icons are stored as relative `textures/…` paths.

## Price calculations

| Unit | Formula |
|------|---------|
| Per Item | `pricePerItem` |
| Half Stack | `pricePerItem × floor(stackSize / 2)` |
| Full Stack | `pricePerItem × stackSize` |
