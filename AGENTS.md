# TinyFarm wiki operations

- Preserve documentation, public paths, navigation, images, styles and scripts.
- Run `npm run deploy:check` for a locked strict build and packaging validation.
- Cloudflare Workers Builds owns production publishing from `main`; deployment
  credentials stay in Cloudflare and GitHub Actions validates only.
- Preserve `docs/CNAME` and the previous Pages deployment for recovery. Verify
  the Worker before attaching the domain and changing DNS. Avoid overlapping
  manual and native deployments.
- Keep migration evidence and unresolved checks in `NOTES.md`; do not claim a
  live deployment or DNS cutover from a successful local build alone.

## Ordinary Worker route source configuration

- Source configuration prepares the ordinary route `tinyfarm.io/*` in zone
  `tinyfarm.io` and keeps workers.dev enabled. Retain the original website A values
  with Cloudflare proxying; do not delete them for this route or add a Custom Domain.
- Keep this an asset-only Worker: no `main`, origin fetch, or fallback script.
  `404-page` returns a terminal missing-page response. Preserve exact hostname
  scope and all existing asset/build settings.
- Release only after verified zone activation, active certificate and DNS import.
  Check the exact native Git commit/build/version, custom-host HTTPS and HTTP
  upgrade, wiki behavior, real 404 and retained records before claiming cutover.
- Route preparation is local validation; live attachment and DNS cutover require
  separate evidence recorded in `NOTES.md`.
