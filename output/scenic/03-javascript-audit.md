# JavaScript Audit

## Summary

The single script is small, dependency-free, organized into focused functions, and generally guards optional page elements. One missing guard causes a deterministic exception on three of four pages; menu focus management is incomplete; and the generic form initializer targets only the first form. Findings: **1 High, 2 Medium, 1 Low, 0 Informational**.

## Finding: Sharing initializer crashes on every non-blog page

**Severity:** High  
**Confidence:** High  
**Files:** `script.js:83`, `script.js:89`, `script.js:102`

### Problem

`sharingIcons()` runs on every page and unconditionally assigns `.href` to `.facebook`, `.x`, `.pinterest`, `.linkedin`, and `.email`. Those elements exist only in `blog/index.html`, so the first assignment throws a `TypeError` on the homepage, contact page, and generic page.

### Why It Matters

Every non-blog page logs an uncaught runtime error. Code added after this initializer would not run, and the shared script is demonstrably not safe across its advertised page scope.

### Recommendation

Return when the sharing component is absent, or initialize only the anchors that exist. Prefer scoping all queries to a detected `.social_sharing` container.

### Example

```js
const sharing = document.querySelector(".social_sharing");
if (!sharing) return;
const facebook = sharing.querySelector(".facebook");
```

## Finding: Mobile menu does not manage focus

**Severity:** Medium  
**Confidence:** High  
**Files:** `script.js:12`, `script.js:26`, `style.css:319`

### Problem

Opening does not move focus into the revealed navigation. Closing by button or Escape does not restore focus to the open trigger. CSS also hides the focused open trigger while the menu is active.

### Why It Matters

Keyboard users can lose visual and logical track of focus, especially after Escape; focus can remain on a hidden control or continue outside the open panel unexpectedly.

### Recommendation

On open, focus the close button or first navigation link; on close (including Escape), restore focus to the open button. Keep `aria-expanded` synchronized through explicit `openMenu`/`closeMenu` functions.

## Finding: Form enhancement operates on only the first form

**Severity:** Medium  
**Confidence:** High  
**Files:** `script.js:36`, `script.js:46`, `contact/index.html:54`, `contact/index.html:97`

### Problem

`document.querySelector("form")` enhances only the first form. On the contact page this is the contact form, so the newsletter gets no validity handling or submit state; on other pages the newsletter is selected. The function name and selector obscure that page-dependent behavior.

### Why It Matters

The same newsletter component behaves differently depending on which page contains it, and future form insertion can silently change which form receives the listener.

### Recommendation

Use explicit component selectors or data attributes and initialize each intended form separately. Limit page URL/path population to the contact form and decide deliberately whether newsletter submission needs the disabling behavior.

## Finding: Share URL parameters use `encodeURI` on the complete URL

**Severity:** Low  
**Confidence:** High  
**Files:** `script.js:68`, `script.js:89`

### Problem

The code interpolates titles and page URLs into query strings and then calls `encodeURI` on the entire result. `encodeURI` intentionally leaves query delimiters such as `&`, `#`, and `?` unescaped within values.

### Why It Matters

Page titles or URLs containing reserved characters can corrupt the destination query or alter the shared value.

### Recommendation

Construct URLs with `URL` and `URLSearchParams`, which encode each parameter value correctly; encode `mailto:` subject/body values with `encodeURIComponent`.

## Positive Findings

- The script uses `textContent` rather than HTML injection and contains no secrets, `eval`, network requests, or unsafe DOM construction.
- It uses native buttons, standard browser APIs, and an Escape-key handler.
- Most optional elements are guarded before use, and the deferred script performs small, event-driven operations rather than polling or heavy work.

