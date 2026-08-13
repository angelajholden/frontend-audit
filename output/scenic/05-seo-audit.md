# SEO Audit

## Summary

All important pages are reachable through ordinary anchors, use clean directory URLs, render content without JavaScript, declare language, and include titles and descriptions. The largest metadata issue is copied placeholder Open Graph data on three pages; page/contact titles and descriptions are duplicated and generic. There are no crawl-blocking directives in the repository. Findings: **0 High, 2 Medium, 2 Low, 1 Informational**.

## Findings

## Finding: Three pages publish incorrect placeholder Open Graph URLs and images

**Severity:** Medium  
**Confidence:** High  
**Files:** `index.html:9`, `contact/index.html:9`, `page/index.html:9`

### Problem

The homepage, contact page, and generic page reuse `og:title="Scenic | Home"`, `og:image="https://example.com/image.jpg"`, and `og:url="https://example.com/page"` regardless of the actual document.

### Why It Matters

Social crawlers can associate shares with an unrelated example URL, display a missing/wrong image, and describe distinct pages as the homepage. These are actively incorrect signals, not merely missing optional metadata.

### Recommendation

Replace each property with its production absolute URL and page-specific title/image, or remove unready Open Graph properties until production values are known.

## Finding: Contact and generic pages duplicate an uninformative title and description

**Severity:** Medium  
**Confidence:** High  
**Files:** `contact/index.html:7`, `contact/index.html:8`, `page/index.html:7`, `page/index.html:8`

### Problem

Both distinct pages use the title `Scenic` and the homepage's generic description. The generic page's visible long title and the contact page's topic are absent from their metadata.

### Why It Matters

Search engines and users cannot distinguish these documents reliably from title/description signals, and search snippets may not describe the destination.

### Recommendation

Give each real page a concise subject-specific title and summary. Replace lorem-ipsum/template metadata when actual content is defined.

## Finding: Placeholder links expose false internal and social destinations

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:112`, `index.html:134`, `blog/index.html:111`, `contact/index.html:112`, `page/index.html:93`

### Problem

The call-to-action and repeated social links all resolve to the current document fragment (`#`) rather than meaningful destinations.

### Why It Matters

Crawlers receive no destination matching the anchor purpose, and users encounter dead navigation. Repeated false links also dilute the otherwise clear internal-link structure.

### Recommendation

Add real URLs or omit the links until their destinations are available.

## Finding: Homepage article collection has an unclear heading relationship

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:56`, `index.html:63`

### Problem

Six article-card `h3` headings appear in an unlabeled grid following the hero `h2`, so the source hierarchy does not identify the collection as its own topic.

### Why It Matters

Machines and users navigating headings receive a less clear representation of how the articles relate to the page.

### Recommendation

Add a meaningful collection `h2` and retain `h3` article titles, or promote the card titles to `h2` if there is no group heading.

## Finding: Deployment indexing files and canonical policy are not defined

**Severity:** Informational  
**Confidence:** Low  
**Files:** `index.html:4`

### Problem

The repository contains no `robots.txt`, sitemap, canonical links, or deployment configuration. A small static site can be valid without canonical tags, but source alone cannot confirm the production host, indexing directives, sitemap availability, or duplicate-host behavior.

### Why It Matters

Production verification is needed to ensure the intended GitHub Pages/custom-domain URLs are indexable and consistently represented.

### Recommendation

At deployment, verify robots/indexing behavior, redirects/host normalization, canonical needs, and sitemap discovery. Add only the files/signals the production setup requires.

## Positive Findings

- Primary navigation is composed of crawlable relative anchors and exposes all four important pages.
- Core page content and headings are present in static HTML and do not depend on JavaScript rendering.
- All pages declare language, viewport, titles, and meta descriptions; the blog article metadata is page-specific and uses production-style absolute Open Graph URLs.
- URL paths are simple and readable, and no `noindex` or blocking robots directive is present in source.

