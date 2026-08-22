# Hiking Route Agent

This repository stores public GPX tracks for hiking routes and provides stable URLs that can be opened from swisstopo and referenced from Notion.

## Workflow

When adding or updating a hike:

1. Read `AGENT.md`, `routes.yaml`, and `README.md` first.
2. Keep GPX files in `gpx/` using a lowercase kebab-case slug, e.g. `gpx/vergeletto-lago-dalzasca-lodano.gpx`.
3. **Treat the uploaded GPX as the canonical source artifact. Store it byte-for-byte unchanged.** Never simplify, regenerate, normalize, reserialize, strip extensions, reduce track points, alter line endings, or otherwise rewrite it before publishing.
4. Compute and record a SHA-256 checksum of the uploaded GPX before publication. After publishing, verify that the published file has the same byte length and SHA-256 whenever the tooling permits.
5. GPX analysis is read-only and separate from publication. It may derive distance, ascent, descent, start/end, waypoints, and notes, but it must never write those changes back into the GPX.
6. If the available GitHub write path cannot preserve the original file exactly, **do not create a substitute GPX**. Stop the GPX publication step and require an exact-file upload path instead. Metadata/README/Notion work may continue only if it does not falsely claim the GPX was published.
7. Never overwrite an existing GPX unless the user explicitly asks to replace that route. When replacing a broken derivative with the user's original upload, the original upload is authoritative.
8. Add or update the matching entry in `routes.yaml` only after the final GPX path is known.
9. Keep the route table in `README.md` in sync with `routes.yaml`.
10. Use the stable raw URL on `main`: `https://raw.githubusercontent.com/mict-zhaw/hikes/main/gpx/<slug>.gpx`.
11. Build the swisstopo web/import URL by Base64-encoding the raw GPX URL and appending it to `https://swisstopo.app/u/`.
12. Also generate the direct app deep link as `swisstopo://share?url=<URL-encoded Base64 raw GPX URL>` for mobile use.
13. Only write GPX/Swisstopo URLs to Notion after confirming that the raw GPX URL resolves to the intended published file.
14. If a `notion_page_id` exists, update that exact Notion page instead of matching by title.
15. Preserve the original source link in Notion; store GPX, Swisstopo web, and Swisstopo app links in their dedicated properties.
16. Prefer one commit per route import or logically grouped update.

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
- `gpx_sha256`
- `raw_gpx_url`
- `swisstopo_url`
- `swisstopo_app_url`
- `notion_page_id`

Do not invent missing route facts. If values are uncertain, leave them empty or add a short note instead of guessing.
