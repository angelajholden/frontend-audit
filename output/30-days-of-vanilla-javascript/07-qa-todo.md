# QA To-Do List

## Fix Before Launch

- [ ] Add focusable move-up/move-down controls to Day 26 and route drag/button changes through the same persisted reorder logic (`day-26/index.html:171`, `components/drag-drop.js:29`).
- [ ] Announce each reordered item’s new position to assistive technology.
- [ ] Make the Day 27 separator focusable, named, and expose current/min/max values (`day-27/index.html:198`).
- [ ] Add arrow/Home/End resizing and Pointer Events to Day 27 (`components/split-view.js:11`).
- [ ] Register only one end-of-resize listener and clean up the drag lifecycle (`components/split-view.js:27`).
- [ ] Stack Day 27 panels and disable resizing when the viewport cannot fit both panels (`style.css:1545`).
- [ ] Replace Day 09’s empty-string checks with native validation or the validity API (`day-09/index.html:167`, `components/form-validation.js:26`).
- [ ] Fix Day 09’s missing-root guard (`components/form-validation.js:2`).
- [ ] Associate form errors with controls/groups and update invalid state in Days 09, 22, and 23.
- [ ] Focus or announce the first actionable form error after submission.
- [ ] Show Day 09 success only after Formspree confirms submission.
- [ ] Keep primary navigation available if JavaScript does not initialize (`style.css:582`).
- [ ] Implement explicit mobile-menu open/close state, entry focus, Escape closure, and focus return (`global/resp-nav.js:6`).
- [ ] Prevent focus from moving behind the open drawer if it remains modal.
- [ ] Move `aria-sort` from buttons to column headers and update it after every sort (`day-16/index.html:173`, `components/sortable-table.js:20`).
- [ ] Register the navigation reset handler once outside the search/item loops (`global/filter-nav.js:24`).
- [ ] Replace Day 19’s generated nonexistent breed URLs with real destinations or non-link text (`components/transforming-api-data.js:66`).

## Verify Manually

- [ ] Keyboard-test shared navigation at desktop and mobile widths, including Tab order, Escape, background isolation, and trigger focus return.
- [ ] Disable JavaScript below 960px and confirm all challenge routes remain reachable.
- [ ] Test Day 26 with keyboard, touch, voice control, and a screen reader; verify order announcements and reload persistence.
- [ ] Test Day 27 with keyboard, mouse, and touch; verify separator value announcements and constraints.
- [ ] Test the full site at 320 CSS pixels, 200% zoom, and 400% zoom; confirm Day 27 and all other pages reflow without page-level horizontal scrolling.
- [ ] Test all invalid, corrected, reset, and valid submission paths in Days 09, 22, and 23 with keyboard and screen reader.
- [ ] Confirm Day 09 rejects malformed email and reports the real Formspree outcome.
- [ ] Verify sortable-table sort direction is announced after each activation.
- [ ] Verify tab names/selection, toast announcements, modal/lightbox initial focus and focus return, and Escape/backdrop closure.
- [ ] Verify lazy-loaded YouTube keyboard operation, focus placement, title, captions, and error behavior.
- [ ] Check visible focus and measured text/control contrast in teal, navy, pink, and each light-mode state.
- [ ] Enable reduced-motion preference and verify smooth scrolling, drawer motion, video fading, and other nonessential transitions are suppressed.
- [ ] Confirm local JSON demos show understandable failure states when a fetch fails.
- [ ] On production, verify custom 404 responses return HTTP 404 and remain `noindex`.
- [ ] Verify production HTTPS/host/trailing-slash redirects and that every canonical resolves to the preferred URL.
- [ ] Verify robots directives and sitemap discovery on the deployed origin.
- [ ] Crawl production for broken internal/external links, especially Day 19 routes, Formspree, YouTube, social links, and MDN references.

## Optional Cleanup

- [ ] Merge duplicate tab `class` attributes in Day 05 and UI Components.
- [ ] Add `aria-labelledby` from each tabpanel to its corresponding tab.
- [ ] Replace Day 05 `href="#"`/Lorem ipsum demo links with real resources or text.
- [ ] Add a `prefers-reduced-motion: reduce` branch to `style.css`.
- [ ] Add an accurate meta and Open Graph description to `privacy-policy/index.html:8`.
- [ ] Remove leftover debugging `console.log` calls from challenge modules.
- [ ] Add intrinsic image dimensions where practical to improve layout reservation.
- [ ] Keep displayed tutorial code synchronized with corrected executable component files.
- [ ] Consider a lightweight static include/build step if repeated page chrome begins drifting; do not treat this as a launch blocker.
