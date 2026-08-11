# HTML Audit

## Summary

The documents use valid-looking HTML5 structure, native controls, landmarks, headings, lists, associated form labels, and descriptive image alternatives. The principal HTML defect is the site-wide set of non-navigating social anchors. Secondary concerns are unexplained new browsing contexts and repeated footer structures that are semantically over-labeled as complementary content. Findings: 0 High, 2 Medium, 2 Low, 0 Informational.

## Finding: Social links are placeholder fragment navigations

**Severity:** Medium  
**Confidence:** High  
**Files:** `index.html:129`, `about/index.html:94`, `contact/index.html:114`, `menu/index.html:556`

### Problem

Each footer contains four genuine-looking social anchors whose `href="#"` only navigates to the top of the current page.

### Why It Matters

The controls communicate navigation but do not reach the named destination, creating broken behavior on every page.

### Recommendation

Use the real profile/service URLs. If destinations do not yet exist, remove the links until they do rather than shipping false navigation.

## Finding: New-window links do not identify the context change

**Severity:** Medium  
**Confidence:** High  
**Files:** `index.html:102`, `about/index.html:67`, `contact/index.html:50`, `contact/index.html:87`, `menu/index.html:529`

### Problem

Map links and the contact form use `target="_blank"`. Only the form's nearby small print warns that a new tab opens; map links do not.

### Why It Matters

Unexpected browsing-context changes can disorient users and complicate back-button navigation. A new tab is not needed for ordinary map navigation by default.

### Recommendation

Remove `target="_blank"` unless the new context is a real requirement. When it is required, communicate it in the link/control's accessible and visible wording.

## Finding: Footer columns misuse multiple aside elements

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:100`, `about/index.html:65`, `contact/index.html:85`, `menu/index.html:527`

### Problem

Every footer column—Directions, Hours, Daily Brew, and Socials—is an `<aside>`. Most are site information rather than content tangential to the page.

### Why It Matters

This creates several complementary landmarks that do not reflect the content relationship and makes landmark navigation noisier.

### Recommendation

Use neutral containers or appropriately labeled sections inside the footer. Reserve `<aside>` for genuinely complementary content.

## Finding: Content images omit intrinsic dimensions

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:39`, `about/index.html:39`, `menu/index.html:58`

### Problem

Authored `<img>` elements do not provide `width` and `height` attributes, including the many menu thumbnails.

### Why It Matters

Although CSS supplies aspect ratios in several contexts, intrinsic dimensions let browsers reserve the correct space before CSS and image data arrive, reducing layout movement and improving resilience.

### Recommendation

Add each asset's intrinsic `width` and `height`; retain responsive `max-width: 100%` and `height: auto` styling.

## Positive Findings

- Every page declares a doctype, language, charset, viewport, title, and one main landmark.
- Primary navigation uses ordinary anchors; menu actions use native buttons with accessible names.
- The contact controls have unique IDs, associated labels, appropriate types, and an explicit submit button type.
- Images consistently include `alt`; lists and menu entries use coherent native structures; no duplicate IDs or invalid nested interactive controls were found.
