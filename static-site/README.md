# hafetelectrical.com — static mirror

A static HTML snapshot of **https://hafetelectrical.com/** suitable for hosting on
GitHub Pages, Netlify, Cloudflare Pages, or any plain static-file host.

## Contents

```
.nojekyll                  # tells GitHub Pages NOT to run Jekyll on this folder
index.html                 # Home
about/index.html           # About
contact/index.html         # Contact
projects/index.html        # Projects
services/index.html        # Services
wp-content/                # Theme (Astra) + Elementor + uploaded images
wp-includes/               # WordPress shipped JS (jQuery, etc.)
```

5 HTML pages, ~290 assets, ~38 MB total.

## How it was generated

1. Crawled the live site with `wget --mirror --page-requisites --convert-links
   --adjust-extension --no-host-directories` using a desktop browser User-Agent
   (the origin's LiteSpeed server rejects the default wget UA with 403).
2. `wp-admin/`, `wp-login.php`, `xmlrpc.php`, `wp-json/`, feeds, trackbacks, and
   `?replytocom=` / `?share=` permutations were excluded via `--reject-regex`.
3. Asset filenames containing WordPress cachebusters (`?ver=…`) were renamed to
   strip the query, and every reference in HTML/CSS/JS was rewritten to match
   (see `../scripts/normalize_static_site.py`).
4. Verified with `../scripts/verify_static_site.sh` — all pages and 98 unique
   relative asset URLs return HTTP 200 from a local `python -m http.server`.

## Local preview

```sh
cd static-site
python3 -m http.server 8000
# open http://127.0.0.1:8000/
```

## Hosting on GitHub Pages

1. Create a new repo (e.g. `hafetelectrical-site`).
2. Push the **contents** of this `static-site/` folder to the repo root (not the
   folder itself).
3. In repo **Settings → Pages**, set source to `main` branch, `/ (root)`.
4. The `.nojekyll` file is already included so Jekyll won't touch the WordPress
   `_`-prefixed assets.

## Known limitations of a static export

These pieces of the live WordPress site are intentionally non-functional in the
static mirror because they require a running PHP + database backend:

- The contact form (WPForms) — submissions go to `admin-ajax.php` which does
  not exist on a static host. Replace with a static form service such as
  Formspree, Netlify Forms, or Web3Forms if you need it to work.
- Search.
- Comments, RSS feeds, REST API (`/wp-json/...`), XML-RPC.
- Anything that depends on `wp-admin`.

All visible page content, images, fonts, layout (Astra theme + Elementor CSS),
and front-end JavaScript animations work exactly as on the live site.
