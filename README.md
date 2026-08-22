# Jonathan James Duah — Portfolio

Personal portfolio for **Jonathan James Duah** — graphic designer, UI/UX designer and product developer based in Accra, Ghana.

Live: _(add your Vercel URL here after first deploy)_

---

## Stack

Single-file static site — no build step. Vanilla HTML, CSS and JavaScript, with Poppins + Inter loaded from Google Fonts.

- Hash-based SPA router (`#/`, `#/work`, `#/work/[slug]`, `#/about`, `#/contact`)
- Dark mode toggle (persisted in `localStorage`, respects `prefers-color-scheme` on first load)
- Category filter on the Work archive
- Cursor-follow hover on project thumbnails
- Scroll-reveal animations (skipped when `prefers-reduced-motion` is set)
- Contact form with client-side validation + honeypot (currently client-only — see _Wiring the contact form_ below)
- WCAG-conscious: keyboard focus states, alt text, semantic HTML

## File structure

```
MyPortfolio/
├── index.html          # the entire site
├── images/
│   ├── portrait.jpg    # About-page portrait
│   ├── cenacle.png     # Cenacle Thanksgiving Service poster
│   ├── pro.png         # Meet With Your MP — Programme Outline
│   └── triplej.png     # Triple J — Hair Products social graphic
└── README.md
```

## Run locally

Any static server will do. Two easy options:

```bash
# Python
python3 -m http.server 8000

# Node
npx serve .
```

Then open `http://localhost:8000`.

Opening `index.html` directly from the filesystem also works, but a local server matches how it'll behave on Vercel.

## Adding a new project

Every project is one entry in the `PROJECTS` array at the top of the `<script>` block inside `index.html`. Copy an existing entry and edit the fields:

```js
{
  slug: 'my-new-project',                         // URL-safe id, used in the hash route
  title: 'Project Name — Short descriptor',       // shown on the card and detail page
  client: 'Client Name',
  year: 2025,
  categories: ['branding', 'editorial'],          // any of the filter categories below
  img: 'images/my-new-project.png',               // relative path from repo root
  summary: 'One-sentence description shown under the title on the project page.',
  deliverables: ['Poster design', 'Layout'],
  role: 'Designer',
  body: 'Long-form paragraph — the story of the project.',
},
```

Available filter categories are declared just below the array in `CATEGORIES`. Add a new one there if you need to.

Drop the cover image into `images/` at the path referenced by `img`.

## Adding the portrait

Replace `images/portrait.jpg` with a new file at the same path. Aim for around 800–1200px on the shorter edge, JPG or PNG, front-lit so it holds up in both light and dark modes.

## Wiring the contact form

The form currently validates on the client and shows a "thanks" message — it doesn't email you yet. To hook it up:

**Option A — Formspree (easiest, no code):**
1. Create a form at [formspree.io](https://formspree.io) and copy the endpoint URL.
2. In `index.html`, find the `wireForm()` function and add a `fetch(...)` call after the validation block, POSTing the form fields to that URL as JSON.

**Option B — Vercel serverless function + Resend:**
1. Sign up at [resend.com](https://resend.com), verify your sending domain, get an API key.
2. Add a Vercel env var `RESEND_API_KEY` and create `api/inquiry.js` that calls Resend's SDK.
3. Update `wireForm()` to POST to `/api/inquiry`.

Either way, keep the honeypot check that's already there — it silently drops bots that fill the hidden `website` field.

## Deploying to Vercel

The repo is a plain static site. Vercel needs no config.

1. Push this repo to GitHub (see below).
2. Go to [vercel.com/new](https://vercel.com/new), import the `MyPortfolio` repo.
3. Framework preset: **Other**. Root directory: `./`. Build command: leave blank. Output directory: leave blank.
4. Deploy.

Every push to `main` redeploys production; every PR gets a preview URL.

## Pushing to GitHub

```bash
git remote add origin https://github.com/jamesjnr1/MyPortfolio.git
git branch -M main
git push -u origin main
```

If prompted for a password, use a Personal Access Token (not your GitHub password). Create one at [github.com/settings/tokens](https://github.com/settings/tokens) with `repo` scope.

## Design notes

- **Typography.** Poppins for display (400/600), Inter for body (400/500). The pairing was chosen after Poppins was specified in the brief; if you want a more editorial feel later, swap `--font-display` to GT Sectra Display or a grotesk like Söhne.
- **Colour tokens.** All colours are CSS variables at the top of the `<style>` block. Light and dark palettes are separate — edit either without touching the other.
- **Accent colour.** `--accent` (vermillion) is used sparingly: nav wordmark dot, hero period, form focus ring, contact email. Adding it anywhere else weakens it.
- **Type scale.** Fluid — every heading uses `clamp()` so nothing needs media-queries for size.

---

© 2026 Jonathan James Duah
