# WireIP Network™ — IP Tokenization Vault

**Institutional Edition v4.1** · Outside The Wire Productions

A self-contained, single-file web application for tokenizing IP (copyright, patent, trademark, trade secret) into CBIA™ Digital Deeds on the WireIP DAG. Includes wallet, marketplace, smart contracts, AI Sentinel agents, validators globe, treasury, and a military-grade security center.

---

## Files in this repo

| File | Purpose |
|---|---|
| `wireip-dag.html` | The entire application — single self-contained HTML file. CSS + JS + SVG inline. Loads three.js and pdf.js from CDN. |
| `vercel.json` | Vercel configuration. Rewrites `/` → `/wireip-dag.html`, sets security headers, configures PDF caching. |
| `package.json` | Minimal package metadata. `npm start` runs a local static server on `:3000` (uses `npx serve`). |
| `.gitignore` | Standard ignores. |
| `README.md` | This file. |
| `WireIP Network™ Black Paper (Institutional Edition).pdf` | (Optional) Drop this PDF into the project root and the app will auto-extract the cover logo via PDF.js and apply it as the brand mark + splash logo. |
| `WireIP Network™ One Pager.pdf` | (Optional) Reference document. |
| `otw-logo.png` (or any common name) | (Optional) Drop a logo file into the project root and the app picks it up automatically as the brand mark. The auto-discovery checks 48 common filename variants. |

---

## Deploy to Vercel

### Option A — Drag & drop on vercel.com

1. Go to [vercel.com/new](https://vercel.com/new)
2. Click **"Other"** under "Import Git Repository"
3. Drag the entire project folder into the upload area
4. Vercel auto-detects it as a static site, picks up `vercel.json`
5. Click **Deploy**

That's it. You'll get a `https://wireip-network-dag.vercel.app` (or your chosen subdomain) URL within ~30 seconds.

### Option B — Vercel CLI (recommended for iteration)

```bash
# 1. Install the CLI once
npm install -g vercel

# 2. cd into the project folder
cd "/Users/torrancesaunders/Documents/Claude/Projects/WireIP Network DAG/"

# 3. Deploy
vercel

# Follow the prompts:
#   - Set up and deploy? Y
#   - Which scope? (your account)
#   - Link to existing project? N
#   - Project name? wireip-network
#   - Directory? ./
#   - Override settings? N

# 4. Production deploy when you're ready
vercel --prod
```

### Option C — GitHub → Vercel (continuous deployment)

```bash
# 1. Initialize git repo
cd "/Users/torrancesaunders/Documents/Claude/Projects/WireIP Network DAG/"
git init
git add .
git commit -m "Initial commit — WireIP v4.1"

# 2. Create a GitHub repo and push
git remote add origin https://github.com/YOUR-USER/wireip-network.git
git branch -M main
git push -u origin main

# 3. On Vercel: New Project → Import Git Repository → pick the repo → Deploy
```

Every push to `main` will auto-redeploy.

---

## Local development

```bash
# from the project folder
npm start
# or:
npx serve . -l 3000
```

Then open `http://localhost:3000`.

The app is a single static HTML file — there's no build step, no bundler, no framework. Edit `wireip-dag.html` directly and refresh the browser.

---

## Custom domain on Vercel

In the Vercel dashboard:

1. Project → Settings → **Domains**
2. Add your domain (e.g. `app.outsidethewireproductions.com`)
3. Follow the DNS instructions Vercel gives you (CNAME or A record)
4. SSL is provisioned automatically

---

## What gets deployed

Just static files. No server, no database, no env vars required to run. The app is fully client-side — all state is held in the browser's `localStorage`. The SecureNode integration calls `http://127.0.0.1:8443` on the user's machine (won't reach a remote SecureNode unless the user's local one is running), so SecureNode functionality is intentionally local-first.

External CDN scripts loaded:

- `three.js` (r128) — used for the Validators Globe
- `pdf.js` (3.11.174) — used to auto-extract the cover logo from the Black Paper PDF if present

If you want a fully-offline build, vendor those two libraries locally and edit the `<script src="…">` tags at the top of `wireip-dag.html` to point at the vendored copies.

---

## Configuring the brand logo

The brand logo is auto-discovered in this priority order on every page load:

1. **A logo image file in the project root** (e.g. `otw-logo.png`, `OTW.png`, `logo.png`, `wireip-logo.png` — 48 candidate names checked)
2. **The cover of the Black Paper PDF** if `WireIP Network™ Black Paper (Institutional Edition).pdf` (or similar) is in the project root — PDF.js renders page 1 and the JS scans for the densest gold-pixel cluster, crops it, and uses it as the brand mark
3. **A previously-uploaded logo** stored in `localStorage` (the user can drop one onto the brand mark in-app at any time)
4. **Nothing** — splash shows just the `⌬ VERIFYING ACCESS` line

To use your own logo: drop `otw-logo.png` (or any image with a sensible filename) next to `wireip-dag.html` and redeploy. Or drop the Black Paper PDF for the auto-extract path.

---

## Security headers

`vercel.json` applies the following headers on every response:

- `X-Frame-Options: DENY` — prevents iframe embedding
- `X-Content-Type-Options: nosniff` — prevents MIME sniffing
- `Referrer-Policy: strict-origin-when-cross-origin` — leak less in the Referer header
- `Permissions-Policy: camera=(), microphone=(), geolocation=()` — blocks browser permission prompts
- `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload` — HSTS (only takes effect on the apex)

Loosen these if you need to embed the app in another site or use sensors.

---

## License

Proprietary. © 2026 Outside The Wire Productions.
