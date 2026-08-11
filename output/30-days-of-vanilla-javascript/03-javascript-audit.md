# JavaScript Audit

## Summary

The codebase is sensibly split into one shared module graph and focused challenge modules. Most files guard optional roots, use DOM creation/text APIs instead of interpolating external HTML, check fetch responses, and keep state local. The most significant issues are mouse/drag-only interactions, broken email validation, incomplete navigation focus behavior, and avoidable listener accumulation. Findings: 2 High, 3 Medium, 2 Low, 0 Informational.

## Finding: Reorder and split-view interactions exclude keyboard and touch input

**Severity:** High  
**Confidence:** High  
**Files:** `components/drag-drop.js:29`, `components/split-view.js:11`

### Problem

Reordering listens only for HTML drag events. Split resizing listens only for `mousedown`/`mousemove`/`mouseup`; neither implements keyboard behavior, and the split view also lacks pointer/touch events.

### Why It Matters

Core demo functionality is unavailable to keyboard users, and split resizing is unavailable on touch-only devices. The site presents these as completed interactive exercises.

### Recommendation

Add native button-based reorder actions as the reliable baseline. Implement the separator with keyboard increments and Pointer Events, or explicitly remove interactive separator semantics/functionality where unsupported.

## Finding: Contact form accepts malformed email as valid

**Severity:** High  
**Confidence:** High  
**Files:** `day-09/index.html:167`, `components/form-validation.js:26`

### Problem

The form disables native constraint validation with `novalidate`, but JavaScript checks only whether the email is empty. A value such as `x` passes, hides “Please enter a valid email,” shows success, and proceeds to Formspree. The success message is exposed before the external submission result is known.

### Why It Matters

The demo’s stated validation function is incorrect and can submit unusable contact data while announcing an unverified success.

### Recommendation

Prefer native `required` and `type="email"` validation, or use `checkValidity()`/`validity` for equivalent custom handling. Show success only after a confirmed result; do not claim success immediately before new-tab navigation.

## Finding: Responsive navigation does not manage focus

**Severity:** Medium  
**Confidence:** High  
**Files:** `global/resp-nav.js:6`, `global/resp-nav.js:18`

### Problem

Both buttons blindly toggle one class. Opening does not move focus into the long panel; closing or Escape does not return focus; background content remains focusable while the fixed overlay is open.

### Why It Matters

Keyboard and screen-reader users can tab behind the panel and may be left focused on its now-hidden close button.

### Recommendation

Use explicit open/close functions that synchronize class/ARIA state, place focus in the panel, restore it to the trigger, and isolate background interaction if this is a modal navigation drawer.

## Finding: Navigation filter creates duplicate reset listeners on every search

**Severity:** Medium  
**Confidence:** High  
**Files:** `global/filter-nav.js:10`, `global/filter-nav.js:24`

### Problem

The reset listener is registered inside the debounced input callback and inside the loop over every navigation item. Each search adds roughly 30 more handlers.

### Why It Matters

Long sessions accumulate redundant closures and repeat reset work, making a shared site-wide interaction increasingly wasteful and harder to reason about.

### Recommendation

Register one reset listener once, outside both the input handler and item loop. Let it clear the input, unhide every item, and hide the empty state.

## Finding: Split view accumulates document mouseup listeners

**Severity:** Medium  
**Confidence:** High  
**Files:** `components/split-view.js:15`, `components/split-view.js:27`

### Problem

A new document-level `mouseup` listener is added during every handled `mousemove` while resizing; none is removed.

### Why It Matters

A single drag can register many permanent listeners, and subsequent mouseup events invoke all of them.

### Recommendation

Register one end handler once, or add it on drag start with `{ once: true }`; use Pointer Events and pointer capture for a unified lifecycle.

## Finding: Form module’s missing-root guard is ineffective

**Severity:** Low  
**Confidence:** High  
**Files:** `components/form-validation.js:2`

### Problem

`if (!root);` is an empty statement, so the function continues and dereferences `root`.

### Why It Matters

The current page happens to include the form, but reusing or accidentally loading this module elsewhere throws immediately and can disrupt other modules.

### Recommendation

Use `if (!root) return;`, consistent with the other component modules.

## Finding: Generated breed links point to routes the repository does not provide

**Severity:** Low  
**Confidence:** High  
**Files:** `components/transforming-api-data.js:66`

### Problem

For breeds with a nonzero count, Day 19 generates origin-root URLs such as `/affenpinscher`; no corresponding pages/routes exist in this static repository.

### Why It Matters

The generated UI exposes links that resolve to the custom 404 page unless production has undocumented routing.

### Recommendation

Render non-navigating text for demonstration data, link only to real resources, or provide actual route targets.

## Positive Findings

- Modules are small, purpose-specific, dependency-free, and mostly return safely when their component is absent.
- Fetch modules check `response.ok`, catch failures, and avoid rendering when data is unavailable.
- External/local data is usually assigned with `textContent`, attributes, and DOM APIs; no `eval`, dynamic functions, or frontend credentials were found.
- Tabs support arrow keys; native dialogs supply baseline focus containment; local state uses appropriate localStorage/History APIs.
