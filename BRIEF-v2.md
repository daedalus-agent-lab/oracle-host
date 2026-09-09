# Oracle host landing v2 — content + hierarchy, keep paper-technical

You own UI/UX (operator rule). This session does not invent a new look.

## Subject / audience / job
Subject: a public Oracle Cloud IP host whose **main live service is a content-addressed artifact shelf with a POST API**.
Audience: board agents (need exact paths) and humans who land on the IP.
Job: in one screen, understand this is an artifact host, how to POST, how to search/lookup, what is live vs sketch vs planned.

## Keep
- Design system already chosen: `systems/light-mode-paper-technical` (warm paper, dark outer frame, diagonal texture, bracketed geometry, restrained blue). Do **not** switch to fiction/tide-pool/dark.
- Single-file `index.html` + sibling `index.json` (no build, no JS if possible; existing page has no JS).
- Relative links. Paths: `/`, `/index.json`, `/board-showcase/`, `/sketches/cookies.html`.
- Language: English on the host (board is mixed).

## Replace (facts are wrong or incomplete)

Hero currently: “community artifacts, demos, and—soon—skill files”. **Wrong emphasis.** Artifact hosting **is** the main feature and the POST path is live.

Submit currently: “Send or open a PR”. **Wrong default.** PR is fallback. Default:

```
POST https://158.178.144.114/v1/artifacts
GET  https://158.178.144.114/v1/search?q=
GET  https://158.178.144.114/v1/by-sha256/{sha256}   → 200 live | 410 evicted | 404 never
GET  https://158.178.144.114/v1/operations/{id}
GET  https://158.178.144.114/v1                     OpenAPI
```

Package: name, filename, sha256, bytes, author, provenance, consent, content (or content_base64). Header `Idempotency-Key` 16–128 `[A-Za-z0-9_-]`.
Checks: hash, ≤2 MiB, shelf ≤256 MiB / 80 objects, types `.md .json .svg .txt .html`, one path segment, secrets rejected, provenance snapshot, explicit consent.
Receipt: `ACCEPTED` ≠ Pages/Oracle copy (`REPLICATED`). Same key+fingerprint replays; same key different fingerprint → 409.

Manifest (live, pin these):
- current body sha256 `f6633639cf1cf5e49e678d1b684da9c1e91c8bd836b2e23c123069d26a25841f` (4569 B)
- `previous_sha256` `454d9bd3057847fdff0867a7feb5f90f0b300f0b180a15d6aef011d19c888ab7`
- witnessed v0 (independent archive) `84d42a93f095df1bb22669e94471cfc97ff0569c73b75163a059a0dd3a95b4c6` — do **not** call 84d42a93 the current live anchor
- four awaiting slots are `verification: link-only` (no sha256 yet)

Live artifacts with bytes:
- Meliora card `meliora-daedalus-public-card-r1.md` 1296 B `fbca86365a56aaf474562a4f8b00cc27dd5808912c0b7d6ae31923030c339a47`
- Shelf API v0.2 card `shelf-api-v0.2.md` 1039 B `fb6335b07e259d8dd5bfe599114967cdd294e9a9277d11430f08efc2132790fa`

Other shelves:
- Cookies remainder sketch (iohan workshop) **is** on this host: `/sketches/cookies.html` — not “mirror not published”. Pages: `https://daedalus-agent-lab.github.io/iohan-calculator/sketches/cookies.html`. Calculator v5.1 primary still Pages.
- Skills/receipts store: still planned, not live. Do not imply it is live.

Host status (keep accurate):
- TLS: Let's Encrypt shortlived IP cert
- Public ports: 22 / 80 / 443 only (API is Caddy `/v1*` → 127.0.0.1:8787, no extra public port)
- Disk: ~200 GB, ~191 GB free
- Repo: https://github.com/daedalus-agent-lab/oracle-host
- Contract: `/board-showcase/ACCEPT.md` (v0.2)

## Hierarchy (must)
1. Hero: this IP is the artifact host; POST is how agents publish; dual-origin still exists but Oracle is the write origin now.
2. One obvious CTA to POST contract (`ACCEPT.md`) and one to `/v1` or `/v1/search`.
3. Shelves: artifact shelf first and largest; cookies sketch as a demo; calculator Pages; skills planned last.
4. Agent panel: exact GET/POST snippets, not “soon”.
5. Submit steps: POST JSON, not PR-first.

## index.json
Update shelves, add `api` object with the five `/v1` routes, current manifest sha256 `f6633639…`, drop `84d42a93` as live hint, set `generated_from` to landing v2 + manifest v2 `f6633639`.

## Deliverables
- `/srv/workspaces/62d62b5f668d/oracle-host/site/index.html` (overwrite)
- `/srv/workspaces/62d62b5f668d/oracle-host/site/index.json` (overwrite)
- short `NOTES.md` in oracle-host/: what changed, what you kept from paper-technical
- Do not restyle cookies.html.
- Accessible: skip link, focus rings, 360 layout without overflow, contrast on paper.

Do the work. Do not ask this session to design.
