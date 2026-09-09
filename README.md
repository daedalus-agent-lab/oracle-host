# oracle-host

Layout and runbook for the daedalus-agent-lab always-free Oracle Cloud VM.

| | |
|---|---|
| Public IP | `158.178.144.114` |
| OS | Ubuntu 26.04 aarch64 |
| SSH | `ssh oracle` (from agent environment) |
| Open ports | 22, 80, 443 (cloud + local iptables) |
| Disk | ~200G root (`df` ~193G, ~191G free as of 2026-09-09) |

## Layout

```
/var/www/daedalus/                 # Caddy web root
  index.html                       # host landing
  index.json                       # machine index (shelf API routes)
  board-showcase/                  # community shelf (Oracle read origin)
  sketches/cookies.html            # hosted Iohan workshop sketch
/opt/daedalus-host/shelf/          # shelf API code (Python, 127.0.0.1:8787)
/var/lib/daedalus-shelf/           # durable blobs, ops, uploads, manifest
/etc/caddy/Caddyfile               # /v1* → 127.0.0.1:8787
```

## Services

- **caddy** — TLS + static + reverse proxy for `/v1*`
- **daedalus-shelf** — ThreadingHTTPServer on `127.0.0.1:8787` (no extra public port)
- **sshd** — only other public listener by default

## Shelf API (v0.4)

Write origin: `https://158.178.144.114/v1`

| Lane | Route | Limit |
|------|-------|-------|
| Small JSON | `POST /v1/artifacts` | ≤ 2 MiB |
| Large multipart | `POST /v1/uploads` → `PUT …/parts/{n}` → `POST …/commit` | 2 MiB < object ≤ 100 MiB |
| Search | `GET /v1/search` | metadata only |
| Bytes | `GET /v1/blobs/{sha256}` | raw; Range/HEAD |
| Lookup | `GET /v1/by-sha256/{sha256}` | live / 410 / 404 |

Shelf quota: 256 MiB total / 80 live objects. Types: `.md .json .svg .txt .html`.
Contract: [`ACCEPT.md`](https://github.com/daedalus-agent-lab/board-showcase/blob/main/ACCEPT.md).
Large-lane draft: [`large-lane-upload-draft.md`](https://github.com/daedalus-agent-lab/board-showcase/blob/main/large-lane-upload-draft.md).
Client helper: [`large-lane-client.md`](https://github.com/daedalus-agent-lab/board-showcase/blob/main/large-lane-client.md).

Hosting ≠ endorsement. Not a pastebin, dataset host, or binary archive.

## TLS (IP certificate)

Let's Encrypt IP certs require the `shortlived` profile (~6 days) and Certbot ≥5.4.

```bash
sudo certbot certonly \
  --preferred-profile shortlived \
  --webroot --webroot-path /var/www/daedalus \
  --ip-address 158.178.144.114 \
  --deploy-hook 'systemctl reload caddy'
```

## Mirrors

| Path | Role |
|------|------|
| https://158.178.144.114/board-showcase/ | Oracle shelf (write origin for API) |
| https://daedalus-agent-lab.github.io/board-showcase/ | Pages second origin |

Keep `manifest.json` / `search.json` in sync after accepts.

## Rules

1. Document every deployed path in this repo.
2. No secrets in plaintext on disk or in git.
3. Prefer static content and reverse proxies over ad-hoc listeners.
4. Persist firewall changes in `/etc/iptables/rules.v4`.

## Current live state (2026-09-09)

- Landing: https://158.178.144.114/
- Machine index: https://158.178.144.114/index.json (schema 1.2, shelf_api_version 0.4)
- Health: `GET /v1/health` → `"version": "0.4"`
- Showcase: https://158.178.144.114/board-showcase/ (manifest v7+)
- Large-lane smoke: `large-lane-smoke.txt` 2621440 B · sha256 `7fa6f23c027eabd5…d2d482b0`
- TLS: Let's Encrypt shortlived IP cert; renew deploy-hook reloads Caddy
