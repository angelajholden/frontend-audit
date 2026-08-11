# JavaScript Audit

## Summary

The 70-line deferred script is small, dependency-free, and uses safe DOM APIs. It guards optional page elements, uses native constraint validation, and avoids DOM injection, requests, secrets, or expensive handlers. Its important weakness is incomplete focus/state handling in the off-canvas menu; the CSS also makes primary navigation unavailable when JavaScript does not run. Findings: 0 High, 2 Medium, 1 Low, 0 Informational.

## Finding: Menu opening and closing does not manage focus

**Severity:** Medium  
**Confidence:** High  
**Files:** `script.js:14`, `script.js:29`

### Problem

Clicking either menu button only toggles a body class and `aria-expanded`. Opening does not move focus into the panel; closing by button or Escape does not return focus to the opener; focus is not contained while the modal-like fixed panel is open.

### Why It Matters

Keyboard and screen-reader users can lose their place, tab into obscured page content, or close the menu while focus remains on a now-hidden control.

### Recommendation

Treat the small-screen panel as one coherent disclosure: explicitly open and close it, focus the close button or first navigation link on open, return focus to the opener on close, and prevent background interaction if it is intended to behave modally.

## Finding: Primary navigation has no no-script fallback at small widths

**Severity:** Medium  
**Confidence:** High  
**Files:** `script.js:14`, `style.css:293`, `style.css:298`

### Problem

The script is the sole mechanism that adds `.menu_active`, while CSS hides navigation at small widths by default.

### Why It Matters

A script error or blocked JavaScript makes every non-current page unreachable through primary navigation.

### Recommendation

Make visible navigation the baseline. Add a short scripting-enabled marker during startup, and scope the collapsed state to that marker; alternatively provide a real no-script navigation path.

## Finding: Menu code models two actions as an undifferentiated toggle

**Severity:** Low  
**Confidence:** High  
**Files:** `script.js:2`, `script.js:16`

### Problem

Both open and close buttons share one listener that blindly toggles state. The close button also lacks `aria-controls`, and state transitions are spread between click and Escape handlers.

### Why It Matters

Toggle semantics are fragile if a control fires in an unexpected state and make focus restoration or future close mechanisms harder to implement consistently.

### Recommendation

Create explicit `openMenu()` and `closeMenu()` functions that own class, ARIA, and focus state. Bind each button to its intended action and route Escape through `closeMenu()`.

## Positive Findings

- The script is deferred, compact, and scoped with `const` and functions rather than mutable global state.
- Optional elements are checked before use, and `textContent`/attributes are used instead of unsafe HTML injection.
- Native form validity is preserved; submission work is limited to context fields and duplicate-submit prevention.
- No third-party JavaScript dependency, async race, high-frequency event, or frontend secret was found.
