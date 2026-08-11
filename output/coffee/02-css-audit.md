# CSS Audit

## Summary

The single authored stylesheet is logically grouped, uses a small custom-property palette, low-specificity selectors, modern Grid/Flexbox, fluid type, and coherent breakpoints. There is no `!important` escalation. The main risks are the JavaScript-dependent mobile navigation presentation and motion without a reduced-motion alternative; rendered contrast and reflow still need browser verification. Findings: 0 High, 1 Medium, 1 Low, 1 Informational.

## Finding: Mobile navigation is hidden unless JavaScript changes a body class

**Severity:** Medium  
**Confidence:** High  
**Files:** `style.css:293`, `style.css:298`, `style.css:352`

### Problem

At 1024px and below, `.navigation` is hidden and moved off-screen. It becomes available only when JavaScript adds `.menu_active`.

### Why It Matters

If the script fails, is blocked, or loads late, all primary navigation disappears behind a button that cannot operate. CSS therefore makes core navigation depend on JavaScript.

### Recommendation

Use a progressive-enhancement marker set by JavaScript and hide/collapse navigation only when scripting is known to be active. Keep ordinary navigation visible in the no-script state.

## Finding: Interface motion has no reduced-motion alternative

**Severity:** Low  
**Confidence:** High  
**Files:** `style.css:97`, `style.css:233`, `style.css:309`, `style.css:683`

### Problem

Links, controls, the off-canvas navigation, and the map zoom animate for 300ms, but the stylesheet has no `prefers-reduced-motion` override.

### Why It Matters

The sliding panel and 1.25× map zoom can be uncomfortable for motion-sensitive users who have requested reduced motion.

### Recommendation

Inside `@media (prefers-reduced-motion: reduce)`, remove or substantially shorten nonessential transitions and transforms.

## Finding: Rendered contrast and focus visibility need browser confirmation

**Severity:** Informational  
**Confidence:** Low  
**Files:** `style.css:92`, `style.css:100`, `style.css:270`, `style.css:400`, `style.css:646`

### Problem

Text and controls are layered over translucent backgrounds and a hero image. Focus frequently shares hover styling, and ordinary links make their underline transparent on focus, though the user-agent outline is not explicitly removed.

### Why It Matters

Final contrast depends on compositing and image pixels; focus visibility depends on browser defaults. Static CSS alone cannot establish the rendered result.

### Recommendation

Measure representative rendered combinations and verify a clearly visible focus indicator in supported browsers. Add explicit `:focus-visible` outlines if defaults are inconsistent.

## Positive Findings

- The cascade is restrained: no ID selectors, no `!important`, and no long specificity chains.
- Grid/Flexbox layouts collapse at content-relevant breakpoints and images are responsive.
- Color tokens, `box-sizing`, fluid typography, and shared component rules improve consistency.
- Form controls retain native focus outlines and the visually-hidden utility follows a conventional clipping pattern.
