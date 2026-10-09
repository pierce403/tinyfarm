# tinyfarm.io
Like a tinyhome, but a farm

## Cloudflare publishing

The prepared Worker `tinyfarm` serves MkDocs output directly through Workers
Static Assets, including directory URLs, search, and the generated real 404 page.
Build locally with `npm ci && npm run build`; check packaging with
`npm run deploy:check`, and preview with `npm run dev`.
`requirements.lock` pins the renderer verified for this migration while the
original `requirements.txt` remains available for dependency maintenance.

Connect `pierce403/tinyfarm` in Cloudflare Workers Builds with production branch
`main`, root `/`, build `npm run build`, and deploy `npx wrangler deploy`.
Use all paths and disable builds for other branches. Cloudflare fetches and
publishes each production commit using credentials retained in Cloudflare.
GitHub Actions validates and retains artifacts; it needs no Cloudflare secrets.
`.nvmrc` and `.python-version` select Node.js 22 and Python 3.12.

The configuration initially uses `tinyfarm.bcrt43.workers.dev`. Attach
`tinyfarm.io` only after its Cloudflare zone/import is verified. Keep `docs/CNAME`
and the prior `gh-pages` deployment for recovery. Do not overlap manual deployment
with native Git deployment. Git connection, deployment, domain and DNS verification
remain separate migration steps recorded in `NOTES.md`.
