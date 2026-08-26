# Strictly Bins — site repo

## Repo layout

```
/
├── index.html
├── 404.html
├── thank-you.html
├── privacy-policy.html
├── terms-of-service.html
├── robots.txt
├── sitemap.xml
├── site.webmanifest
├── vercel.json
├── assets/
│   ├── og-image.png            1200x630 social share card
│   ├── favicon.ico             multi-size
│   ├── icon-192.png            PWA
│   ├── icon-512.png            PWA
│   ├── strictlybins-logo.png   400w, email signature
│   └── strictlybins-logo@2x.png
└── media/
    ├── hero-loop.mp4           MISSING — referenced by index.html
    ├── hero-loop.webm          MISSING — referenced by index.html
    └── hero-poster.jpg         MISSING — referenced by index.html
```

## Blockers before go-live

1. **Hero video files are missing.** `index.html` references
   `/media/hero-loop.mp4`, `/media/hero-loop.webm` and `/media/hero-poster.jpg`.
   None are in the repo. The hero will render empty.

2. **The enquiry form is not wired to anything.** `<form id="enq" novalidate>`
   has no `action` and there is no `fetch()` in the page. Submissions go
   nowhere. Formspree endpoint or a serverless function still to be connected.

3. **GA4 is not installed.** No `gtag` on any page. Every UTM on `/drop`,
   `/bin` and `/join` is currently inert, and the thank-you page fires no
   conversion. The QR attribution work does nothing until this is added.

4. **No `og:image` meta tag.** `assets/og-image.png` is now in the repo but
   the tag still needs adding to each page's `<head>`:

   ```html
   <meta property="og:image" content="https://strictlybins.com.au/assets/og-image.png">
   <meta property="og:image:width" content="1200">
   <meta property="og:image:height" content="630">
   <meta name="twitter:card" content="summary_large_image">
   <meta name="twitter:image" content="https://strictlybins.com.au/assets/og-image.png">
   ```

5. **DNS not pointed at Vercel yet.** Until `strictlybins.com.au` resolves,
   the flyer QR codes fail and the email signature logo 404s.

## Notes

- `styles.css` is **not referenced by any page** — every page carries its own
  inline `<style>` block. Do not add it to the repo unless the pages are
  refactored to use it.
- Favicons are currently inlined as base64 in each page's `<head>`. That works,
  but `/favicon.ico` is also requested directly by some crawlers, so the file
  is included.
- Fonts load from Google Fonts (Baloo 2, Figtree). These are stand-ins pending
  the Gilroy decision.
- Internal anchors are written as `index.html#pricing`. With `cleanUrls: true`
  these are better as `/#pricing`.

## Redirects

| Path | Goes to | Used by |
|---|---|---|
| `/drop` | `/` + letterbox UTM | DL flyer QR |
| `/bin` | `/` + bin tag UTM | Bin tag QR |
| `/join` | `/` + join UTM | Shortlink |
