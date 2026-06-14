# Prospectio — landing page (prospectio.co.uk)

Static marketing site for Prospectio, a UK property prospecting and lead-intelligence tool built by Antek Automation. Booking-led: the single conversion action is **"Book a 15-minute call"**, which opens a Brevo scheduler in a modal. Pre-launch — no pricing, no invented proof.

## Stack

Pure static HTML / CSS / vanilla JS. No build step, no framework, no server. Brand tokens inherit from `styles.css` (which `@import`s `tokens/*` and the Google-Fonts webfonts).

## Files

```
index.html       the page (semantic HTML + JSON-LD + booking modal)
landing.css        page styles (inherits brand tokens from styles.css)
styles.css         brand entry point — @imports tokens/*
tokens/            colours, fonts, typography, spacing (CSS custom properties)
assets/            favicon.svg, logo-stacked.svg, og.png
privacy.html       privacy stub
terms.html         terms stub
robots.txt         allows all crawlers incl. GPTBot/ClaudeBot/PerplexityBot/Google-Extended
sitemap.xml        /, /privacy.html, /terms.html
llms.txt           markdown summary for AI assistants
site.webmanifest   PWA manifest
```

## Preview locally

```
cd "Propsectio Landing Page"
python3 -m http.server 8799
# open http://localhost:8799/index.html
```

(Serve over HTTP, not `file://` — the manifest and absolute asset paths resolve correctly that way.)

## SEO / GEO built in

- Per-page title + meta description, canonical, Open Graph + Twitter cards, favicon, web manifest, `theme-color`, `lang="en-GB"`.
- JSON-LD `@graph`: Organization (Prospectio) · Organization (Antek Automation) · WebSite · SoftwareApplication · FAQPage · WebPage · BreadcrumbList. FAQ visible copy matches the schema word-for-word.
- `robots.txt` explicitly allows AI crawlers; `llms.txt` gives assistants a clean entity summary; citation-first lead sentences under the key headings.

## Before launch — required swaps

- **NAP placeholders.** `hello@prospectio.co.uk` and `+44 (0)23 9200 4120` appear in the footer, the closing CTA and the JSON-LD `contactPoint`. Replace all three with the real Prospectio/Antek details (keep them identical across the three).
- **OG image.** `assets/og.png` must be served at the site root in production — the meta tags reference `https://prospectio.co.uk/og.png` (absolute). Either move it to root on deploy or update the three `og:image`/`twitter:image`/`primaryImageOfPage` URLs.
- **Organization `sameAs`.** The Antek Automation Organization node has no `sameAs` yet (no profile URLs invented). Add the real Antek website + social/LinkedIn URLs to that node once known.
- **Brevo link.** Booking modal embeds `https://meet.brevo.com/andy-norman/borderless`. Confirm this is the correct public scheduler before launch.
- **Manifest icons.** Currently SVG. For full PWA install support, add 192px and 512px PNG icons and reference them in `site.webmanifest`.

## GEO launch tasks

- Submit `sitemap.xml` to **Bing Webmaster Tools** (ChatGPT discovers pages via Bing).
- Verify the site at **Brave Search Webmaster Tools** (Claude discovers via Brave Search).
- Refresh cornerstone copy periodically — AI engines weigh recency.

## Deploy

Pure static — host anywhere:
- **Vercel** — drop the folder in as a static project (no framework preset).
- **Coolify (Contabo)** — static-site / Nginx container serving this directory.
- Any object store + CDN (S3/R2 + Cloudflare). Ensure the CDN/WAF does not block AI bot user-agents.

## Possible future phase (not built)

The original build prompt specified a Next.js + Tailwind app with a server-side **early-access form** (name/email/company/role → Cloudflare Turnstile → n8n webhook or Brevo API). That was deliberately deferred: this page is booking-led and static. If lead-capture is later wanted, port to Next.js (App Router, SSG) and add the form + `/api` route then.
