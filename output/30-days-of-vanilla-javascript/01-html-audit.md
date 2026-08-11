# HTML Audit

## Summary

The site has a strong document baseline: every page declares language and viewport metadata, challenge pages use clear headings and sections, shared navigation uses anchors, and demos generally use native buttons, forms, tables, and dialogs. The most consequential markup defects are non-operable custom separator/drag patterns and incorrect ARIA placement in the sortable table. Duplicate attributes and incomplete tab relationships affect two tab demos. Findings: 1 High, 2 Medium, 2 Low, 0 Informational.

## Finding: Custom reorder and separator controls lack operable HTML semantics

**Severity:** High  
**Confidence:** High  
**Files:** `day-26/index.html:171`, `day-27/index.html:198`

### Problem

Day 26 makes plain list items draggable without focusable controls or an alternate reorder mechanism. Day 27 puts `role="separator"` on an unfocusable empty `<div>` without value attributes or keyboard instructions.

### Why It Matters

Both demos expose functionality through pointer/mouse gestures that their markup does not make available to keyboard or assistive-technology users.

### Recommendation

For reordering, provide visible move-up/move-down buttons per item (drag can remain an enhancement). Make the separator focusable and implement the complete adjustable-separator pattern (`tabindex`, name, current/min/max values, and keyboard behavior), or present the split as noninteractive content.

## Finding: Sort state is attached to buttons instead of column headers

**Severity:** Medium  
**Confidence:** High  
**Files:** `day-16/index.html:173`

### Problem

`aria-sort` is placed on each button nested within a `<th>`. The attribute applies to the columnheader itself, not the button.

### Why It Matters

Assistive technology may not announce the table’s changing sort state as intended even though the buttons are operable.

### Recommendation

Keep the button for the action, but move and update `aria-sort` on the corresponding `<th scope="col">`.

## Finding: Form errors are not related to their controls

**Severity:** Medium  
**Confidence:** High  
**Files:** `day-09/index.html:173`, `day-22/index.html:190`, `day-23/index.html:192`

### Problem

Inline error paragraphs have IDs but the associated inputs/groups do not reference them with `aria-describedby`/`aria-errormessage`; invalid state is not reflected with `aria-invalid`.

### Why It Matters

Errors displayed after submission may not be announced with or discoverable from the field that needs correction.

### Recommendation

Associate each error with its control or fieldset, set invalid state only while applicable, and use a summary/live region when appropriate. Preserve visible labels and native validation wherever possible.

## Finding: Tab buttons contain duplicate class attributes

**Severity:** Low  
**Confidence:** High  
**Files:** `day-05/index.html:164`, `ui-components/index.html:258`

### Problem

Each tab button declares `class` twice. HTML parsing keeps one value and discards the other, so the intended `tab_button` class is not reliably present.

### Why It Matters

Duplicate attributes are invalid and make styling/query behavior dependent on parser recovery.

### Recommendation

Combine both tokens into one attribute: `class="button tab_button"`.

## Finding: Tab panels omit their label relationship

**Severity:** Low  
**Confidence:** High  
**Files:** `day-05/index.html:175`, `ui-components/index.html:269`

### Problem

Buttons correctly reference panels with `aria-controls`, but each `role="tabpanel"` lacks `aria-labelledby` pointing back to its tab.

### Why It Matters

When users navigate directly into panel content, the panel’s name may not be conveyed.

### Recommendation

Add `aria-labelledby="tab-1"` (and matching values) to each panel.

## Positive Findings

- Documents consistently provide doctype, `lang="en"`, charset, viewport, title, header/main/footer landmarks, and skip navigation on content pages.
- Primary navigation uses normal links; actions generally use typed native buttons.
- Forms use visible labels, tables use captions/scopes, dialogs use native `<dialog>`, images carry alternative text, and decorative SVGs are normally hidden.
- Challenge content is divided with meaningful headings, lists, sections, code blocks, and author asides.
