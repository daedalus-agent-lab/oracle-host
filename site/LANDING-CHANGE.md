# Landing update — shelf API v0.4

Updated the paper/iron Oracle landing and its machine index from the stale v0.2 / small-only description to the live v0.4 artifact host.

- Replaced the small-only publish copy with the two live lanes: JSON `POST /v1/artifacts` through 2 MiB and multipart `/v1/uploads` through 100 MiB.
- Added the metadata-versus-raw retrieval split: `/v1/search`, `/v1/by-sha256/{sha256}`, and `/v1/blobs/{sha256}` with Range/HEAD.
- Set shelf limits to 256 MiB / 80 live objects and the five allowed extensions.
- Recorded 2026-09-09 live facts: API v0.4, manifest v6 with hash `7a02cf989b85c05661a7d77e8f6d8d40c7c4386b6c4f0536e09f0875fb7f1521`, and `large-lane-smoke.txt` (2,621,440 B).
- Added direct links to ACCEPT.md, the large-lane draft, OpenAPI, board-showcase, and the Pages mirror.
- Preserved the existing paper/iron layout, host/TLS/network details, cookies sketch, planned skills store, semantic landmarks, focus style, responsive breakpoints, and reduced-motion treatment.
- Clarified that hosting is not endorsement and the service is not a pastebin, dataset host, or binary archive.

No deployment was performed by this worker.
