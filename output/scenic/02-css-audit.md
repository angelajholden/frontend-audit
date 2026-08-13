# CSS Audit

## Summary

The authored stylesheet is readable, sensibly sectioned, uses custom properties and modern Grid/Flexbox, and includes responsive rules. Specificity remains controlled and there is no `!important`. The main concerns are focus indicators that disappear or depend on subtle color changes, animation without a reduced-motion override, and mobile navigation transition/state behavior that needs verification. Findings: **0 High, 2 Medium, 1 Low, 1 Informational**.

## Finding: Global link focus styling removes the persistent indicator

**Severity:** Medium  
**Confidence:** High  
**Files:** `style.css:101`, `style.css:111`, `style.css:910`, `style.css:915`

### Problem

Links are normally underlined, but the combined `:focus`, `:hover`, and `:active` rules make their underline transparent. Most links receive no explicit outline, background, or other persistent focus treatment from authored CSS.

### Why It Matters

Keyboard users may lose the only obvious authored indication of which link is focused; browser defaults vary and can be visually weak against the site's colors.

### Recommendation

Keep the underline for keyboard focus and add a clear `:focus-visible` outline with offset. Reserve underline removal for hover only if desired.

### Example

```css
a:focus-visible {
  outline: 3px solid currentColor;
  outline-offset: 3px;
  text-decoration-color: currentColor;
}
```

## Finding: Authored and third-party motion has no reduced-motion treatment

**Severity:** Medium  
**Confidence:** High  
**Files:** `index.html:25`, `index.html:50`, `style.css:81`, `style.css:285`, `style.css:450`, `animate.css:1`

### Problem

Pages opt into Animate.css entrance animations and authored rules animate the menu, images, colors, and icons, but `style.css` contains no `prefers-reduced-motion` override.

### Why It Matters

Users who request reduced motion still receive page entrances, sliding navigation, and scaling effects, which can cause discomfort and make interaction harder.

### Recommendation

Add a reduced-motion media query that disables nonessential animations/transitions and removes the fixed-background parallax effect.

## Finding: Mobile menu hides its trigger while open

**Severity:** Low  
**Confidence:** Medium  
**Files:** `style.css:269`, `style.css:314`, `style.css:319`

### Problem

At narrow widths, opening the menu sets the open button to `visibility: hidden`; the separate close button is moved into view with the panel. Focus remains on the now-hidden trigger until the user tabs or the browser adjusts it.

### Why It Matters

The focused element may become visually absent, and close/reopen focus behavior can feel discontinuous for keyboard users.

### Recommendation

Keep the controlling trigger available and swap its icon/label, or explicitly coordinate focus with the close button in JavaScript. Verify the complete transition in a browser.

## Finding: Several responsive choices require rendered verification

**Severity:** Informational  
**Confidence:** Low  
**Files:** `style.css:325`, `style.css:385`, `style.css:482`, `style.css:839`

### Problem

Large hero/subscribe regions use aspect ratios, the inner hero uses fixed 48rem/30rem heights, and faux parallax uses `background-attachment: fixed`.

### Why It Matters

Source alone cannot establish whether long text, zoom, small landscape viewports, and mobile browser behavior reflow without clipping or excessive whitespace.

### Recommendation

Verify these regions at 320 CSS pixels, landscape mobile sizes, and 200%/400% zoom; adjust only where a concrete failure appears.

## Positive Findings

- The stylesheet is divided into recognizable component sections and reuses a compact custom-property palette.
- Selectors are short and component-scoped; IDs and `!important` are not used for styling.
- Images use responsive defaults, card/media cropping uses explicit aspect ratios, and grids collapse at practical breakpoints.
- Native document flow, Grid, and Flexbox handle primary layout; absolute positioning is limited to layered imagery and the mobile panel.

