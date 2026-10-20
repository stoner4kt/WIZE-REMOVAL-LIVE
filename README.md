# Wize Furniture Removal — Website

Static HTML/CSS/JS website for Wize Furniture Removal (`wizeremoval.co.za`).
No build step required — files are served directly from the repo (cPanel/Apache).

## Image Optimization

This repository includes an automated image-optimization pipeline.

### How it works

A GitHub Actions workflow (`.github/workflows/convert-images-to-webp.yml`)
automatically converts `.png`, `.jpg` and `.jpeg` images to `.webp` whenever
image files are pushed to any branch. The workflow only triggers when image
files change, so normal code pushes do not run it.

### What happens after a push

1. The workflow detects which PNG/JPG/JPEG files changed and converts them with
   Google's `cwebp` (`-q 80`).
2. A bot commit (`github-actions[bot]`) adds the new `.webp` files alongside the
   originals and pushes them back to the same branch.
3. **Originals are preserved by default** — nothing is deleted. Review the new
   `.webp` files before merging.

### `<picture>` fallback support

- **HTML `<img>` tags** already use a `<picture>` element with a
  `<source type="image/webp">` and the original image as the fallback, so
  browsers that don't support WebP still display the original file.
- **CSS `url()` references** (e.g. the inline hero background) point directly at
  the `.webp` file. There is no `<picture>` fallback for CSS — the site already
  targets browsers that support WebP (all modern browsers do, and the `.htaccess`
  includes the WebP MIME/expiry rules).

### Opting out or deleting originals

To automatically delete the original PNG/JPG/JPEG after conversion (not
recommended unless you have a backup), open
`.github/workflows/convert-images-to-webp.yml` and append `rm "$src"` inside the
conversion loop after a successful `cwebp` run:

```bash
if cwebp -q 80 "${src}" -o "${dst}" 2>/dev/null; then
  converted=$((converted + 1))
  rm "$src"            # delete the original after conversion
fi
```

Until you do that, the originals stay in the repo and both formats are served via
the `<picture>` fallback.

> Note: The social-share metadata (`og:image` / `twitter:image` / JSON-LD) is
> intentionally left on the original JPG/PNG files for crawler compatibility.
> See `TODO.md` for the list.