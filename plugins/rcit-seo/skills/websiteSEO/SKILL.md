---
name: websiteSEO
description: Apply comprehensive SEO and metadata to a website project. Adds primary meta tags (title, description, keywords, robots, canonical, hreflang, theme-color, geo), Open Graph + Twitter Card social embeds, JSON-LD structured data (LocalBusiness / Organization / WebSite / Product / Service / BreadcrumbList / FAQPage), robots.txt, sitemap.xml, performance hints (preconnect, preload), accessibility/SEO basics (semantic HTML, alt text, heading order, hreflang), and an .htaccess SEO block (clean URLs, redirects, image caching, security headers). TRIGGER when the user asks to add SEO, metadata, meta tags, Open Graph, structured data / schema.org / JSON-LD, sitemap, robots.txt, social embeds, "make this rank better", "make it search engine friendly", or whenever a new public-facing webpage / index.html is created or audited.
---

# Website SEO Playbook

Apply this playbook anytime a website needs SEO/metadata, or whenever a new public webpage is created. Goal: make the site discoverable, shareable, and machine-readable — without slowing it down.

## 0. Discovery — gather these before writing anything

Ask once, then proceed silently. If unknown, mark `TODO` in the output for the user to fill in.

- **Canonical URL** of the deployed site (e.g. `https://raceconz.com/`)
- **Brand / business name** + tagline + short (≤155 char) description
- **Logo path** + a 1200×630 social-share image (fallback: logo)
- **Primary location** (street, city, region, country, postal code) — for LocalBusiness sites
- **Contact channels** (phone in E.164, email, social profiles)
- **Languages / locales** the site is published in (default `en`, set `lang` attr accordingly)
- **Page type**: marketing site / blog / e-commerce / SaaS / local business / portfolio
- Any **existing brand guidelines** for `theme-color` (use the primary brand hex)

## 1. The mandatory `<head>` block

Always include, in this order:

```html
<!DOCTYPE html>
<html lang="<LOCALE>">  <!-- e.g. "en-PH", "en", "es-MX" -->
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>

<!-- PRIMARY -->
<title><PRIMARY KEYWORD> — <BRAND> | <LOCATION OR USP></title>  <!-- ≤60 chars -->
<meta name="description" content="<140-155 chars, include primary keyword + value prop>"/>
<meta name="keywords" content="<5-12 comma-separated phrases, real searches>"/>
<meta name="author" content="<BRAND or person>"/>
<meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1"/>
<meta name="theme-color" content="<#BRANDHEX>"/>
<link rel="canonical" href="<ABSOLUTE URL>"/>
<!-- For multi-locale sites: -->
<link rel="alternate" hreflang="<locale>" href="<URL>"/>
<link rel="alternate" hreflang="x-default" href="<URL>"/>

<!-- LOCAL BUSINESS ONLY -->
<meta name="geo.region" content="<ISO-3166-2, e.g. PH-CAM>"/>
<meta name="geo.placename" content="<City, Region>"/>

<!-- OPEN GRAPH -->
<meta property="og:type" content="website"/>  <!-- "article" for blog posts, "product" for PDPs -->
<meta property="og:site_name" content="<BRAND>"/>
<meta property="og:title" content="<same or shorter than <title>>"/>
<meta property="og:description" content="<same as meta description>"/>
<meta property="og:url" content="<canonical>"/>
<meta property="og:image" content="<ABSOLUTE 1200x630 URL>"/>
<meta property="og:image:alt" content="<describe the share image>"/>
<meta property="og:locale" content="<en_US format>"/>

<!-- TWITTER / X -->
<meta name="twitter:card" content="summary_large_image"/>
<meta name="twitter:title" content="<title>"/>
<meta name="twitter:description" content="<desc>"/>
<meta name="twitter:image" content="<absolute URL>"/>

<!-- PERFORMANCE -->
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin/>
<link rel="icon" type="image/png" href="<favicon>"/>
<link rel="apple-touch-icon" href="<180x180 PNG>"/>
```

## 2. JSON-LD structured data — pick the schemas that fit

Place inside `<head>` as `<script type="application/ld+json">` blocks. Always validate against [https://validator.schema.org/](https://validator.schema.org/) before shipping.

**Always include `WebSite`:**
```json
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "@id": "<URL>#website",
  "url": "<URL>",
  "name": "<BRAND>",
  "publisher": { "@id": "<URL>#org" },
  "inLanguage": "<locale>"
}
```

**Pick ONE of these as the publishing entity:**

- `Organization` — for non-local SaaS / global brand
- `LocalBusiness` (or a subtype: `ProfessionalService`, `Store`, `Restaurant`, `MedicalBusiness`, etc.) — for businesses with a physical address. Include `address` (PostalAddress), `telephone`, `priceRange`, `geo` (if known), `areaServed`, `openingHours`, `sameAs`.
- `Person` — for personal portfolios.

**Add when applicable:**

- `BreadcrumbList` — multi-page sites with a hierarchy
- `Product` + `Offer` — e-commerce PDPs (price, currency, availability, sku, brand). If quote-only, use `Service` instead and set `priceRange` on the parent business.
- `Service` — services (provider link to the business `@id`)
- `Article` / `BlogPosting` — blog posts (headline, datePublished, author, image)
- `FAQPage` — pages with Q&A blocks
- `Event`, `Recipe`, `Course`, `JobPosting`, `Review` — when content matches
- `ItemList` — for category / catalog pages

**Cross-link with `@id`** so Google understands the entities are the same thing across blocks.

## 3. `robots.txt` (site root)

```
User-agent: *
Allow: /

Disallow: /admin/
Disallow: /api/
Disallow: /<sensitive folders>

# Optional — block AI training bots:
User-agent: GPTBot
Disallow: /
User-agent: CCBot
Disallow: /
User-agent: anthropic-ai
Disallow: /
User-agent: ClaudeBot
Disallow: /
User-agent: Google-Extended
Disallow: /

Sitemap: <ABSOLUTE>/sitemap.xml
```

## 4. `sitemap.xml` (site root)

For static / small sites, hand-author. For dynamic, generate at build. Include `<lastmod>` (ISO 8601), `<changefreq>`, `<priority>` (0.0–1.0). Include `image:image` blocks under URLs that have key images.

## 5. `.htaccess` (Apache) — SEO + caching block

```apache
# Force HTTPS (after SSL is active)
RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

# Force non-www (or www — pick one) to avoid duplicate content
RewriteCond %{HTTP_HOST} ^www\.(.+)$ [NC]
RewriteRule ^ https://%1%{REQUEST_URI} [L,R=301]

# Drop trailing slash on files / clean URLs (.html invisible)
RewriteCond %{REQUEST_FILENAME}.html -f
RewriteRule ^(.+?)/?$ $1.html [L]

# Image caching (1 month) and gzip — see Performance section
```

## 6. On-page SEO essentials

- **One `<h1>` per page** — contains primary keyword, sits above the fold
- **Heading order**: `h1 → h2 → h3` (don't skip levels)
- **Every `<img>` has meaningful `alt`** — describe content, not "image of"
- **Internal links use descriptive anchor text** — never "click here"
- **Use semantic HTML**: `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`
- **Mobile-first responsive** — viewport tag set, tap targets ≥ 44×44px
- **URL structure**: short, lowercase, hyphenated, no query strings for indexable pages
- **Page weight target**: < 1 MB initial transfer; LCP < 2.5s; CLS < 0.1; INP < 200ms

## 7. Performance hints that move SEO

Google ranks Core Web Vitals. Quick wins:

- `<link rel="preconnect">` for third-party origins (fonts, analytics)
- `loading="lazy"` on below-fold images; `fetchpriority="high"` on the LCP image
- `<img>` always has explicit `width` + `height` to prevent CLS
- Inline critical CSS for above-the-fold; defer the rest
- Compress images (WebP/AVIF), serve responsive `srcset`
- Minify HTML/CSS/JS in production
- Long-cache static assets via `Cache-Control: public, max-age=2592000, immutable`

## 8. Accessibility (also helps SEO)

- `lang` attr set on `<html>`
- Form inputs have associated `<label>`
- Focus states are visible
- Color contrast ≥ 4.5:1 for body text
- `aria-label` on icon-only buttons
- Skip-to-content link for keyboard users

## 9. Validation checklist before shipping

Run through every time:

- [ ] [validator.schema.org](https://validator.schema.org/) — no errors on JSON-LD
- [ ] [search.google.com/test/rich-results](https://search.google.com/test/rich-results) — page is eligible for rich results
- [ ] [pagespeed.web.dev](https://pagespeed.web.dev/) — Core Web Vitals all green
- [ ] [opengraph.xyz](https://www.opengraph.xyz/) — share preview looks right
- [ ] View-source: title, description, canonical, og:image all present
- [ ] No `noindex` left from staging
- [ ] `robots.txt` reachable at `/robots.txt`
- [ ] `sitemap.xml` reachable and listed in `robots.txt`
- [ ] After deploy: submit sitemap in Google Search Console + Bing Webmaster Tools

## 10. Common mistakes to avoid

- Stuffing `meta keywords` with hundreds of terms — Google ignores it; keep it tight
- Using the same `<title>` across pages — every page needs a unique one
- `og:image` paths that are relative — must be absolute URLs
- Forgetting `og:image:alt` and `og:locale`
- Putting JSON-LD inside `<body>` instead of `<head>` (works but `<head>` is conventional)
- Duplicate H1s, missing H1, or H1 hidden by CSS
- Blocking JS/CSS in robots.txt — Google needs to render the page
- Setting both `noindex` AND `disallow` — Google can't see the noindex if disallowed
