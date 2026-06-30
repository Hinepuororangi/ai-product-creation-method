# PWA App Icon Setup — Canonical Method
*Supersedes the 4-size icon guidance in v3 Phase 5. One PNG file, referenced twice.*

## Steps

1. Create or get your icon image as a single PNG, 512x512.
2. Upload the PNG to your GitHub repo via **Upload Files** (never the mobile editor — it strips DOCTYPE).
3. Open `index.html` in GitHub → edit.
4. In the `<head>` section, add (using your actual filename):
```html
<link rel="apple-touch-icon" href="your-icon-name.png">
<link rel="icon" href="your-icon-name.png">
<link rel="manifest" href="manifest.json">
```
5. Create `manifest.json` — same PNG file, referenced at both sizes:
```json
{
  "name": "Your App Name",
  "short_name": "Your App Name",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#FFFFFF",
  "theme_color": "#000000",
  "icons": [
    {
      "src": "your-icon-name.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "your-icon-name.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ]
}
```
6. Upload `manifest.json` to the same repo.
7. Commit changes → Vercel redeploys automatically.
8. Remove any old home screen shortcut for this app.
9. Open the live URL in Chrome → ⋮ menu → **Add to Home Screen**.

## Why both sizes in one file

Some Android launchers and Chrome versions fall back to a low-res or generic
icon on the home screen tile if no 192x192 entry exists in the manifest,
even though the PWA installs successfully. Browsers will downscale the
single 512px PNG to render the 192px slot, so no second image file is
needed — just the second manifest entry.

## What this replaces

The original Phase 5 guidance (v3 doc) called for four separately-generated
sizes: 512, 192, 180, 32px, with extra head tags for Apple status bar style
and explicit favicon sizing. That approach is no longer canonical. Use this
single-PNG method going forward for all new builds and icon rebuilds.

---
*Added: June 2026, during Tohu.AI rebrand icon build.*
