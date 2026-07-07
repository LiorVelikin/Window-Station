# SF Window Station — Website

Static marketing site for San Francisco Window Station (window & door installation, Bay Area).
Hosted on GitHub Pages: https://liorvelikin.github.io/Window-Station/

## ⚠️ One-time setup required: lead form delivery

All forms (the contact forms and the 4-step estimate wizard on `estimate.html`) read their
submission endpoint from **`js/config.js`**:

```js
window.WS_CONFIG = {
  formEndpoint: "", // <-- paste your Formspree endpoint here
  ...
};
```

1. Create a free form at [formspree.io](https://formspree.io) (submissions get emailed to you).
2. Copy the endpoint it gives you, e.g. `https://formspree.io/f/xabcdefg`.
3. Paste it into `formEndpoint` in `js/config.js`. Done — every form on the site starts delivering leads.

Until this is set, forms show a graceful "call us" fallback instead of failing silently.

## Analytics (optional)

Each page has a commented-out Google Analytics 4 snippet in `<head>`. Create a GA4 property,
then uncomment the snippet and replace `GA_MEASUREMENT_ID` with your measurement ID.

## Structure

- `index.html` — homepage
- `estimate.html` — 4-step lead-qualification wizard (primary conversion page; all "Free Estimate" CTAs point here)
- `contact.html` — classic contact page with quick form
- `windows.html`, `window-replacement.html`, `vinyl/wood/aluminum/fiberglass-windows.html` — window service pages
- `doors.html`, `entry-doors.html`, `patio-doors.html` — door service pages
- `blog.html` + `blog-*.html` — SEO blog (SF/Bay Area focused)
- `service-areas.html`, `gallery.html`, `faq.html`, `about.html`
- `style.css` — single shared stylesheet (design tokens at top)
- `js/config.js` — site configuration (form endpoint, phone)
- `sitemap.xml`, `robots.txt`

## Lead qualification logic (estimate.html)

- ZIP codes outside Bay Area prefixes (940–951) are politely routed to a phone call instead of the form.
- Renters are warned that owner authorization is required (lead still captured, flagged).
- Every submission includes a `lead_quality` field: owner/renter, active/researching, in-area/out-of-area.

## Custom domain note

Canonical URLs, Open Graph tags, and the sitemap currently point at the GitHub Pages URL.
If the site moves to a custom domain, search-replace `https://liorvelikin.github.io/Window-Station/`
across all HTML files and `sitemap.xml`/`robots.txt`.
