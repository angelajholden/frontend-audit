# Accessibility Audit

## Summary

The site demonstrates many strong accessibility foundations: all documents declare language, meaningful images have alternatives, forms are visibly labeled, icon controls have accessible names, skip links are widespread, and native buttons/dialogs/tables are common. Two completed demos nevertheless make their essential interaction pointer-only, and form/navigation state handling creates additional barriers. Findings: 2 High, 4 Medium, 2 Low, 1 Informational. Five areas require manual verification.

## WebAIM Million Priority Findings

- **Low contrast text:** No obvious source-defined failures found in the principal theme combinations; rendered interactive states still require verification.
- **Missing alternative text:** No obvious issues found; authored `<img>` elements include `alt`, and dynamically created images receive alternatives.
- **Missing form labels:** No obvious issues found; user-facing controls have visible or visually hidden labels.
- **Empty links:** No empty accessible names found. Day 05 contains named placeholder `href="#"` demo links, which are nonfunctional but not empty.
- **Empty buttons:** No obvious issues found; icon-only buttons have `aria-label` or hidden SVG plus text.
- **Missing document language:** No issues found; every HTML document declares `lang="en"`.

## Findings

## Finding: Drag-and-drop list has no keyboard alternative

**Severity:** High  
**Confidence:** High  
**Files:** `day-26/index.html:171`, `components/drag-drop.js:29`  
**WCAG:** 2.1.1 Keyboard — Level A; 2.5.7 Dragging Movements — Level AA

### Problem

Items can be reordered only through dragging plain list items.

### User Impact

Keyboard, switch, voice-control, and some touch users cannot perform the exercise’s central action.

### Recommendation

Add focusable move-up/move-down controls and announce the new position; keep drag-and-drop only as an enhancement.

## Finding: Split-view separator is mouse-only and structurally incomplete

**Severity:** High  
**Confidence:** High  
**Files:** `day-27/index.html:198`, `components/split-view.js:11`  
**WCAG:** 2.1.1 Keyboard — Level A; 4.1.2 Name, Role, Value — Level A

### Problem

The separator cannot receive focus, has no accessible name or current/min/max values, and reacts only to mouse events.

### User Impact

Keyboard, screen-reader, and touch-only users cannot resize the panels or determine the separator state.

### Recommendation

Implement the adjustable-separator pattern with focus, name/value properties, arrow/Home/End keys, and Pointer Events; provide a responsive non-resizable fallback.

## Finding: Form errors are not programmatically associated or focused

**Severity:** Medium  
**Confidence:** High  
**Files:** `day-09/index.html:173`, `components/form-validation.js:26`, `components/multistep-form.js:36`  
**WCAG:** 3.3.1 Error Identification — Level A; 3.3.3 Error Suggestion — Level AA; 4.1.3 Status Messages — Level AA

### Problem

Errors are visually unhidden but not connected to controls, invalid state is not conveyed, focus stays on submit, and Day 09’s “success” appears before success is known.

### User Impact

Screen-reader and keyboard users may not know an error appeared, which field it describes, or whether submission actually succeeded.

### Recommendation

Use native validation where possible; associate errors, set invalid state, focus the first invalid control or a summary, and announce only confirmed status.

## Finding: Mobile navigation does not provide a coherent focus boundary

**Severity:** Medium  
**Confidence:** High  
**Files:** `global/resp-nav.js:6`, `style.css:582`  
**WCAG:** 2.4.3 Focus Order — Level A; 2.4.11 Focus Not Obscured (Minimum) — Level AA

### Problem

The fixed drawer opens without moving focus, allows focus behind it, and closes without restoring focus.

### User Impact

Users can lose position or interact with obscured content while the navigation covers the page.

### Recommendation

Manage focus on entry/exit and make background content inert if the drawer is modal.

## Finding: Sort state is exposed on the wrong element

**Severity:** Medium  
**Confidence:** High  
**Files:** `day-16/index.html:173`, `components/sortable-table.js:20`  
**WCAG:** 4.1.2 Name, Role, Value — Level A

### Problem

JavaScript updates `aria-sort` on nested buttons instead of columnheader cells.

### User Impact

Screen-reader users may activate sorting without receiving the resulting column/direction state.

### Recommendation

Update `aria-sort` on the `<th>` and keep the button’s accessible name concise.

## Finding: Split view cannot reflow at narrow widths or high zoom

**Severity:** Medium  
**Confidence:** High  
**Files:** `style.css:1545`, `style.css:1560`  
**WCAG:** 1.4.10 Reflow — Level AA

### Problem

Two 240px-minimum panels remain in one row at every viewport width.

### User Impact

Low-vision users zooming content and users on narrow screens must pan horizontally to read and operate the demo.

### Recommendation

Stack panels and disable the separator interaction below an appropriate width.

## Finding: Tab panel names are incomplete

**Severity:** Low  
**Confidence:** High  
**Files:** `day-05/index.html:175`, `ui-components/index.html:269`  
**WCAG:** 4.1.2 Name, Role, Value — Level A

### Problem

Tabpanels are not labeled by their corresponding tabs.

### User Impact

Panel context can be unclear when navigating directly into its contents.

### Recommendation

Add matching `aria-labelledby` references and correct the duplicate class attributes.

## Finding: Motion preferences are ignored

**Severity:** Low  
**Confidence:** High  
**Files:** `style.css:33`, `style.css:59`, `style.css:591`  
**WCAG:** 2.3.3 Animation from Interactions — Level AAA (advisory)

### Problem

Smooth scroll, drawer movement, and other transitions remain enabled under reduced-motion preferences.

### User Impact

Motion-sensitive users cannot reduce avoidable page movement.

### Recommendation

Disable nonessential motion under `prefers-reduced-motion: reduce`.

## Finding: Rendered component behavior requires assistive-technology verification

**Severity:** Informational  
**Confidence:** Low  
**Files:** `day-04/index.html:172`, `day-06/index.html:170`, `day-20/index.html:228`

### Problem

Source cannot confirm focus return/announcement behavior across native dialogs, toasts, loaded video, lightbox navigation, themes, and browsers.

### User Impact

Implementation details may work differently across browser and assistive-technology combinations.

### Recommendation

Run the targeted checks below before launch.

## Manual Testing Checklist

- Keyboard and screen-reader test the responsive navigation, tabs, modal, lightbox, toast controls, sortable table, and all form validation paths.
- Confirm focus returns to each dialog/lightbox trigger after button, backdrop, and Escape closure.
- Test Day 26 reordering and Day 27 resizing with keyboard, touch, voice control, and screen-reader announcements after fixes.
- Test every theme/light-mode combination for focus visibility and rendered contrast.
- Verify 200%/400% zoom and 320 CSS-pixel reflow, especially navigation, tables, code blocks, lightbox, and split view.
- Verify lazy-loaded YouTube focus placement, keyboard operation, title, captions, and reduced-motion behavior.

## Positive Findings

- All documents declare English; images consistently have alternatives; form fields have labels; icon controls have names.
- Content pages provide a skip link to a real target, and main landmarks/headings are consistent.
- Native controls, dialog, tables, fieldsets/legends, lists, and restrained ARIA provide a strong baseline.
- The tab script supports arrow navigation, toast content uses a polite live region, and decorative SVGs are normally hidden.
