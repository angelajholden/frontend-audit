# Accessibility Audit

## Summary

Scenic starts from strong native structure: landmarks, buttons, lists, labeled fields, language declarations, and hidden text for icon links are present. Confirmed barriers are the contact page's duplicate form IDs, absent reduced-motion handling, incomplete mobile-menu focus management, a placeholder image alternative, and missing skip navigation. Findings: **0 High, 3 Medium, 3 Low, 0 Informational**. **Five** project-specific areas require manual verification.

## WebAIM Million Priority Findings

- **Low contrast text:** Unable to determine fully from source. Solid authored color pairs appear plausible, but translucent panels, image backgrounds, focus states, and font rendering require browser measurement.
- **Missing alternative text:** No `img` lacks an `alt` attribute; one blog image has obviously unhelpful placeholder alternative text.
- **Missing form labels:** Labels are present, but duplicate IDs on the contact page cause newsletter labels to resolve to the wrong controls.
- **Empty links:** No link has an empty accessible name, but placeholder `href="#"` destinations make the call-to-action and footer social links nonfunctional.
- **Empty buttons:** No obvious issues found; icon-only menu buttons have labels and the print button includes text.
- **Missing document language:** No issues found; all four documents use `<html lang="en">`.

## Findings

## Finding: Duplicate IDs break newsletter label associations

**Severity:** Medium  
**Confidence:** High  
**Files:** `contact/index.html:66`, `contact/index.html:68`, `contact/index.html:98`, `contact/index.html:100`  
**WCAG:** 1.3.1 Info and Relationships — Level A; 4.1.2 Name, Role, Value — Level A

### Problem

The contact and newsletter forms both use `name` and `email` IDs. Newsletter labels therefore point to IDs already used by the contact controls.

### User Impact

Screen-reader label relationships are ambiguous, and clicking the newsletter labels may focus the contact form fields instead of their adjacent controls.

### Recommendation

Assign unique, component-scoped IDs and matching `for` attributes to every control.

## Finding: Mobile menu does not coordinate keyboard focus

**Severity:** Medium  
**Confidence:** Medium  
**Files:** `script.js:12`, `script.js:26`, `style.css:314`, `style.css:319`  
**WCAG:** 2.4.3 Focus Order — Level A; 2.4.7 Focus Visible — Level AA

### Problem

The menu reveals a full-width navigation panel but does not move focus into it or return focus to the trigger when closed. The focused open trigger becomes `visibility: hidden` while active.

### User Impact

Keyboard and screen-reader users can lose their position or encounter focus outside the visible menu, particularly after closing with Escape.

### Recommendation

Move focus to the close control or first link on open, restore it to the trigger on close, and ensure the focused element remains visibly indicated.

## Finding: Motion preferences are not honored

**Severity:** Medium  
**Confidence:** High  
**Files:** `index.html:25`, `index.html:50`, `style.css:81`, `style.css:285`, `style.css:450`  
**WCAG:** 2.3.3 Animation from Interactions — Level AAA (advisory for this audit)

### Problem

Animate.css entrance effects and authored sliding, scaling, and color transitions remain active regardless of `prefers-reduced-motion`.

### User Impact

People with vestibular or motion sensitivities cannot use their operating-system preference to suppress nonessential motion.

### Recommendation

Disable nonessential animation, smooth transitions, scale effects, and fixed-background parallax inside `@media (prefers-reduced-motion: reduce)`.

## Finding: No skip link bypasses repeated navigation

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:25`, `blog/index.html:26`, `contact/index.html:25`, `page/index.html:25`  
**WCAG:** 2.4.1 Bypass Blocks — Level A

### Problem

Each page begins with repeated header navigation and provides no skip link to `main`.

### User Impact

Keyboard and switch users must traverse the header controls on every page before reaching the primary content.

### Recommendation

Add a visible-on-focus “Skip to main content” link before the header and a stable target ID on `main`.

## Finding: Blog image announces placeholder text

**Severity:** Low  
**Confidence:** High  
**Files:** `blog/index.html:50`  
**WCAG:** 1.1.1 Non-text Content — Level A

### Problem

The featured image alternative is `placeholcer text`.

### User Impact

People who cannot perceive the image hear irrelevant placeholder text instead of useful replacement content.

### Recommendation

Use an accurate contextual description, or an empty alternative if the image is decorative.

## Finding: Placeholder destinations create misleading links

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:112`, `index.html:134`, `blog/index.html:111`, `contact/index.html:112`, `page/index.html:93`  
**WCAG:** 2.4.4 Link Purpose (In Context) — Level A

### Problem

The call-to-action and footer social links have meaningful accessible names but use `href="#"`, so their actual behavior is to jump to the document top.

### User Impact

Users who activate a promised action or social destination receive an unexplained focus/scroll change instead.

### Recommendation

Add genuine destinations or remove the placeholder links until they exist.

## Manual Testing Checklist

- Keyboard-test opening, traversing, closing, and Escape behavior of the mobile menu; confirm visible focus and focus restoration.
- Check all interactive controls with `:focus-visible`, especially ordinary text links, social icons, and submit buttons.
- Test both contact-page forms with a screen reader and by clicking labels after IDs are corrected; confirm native error announcements and focus behavior.
- Measure rendered contrast for translucent hero/page panels, muted metadata, placeholders, and every hover/focus state.
- Verify reflow and operability at 200% and 400% zoom and at 320 CSS pixels, including long page headings, forms, social links, and fixed/aspect-ratio regions.

## Positive Findings

- All documents declare English and include clear `main`, header, navigation, and footer structure.
- Menu and print interactions use native buttons; icon-only menu controls have accessible names and decorative SVGs are hidden.
- Forms use visible-to-assistive-technology native labels, suitable input types, required attributes, and useful autocomplete tokens on the contact form.
- Every image has an `alt` attribute, and footer/social icon links include screen-reader text.

