# Accessibility Audit

## Summary

Source shows a good semantic baseline: language is declared, images have alternatives, controls have names, form labels are associated, and native elements are used. The highest-impact barrier is the small-screen menu's incomplete focus behavior and lack of a JavaScript-failure fallback. A skip mechanism and reduced-motion handling are also absent. Findings: 0 High, 2 Medium, 2 Low, 1 Informational. Two findings/items require rendered or manual verification.

## WebAIM Million Priority Findings

- **Low contrast text:** Unable to determine conclusively from source because several backgrounds are translucent or image-backed; test rendered combinations.
- **Missing alternative text:** No obvious issues found; every authored `<img>` has an `alt` attribute.
- **Missing form labels:** No obvious issues found; visible controls have associated, visually hidden labels.
- **Empty links:** No empty accessible names, but four named social links per page have nonfunctional `href="#"` destinations.
- **Empty buttons:** No obvious issues found; icon buttons have `aria-label` and decorative SVGs are hidden.
- **Missing document language:** No issues found; all pages use `<html lang="en">`.

## Findings

## Finding: Off-canvas menu does not control focus or background interaction

**Severity:** Medium  
**Confidence:** High  
**Files:** `script.js:14`, `script.js:29`, `style.css:298`  
**WCAG:** 2.1.1 Keyboard — Level A; 2.4.3 Focus Order — Level A

### Problem

The menu opens visually without moving focus into it or preventing focus from moving into covered content. Escape and the close button hide the panel without returning focus to the opener.

### User Impact

Keyboard and screen-reader users may have to discover the opened content manually, can tab behind it, and can be left focused on an invisible close button.

### Recommendation

Centralize open/close behavior; move focus into the panel on open and back to the opener on close. If the panel is modal, make the rest of the page inert while open and verify a logical focus cycle.

## Finding: Small-screen navigation becomes unavailable when scripting fails

**Severity:** Medium  
**Confidence:** High  
**Files:** `style.css:293`, `style.css:298`, `script.js:14`  
**WCAG:** 2.1.1 Keyboard — Level A

### Problem

At narrow widths, CSS hides the primary navigation until JavaScript adds a class.

### User Impact

Users whose script does not execute cannot navigate to About, Menu, or Contact from the page.

### Recommendation

Keep navigation visible by default and apply the collapsed enhancement only after JavaScript marks the page as script-enabled.

## Finding: Pages lack a way to bypass repeated navigation

**Severity:** Low  
**Confidence:** High  
**Files:** `index.html:16`, `about/index.html:16`, `contact/index.html:16`, `menu/index.html:16`  
**WCAG:** 2.4.1 Bypass Blocks — Level A

### Problem

All pages begin with repeated branding and navigation, but no skip link targets the main content.

### User Impact

Keyboard users must traverse repeated header controls on every page; this is especially repetitive with the expanded desktop menu.

### Recommendation

Add a first-focusable “Skip to main content” link and a stable target on `<main>`; reveal the link on focus.

## Finding: Motion preferences are not honored

**Severity:** Low  
**Confidence:** High  
**Files:** `style.css:309`, `style.css:683`  
**WCAG:** 2.3.3 Animation from Interactions — Level AAA (advisory)

### Problem

The navigation slides and the linked map scales on interaction, with no reduced-motion branch.

### User Impact

Motion-sensitive users may experience discomfort from avoidable movement.

### Recommendation

Disable nonessential transitions/transforms under `prefers-reduced-motion: reduce`.

## Finding: Rendered contrast and focus visibility require verification

**Severity:** Informational  
**Confidence:** Low  
**Files:** `style.css:92`, `style.css:279`, `style.css:400`, `style.css:646`  
**WCAG:** 1.4.3 Contrast (Minimum) — Level AA; 2.4.7 Focus Visible — Level AA

### Problem

Opacity and hero imagery prevent reliable source-only contrast conclusions; focus styling sometimes depends on browser outlines while hover/focus rules remove link underlines.

### User Impact

If the composites or default outlines are weak, low-vision and keyboard users may struggle to read or locate controls.

### Recommendation

Measure rendered text/control states and inspect keyboard focus in supported browsers; add explicit high-contrast `:focus-visible` indicators where needed.

## Manual Testing Checklist

- At widths below 1024px, operate the menu using Tab, Shift+Tab, Enter/Space, and Escape; verify entry focus, background isolation, logical order, and focus return.
- Disable JavaScript at a narrow width and confirm every primary destination remains reachable.
- At 200% and 400% zoom, verify hero, menu item/price rows, form, and footer reflow without clipping or two-dimensional scrolling.
- Measure contrast for hero text panels, translucent desktop navigation pills, placeholders, footer text, and all focus/hover states.
- Submit invalid and valid contact forms with a screen reader; verify native errors are understandable and the new-tab result/disabled originating button are not confusing.
- Confirm the YouTube embed is keyboard operable and has useful controls/captions for its actual content.

## Positive Findings

- All documents declare English, and all images have useful-looking alternatives.
- Form controls have programmatic labels, correct types, autocomplete tokens, constraints, and a native submit button.
- Main navigation and actions use anchors and buttons rather than generic clickable elements.
- Landmarks, headings, lists, `aria-controls`, `aria-expanded`, and hidden decorative SVGs provide a strong baseline without excessive ARIA.
