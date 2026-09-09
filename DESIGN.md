# Oracle host — design brief

## Subject, audience, job
A public, permanent address for small, verifiable work made by people and agents. A visitor should be able to find the shelf and understand its rules; an agent should be able to fetch the same facts without parsing the page.

## Direction
**`systems/light-mode-paper-technical`**. The host is rendered as an address plate and archive ledger: a warm, lightly gridded paper field held inside a near-black perimeter. The direction suits a static host because it makes file paths, hashes, and status cues feel native to the page rather than bolted-on developer chrome.

The split in the hero has a job: the left side answers “what is this address?” and the right side is the machine-readable dispatch plate. Lower sections use shelves and ledger rows, not a generic set of marketing cards.

## Visual system

| Token | Hex | Use |
|---|---:|---|
| Charcoal frame | `#161817` | page surround, dark rails, active controls |
| Iron | `#29302d` | secondary dark detail |
| Field paper | `#e9e4d7` | main reading surface |
| Ledger paper | `#f5f1e7` | raised or inset surface |
| Graphite ink | `#20231f` | headings and body text |
| Dust ink | `#62675e` | supporting copy and rules |
| Signal blue | `#165f83` | active link, verified signal, focus ring |
| Clay seal | `#a64732` | limited warning / future-state marker |

**Type:** `Arial Narrow` / `Roboto Condensed` if installed, falling back to a compact system sans for display and navigation; an ordinary system sans for reading; `ui-monospace` only for literals, checksums, paths, and operational data. No externally loaded font is necessary for the host to remain reliable.

**Scale:** 12px utility, 15–17px body, 21–28px section titles, `clamp(44px, 7.3vw, 106px)` hero. Body line length stays under 68ch.

**Spacing:** 8px base. Major composition gaps: 32/56/88px. The desktop frame has 20px outer padding; it tightens to 10px on a phone.

**Shape and depth:** the outer paper sheet has a 22px radius; internal information is crisp, 0–4px. Rules, brackets, and a low-contrast diagonal paper grid carry structure. Shadows only lift the whole sheet from the charcoal field.

## Hero idea
The `158.178.144.114` address is treated as a typographic object: an oversized IP engraved into a paper dispatch plate. A short status matrix sits alongside it, so the page’s first visual is also its most useful fact.

## Motion and access
Only a brief page-load settling of the main sheet and a link underline state. All motion is removed for `prefers-reduced-motion`. Visible keyboard focus, semantic landmarks, stable paths, high contrast, and no JavaScript dependency.

## Deliberate exclusions
- No dark SaaS card grid, neon/cyber imagery, gradients as decoration, stock art, testimonials, counters, invented activity, or faux dashboard.
- No tracked all-caps eyebrow parade, decorative middle-dot metadata, or arrows added to link labels.
- No hash shown as evidence unless it comes from the supplied manifest.
- No claim that artifacts are endorsed by this host.

## Anti-slop review
The generic card stack was replaced with shelf rails and a dispatch plate because they encode actual concepts: the host address, files, and acceptance checks. The accent colors only distinguish trusted paths and planned/future work. There are no decorative icons; every rule and bracket defines a content boundary or location.
