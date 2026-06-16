# Margin OS — Landing page (marginos.in)

A standalone, static waitlist landing site for **marginos.in**. It is intentionally
**separate from the Shopify app** (which stays on `app.marginos.in`) so deploying or
editing it never touches the app that's in App Store review.

## Files
- `index.html` — landing page + waitlist form
- `privacy.html` → served at `/privacy` (the URL submitted to Shopify)
- `terms.html` → served at `/terms`
- `vercel.json` — `cleanUrls` so `/privacy` and `/terms` work without `.html`

No build step. It's plain HTML/CSS/JS.

## 1. Set up the waitlist (Formspree — free, ~2 min)
1. Create a free form at https://formspree.io → copy its endpoint (looks like `https://formspree.io/f/abcdwxyz`).
2. In `index.html`, replace **both** occurrences of `YOUR_FORM_ID`:
   `action="https://formspree.io/f/YOUR_FORM_ID"` → your real endpoint.
   (There are two forms: the hero and the bottom CTA.)
3. Submissions arrive in your Formspree inbox / email. Export anytime.

_Alternatives if you prefer: a Tally embed, or a tiny Vercel function writing to your
Neon DB (you already have one) — ask and I can wire either later._

## 2. Deploy as a NEW Vercel project
> ⚠️ Do **not** add these files to the Shopify app repo — keep this its own project so
> the app deployment is never affected.

- Push this folder to a new Git repo (or `vercel deploy` from here), and import it as a
  **new Vercel project**. Vercel auto-detects a static site (no framework/build needed).
- Deploy and test it first on the temporary `*.vercel.app` URL:
  - `/` → landing loads, waitlist works
  - `/privacy` and `/terms` → load correctly

## 3. Point marginos.in at this project
Your apex `marginos.in` + `www.marginos.in` currently sit on the **app** project. Move
them to this landing project (this does NOT affect `app.marginos.in`):

1. Vercel → **app project (margin-os)** → Settings → Domains → **remove** `marginos.in`
   and `www.marginos.in`. (`app.marginos.in` stays — untouched.)
2. Vercel → **this landing project** → Settings → Domains → **add** `marginos.in`
   (set as primary) and `www.marginos.in` (redirect to apex). DNS already points to
   Vercel, so it just reassigns; SSL re-issues in a minute.

Tip: have this project fully deployed and tested on its `.vercel.app` URL **before** the
move, so `/privacy` is ready the instant the domain switches (minimal downtime while the
app is in review).

## Result
- `marginos.in` → this landing page + waitlist
- `marginos.in/privacy`, `marginos.in/terms` → preserved (what you submitted to Shopify)
- `app.marginos.in` → the Shopify app, **unchanged**
