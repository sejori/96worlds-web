# 96worlds-web

Marketing site for **96worlds.gg** — *Create a world. Write the rules. Bring your friends!* — plus a placeholder for the upcoming Web Studio. Deployed to GitHub Pages at <https://sejori.github.io/96worlds-web/>.

## What's here

- **Landing page** (`index.html`): hero, template showcase carousel (six starter templates with prompt + recorded gameplay video), "how it works", and OS-aware launcher download cards.
- **Web Studio placeholder** (`studio.html`): "under construction" page; the planned design is in [`docs/web-studio-handover.md`](./docs/web-studio-handover.md).
- **Localisation**: English, Português (BR) and 日本語 via the zero-dependency `src/i18n.ts`.
- **Theming**: light/dark following the system setting, with a stored override (`96worlds_theme`).
- **Auth client** (`src/auth.ts`): Google Identity Services plus a guest creator mode; the session JWT is stored in `localStorage` under `playable_session_jwt` (legacy key name, kept for compatibility).

Brand: brush-stroke display type (Knewave) and a 世界 *sekai* ("world") hanko seal on a white · blue · red palette. The wordmark is **96** (red) + **worlds** (ink) + **.gg** (blue).

## Develop

```bash
pnpm install
pnpm run dev          # http://127.0.0.1:5173
pnpm run build        # typecheck + production bundle in dist/
pnpm exec playwright test   # E2E suite against the dev server
```

Brand assets (`public/og-image.png`, `favicon.png`, `apple-touch-icon.png`) are regenerated with `node scripts/generate_brand_assets.js` (needs Playwright's Chromium and network access for the web fonts).

## Configuration

`.env` holds public, non-secret build values:

| Variable | Purpose |
|---|---|
| `VITE_SITE_URL` | Absolute site URL, used for OpenGraph / Twitter card URLs |
| `VITE_LAUNCHER_RELEASES_URL` | GitHub Releases "latest/download" base for the launcher binaries |
| `VITE_API_BASE_URL` | Backend base URL (defaults to the Cloud Run `96worlds-server` deployment; set as a repo variable in CI) |

Launcher downloads are expected as release assets named `96WorldsLauncher-win64.exe`, `96WorldsLauncher-macOS.dmg` and `96WorldsLauncher-x86_64.AppImage` in `sejori/96worlds-client`.

## Deployment

Pushing to `main` runs `.github/workflows/deploy.yml`: install → Playwright tests → build → publish `dist/` to GitHub Pages. The site is served under the sub-path `/96worlds-web/`, so `vite.config.ts` uses `base: './'` and every asset path must be **relative** (`./showcase/...`, never `/showcase/...`).

## Related repositories

- [`96worlds-server`](https://github.com/sejori/96worlds-server) — Rust control plane and build pipeline.
- [`96worlds-client`](https://github.com/sejori/96worlds-client) — Godot client.
