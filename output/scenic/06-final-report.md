# Frontend Audit Report

## Executive Summary

Scenic is a fundamentally sound, small static site with clear native markup, restrained CSS, and lightweight JavaScript. Its largest immediate risk is a deterministic JavaScript exception on three pages. Accessibility and release quality also depend on correcting duplicate form IDs, menu focus behavior, placeholder links, focus visibility, motion preferences, and copied social metadata. This report consolidates **1 High, 7 Medium, 4 Low, and 2 Informational** findings. Browser, keyboard, assistive-technology, responsive, and deployment checks are still required.

## Project Architecture

Four hand-authored HTML documents (`index.html` plus `blog/`, `contact/`, and `page/`) are served directly. They duplicate shared header/footer markup and load one authored `style.css`, vendored Normalize.css and Animate.css, Google Fonts, and one deferred vanilla `script.js`. There is no framework, templating system, package manager, or build step. Mailchimp and Formspree receive form submissions.

The simplicity makes defects easy to isolate, but duplicated chrome increases the breadth and maintenance cost of fixes. Generated/vendor assets were excluded; findings target editable HTML, CSS, and JavaScript.

## What the Project Does Well

- Static, crawlable documents expose important content and navigation without JavaScript rendering.
- Pages have complete document shells, language declarations, `main` landmarks, lists for navigation, and native buttons/forms.
- CSS uses short selectors, custom properties, modern Grid/Flexbox, responsive image rules, and no `!important` or ID styling.
- JavaScript is small, dependency-free, event-driven, and avoids unsafe HTML injection.
- Icon controls and links generally have accessible names, and all images include `alt` attributes.

## Priority Findings

## Finding: Shared social initializer crashes on three pages

**Severity:** High  
**Confidence:** High  
**Categories:** JavaScript  
**Files:** `script.js:83`, `script.js:89`, `script.js:102`

### Problem

`sharingIcons()` runs on all pages but dereferences sharing anchors that exist only on the blog page.

### Impact

The homepage, contact page, and generic page end startup with an uncaught `TypeError`; any later initialization would be skipped and production consoles report a real runtime failure.

### Recommendation

Detect and scope to `.social_sharing`, returning immediately when it is absent; guard component elements before assigning URLs.

## Finding: Duplicate contact-page IDs break label targets

**Severity:** Medium  
**Confidence:** High  
**Categories:** HTML | Accessibility  
**Files:** `contact/index.html:66`, `contact/index.html:68`, `contact/index.html:98`, `contact/index.html:100`

### Problem

The contact and newsletter forms both use `id="name"` and `id="email"`.

### Impact

Newsletter labels can focus the wrong controls, assistive relationships are ambiguous, and the document is invalid.

### Recommendation

Use unique component-prefixed IDs and matching `for` attributes.

## Finding: Mobile navigation loses focus continuity

**Severity:** Medium  
**Confidence:** Medium  
**Categories:** CSS | JavaScript | Accessibility  
**Files:** `script.js:12`, `script.js:26`, `style.css:314`, `style.css:319`

### Problem

The menu neither moves focus on open nor restores it on close; CSS hides the open trigger while it may still hold focus.

### Impact

Keyboard and screen-reader users can lose track of the active control or navigate outside the visible panel.

### Recommendation

Implement explicit open/close functions, synchronize `aria-expanded`, focus a logical control on open, and return focus to the trigger on close/Escape.

## Finding: Placeholder links publish false actions

**Severity:** Medium  
**Confidence:** High  
**Categories:** HTML | Accessibility | SEO  
**Files:** `index.html:112`, `index.html:134`, `blog/index.html:111`, `contact/index.html:112`, `page/index.html:93`

### Problem

The homepage CTA and five footer social links per page use `href="#"`.

### Impact

Named actions unexpectedly jump to the top, misleading users and exposing no crawlable destination matching their purpose.

### Recommendation

Provide real destinations or remove the links until those destinations exist.

## Finding: Keyboard link focus can become visually indistinct

**Severity:** Medium  
**Confidence:** Medium  
**Categories:** CSS | Accessibility  
**Files:** `style.css:101`, `style.css:111`, `style.css:910`, `style.css:915`

### Problem

The shared focus rule makes link underlines transparent and does not add an explicit persistent focus indicator.

### Impact

Keyboard users may not be able to locate the focused link reliably across browsers and backgrounds.

### Recommendation

Retain the underline for focus and add a high-visibility `:focus-visible` outline with offset. Verify every link/control state in the rendered site.

## Finding: Motion preference is ignored

**Severity:** Medium  
**Confidence:** High  
**Categories:** CSS | Accessibility  
**Files:** `index.html:25`, `index.html:50`, `style.css:81`, `style.css:285`, `style.css:450`

### Problem

Animate.css entrances, menu sliding, card scaling, transitions, and fixed-background effects have no reduced-motion alternative.

### Impact

Users who request reduced motion still receive nonessential animation that may cause discomfort.

### Recommendation

Add a `prefers-reduced-motion: reduce` override for authored and integrated effects.

## Finding: Page metadata is copied and points to example.com

**Severity:** Medium  
**Confidence:** High  
**Categories:** SEO  
**Files:** `index.html:7`, `contact/index.html:7`, `page/index.html:7`

### Problem

Three pages reuse generic titles/descriptions and homepage Open Graph fields; their `og:url` and `og:image` point to `example.com`.

### Impact

Distinct documents are difficult to identify in search results, and social shares can display an unrelated URL, title, or broken image.

### Recommendation

Set page-specific titles/descriptions and correct absolute production Open Graph properties, or remove placeholder properties until ready.

## Finding: Form initializer silently targets only the first form

**Severity:** Medium  
**Confidence:** High  
**Categories:** JavaScript  
**Files:** `script.js:36`, `contact/index.html:54`, `contact/index.html:97`

### Problem

The generic `document.querySelector("form")` means the enhanced form changes by page and the contact page newsletter is never initialized.

### Impact

Identical newsletter markup behaves inconsistently and future form ordering can silently redirect the behavior.

### Recommendation

Use explicit component selectors/data attributes and initialize each intended form independently.

## Finding: Blog image has placeholder alternative text

**Severity:** Low  
**Confidence:** High  
**Categories:** HTML | Accessibility  
**Files:** `blog/index.html:50`

### Problem

The featured image uses `alt="placeholcer text"`.

### Impact

Its nonvisual replacement content is meaningless.

### Recommendation

Provide accurate contextual text or `alt=""` when genuinely decorative.

## Finding: Repeated navigation has no skip link

**Severity:** Low  
**Confidence:** High  
**Categories:** HTML | Accessibility  
**Files:** `index.html:25`, `blog/index.html:26`, `contact/index.html:25`, `page/index.html:25`

### Problem

No mechanism lets keyboard users bypass the repeated header.

### Impact

Users must traverse the same controls before reaching `main` on every page.

### Recommendation

Add a visible-on-focus skip link before the header targeting the main landmark.

## Finding: Homepage article heading relationship is unclear

**Severity:** Low  
**Confidence:** High  
**Categories:** HTML | Accessibility | SEO  
**Files:** `index.html:56`, `index.html:63`

### Problem

An unlabeled article grid starts at `h3` after the hero's `h2`.

### Impact

Heading navigation and machine interpretation imply an unclear relationship between the hero and article collection.

### Recommendation

Add a meaningful collection `h2` or promote card headings to `h2` if no group heading is appropriate.

## Finding: Share parameters are not encoded as parameter values

**Severity:** Low  
**Confidence:** High  
**Categories:** JavaScript  
**Files:** `script.js:89`

### Problem

The complete share URL is passed through `encodeURI`, leaving reserved characters inside interpolated titles/URLs unescaped.

### Impact

Titles or page URLs containing `&`, `#`, or similar characters can corrupt sharing queries.

### Recommendation

Build endpoints with `URL` and `URLSearchParams`; encode mail subject/body values individually.

## Finding: Responsive layout needs browser verification

**Severity:** Informational  
**Confidence:** Low  
**Categories:** CSS | Accessibility  
**Files:** `style.css:325`, `style.css:385`, `style.css:482`, `style.css:839`

### Problem

Fixed-height inner heroes, aspect-ratio content regions, parallax, long headings, and translucent overlays cannot be fully assessed from source.

### Impact

Small landscapes and high zoom may reveal clipping, overlap, contrast, or excess-spacing issues.

### Recommendation

Verify at 320 CSS pixels, small landscape viewports, and 200%/400% zoom before changing the working layout.

## Finding: Production indexing policy requires deployment verification

**Severity:** Informational  
**Confidence:** Low  
**Categories:** SEO  
**Files:** `index.html:4`

### Problem

No robots, sitemap, canonical, redirect, or host configuration is represented in the repository.

### Impact

Source cannot confirm production crawl directives, preferred host/URL behavior, or sitemap discovery.

### Recommendation

Validate those signals on the deployed site and add only those required by its hosting/duplicate-URL setup.

## Recommended Fix Order

### Fix First

1. Guard and scope `sharingIcons()` so all pages initialize without errors.
2. Make contact-page IDs unique and restore correct label relationships.
3. Replace/remove all placeholder links.
4. Implement reliable mobile-menu focus movement/restoration.

### Fix Next

1. Add visible `:focus-visible` treatment and test every interactive state.
2. Honor reduced-motion preferences.
3. Correct page-specific title, description, and Open Graph data.
4. Scope form initialization explicitly.

### Nice to Improve

1. Correct the blog image alternative.
2. Add skip navigation.
3. Clarify the homepage article heading structure.
4. Construct share parameters with `URLSearchParams`.

## Quick Wins

- Add one early return around the absent sharing component to eliminate three page errors.
- Rename four contact-page IDs/label targets.
- Replace placeholder `#` links with actual URLs or remove them.
- Correct the single placeholder `alt` value.
- Add one reusable `:focus-visible` rule and one reduced-motion media query.

## Accessibility Summary

The WebAIM priority review found one unhelpful alternative and broken label relationships from duplicate IDs; no image lacks `alt`, no button/link lacks an accessible name, and every document declares language. The principal interaction risk is mobile-menu focus, followed by focus visibility, motion, skip navigation, and placeholder behavior. Contrast, forms, menu operation, zoom, and reflow still need manual testing; this source audit does not establish WCAG compliance.

## Manual Testing Required

- Keyboard and screen-reader test the narrow-screen menu, including Escape and focus return.
- Confirm strong visible focus across text links, buttons, social controls, and both forms.
- Test labels, required-field errors, invalid email handling, successful Formspree/Mailchimp submissions, popup behavior, and disabled-button recovery.
- Measure contrast on translucent/image-backed regions and all default/hover/focus/disabled states.
- Test 320px reflow, mobile landscape, 200%/400% zoom, long headings, aspect-ratio sections, and parallax behavior.
- Test reduced-motion behavior after its override is added.
- Test share destinations with query strings and reserved characters.
- Verify production links, Open Graph previews, robots/indexing, preferred host/canonical policy, and sitemap discovery.

## SEO Summary

The site is statically rendered and all important pages are reachable through ordinary links, with no repository-level crawl block. Correct copied Open Graph fields and generic inner-page metadata before launch, remove placeholder destinations, and clarify the homepage article heading relationship. Production indexing infrastructure must be checked on the deployed host.

## Lower-Priority Cleanup

Shared header/footer markup is duplicated across four files; careful synchronized edits are necessary. A static include/build step could reduce drift later, but the current four-page scale does not justify a framework rewrite. Logo-container semantics also differ between home and inner pages and can be normalized during routine markup cleanup.

## Final Recommendation

The frontend is fundamentally sound and needs targeted fixes, not restructuring. Resolve the shared-script crash first, then repair form relationships, link destinations, and menu/focus behavior. After metadata and motion updates, run the focused manual test set before launch.
