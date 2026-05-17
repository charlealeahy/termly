# EarthLive.TV — Termly / Google Consent Mode Fix

Complete `<head>` replacement for the base template. Fixes the Termly GCM scan error "Consent update signals incorrect" and unblocks AdSense in the EU/UK.

## What changed (4 things)

1. **Removed the duplicate `adsbygoogle.js` script** — it currently appears twice in `<head>` (once near the top, once after the gtag config).
2. **Added a `gtag('consent', 'default', {...})` block** before any Google script — this is the missing piece causing Termly's "Consent update signals incorrect" error.
3. **Reordered the sequence:** Termly → consent default → Google Analytics → AdSense. AdSense must load after the consent default is declared, not before.
4. **Defined the `gtag()` function once at the top**, then reused it (instead of redefining it later) — cleaner and avoids any chance of overwriting the consent call.

## Replace the entire current `<head>` block with this

```html
<head>
<title>EarthLive.TV - Explore the World Through Live Webcams</title>

<!-- 1. Termly FIRST — must NOT be async, must execute before anything else -->
<script src="https://app.termly.io/resource-blocker/3ac0d031-0a38-436b-9f5a-d32a8c76fdfc?autoBlock=on"></script>

<!-- 2. Default consent BEFORE any Google script touches the page -->
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('consent', 'default', {
    'ad_storage': 'denied',
    'ad_user_data': 'denied',
    'ad_personalization': 'denied',
    'analytics_storage': 'denied',
    'wait_for_update': 500
  });
</script>

<!-- 3. Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-R50G773QN9"></script>
<script>
  gtag('js', new Date());
  gtag('config', 'G-R50G773QN9');
</script>

<!-- 4. AdSense — ONCE only (was duplicated before) -->
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-4204717840290323" crossorigin="anonymous"></script>

<meta charset="utf-8">
<meta content="width=device-width, initial-scale=1.0" name="viewport">

<style>.toast-top-center{position:fixed!important;top:12px!important;left:50%!important;transform:translateX(-50%)!important;margin:0 auto!important;z-index:9999!important}.toast{width:500px!important}.scrollable::-webkit-scrollbar{width:6px!important}.scrollable::-webkit-scrollbar-track{background:rgba(255,255,255,0.05)!important;border-radius:4px!important}.scrollable::-webkit-scrollbar-thumb{background:rgba(255,255,255,0.25)!important;border-radius:4px!important}.scrollable::-webkit-scrollbar-thumb:hover{background:rgba(255,255,255,0.35)!important}@media (prefers-reduced-motion:reduce){.mobile-menu{transition:none}}</style>

<link rel="icon" href="/static/assets/favicon.png" type="image/png">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="/static/dist/output.css" onload="this.media='all'">
<link rel="preload" href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;700;800&display=swap" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;700;800&display=swap"></noscript>
<link rel="preload" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined&display=swap" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined&display=swap"></noscript>

<style>@font-face{font-family:'Plus Jakarta Sans Fallback';src:local('Arial');ascent-override:95%;descent-override:25%;line-gap-override:0%;size-adjust:98%}.material-symbols-outlined{font-variation-settings:'FILL' 0,'wght' 400,'GRAD' 0,'opsz' 24;font-display:swap;display:inline-block;width:24px;height:24px;line-height:24px;overflow:hidden;vertical-align:middle}.aspect-video{aspect-ratio:16 / 9}</style>

<style>.scroll-btn{display:inline-flex;align-items:center;justify-content:center;width:44px;height:44px;border-radius:9999px;background:rgba(255,255,255,0.10);border:1px solid rgba(255,255,255,0.22);color:rgba(255,255,255,0.92);box-shadow:0 10px 28px rgba(0,0,0,0.30),inset 0 1px 0 rgba(255,255,255,0.18);backdrop-filter:blur(10px);-webkit-backdrop-filter:blur(10px);transition:transform 160ms ease,background 160ms ease,border-color 160ms ease,box-shadow 160ms ease,opacity 160ms ease;cursor:pointer;user-select:none}.scroll-btn:hover{background:rgba(255,255,255,0.16);border-color:rgba(255,255,255,0.35);transform:translateY(-50%) scale(1.04);box-shadow:0 14px 34px rgba(0,0,0,0.36),inset 0 1px 0 rgba(255,255,255,0.22)}.scroll-btn:active{transform:translateY(-50%) scale(0.98);background:rgba(255,255,255,0.12);box-shadow:0 8px 18px rgba(0,0,0,0.28),inset 0 2px 10px rgba(0,0,0,0.18)}.scroll-btn:focus-visible{outline:none;box-shadow:0 0 0 3px rgba(59,130,246,0.55),0 14px 34px rgba(0,0,0,0.36),inset 0 1px 0 rgba(255,255,255,0.22)}.scroll-btn svg{width:22px;height:22px;stroke-width:2.25;filter:drop-shadow(0 1px 1px rgba(0,0,0,0.25))}@media (hover:hover) and (pointer:fine){#section-recently-added-videos .scroll-btn,#section-popular-videos .scroll-btn{opacity:0;transform:translateY(-50%) scale(0.98)}#section-recently-added-videos:hover .scroll-btn,#section-popular-videos:hover .scroll-btn{opacity:1;transform:translateY(-50%) scale(1)}}.animate-spin{animation:spin 1s linear infinite;transform-origin:center center}@keyframes spin{from{transform:rotate(0deg)}to{transform:rotate(360deg)}}</style>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.7.1/jquery.min.js" integrity="sha512-v2CJ7UaYy4JwqLDIrZUI/4hqeoQieOmAZNXBeQyjo21dadnwR+8ZaIJVT8EE2iyI61OV8e6M8PP2/4hpQINQ/g==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>

<meta name="description" content="Discover the world through live webcams and real-time cameras covering cities, landmarks, traffic, nature, wildlife, and space.">
<link rel="canonical" href="https://earthlive.tv/">
<meta name="robots" content="index, follow">
<meta property="og:locale" content="en_US">
<meta property="og:type" content="website">
<meta property="og:title" content="EarthLive.TV - Explore the World Through Live Webcams">
<meta property="og:description" content="Discover the world through live webcams and real-time cameras covering cities, landmarks, traffic, nature, wildlife, and space.">
<meta property="og:url" content="https://earthlive.tv/">
<meta property="og:site_name" content="EarthLive.TV">
<meta property="og:image" content="https://earthlive.tv/static/assets/png/LOGO2.webp">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@EarthLiveTV">
<meta name="twitter:title" content="EarthLive.TV - Explore the World Through Live Webcams">
<meta name="twitter:description" content="Discover the world through live webcams and real-time cameras covering cities, landmarks, traffic, nature, wildlife, and space.">
<meta name="twitter:image" content="https://earthlive.tv/static/assets/png/LOGO2.webp">

<script type="application/ld+json">{"@context":"https://schema.org","@type":"WebSite","name":"EarthLive.TV","url":"https://earthlive.tv/","description":"Discover the world through live webcams and real-time cameras covering cities, landmarks, traffic, nature, wildlife, and space.","publisher":{"@type":"Organization","name":"EarthLive.TV","url":"https://earthlive.tv/","logo":{"@type":"ImageObject","url":"https://earthlive.tv/static/assets/png/LOGO2.webp"},"sameAs":["https://x.com/EarthLiveTV","https://www.instagram.com/earthlivetv/","https://www.youtube.com/@EarthLiveTV"]}}</script>
</head>
```
