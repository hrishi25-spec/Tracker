# Team Task Trackers

Self-contained, single-file HTML task trackers for hackathon teams. No build step, no dependencies — open the file in a browser and go.

## Files

| File | What it is |
|---|---|
| `index.html` | **Face Liveness & PAD** build tracker — MOSIP Decode 2026, Registration Client, two-person team (owners A / B). 34 tasks across 6 phases. |
| `SIH PLAN.html` | **Checkpoint Screening** build tracker — SIH project, six-person team. Syncs through a Firebase Realtime Database URL configured in the file. |
| `tracker.html`, `new` | Earlier iterations of the Face Liveness & PAD tracker, kept for reference. `index.html` supersedes them. |

## Running

No install or build needed. Either open `index.html` directly, or serve the folder:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## How persistence works

- **`index.html`** uses the host environment's `window.storage` API when available, and falls back to `localStorage` everywhere else, so progress survives reloads on any device. The sync indicator in the header is honest about which mode is active:
  - `synced` — host storage (shared across the host's sessions)
  - `saved on this device` — localStorage only (per-browser, per-device)
  - `offline` — the save failed; check the browser's storage permissions
- **`SIH PLAN.html`** writes to Firebase Realtime Database (configured via `FIREBASE_URL` near the bottom of the file) with localStorage as offline fallback, and polls every few seconds so teammates see each other's updates.

Note: with `index.html` alone, progress is **per-device** — two teammates each keep their own copy of the checklist. True cross-device sharing needs a backend (as in `SIH PLAN.html`).

## Customizing

Task data lives in plain JS constants at the top of each file's `<script>` block:

- `index.html` — the `PHASES` array (phases → tasks with `id` and `owner`: `'A'`, `'B'`, or `'both'`)
- `SIH PLAN.html` — the `PHASES` and `ROLES` arrays (roles → tasks with a phase `p`)

Edit those, and the UI (progress gauge, counts, legend) recomputes itself.

## Cleanup candidates

`tracker.html` and `new` are older drafts of the same tracker and can be deleted once you're happy with `index.html`.
