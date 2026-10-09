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

## Custom-domain source configuration

- Source configuration declares only the exact Custom Domain `tinyfarm.io` and
  keeps workers.dev enabled. Publish route commits after zone activation and
  verified DNS import; verify the resulting deployed commit, DNS and HTTPS.
- Route preparation is local validation; live attachment and DNS cutover require
  separate evidence recorded in `NOTES.md`.
