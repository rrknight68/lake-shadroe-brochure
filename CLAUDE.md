Do not pause and ask me to type 1 or 2 to continue.
Execute all steps autonomously and report back when complete.

## Repo layout
Single-page Lake Shadroe brochure (v8 build). 5.3MB HTML at the root with base64-embedded
images. Vercel project: `lake-shadroe-brochure` (sourceless GitHub link — no auto-deploy from
push; ship with `vercel --prod` from this directory).

## Co-broker pattern (added 2026-06-03)
Each co-broker gets a standalone HTML at `brokers/<slug>.html` (clone of index.html with a
"Your Local Broker" block spliced in just before `<!-- CLOSING -->`). Headshots live at
`public/assets/brokers/<slug>.jpg`. The clean URL `/brokers/<slug>` is wired via
`vercel.json` rewrites. Companion PDF goes at repo root as
`Lake-Shadroe-Brochure-<First>-<Last>.pdf`, generated with:

  /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --headless=new \
    --disable-gpu --no-pdf-header-footer --print-to-pdf=<out>.pdf \
    --virtual-time-budget=15000 file://$(pwd)/brokers/<slug>.html

## Known state caveats
- `index.html` is truncated mid-`data-cfemail` attribute at EOF (deployed that way for 35+
  days; browsers recover). Derivative files under `brokers/` replace the truncated tail with
  a clean mailto + `</body></html>`.
- No FL Realtor license footer is in the source brochure — Lake Shadroe Resort & Marina LLC
  is the developer/seller and is exempt from FL cooperating-broker advertising rules.
  Co-brokers featured on derivative versions need no license number on the brochure either.
- **Do NOT remove `"outputDirectory": "."` from `vercel.json`** while the `public/`
  directory exists. Vercel auto-detects `public/` as a Next.js project and produces
  empty-routed deployments (every route 404s) without the explicit override.
- **PDF generation requires HTTP, not file://** — Chrome's `file://` loader resolves the
  leading `/` in `<img src="/public/...">` against the filesystem root, so the headshot
  won't load. Spin up `python3 -m http.server 8765 --bind 127.0.0.1` in the repo root,
  then point Chrome headless at `http://127.0.0.1:8765/brokers/<slug>.html`.
