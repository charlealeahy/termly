Hey Ejer thanks for the changes 🙂

NEARLY there! The good news is adverts are now firing in all countries where restrictions apply. Now it's just the actual advert getting blocked again by CSP.

## Root Cause

Post-consent, AdSense serves ads through iframes from these domains — which the old CSP blocked. Vietnam/US worked because AdSense serves lighter ad formats there that didn't hit the missing CSP entries.

## Fix

Replace the entire existing CSP header value with the following string:

```
default-src 'self' 'unsafe-inline' https://fonts.googleapis.com https://fonts.gstatic.com https://cdnjs.cloudflare.com https://cdn.jsdelivr.net/ https://accounts.google.com/ http://cdnjs.cloudflare.com/; frame-ancestors 'self'; form-action 'self'; script-src 'self' 'unsafe-inline' https://ep2.adtrafficquality.google https://*.google.com https://*.googlesyndication.com https://*.doubleclick.net https://faves.grow.me https://cdn.prod.uidapi.com https://app.termly.io https://cdn.tailwindcss.com https://youtube.com https://www.youtube.com https://code.jquery.com https://cdnjs.cloudflare.com https://cdn.jsdelivr.net/ https://accounts.google.com/ https://www.google.com https://www.gstatic.com https://code.jquery.com/ http://cdnjs.cloudflare.com/ http://code.jquery.com/ https://static.cloudflareinsights.com https://www.googletagmanager.com https://pagead2.googlesyndication.com https://googleads.g.doubleclick.net; object-src 'none'; img-src 'self' data: https://flagsapi.com https://flagcdn.com/ https://i.ytimg.com https://www.youtube-nocookie.com https://lh3.googleusercontent.com https://placeholder.pics https://a.basemaps.cartocdn.com https://b.basemaps.cartocdn.com https://c.basemaps.cartocdn.com https://accounts.google.com/ https://images.unsplash.com https://www.google-analytics.com https://pagead2.googlesyndication.com https://ep1.adtrafficquality.google https://ep2.adtrafficquality.google https://*.google.com https://*.googlesyndication.com https://*.doubleclick.net; connect-src 'self' https://api.grow.me https://*.google.com https://*.googlesyndication.com https://*.doubleclick.net https://faves.grow.me https://*.grow.me https://*.growplow.events https://app.termly.io https://*.consent.api.termly.io https://ep1.adtrafficquality.google https://cdnjs.cloudflare.com https://cdn.jsdelivr.net/ https://accounts.google.com/ https://www.google.com http://cdnjs.cloudflare.com/ https://www.google-analytics.com https://pagead2.googlesyndication.com https://*.google-analytics.com https://*.adtrafficquality.google https://region1.google-analytics.com https://*.googlesyndication.com https://*.doubleclick.net; frame-src 'self' https://app.grow.me https://app.termly.io https://ep2.adtrafficquality.google https://pagead2.googlesyndication.com https://*.googlesyndication.com https://*.doubleclick.net https://*.safeframe.googlesyndication.com https://googleads.g.doubleclick.net https://youtube.com https://www.youtube.com https://www.youtube-nocookie.com https://lh3.googleusercontent.com https://accounts.google.com/ https://www.google.com
```

## What Changed

**`frame-src`** — Added:
- `https://*.googlesyndication.com`
- `https://*.doubleclick.net`
- `https://*.safeframe.googlesyndication.com`

**`connect-src`** — Added:
- `https://*.adtrafficquality.google`
- `https://region1.google-analytics.com`
- `https://*.googlesyndication.com`
- `https://*.doubleclick.net`

**`img-src`** — Added:
- `https://*.google.com`
- `https://*.googlesyndication.com`
- `https://*.doubleclick.net`

**`img-src`** — Removed:
- `https://www.google.de` *(redundant, covered by `*.google.com`)*
