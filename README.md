# SF Window Station — Website

Static marketing site for San Francisco Window Station (window & door installation, Bay Area).
Built for Netlify hosting.

This is a corrected working copy of the site — see "What changed from the original" below
for what was fixed versus the version in the `Window-Station` GitHub repo.

## One-time setup: lead notifications

Forms (`contact.html` and the 4-step estimate wizard on `estimate.html`) use **Netlify Forms**
— no endpoint, API key, or third-party service needed. Netlify detects both forms automatically
at deploy time (they carry `data-netlify="true"`).

To get emailed when a lead comes in:

1. Deploy the site to Netlify.
2. In the Netlify dashboard: **Site configuration → Forms → Form notifications → Add notification → Email notification**.
3. Pick the `contact` and `estimate` forms and the email address to notify.

Submissions also appear under **Site → Forms** in the dashboard regardless of whether email
notifications are set up. Both forms include a hidden honeypot field (`bot-field`) for basic
spam filtering — no CAPTCHA needed for this volume.

## Analytics (optional)

Each page has a commented-out Google Analytics 4 snippet in `<head>`. Create a GA4 property,
then uncomment the snippet and replace `GA_MEASUREMENT_ID` with your measurement ID.

## Structure

- `index.html` — homepage
- `estimate.html` — 4-step lead-qualification wizard (primary conversion page; all "Free Estimate" CTAs point here)
- `contact.html` — classic contact page with quick form, submits to `thank-you.html`
- `thank-you.html` — post-submit landing page for the contact form (noindex)
- `windows.html`, `window-replacement.html`, `vinyl/wood/aluminum/fiberglass-windows.html` — window service pages
- `doors.html`, `entry-doors.html`, `patio-doors.html` — door service pages
- `{city}-window-replacement.html` (12 pages) — dedicated Google Ads / local-SEO landing pages for
  San Francisco, Daly City, South San Francisco, San Bruno, Millbrae, Burlingame, San Mateo,
  Foster City, San Carlos, Redwood City, Menlo Park, and Palo Alto. Each has its own embedded
  Netlify-wired form, city-specific neighborhoods, testimonial, and FAQ item — same pattern as
  windows4u's East Bay city pages. Linked from `service-areas.html`'s Peninsula & South Bay
  section and listed in `sitemap.xml`.
- `blog.html` + `blog-*.html` — SEO blog (SF/Bay Area focused)
- `service-areas.html`, `gallery.html`, `faq.html`, `about.html`
- `style.css` — single shared stylesheet (design tokens at top)
- `netlify.toml` — build config, security headers, caching
- `sitemap.xml`, `robots.txt`

## Lead qualification logic (estimate.html)

- ZIP codes outside Bay Area prefixes (940–951) are politely routed to a phone call instead of the form.
- Renters are warned that owner authorization is required (lead still captured, flagged).
- Every submission includes a `lead_quality` field: owner/renter, active/researching, in-area/out-of-area.

## Custom domain note

Canonical URLs, Open Graph tags, and the sitemap currently point at a placeholder
(`https://sfwindowstation.example.com/`) since the real domain isn't decided yet.
Once it is, search-replace that string across all HTML files, `sitemap.xml`, and `robots.txt`,
and consider adding domain-canonicalization redirects (http→https, www→apex) to `netlify.toml`,
matching the pattern used on the windows4u site — unless you'd rather let Netlify's own
"Primary domain" dashboard setting handle that.

## What changed from the original repo

The site as pulled from `github.com/LiorVelikin/Window-Station` was built for GitHub Pages +
Formspree. Since this is moving to Netlify, the following was corrected:

- **Forms rewired to Netlify Forms**, replacing the Formspree/`js/config.js` setup entirely
  (no more pasting an endpoint URL — it just works once deployed). `js/config.js` was removed
  since nothing else referenced it.
- **`estimate.html`'s wizard** (project type, quantity, property type, ownership, timeline,
  budget) is driven by buttons, not `<input>` elements, so those selections had no field Netlify
  could detect in the static HTML. Added hidden inputs that the wizard JS keeps in sync, and
  submission now builds its payload from the actual `<form>` via `FormData` instead of a
  hand-assembled object.
- **`contact.html` had `novalidate` with no replacement validation logic**, so required fields
  weren't actually enforced before submission. Removed `novalidate` so the browser's native
  required/email/phone validation applies, and switched the form to a plain native POST to
  `/thank-you.html` (new page, added) instead of a hand-rolled `fetch()` call.
- **Canonical URLs, Open Graph tags, sitemap, and robots.txt** pointed at the GitHub Pages URL;
  swapped for a placeholder pending the real domain (see above).
- **Added `netlify.toml`** with security headers and asset caching, matching the pattern already
  proven on the windows4u site.
