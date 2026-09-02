# dWallet / DrumWave — institutional website (2026)

Static, multi-page marketing site, no build step. dWallet brand
("Your life creates data"), DrumWave company.

> **Status: first version, still being refined.** This repository is being
> handed off to the IT team to deploy to production, but the content and
> code will still go through revisions before the final version. See
> [What's missing](#whats-missing--next-steps) below.

## For the IT team

This is a **100% static** site — plain HTML, CSS and JS, no Node, no
build, no server-side dependencies. Any static file host works
(GitHub Pages, Netlify, Vercel, Hostinger, S3, etc.).

- **Final domain:** not yet decided. That's why we're asking for a
  **test/preview URL** (temporary subdomain, GitHub Pages, or whatever is
  simplest to set up) for internal sharing while adjustments continue —
  before pointing the final domain.
- **Build:** none. Just serve the files from the root of the repository.
- **Entry point:** [`index.html`](index.html).
- **Routes:** every page is a separate `.html` file (no router,
  no SPA) — see [Pages](#pages) below.
- **HTTPS / www → apex redirect (or vice versa):** to be defined together
  with the final domain choice.

If any question comes up about which file corresponds to which URL, or
how to point the domain once it's defined, those are the only
configurations still missing — the code itself doesn't require anything
beyond serving static files.

## What it is

Institutional website for dWallet/DrumWave, with:

- **Home** ([`index.html`](index.html)) — scroll-driven brand experience,
  with animated acts and an interactive life timeline (age 18 → 70),
  showing how personal data accumulates value over a lifetime.
- **Business** ([`business.html`](business.html)) — page aimed at
  companies/partners, presenting the product from a B2B perspective.
- **Contact** ([`contact.html`](contact.html)) — contact page.
- **Resources** ([`resources.html`](resources.html) +
  [`resources/post.html`](resources/post.html)) — listing and reader for
  posts (news, press, institutional content), powered by
  [`assets/data/resources.json`](assets/data/resources.json).
- **Privacy Policy** ([`privacy-policy.html`](privacy-policy.html)).

All pages share the same nav/footer pattern and link to each other.

## Repository structure

```
index.html              Home — scroll experience + timeline
business.html            Business page (B2B)
contact.html             Contact page
resources.html            Resources/posts listing
resources/post.html       Individual post reader (?slug=...)
privacy-policy.html       Privacy policy

assets/
  img/                   Site images (photos, icons, logo)
  js/                    GSAP + ScrollTrigger, bundled locally
  data/resources.json    Resources post data (generated via CMS/)

CMS/
  build_resources_json.py   Converts Webflow CSV export into resources.json
  *.csv                      Raw export from the old CMS (Webflow)

Figma/                  Design reference (NOT part of the published site
                         — it's in .gitignore, exists locally only)

site-antigo/             Snippets from the previous site (Webflow) kept as
                         historical reference (analytics, HubSpot form).
                         Not loaded by the current pages — see
                         site-antigo/README.md before reusing any snippet.

download-images.sh       Downloads the home page photos from assets/img/manifest.json
```

## Running locally

Just open `index.html` in the browser — no server needed.

- GSAP and ScrollTrigger are already bundled in `assets/js/`, so the
  animation works offline.
- Fonts (Titillium Web + Open Sans) come from Google Fonts and fall back
  to the system default font if offline.
- The home page photos are already committed in `assets/img/`. If a file
  is missing, the `<img>` tags automatically fall back to the hosted URL
  in `manifest.json` — run `bash download-images.sh` to download
  everything again locally.

## Resources content (CMS)

`assets/data/resources.json` is generated from a CSV export of the old
Webflow site:

```bash
cd CMS
python3 build_resources_json.py
```

This reads the most recent CSV in the folder and rewrites
`assets/data/resources.json`. Run it again whenever the CMS export is
updated. Today this is a manual process — there's no automatic
integration with any CMS.

## What's already done

- Home, Business, Contact, Resources (listing + post) and Privacy Policy —
  navigable and linked to each other.
- Interactive home page timeline (drag, keyboard, synced with scroll).
- Responsive (mobile/desktop) and respects `prefers-reduced-motion`.
- Home page photos and assets already committed (don't depend on internet).
- Resources content loaded from real data migrated from the old CMS
  (Webflow).

## What's missing / next steps

This is a **first version** for internal review, not the final version.
Still pending:

- **Final domain** — to be decided.
- **Analytics/tracking** — the old site had GA4, Google Ads, PostHog and
  Cookie Script (see `site-antigo/README.md`); none of that has been
  reimplemented yet on the new site. Needs a conscious decision on which
  ones to keep.
- **Contact/newsletter form** — the old site integrated with HubSpot;
  the current `contact.html` page still needs a review on how (or whether)
  this will be reconnected.
- **General content and copy review** — text, images and pages will still
  go through adjustments.
- **SEO** — meta tags, sitemap, Search Console, Organization JSON-LD
  (existed on the old site) still need to be reviewed/reintroduced.
- Reference design in `Figma/` is still being used as the source of
  truth for pending visual adjustments (local folder only, outside git).

Any changes made from here on should be treated as iteration on this
base — not as a rework from scratch.
