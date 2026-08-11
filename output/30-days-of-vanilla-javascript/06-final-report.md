# Frontend Audit Report

## Executive Summary

“30 Days of Vanilla JavaScript” is fundamentally well structured: static crawlable documents, strong metadata, native HTML, coherent shared CSS, and focused dependency-free modules. The most important risks are isolated to interactive demos: Day 26 reordering and Day 27 resizing exclude keyboard users; Day 09’s email validation accepts invalid addresses; and the shared mobile drawer has incomplete focus/progressive-enhancement behavior. Consolidated findings: 2 High, 6 Medium, 4 Low, 2 Informational. Manual browser, keyboard, assistive-technology, zoom, touch, and deployment testing remains required.

## Project Architecture

The site consists of a landing/day-01 document, day-02 through day-30 challenge documents, a UI-components page, privacy policy, and custom 404. All HTML is authored directly; repeated page chrome is copied rather than templated.

One large but organized `style.css` supplies themes, layouts, components, and responsive behavior. `script.js` is the shared ES-module entry point, importing global utilities and shared video behavior; challenge pages load a focused module from `components/`. Local JSON supports fetch/render demos. Prism is vendored, while Google Fonts, YouTube, and Formspree are external runtime dependencies. There is no build system.

## What the Project Does Well

- Static content, ordinary links, unique titles/descriptions, canonicals, clear headings, and a properly `noindex` custom 404.
- Consistent language declarations, skip navigation, landmarks, image alternatives, visible form labels, native buttons/dialogs/tables, and restrained ARIA.
- Low-specificity themed CSS with strong source-level contrast, responsive images/layouts, shared tokens, and no specificity escalation.
- Small, focused JavaScript modules; safe text/DOM APIs; guarded fetch handling; no framework, secret, `eval`, or untrusted HTML interpolation.
- Several thoughtful interaction details, including tab arrow keys, toast live regions, native dialog use, and local state persistence.

## Priority Findings

## Finding: Day 26 reordering is drag-only

**Severity:** High  
**Confidence:** High  
**Categories:** HTML | JavaScript | Accessibility  
**Files:** `day-26/index.html:171`, `components/drag-drop.js:29`

### Problem

Plain list items can be reordered only with HTML drag events; no focusable move controls or keyboard path exists.

### Impact

Keyboard, switch, voice-control, and some touch users cannot perform the demo’s primary function. This conflicts with WCAG 2.1.1 and 2.5.7 expectations for equivalent non-drag operation.

### Recommendation

Add native move-up/move-down buttons for each item, update/persist the order through one shared function, and announce the item’s new position. Retain drag as an optional enhancement.

## Finding: Day 27’s split view is mouse-only, unnamed, and nonresponsive

**Severity:** High  
**Confidence:** High  
**Categories:** HTML | CSS | JavaScript | Accessibility  
**Files:** `day-27/index.html:198`, `components/split-view.js:11`, `style.css:1545`

### Problem

An unfocusable empty `role="separator"` reacts only to mouse events, exposes no name/value state, and separates two 240px-minimum panels with no narrow-screen fallback. Its script also registers a permanent `mouseup` listener during every mousemove.

### Impact

Keyboard, screen-reader, and touch users cannot resize it; narrow/zoomed layouts overflow horizontally; repeated dragging accumulates listeners.

### Recommendation

Implement a complete adjustable separator with focus, name, current/min/max values, keyboard increments, Pointer Events, and one clean drag lifecycle. Stack panels and remove resizing below a content-driven breakpoint.

## Finding: Day 09 validates and announces submission incorrectly

**Severity:** Medium  
**Confidence:** High  
**Categories:** HTML | JavaScript | Accessibility  
**Files:** `day-09/index.html:167`, `components/form-validation.js:2`, `components/form-validation.js:26`

### Problem

`novalidate` disables native checks, but JavaScript only tests empty strings, so malformed email passes. Errors are not associated with controls or focused, and success appears before Formspree confirms submission. The root guard is also an ineffective empty statement.

### Impact

Invalid contact data can be sent; screen-reader/keyboard users may not discover errors; users receive a false success signal.

### Recommendation

Restore native constraints (`required`, `type="email"`) or use the validity API, associate and focus errors, set invalid state, fix the guard, and report success only after a confirmed response.

## Finding: Shared mobile navigation is neither resilient nor focus-complete

**Severity:** Medium  
**Confidence:** High  
**Categories:** CSS | JavaScript | Accessibility  
**Files:** `style.css:572`, `style.css:582`, `global/resp-nav.js:6`

### Problem

CSS hides the route list below 960px before JavaScript succeeds. Opening does not move/contain focus, and closing or Escape does not restore focus; both controls blindly toggle state.

### Impact

A module failure removes primary navigation. With scripts working, keyboard users can tab behind the drawer or remain on hidden content.

### Recommendation

Keep baseline navigation available and collapse only after a script-enabled marker. Use explicit open/close functions, synchronized ARIA, entry/return focus, and background inertness if the drawer is modal.

## Finding: Form error semantics are incomplete across validation demos

**Severity:** Medium  
**Confidence:** High  
**Categories:** HTML | JavaScript | Accessibility  
**Files:** `day-09/index.html:173`, `day-22/index.html:190`, `day-23/index.html:192`, `components/multistep-form.js:36`

### Problem

Inline errors are visually toggled but not programmatically tied to fields/groups, invalid state is not exposed, and the first error is not focused or summarized.

### Impact

Assistive-technology users may not know an error appeared or where to correct it.

### Recommendation

Prefer native constraints, connect error text, update invalid state, and direct focus/announcement to the first actionable error.

## Finding: Sortable table exposes state on the wrong element

**Severity:** Medium  
**Confidence:** High  
**Categories:** HTML | JavaScript | Accessibility  
**Files:** `day-16/index.html:173`, `components/sortable-table.js:20`

### Problem

`aria-sort` is set and updated on buttons rather than their columnheader cells.

### Impact

Screen-reader users may activate sorting without hearing which column/direction is current.

### Recommendation

Keep buttons as actions but move/update `aria-sort` on the matching `<th>`.

## Finding: Navigation search accumulates reset handlers

**Severity:** Medium  
**Confidence:** High  
**Categories:** JavaScript | Performance | Maintainability  
**Files:** `global/filter-nav.js:10`, `global/filter-nav.js:24`

### Problem

Every debounced input run adds one reset listener for every navigation item.

### Impact

Shared navigation accumulates redundant closures and repeats reset work after continued searching.

### Recommendation

Register one reset listener once outside the input callback and item loop.

## Finding: Day 19 creates broken breed links

**Severity:** Medium  
**Confidence:** High  
**Categories:** JavaScript | SEO  
**Files:** `components/transforming-api-data.js:66`

### Problem

Generated links target origin-root breed slugs for which no static routes exist.

### Impact

Users and crawlers are sent to 404s unless undocumented production routing supplies those pages.

### Recommendation

Render demo labels as text, use legitimate external targets, or create real destination routes.

## Finding: Tab semantics and attributes need correction

**Severity:** Low  
**Confidence:** High  
**Categories:** HTML | Accessibility  
**Files:** `day-05/index.html:164`, `day-05/index.html:175`, `ui-components/index.html:258`

### Problem

Tab buttons duplicate the `class` attribute and tabpanels do not reference their tabs with `aria-labelledby`.

### Impact

Parser recovery discards an intended class, and panel context can be incomplete for assistive technology.

### Recommendation

Merge class tokens and add matching panel label references.

## Finding: Nonessential motion ignores user preferences

**Severity:** Low  
**Confidence:** High  
**Categories:** CSS | Accessibility  
**Files:** `style.css:33`, `style.css:59`, `style.css:591`

### Problem

Smooth scrolling and many transitions remain enabled for reduced-motion users.

### Impact

Motion-sensitive visitors cannot suppress avoidable movement.

### Recommendation

Disable smooth scroll and nonessential transitions under `prefers-reduced-motion: reduce`.

## Finding: Privacy metadata is empty

**Severity:** Low  
**Confidence:** High  
**Categories:** SEO  
**Files:** `privacy-policy/index.html:8`

### Problem

Meta and Open Graph descriptions are present with empty content.

### Impact

Search/social systems receive no authored summary for that page.

### Recommendation

Add an accurate summary or omit empty metadata.

## Finding: Day 05 contains placeholder demo links

**Severity:** Low  
**Confidence:** High  
**Categories:** HTML | Accessibility  
**Files:** `day-05/index.html:177`

### Problem

Three visible links use `href="#"` and Lorem ipsum labels.

### Impact

They look functional but only jump to the page top, weakening the otherwise complete tabs demonstration.

### Recommendation

Use real resource targets or non-link sample text.

## Finding: Rendered accessibility states need manual confirmation

**Severity:** Informational  
**Confidence:** Low  
**Categories:** CSS | JavaScript | Accessibility  
**Files:** `style.css:145`, `day-04/index.html:172`, `day-20/index.html:228`

### Problem

Static source cannot prove focus visibility/return, announcements, contrast across all themes, loaded-video behavior, or cross-browser dialog/lightbox interaction.

### Impact

Real browser and assistive-technology behavior may reveal barriers not visible in source.

### Recommendation

Perform the project-specific tests below.

## Finding: Deployment indexing behavior is unverified

**Severity:** Informational  
**Confidence:** Low  
**Categories:** SEO  
**Files:** `404.html:6`, `index.html:17`

### Problem

Source has sensible canonicals and a noindex 404 but cannot establish live status codes, redirects, robots headers, or sitemap behavior.

### Impact

Incorrect host behavior could undermine otherwise strong crawl/index signals.

### Recommendation

Test the deployed origin and add/fix only the deployment signals actually needed.

## Recommended Fix Order

### Fix First

1. Add non-drag keyboard reordering to Day 26.
2. Rebuild Day 27’s separator interaction and narrow-screen layout.
3. Correct Day 09 validation, error communication, and success handling.
4. Make shared mobile navigation resilient and focus-correct.

### Fix Next

1. Correct error semantics in Days 22/23.
2. Move sortable-table state to column headers.
3. Stop navigation-filter listener accumulation.
4. Remove or repair Day 19’s broken generated routes.

### Nice to Improve

- Complete tab attributes/relationships and replace Day 05 placeholders.
- Honor reduced motion.
- Populate privacy metadata.

## Quick Wins

- Move `aria-sort` to `<th>` and update the same element in the sorter.
- Merge duplicate tab classes and add three `aria-labelledby` attributes in both demos.
- Move one reset listener outside the navigation filter loop.
- Add a single reduced-motion media query for global transitions and smooth scrolling.
- Replace empty privacy description values with one accurate summary.

## Accessibility Summary

The WebAIM Million priority baseline is strong: no missing language, alternative text, labels, or accessible control names were found, and principal source-defined color pairs have sufficient contrast. The serious barriers are interaction-specific rather than site-wide: drag-only reorder, mouse-only separator, incomplete form error communication, sortable-state placement, and drawer focus handling. Manual testing is still necessary; this report does not establish WCAG or legal compliance.

## Manual Testing Required

- Operate shared navigation entirely by keyboard at desktop and mobile widths; verify no-script access, entry focus, focus containment/order, Escape, and focus return.
- Test Day 26 with keyboard, touch, voice control, and a screen reader after adding non-drag controls; verify order announcements and persistence.
- Test Day 27 with keyboard, mouse, touch, 320px width, and 200%/400% zoom; verify value announcements and no horizontal page scrolling.
- Run every invalid/valid path in Days 09, 22, and 23 with keyboard and screen reader; verify associations, focus, status, reset, and real Formspree result.
- Verify sortable-table direction announcement, tabs, toasts, modal/lightbox focus return, and lazy YouTube focus/captions.
- Inspect focus visibility and measured contrast across teal/navy/pink and light modes; test reduced-motion behavior.
- On production, verify 404 HTTP status, canonicals/redirects, robots directives, sitemap discovery, external links, Formspree, YouTube, local JSON fetches, and generated breed routes.

## SEO Summary

The static architecture, normal anchors, descriptive titles/meta descriptions, canonicals, headings, and noindex 404 are strong. Day 19’s generated nonexistent routes are the only clear crawl defect. Complete the privacy description and verify live routing, status codes, redirects, robots, and sitemap behavior before launch.

## Lower-Priority Cleanup

The 1,893-line stylesheet remains understandable because it is grouped, but component extraction could become useful if the site grows. Repeated HTML chrome is a maintenance risk across more than 30 pages; a lightweight static include/build approach may eventually reduce drift, but is not required to fix current defects. Remove debugging logs and reconcile tutorial code blocks whenever implementation changes so educational examples remain accurate.

## Final Recommendation

The frontend is fundamentally sound and does not need a framework or rewrite. Problems are concentrated in several advanced interactions plus shared mobile navigation. Address the two pointer-only demos first, then form correctness and drawer focus/resilience. The remaining work is targeted semantic, listener, metadata, and manual-QA cleanup.
