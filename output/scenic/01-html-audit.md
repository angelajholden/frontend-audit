# HTML Audit

## Summary

The four documents have complete document shells, useful landmarks, native controls, lists for navigation, and generally coherent headings. The main defects are repeated placeholder links, duplicate IDs on the contact page, an unhelpful blog image alternative, and a homepage section whose heading hierarchy starts at `h3`. Findings: **0 High, 2 Medium, 3 Low, 0 Informational**.

## Finding: Placeholder links do not provide real navigation

**Severity:** Medium  
**Confidence:** High  
**Files:** `index.html:112`, `index.html:134`, `blog/index.html:111`, `contact/index.html:112`, `page/index.html:93`

### Problem

The homepage call-to-action and all five footer social links on every page use `href="#"`. Activating them returns users to the top of the current document instead of fulfilling the stated purpose.

### Why It Matters

These are exposed as real links but have placeholder behavior, producing misleading navigation for pointer, keyboard, and assistive-technology users.

### Recommendation

Supply the actual destinations. Until destinations exist, remove the controls rather than publishing false links.

## Finding: Contact page repeats form-control IDs

**Severity:** Medium  
**Confidence:** High  
**Files:** `contact/index.html:66`, `contact/index.html:68`, `contact/index.html:98`, `contact/index.html:100`

### Problem

Both the contact form and newsletter form use `id="name"` and `id="email"`. The newsletter labels' `for` values therefore resolve to the first controls with those IDs, not reliably to their adjacent newsletter inputs.

### Why It Matters

Duplicate IDs make the document invalid and break deterministic label/control relationships. Activating a newsletter label can focus the contact form instead.

### Recommendation

Give every control a page-unique ID such as `contact-name`, `contact-email`, `newsletter-name`, and `newsletter-email`, updating each label's `for` value.

## Finding: Homepage card headings lack an identifying section heading

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:56`, `index.html:63`

### Problem

The card grid is a generic `div` containing six `article` elements whose titles begin at `h3`, but the grid has no `h2` or other section heading that explains the collection.

### Why It Matters

The headings appear as children of the hero's `h2` in the document outline even though they represent a separate collection, making heading navigation and content structure less clear.

### Recommendation

Mark the collection as a section with a meaningful `h2` (visually hidden if the design does not call for a visible label), then retain `h3` card titles; or use `h2` for the cards if no group heading is appropriate.

## Finding: Blog image alternative is placeholder text

**Severity:** Low  
**Confidence:** High  
**Files:** `blog/index.html:50`

### Problem

The article's featured image has `alt="placeholcer text"`, which is neither a description nor an intentional empty alternative.

### Why It Matters

The malformed placeholder is announced as the image's replacement content and does not convey its purpose.

### Recommendation

Replace it with context-appropriate alternative text or `alt=""` if the image is decorative and the surrounding content makes it redundant.

## Finding: Logo markup changes semantic level between pages

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:28`, `blog/index.html:29`, `contact/index.html:28`, `page/index.html:28`

### Problem

The site logo is the homepage `h1` but becomes a generic `div` on inner pages. While the inner pages correctly provide content `h1` elements, duplicating the same visual component with different element types makes shared markup harder to maintain.

### Why It Matters

The inconsistency encourages page-specific CSS/markup drift in already duplicated site chrome.

### Recommendation

Use a consistent neutral logo container and give every page a content-specific `h1`; on the homepage, make the visible hero/site heading the page heading.

## Positive Findings

- Every page has a doctype, language, charset, viewport, title, and one `main` landmark.
- Navigation uses native anchors in lists, while menu, print, and submit actions use native buttons with explicit types where needed.
- Visible form controls have native label associations (apart from the contact-page duplicate-ID defect), and relevant input types/autocomplete values are used.
- Images consistently include `alt` attributes, and figure/image markup is structurally valid.

