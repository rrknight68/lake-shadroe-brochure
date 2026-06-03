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
- No FL Realtor license footer (SL3481954) is in the source brochure — the closing strip is
  just a SVG "VANTAGE POINT INVESTMENTS" wordmark with a contact email. If/when compliance
  language gets added, add it once at the source so every broker version inherits.
