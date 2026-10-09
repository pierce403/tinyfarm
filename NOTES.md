# Hosting notes

## 2026-10-09: Cloudflare migration preparation

Refreshed HTTPS origin `pierce403/tinyfarm` on production `main` at
`84bba62d94a66bfb320095424f9d80d7bc9a0467`; local and remote heads matched before changes.
Prepared Static Assets Worker `tinyfarm` in account
`b6d1478423f0bb0c0477df387305e46b`, with workers.dev enabled, directory redirects
and generated real 404s. Logs/traces are configured for Worker execution;
direct static asset responses do not run application code. The initial Worker-only
configuration omitted custom routes; the follow-up below declares the hostname.

Wrangler remains pinned to 4.145.0. Its bundled workerd rejects compatibility
date 2026-10-09 and supports dates through 2026-10-07, so these asset-only
Workers use 2026-10-07 for matching local and production configuration. No
runtime API requires a later date. A deployment dry run alone did not catch
this runtime constraint.

The pinned Python lock preserves the previously verified MkDocs 1.6.1 / Material
9.7.7 renderer and existing plugins. Original requirements and source CNAME remain;
the Worker build excludes output CNAME. GitHub Actions now validates only and
retains a site artifact, preserving the last Pages deployment for recovery.
Cloudflare build `npm run build` and deploy `npx wrangler deploy` avoid duplicate
builds; credentials belong in Cloudflare, never GitHub secrets.

Validation passed with Python 3.12.7: strict pinned MkDocs build, Wrangler
4.145.0 deployment dry run (74 asset files, no bindings), and
8 local Wrangler HTTP checks. The checks verify homepage/article
canonical URLs, directory 307 redirects, linked CSS, populated search
(8 entries), and genuine missing-page 404s. Output CNAME and absent
robots.txt return 404, preserving source/recovery files and current robots behavior.
Local test servers were stopped after verification. Native Git
connection, exact deployed commit, workers.dev checks, domain attachment and
DNS/mail cutover remain pending separate verification steps.

Added the missing canonical site URL `https://tinyfarm.io/`. Pre-existing untracked
`site/` was generated baseline output and is now ignored; source content is preserved.

## 2026-10-09: superseded local Custom Domain proposal

The earlier local commit `5b9b9905006314cacd1a7a4a1306e0161d91a31b` prepared an exact Custom Domain and was
not pushed. Later native pilot attempts on other migration zones failed with
Cloudflare error 100117 because imported externally managed apex A records
blocked Custom Domain provisioning. Wrangler's noninteractive override flags
did not replace those records. The ordinary-route child below supersedes that
proposal without rewriting its history or deleting DNS records.

## 2026-10-09: ordinary Worker route preparation

The source now prepares
`routes: [{"pattern": "tinyfarm.io/*", "zone_name": "tinyfarm.io"}]`.
The pattern covers paths only on `tinyfarm.io`; no other hostname is added.
All other Wrangler settings, workers.dev availability, compatibility date
2026-10-07, pinned Wrangler 4.145.0, renderer, dependencies, build scripts and
public content remain unchanged. There is no Worker script (`main`) or origin
fetch fallback. Static Assets retains directory redirects and terminal
`404-page` responses for missing paths.

For cutover, retain the original apex website A values `185.199.108.153`, `185.199.109.153`
with Cloudflare proxying enabled. Ordinary routing uses the existing proxied
records; it does not provision Custom Domain DNS or require deleting them.
Retain source CNAME, previous Pages deployment and DNS snapshots for recovery;
mail, identity, verification and unrelated records remain in the import checks.

Before this edit, the clean `main` checkout was at `5b9b9905006314cacd1a7a4a1306e0161d91a31b` and refreshed
HTTPS `origin/main` remained at the initial Worker-only commit `30fb44d1c3bf2016c30214dde1febfba55ff4e6f`.
All four Group B native connections and 34 workers.dev HTTPS checks were
recorded by 2026-10-09 22:00:08 UTC, separately from exact Git deployment identity.
The zone snapshot at 22:05:57 UTC recorded `tinyfarm.io` as pending; dashboard
certificate observations through 22:07:55 UTC recorded Pending Validation(TXT). These
dated observations do not establish this new route's live deployment.

Release gates are an active zone, active certificate, verified proxied DNS import,
successful native Git build for the exact new commit and its deployed version,
then credential-free custom-host HTTPS, HTTP upgrade, wiki pages/canonicals,
CSS, populated search, configured feeds and a genuine 404. Confirm preserved
DNS/mail records separately. Tinyfarm's pending zone/certificate observations
require fresh confirmation before its route can be released.

The ordinary-route strict locked build and Wrangler 4.145.0 dry run passed at
2026-10-09T22:20:37.416433+00:00. Validation used Python 3.12.7 and Node.js 24.19.0;
the repository's production runtime selectors remain unchanged. Wrangler reported
74 asset entries, no bindings and successful dry-run completion. Generated
index, real 404 and populated search (8 entries) are present;
source CNAME is preserved and output CNAME is excluded. The only changes are
`wrangler.jsonc`, `README.md`, `AGENTS.md` and this note. No push, remote deployment,
CF/DNS/registrar or runtime mutation occurred during this local source preparation.

References:

- https://developers.cloudflare.com/workers/configuration/routing/routes/
- https://developers.cloudflare.com/workers/static-assets/routing/static-site-generation/
