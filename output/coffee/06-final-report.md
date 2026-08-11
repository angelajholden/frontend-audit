# Frontend Audit Report

## Executive Summary

The `coffee` frontend is fundamentally sound and unusually easy to reason about: four static pages, one authored stylesheet, and 70 lines of vanilla JavaScript. Its strongest areas are native HTML, form labeling, image alternatives, crawlable navigation, restrained CSS, and safe scripting. The primary risks are an incomplete mobile-menu accessibility/progressive-enhancement pattern and unfinished site content/navigation. Consolidated findings: 0 High, 4 Medium, 3 Low, 2 Informational. Manual browser, keyboard, zoom, contrast, form, and embed testing remains required.

## Project Architecture

The server delivers authored HTML directly from `index.html`, `about/index.html`, `contact/index.html`, and `menu/index.html`; there is no framework, template engine, or build process. Header and footer markup is copied across pages.

`style.css` provides site styles with custom properties, Grid/Flexbox, fluid sizing, and responsive rules; third-party `normalize.css` supplies the reset. Deferred `script.js` controls the off-canvas menu, copyright year, and contact form. External services are Google Fonts, YouTube, Google Maps links, and Formspree. Deployment behavior was unavailable.

## What the Project Does Well

- Static, crawlable page content with descriptive titles and logical H1–H3 structures.
- Native links, buttons, form controls, landmarks, lists, and restrained ARIA.
- Associated form labels, useful image alternatives, document language, and decorative SVG hiding.
- Coherent, low-specificity responsive CSS without `!important` or brittle absolute page layout.
- Small, guarded JavaScript that avoids injection, dependencies, unnecessary async work, and frontend secrets.

## Priority Findings

## Finding: Mobile navigation is not resilient or focus-complete

**Severity:** Medium  
**Confidence:** High  
**Categories:** CSS | JavaScript | Accessibility  
**Files:** `style.css:293`, `style.css:298`, `script.js:14`, `script.js:29`

### Problem

At small widths CSS hides primary navigation until JavaScript toggles a body class. The opened fixed panel does not receive or contain focus; closing by button or Escape does not restore focus. Both controls blindly toggle rather than using explicit open/close transitions.

### Impact

A failed or blocked script removes core navigation. With scripting enabled, keyboard and screen-reader users can tab behind the panel or remain focused on a hidden close button.

### Recommendation

Keep navigation visible as the no-script baseline and collapse it only after a scripting-enabled marker is set. Implement centralized `openMenu()`/`closeMenu()` functions that synchronize class and ARIA state, move focus into the menu, return it to the opener, and isolate the background if the panel is modal.

## Finding: Site-wide social controls do not navigate

**Severity:** Medium  
**Confidence:** High  
**Categories:** HTML | Accessibility | SEO  
**Files:** `index.html:129`, `about/index.html:94`, `contact/index.html:114`, `menu/index.html:556`

### Problem

Four named social links on every page use `href="#"`.

### Impact

They appear functional to users and assistive technology but only jump to the same page, and expose false repeated destinations to crawlers.

### Recommendation

Replace them with real profile URLs or remove them until those destinations exist.

## Finding: Placeholder and contradictory content remains in production pages

**Severity:** Medium  
**Confidence:** High  
**Categories:** HTML | Accessibility | SEO  
**Files:** `about/index.html:43`, `about/index.html:48`, `about/index.html:69`, `contact/index.html:43`, `menu/index.html:43`

### Problem

Several page introductions are Lorem ipsum. About claims a St. Paul, Minnesota location while all map content identifies La Mesa, California.

### Impact

Users receive incomplete/conflicting business information, and page/location meaning is unclear to search systems.

### Recommendation

Replace placeholder text with accurate copy and make the address/location consistent across headings, content, map URLs, and alternatives.

## Finding: Pages provide no authored search summaries

**Severity:** Medium  
**Confidence:** High  
**Categories:** SEO  
**Files:** `index.html:4`, `about/index.html:4`, `contact/index.html:4`, `menu/index.html:4`

### Problem

All pages omit meta descriptions.

### Impact

Search systems must infer summaries from body text, which is particularly weak while placeholder copy remains.

### Recommendation

After finalizing content, add an accurate, distinct description for each page.

## Finding: Pages lack a skip link

**Severity:** Low  
**Confidence:** High  
**Categories:** HTML | Accessibility  
**Files:** `index.html:16`, `about/index.html:16`, `contact/index.html:16`, `menu/index.html:16`

### Problem

There is no mechanism to bypass the repeated branding/navigation block.

### Impact

Keyboard users must traverse repeated header controls on every page.

### Recommendation

Add a first-focusable skip link to a stable main-content target and reveal it on focus.

## Finding: New tabs are used without consistent warning

**Severity:** Low  
**Confidence:** High  
**Categories:** HTML | Accessibility  
**Files:** `index.html:102`, `about/index.html:67`, `contact/index.html:50`, `menu/index.html:529`

### Problem

Map links and the form open new tabs; only the form has nearby warning text.

### Impact

Unexpected context changes can disorient users and create confusing back-button behavior.

### Recommendation

Prefer same-context navigation/submission. Where a new context is essential, disclose it in visible and accessible control wording.

## Finding: Nonessential motion ignores user preferences

**Severity:** Low  
**Confidence:** High  
**Categories:** CSS | Accessibility  
**Files:** `style.css:309`, `style.css:683`

### Problem

The menu slides and map enlarges with no `prefers-reduced-motion` override.

### Impact

Avoidable motion may cause discomfort for motion-sensitive users.

### Recommendation

Remove or shorten those transitions under `prefers-reduced-motion: reduce`.

## Finding: Image dimensions could improve rendering stability

**Severity:** Informational  
**Confidence:** High  
**Categories:** HTML | CSS  
**Files:** `index.html:39`, `menu/index.html:58`

### Problem

Images omit intrinsic `width` and `height` attributes, although CSS reserves aspect ratios in common layouts.

### Impact

Browser layout is less resilient before CSS and image metadata arrive.

### Recommendation

Add intrinsic dimensions while retaining responsive sizing.

## Finding: Deployment and rendered-state checks remain open

**Severity:** Informational  
**Confidence:** Low  
**Categories:** CSS | Accessibility | SEO  
**Files:** `style.css:279`, `style.css:400`, `index.html:4`

### Problem

Static source cannot establish composited contrast, browser focus rendering, live redirects/canonicals, robots behavior, or sitemap discovery.

### Impact

Important accessibility and indexing behavior may differ after rendering and deployment.

### Recommendation

Complete the targeted manual and live-site checks below once a production origin is available.

## Recommended Fix Order

### Fix First

1. Make mobile navigation available without JavaScript and correct its focus/open/close model.
2. Replace or remove every placeholder social link.
3. Replace Lorem ipsum and resolve the St. Paul/La Mesa contradiction.

### Fix Next

1. Add page-specific meta descriptions after copy is final.
2. Add a skip link and remove unnecessary new-tab behavior.
3. Honor reduced-motion preferences.

### Nice to Improve

- Add intrinsic image dimensions.
- Replace footer `<aside>` elements with structures matching their actual relationships.

## Quick Wins

- Removing 16 copied `href="#"` controls eliminates a site-wide functional, accessibility, and SEO defect.
- A visible-on-focus skip link benefits every page with little code.
- One reduced-motion media query covers the menu, map, and minor control transitions.
- Explicit `openMenu()` and `closeMenu()` functions provide one place to fix ARIA, focus, Escape, and future close behavior.

## Accessibility Summary

The six WebAIM Million priorities are strong in source: language, image alternatives, form labels, and button/link accessible names are present; contrast requires rendering checks. The links are named but their `#` destinations are nonfunctional. The principal barrier is mobile-menu keyboard/focus behavior and its no-script failure mode. This report does not establish WCAG or legal compliance.

## Manual Testing Required

- Keyboard-test the small-screen menu, including entry focus, focus order/containment, Escape, and focus return.
- Disable JavaScript below 1024px and verify all primary destinations remain reachable.
- Test 200%/400% zoom and narrow reflow for hero content, menu price rows, form, and footer.
- Measure rendered contrast on hero overlays, navigation pills, placeholders, footer, and interactive states; inspect `:focus-visible` in supported browsers.
- Submit invalid and valid forms with keyboard and screen reader; verify native errors, disabled-state communication, and new-tab behavior.
- Verify YouTube keyboard controls and captions for the actual embedded media.

## SEO Summary

Static content and standard anchors make the core site crawlable, and page titles/headings are sensible. No source-level indexing block exists. Complete content and descriptions before launch. Once the deployment origin is known, inspect live status codes, redirects, robots directives, canonical URL policy, and sitemap behavior rather than adding speculative tags.

## Lower-Priority Cleanup

Footer landmark semantics and image dimension attributes are worthwhile targeted improvements. Repeated page chrome is a maintenance risk, but a build/template migration is not justified solely for four small static pages; keep copied sections synchronized if the architecture remains unchanged.

## Final Recommendation

The frontend needs targeted fixes, not restructuring or a rewrite. Correct the mobile navigation first because one root change improves functionality, keyboard access, ARIA state, and resilience. Then remove false links and finish the content/metadata. The remaining work is small, localized hardening plus manual verification.
