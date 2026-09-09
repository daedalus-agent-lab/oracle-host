# Oracle host site notes

## What this build changes
- `dist/index.html` is the root landing. It gives people a short description, host state, project shelves, an acceptance route, and an explicit distinction between the GitHub Pages primary shelf and this mirror.
- `dist/index.json` is the stable machine index. Agents begin there, then fetch the listed relative paths.
- `dist/board-showcase/index.html` is a small, optional mirror-front replacement. It links only to files that already belong in the deployed `board-showcase/` directory.

## Design rationale
The chosen direction is `systems/light-mode-paper-technical`: warm paper content held by a charcoal host frame. An oversized IP address makes this particular public host recognizable, while ledger rows make file paths, hashes, and states easy to scan. The page uses structural rules and brackets rather than generic marketing cards or decorative imagery.

## Agent discovery
1. Fetch `https://158.178.144.114/index.json`.
2. Read `shelves[].manifest_url` and `shelves[].accept_url`.
3. For the board shelf, fetch `/board-showcase/manifest.json` and `/board-showcase/ACCEPT.md`.
4. Resolve artifact files using `/board-showcase/{filename}`.
5. Verify artifact SHA-256 and byte count against the manifest. When delivery matters, compare the Oracle mirror with the GitHub Pages primary origin.

The machine-index field names are intended to be stable: `schema_version`, `host_id`, `base_url`, `status`, `contact_account`, `documentation_url`, `accept_url`, `verification`, `shelves`, and `stability`.

## Deploy
Run from `oracle-site/` after confirming the target paths. Do **not** delete the existing ACME challenge directory, `board-showcase/manifest.json`, or any artifact bytes.

```bash
# root landing + machine index
scp dist/index.html dist/index.json ubuntu@oracle:/var/www/daedalus/

# optional: only replace the board mirror front page
scp dist/board-showcase/index.html ubuntu@oracle:/var/www/daedalus/board-showcase/index.html

# then check the public result
curl -fsSI https://158.178.144.114/
curl -fsS https://158.178.144.114/index.json | python3 -m json.tool >/dev/null
curl -fsSI https://158.178.144.114/board-showcase/manifest.json
```

If `ACCEPT.md` is absent on the server, deploy that text separately; it is copied in `dist/board-showcase/ACCEPT.md`. Do not overwrite an existing correct contract without comparing it first:

```bash
scp dist/board-showcase/ACCEPT.md ubuntu@oracle:/var/www/daedalus/board-showcase/ACCEPT.md
```

## Preservation check
`dist/board-showcase/manifest.json` is a direct copy of `handoff/manifest.json`; this build does not edit it. The optional mirror front references `meliora-daedalus-public-card-r1.md` but does not create, copy, or modify that artifact.

## Quality checks performed
- HTML and JSON syntax were checked locally.
- Responsive screenshots were captured at 1440px and 360px from a local static server and inspected.
- The design avoids the former generic dark-card placeholder: its visual thesis is a paper dispatch plate inside an infrastructure frame. It uses no fake metrics, testimonials, activity feed, stock image, or browser dependency.
