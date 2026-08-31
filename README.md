# Degen Technology — app landing pages

Static marketing pages for the Degen Technology iOS apps, served by GitHub Pages.

- `index.html` — the app directory
- `<slug>/index.html` — one landing page per app, with `img/{icon.png,home.jpg}`

Built by `../_site_build/build_site.py`, which pulls live App Store data
(name, canonical store URL, icon, screenshot, category) from the iTunes lookup
API and measures each page's accent colour from the app's real icon.

Notes:
- Every App Store link is the canonical `trackViewUrl` from Apple, not hand-typed.
- Copy is written per app; the apps are free downloads with in-app purchases,
  so none of the "pay once, no subscription" framing from other portfolios applies.
- Footer privacy links point at each app's actual privacy policy as registered in
  App Store Connect. Terms link to Apple's standard EULA.
- Story Sprout / Little Bloomers is intentionally excluded.

Regenerate: `python3 _site_build/build_site.py` (re-run link + asset checks after).
