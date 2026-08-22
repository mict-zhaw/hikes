# Hiking Route Agent

This repository stores public GPX tracks for hiking routes and provides stable URLs that can be opened from swisstopo and referenced from Notion.

## Workflow

When adding or updating a hike:

1. Read `AGENT.md`, `routes.yaml`, and `README.md` first.
2. Keep GPX files in `gpx/` using a lowercase kebab-case slug, e.g. `gpx/vergeletto-lago-dalzasca-lodano.gpx`.
3. **Preserve every uploaded GPX file byte-for-byte. Never simplify, regenerate, normalize, reserialize, strip extensions, reduce track points, or otherwise rewrite it before committing.** Analysis of the GPX must happen separately from the stored artifact.
4. Never overwrite an existing GPX file unless the user explicitly asks to replace that route. When replacing a broken derivative with the user's original upload, treat the original upload as authoritative.
5. Add or update the matching entry in `routes.yaml`.
6. Keep the route table in `README.md` in sync with `routes.yaml`.
7. Use the stable raw URL on the `main` branch: `https://raw.githubusercontent.com/mict-zhaw/hikes/main/gpx/<slug>.gpx`.
8. Build the swisstopo web/import URL by Base64-encoding the raw GPX URL and appending it to `https://swisstopo.app/u/`.
9. Also generate the direct app deep link in the form `swisstopo://share?url=<URL-encoded Base64 raw GPX URL>` when useful for Notion/mobile use.
10. If a `notion_page_id` exists, update that exact Notion page instead of matching by title.
11. Preserve the original source link in Notion; store GPX and swisstopo links in their dedicated properties.
12. Prefer one commit per route import or logically grouped update.

## Route metadata

Each `routes.yaml` entry should contain when available:

- `slug`
- `name`
- `region`
- `from`
- `to`
- `distance_km`
- `ascent_m`
- `descent_m`
- `gpx`
- `raw_gpx_url`
- `swisstopo_url`
- `swisstopo_app_url`
- `notion_page_id`

Do not invent missing route facts. If values are uncertain, leave them empty or add a short note instead of guessing.
