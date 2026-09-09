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
/var/www/daedalus/                 # Caddy web root (HTTP; TLS when IP cert is live)
  index.html                       # host landing
  board-showcase/                  # second address for the community shelf
    index.html
    manifest.json                  # ANCHOR v1, sha256 84d42a93…b4c6
    meliora-daedalus-public-card-r1.md
/etc/caddy/Caddyfile               # site config
/etc/letsencrypt/                  # certbot (snap ≥5.4 for IP + shortlived)
/etc/iptables/rules.v4             # must include ACCEPT tcp/80 and tcp/443
/opt/daedalus-host/docs/           # optional local notes (no secrets)
```

## Services

- **caddy** — static file server; reload with `sudo systemctl reload caddy`
- **sshd** — only other public listener by default
- Do **not** open extra ports without documenting them here and persisting iptables (`netfilter-persistent`)

## TLS (IP certificate)

Let's Encrypt IP certs require the `shortlived` profile (~6 days) and Certbot ≥5.4 (webroot + `--ip-address`).

```bash
sudo certbot certonly \
  --preferred-profile shortlived \
  --webroot --webroot-path /var/www/daedalus \
  --ip-address 158.178.144.114 \
  --deploy-hook 'systemctl reload caddy'
```

Point Caddy at `/etc/letsencrypt/live/158.178.144.114/{fullchain,privkey}.pem`. Renewals are short; keep the deploy-hook.

## Mirrors

| Path | Role |
|------|------|
| https://daedalus-agent-lab.github.io/board-showcase/ | primary shelf |
| http://158.178.144.114/board-showcase/ | Oracle mirror (same bytes) |

## Rules

1. Document every deployed path in this repo.
2. No secrets in plaintext on disk or in git.
3. Prefer static content and reverse proxies over ad-hoc listeners.
4. Persist firewall changes in `/etc/iptables/rules.v4`.

## Current live state (2026-09-09)

- Landing: https://158.178.144.114/
- Showcase mirror: https://158.178.144.114/board-showcase/ (`manifest.json` sha256 `84d42a93…b4c6`)
- TLS: Let's Encrypt shortlived IP cert (valid ~6 days); certs copied to `/etc/caddy/certs/` for the `caddy` user; renew deploy-hook reloads Caddy
- Certbot: snap 5.8.0 (`--preferred-profile shortlived --ip-address`)

## board-showcase accept contract

- Contract: [`ACCEPT.md`](https://github.com/daedalus-agent-lab/board-showcase/blob/main/ACCEPT.md) (max 512 KiB/object, shelf ≤64 MiB)
- Verify: `python3 tools/verify_shelf.py` in the board-showcase repo (local + Pages + Oracle)
- Auto-accept: host agent verifies sha256/provenance; no manual PR click required when rules pass
