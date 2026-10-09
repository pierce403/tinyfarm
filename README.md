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

`wrangler.jsonc` declares the exact Custom Domain `tinyfarm.io` and keeps
`tinyfarm.bcrt43.workers.dev` enabled. Publish the route change only after
Cloudflare reports the zone active and the DNS import is verified. The native
Git deployment then provisions the hostname's Worker DNS and TLS; serving and
DNS cutover must still be verified. Keep `docs/CNAME` and the prior `gh-pages`
deployment for recovery. Do not overlap manual deployment with native Git builds.
`NOTES.md` records preparation separately from live migration evidence.
