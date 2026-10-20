# Manual Review TODO

Files with image references intentionally left unchanged. Review these manually
before/after merging the WebP pipeline — any changes here may affect social
sharing previews or require re-verification after conversion.

## Social/crawler metadata (kept on original JPG/PNG)
These are `og:` / `twitter:` meta tags and JSON-LD structured data. Most social
platforms and crawlers (Facebook, Twitter/X, Google, WhatsApp, LinkedIn) do not
reliably support WebP for share previews, so they point at the original images.

- `about.html` lines 21, 30 — `og:image` / `twitter:image` → `images/truck6.jpg`
- `atlantic-seaboard.html` lines 22, 30 — `og:image` / `twitter:image` → `images/truck4.jpg`
- `contact.html` lines 21, 30 — `og:image` / `twitter:image` → `images/truck8.jpg`
- `faqs.html` lines 21, 28 — `og:image` / `twitter:image` → `images/truck8.jpg`
- `index.html` lines 25, 34 — `og:image` / `twitter:image` → `images/truck8.jpg`
- `index.html` lines 57–58 — JSON-LD `"logo"` / `"image"` → `images/logo.png` / `images/truck8.jpg`
- `northern-suburbs.html` lines 24, 32 — `og:image` / `twitter:image` → `images/truck6.jpg`
- `services.html` lines 21, 30 — `og:image` / `twitter:image` → `images/furniture1.jpg`
- `single-item-removal.html` lines 25, 34 — `og:image` / `twitter:image` → `images/furniture1.jpg`
- `southern-suburbs.html` lines 22, 30 — `og:image` / `twitter:image` → `images/truck1.jpg`

## PWA / favicon icons (kept as PNG)
Web app manifest + favicon icons must remain PNG for browser/PWA compatibility.

- `images/favicon_io/` (`.png`, `.webmanifest`, `site.webmanifest`) — excluded from
  WebP conversion by the workflow and left untouched here.

## Not converted to WebP (original served directly)
- `images/favicon_io/favicon.svg` — SVG, not a raster image; untouched.

## Deployment
- Note: `.htaccess` already includes MIME and Expires rules for `image/webp`,
  so the cPanel/Apache host serves `.webp` files correctly out of the box.