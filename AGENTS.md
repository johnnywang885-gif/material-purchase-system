# AGENTS.md

## Deployment (critical)

GitHub Pages serves from `gh-pages` branch, **not** `master`.

After any change to `index.html` or `README.md`:
```bash
git add index.html README.md
git commit -m "..."
git push origin master
# Then sync to gh-pages:
git checkout gh-pages && git merge master && git push origin gh-pages && git checkout master
```

Forgetting the `gh-pages` push means the live site never updates.

## Architecture

Single-file app: `index.html` (HTML + CSS + JS, ~1060 lines). No build step, no npm, no framework.

- **Firebase Realtime Database** for cross-device sync (compat SDK via CDN, not ES modules)
- **localStorage** as offline fallback only
- **Room ID** via URL param `?room=<name>`, default `wgcs`
- Firebase config and DB region are hardcoded in `index.html` (databaseURL points to `asia-southeast1`)

## Firebase

- Project: `wgcs-purchase`
- Database region: **asia-southeast1** (Singapore), not asia-east1
- Rules: `.read: true, .write: true` (public, no auth)
- SDK: Firebase v9 compat (`firebase-app-compat.js`, `firebase-database-compat.js`)

## Admin password

`0519` — stored in plaintext in `index.html`. Admin login required to view summary and print tabs.

## Gotchas

- Firebase SDK loaded via `<script>` CDN tags, not npm imports — do not add `import` statements
- `event.target` is used in `switchTab()` — do not refactor to arrow callbacks without fixing this
- Mobile responsive CSS at `@media (max-width: 768px)` and `420px` — test changes on both breakpoints
- All CSS/JS is inline in `index.html` — no separate files
