# Scenic Pre-Launch QA To-Do

## Fix Before Launch

- [ ] Guard and scope the sharing initializer so it does not throw on pages without `.social_sharing` (`script.js:83-93`).
- [ ] Give contact and newsletter inputs unique IDs and update their labels (`contact/index.html:66-101`).
- [ ] Replace the CTA and footer `href="#"` placeholders with real destinations, or remove them (`index.html:112`, repeated footer social links).
- [ ] Move focus into the mobile menu when opened and restore it to the trigger when closed by button or Escape (`script.js:12-33`).
- [ ] Add a persistent, high-visibility `:focus-visible` treatment for links and controls (`style.css:101-115`).
- [ ] Add a `prefers-reduced-motion: reduce` treatment for Animate.css classes, menu transitions, image scaling, and parallax (`style.css`).
- [ ] Replace generic page/contact titles and descriptions with page-specific metadata (`contact/index.html:7-8`, `page/index.html:7-8`).
- [ ] Replace or remove `example.com` Open Graph URLs/images and set page-specific Open Graph values (`index.html:9-15`, `contact/index.html:9-15`, `page/index.html:9-15`).
- [ ] Initialize contact and newsletter forms with explicit component selectors rather than the first `form` (`script.js:36-57`).
- [ ] Replace `alt="placeholcer text"` with an accurate or intentionally empty alternative (`blog/index.html:50`).
- [ ] Add a visible-on-focus skip link targeting the main content on all four pages.
- [ ] Add a meaningful `h2` for the homepage article collection or promote its card headings to `h2` (`index.html:56-103`).
- [ ] Build sharing query parameters with `URLSearchParams` and encode mail fields individually (`script.js:89-93`).

## Verify Manually

- [ ] Confirm all four pages load without console errors.
- [ ] Keyboard-test the mobile menu at its narrow-screen breakpoint, including focus visibility, tab order, close button, Escape, and focus return.
- [ ] Screen-reader-test the menu state and both contact-page forms after IDs are corrected.
- [ ] Verify label clicks focus their adjacent contact/newsletter controls.
- [ ] Test empty and invalid form submissions; confirm errors are perceivable and focus behavior is useful.
- [ ] Test successful Formspree and Mailchimp submissions, popup behavior, and whether disabled submit buttons recover after errors or returning to the page.
- [ ] Measure text and control contrast on hero images, translucent page panels, metadata, placeholders, footer regions, and all interaction states.
- [ ] Test at 320 CSS pixels, mobile landscape, 200% zoom, and 400% zoom for clipping, overlap, horizontal scrolling, and lost controls.
- [ ] Confirm long headings, forms, social controls, fixed/aspect-ratio regions, and faux parallax remain usable.
- [ ] Enable the operating-system reduced-motion preference and confirm nonessential motion is suppressed.
- [ ] Test social sharing with production URLs containing query strings and titles containing `&`, `#`, or punctuation.
- [ ] Verify every CTA, navigation, footer, share, form, and external link reaches the intended production destination.
- [ ] Validate production Open Graph previews and absolute image URLs.
- [ ] Confirm production robots/indexing behavior, preferred host redirects/canonical policy, and sitemap discovery.

## Optional Cleanup

- [ ] Normalize the logo container semantics between the homepage and inner pages (`index.html:28`; inner pages around line 28).
- [ ] Consider a lightweight static include/build approach if duplicated header/footer markup begins to drift.
- [ ] Review fixed-height/aspect-ratio layout rules only where manual testing demonstrates a concrete problem (`style.css:325-418`, `style.css:482-532`, `style.css:839-900`).
