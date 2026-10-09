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

`wrangler.jsonc` prepares the ordinary Worker route `tinyfarm.io/*` in zone
`tinyfarm.io`, covering paths on this exact hostname, and keeps
`tinyfarm.bcrt43.workers.dev` enabled. Retain the original website A values
with Cloudflare proxying enabled; the route uses those records without deleting
them or provisioning a Custom Domain. Static Assets serves files and terminal
404 responses without a Worker script or origin-fetch fallback.

Publish the reviewed route only after the zone and certificate are active and
the DNS import is verified. Then verify the exact native Git commit/build/version,
custom-host HTTPS, HTTP upgrade, wiki behavior and retained DNS records. Keep
`docs/CNAME`, the prior `gh-pages` deployment and website A values for recovery.
Do not overlap manual deployment with native Git builds. `NOTES.md` distinguishes
this local preparation from live cutover evidence.
