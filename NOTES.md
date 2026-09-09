# Landing v2 notes

## Changed

- Repositioned the Oracle host as the live, content-addressed artifact shelf and write origin.
- Put the POST contract and `/v1` OpenAPI route in the hero; made POST JSON the normal publish flow and PRs a fallback.
- Added the five exact API routes, package fields, idempotency rule, limits, allowed types, retrieval states, and the `ACCEPTED` / `REPLICATED` distinction.
- Corrected shelf states: the Cookies remainder sketch is hosted at `/sketches/cookies.html`; Calculator v5.1 is Pages-primary; skills/receipts remain planned.
- Replaced the obsolete live anchor language with the current manifest SHA-256, previous SHA-256, byte size, two known objects, and the witnessed-v0 clarification.
- Updated `index.json` with the API object, shelf states, manifest data, and v2 provenance string.

## Kept

- The existing `systems/light-mode-paper-technical` language: warm paper, dark external frame, diagonal paper texture, bracket corners, technical labels, restrained blue, compact monospace details, and no JavaScript.
- Relative internal links, single-file landing structure, skip link, visible keyboard focus treatment, and reduced-motion handling.
- `site/sketches/cookies.html` was not changed.

## Responsive note

At 360 px, navigation becomes a two-column link grid, hero/API/shelf grids collapse to one column, long hashes wrap, and publish steps stack. The OpenAPI URL may wrap within the dark endpoint block by design; no horizontal scrolling is introduced by its layout rules.
