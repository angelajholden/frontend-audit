# Prompt 04: Accessibility Audit

Audit the project's frontend for accessibility issues.

Use the Project Discovery report and the previous HTML, CSS, and JavaScript audits to identify the correct source files and relevant implementation patterns.

Do not modify any files.

## Goal

Evaluate the website for practical, high-impact accessibility barriers, with primary emphasis on the most common accessibility failures found across the web.

This audit should be guided by the findings of the **WebAIM Million** study and by WCAG 2.2 Level A and AA where the source code provides enough evidence to identify a likely failure.

The primary question is:

**What accessibility problems in this project are most likely to create real barriers for users, and which should be fixed first?**

This is not intended to be an exhaustive theoretical WCAG audit.

Prioritize common, concrete, user-impacting problems over obscure edge cases.

## WebAIM Million Priority

The WebAIM Million consistently identifies six categories that account for the overwhelming majority of automatically detectable accessibility errors:

1. Low contrast text
2. Missing alternative text for images
3. Missing form input labels
4. Empty links
5. Empty buttons
6. Missing document language

Evaluate these categories first and give them clear visibility in the report.

Do not stop after these six categories. They are the priority, not the entire scope.

## Scope and Limitations

This is primarily a source-code audit.

Static analysis can identify many accessibility problems, but it cannot determine whether a website is fully accessible or WCAG conformant.

Do not claim:

- the website is accessible
- the website is inaccessible in every context
- the website passes WCAG
- the website fails WCAG comprehensively
- the website is ADA compliant
- the website satisfies any legal accessibility requirement

unless a specific finding can actually be supported by the available evidence.

Clearly distinguish:

- issues directly supported by source code
- likely issues requiring browser verification
- items requiring manual accessibility testing

## Priority 1: WebAIM Million Common Errors

### 1. Low Contrast Text

Inspect authored foreground and background colors where contrast can reasonably be determined.

Pay particular attention to:

- body text
- small text
- muted text
- links
- form labels
- placeholder text
- buttons
- navigation
- alerts
- validation messages
- disabled or secondary states

Where exact colors are available, identify combinations that appear likely to fall below WCAG 2.2 AA contrast requirements.

Do not estimate contrast ratios from color names or incomplete styling information.

If background layering, opacity, images, gradients, runtime styles, or inherited values make the result uncertain, mark the finding for browser verification.

Do not flag low contrast merely because a color appears visually subtle.

### 2. Missing Image Alternative Text

Inspect meaningful `<img>` elements.

Identify:

- images without an `alt` attribute
- linked images without an accessible name
- obviously unhelpful alternative text such as filenames or generic descriptions
- repeated alternative text that appears unrelated to the actual image
- meaningful images explicitly given `alt=""`

Do not assume every image needs descriptive alternative text.

Decorative images may appropriately use:

```html
alt=""
```

Distinguish between:

- missing alternative text
- intentionally empty alternative text
- questionable alternative text
- alternative text whose quality requires human review

Do not invent alternative text unless necessary to illustrate a recommendation.

### 3. Missing Form Labels

Inspect form controls for accessible names.

Look for:

- inputs without associated `<label>` elements
- `<label>` elements whose `for` does not match an input ID
- placeholders being used as the only label
- unlabeled textareas
- unlabeled selects
- unlabeled custom controls
- icon-only form controls without an accessible name

Prefer native `<label>` relationships where appropriate.

Do not recommend `aria-label` as the default solution when a visible native label is appropriate.

### 4. Empty Links

Identify links that do not have a meaningful accessible name.

Examples include:

- empty `<a>` elements
- icon-only links without accessible text
- linked images with missing alternative text
- links whose text is hidden incorrectly
- links whose accessible name depends entirely on unavailable generated content

Distinguish empty links from links whose wording is merely ambiguous.

Also identify repeated generic link text such as:

- "click here"
- "read more"
- "more"
- "learn more"

when the lack of context is likely to make navigation difficult.

### 5. Empty Buttons

Identify buttons that do not have a meaningful accessible name.

Examples include:

- empty `<button>` elements
- icon-only buttons without accessible text
- controls whose label exists only as a CSS background image
- buttons containing SVG icons with no usable accessible name

Prefer visible text where appropriate.

When an icon-only control is legitimate, ensure it has an accessible name using an appropriate technique.

Do not add redundant ARIA when button text already provides an accessible name.

### 6. Missing Document Language

Inspect the root `<html>` element.

Identify:

- missing `lang`
- empty `lang`
- obviously invalid language values
- language declarations that clearly contradict the primary language of the page when this can be determined from source

Do not speculate about multilingual content when the repository does not provide enough context.

## Priority 2: Common Structural Accessibility Problems

After evaluating the WebAIM Million priority categories, review the following common structural issues.

### 7. Semantic HTML

Review whether native HTML semantics provide appropriate meaning.

Pay attention to:

- buttons
- links
- navigation
- forms
- headings
- lists
- tables
- landmarks
- details/summary
- dialogs

Generic elements should not recreate native interactive controls without a clear reason.

When the accessibility problem originates in incorrect HTML, recommend correcting the HTML rather than adding ARIA or additional JavaScript.

### 8. Heading Structure

Review headings for meaningful document organization.

Look for:

- text visually styled as headings but not marked up as headings
- headings used solely for visual styling
- empty headings
- significant skipped heading levels
- heading structure that does not reflect content relationships

Do not mechanically require exactly one `<h1>` without considering the actual document structure.

Focus on whether headings provide useful navigation and hierarchy.

### 9. Landmarks

Review use of:

- `<header>`
- `<nav>`
- `<main>`
- `<aside>`
- `<footer>`
- corresponding ARIA landmarks when necessary

Check whether the primary content can be identified.

Look for:

- missing main content landmarks
- unnecessarily duplicated landmarks
- multiple navigation regions that cannot be distinguished where labels are needed
- redundant landmark roles on native semantic elements

Do not add ARIA landmarks where native HTML already provides them.

### 10. Skip Navigation

Determine whether pages with substantial repeated navigation provide a practical way to bypass it.

If a skip link exists, check whether:

- it can receive keyboard focus
- it becomes visible when appropriate
- its target exists
- the target works
- it does not move focus somewhere unexpected

Do not automatically require a skip link when the page structure does not contain meaningful repeated navigation.

### 11. Keyboard Accessibility

Review interactive functionality for clear source-level keyboard barriers.

Look for:

- click handlers on non-interactive elements
- interactions dependent entirely on mouse events
- custom controls without equivalent keyboard behavior
- keyboard traps
- inaccessible menus
- inaccessible disclosure controls
- controls removed from the tab order incorrectly

Prefer native interactive elements because they provide keyboard behavior automatically.

Do not recreate native keyboard behavior with JavaScript when changing the element itself is the better solution.

### 12. Focus Visibility

Inspect CSS for:

- `outline: none`
- `outline: 0`
- focus styles removed without replacement
- focus indicators that appear indistinguishable from the surrounding interface
- interactive elements that only provide hover states

Do not flag outline removal when a clear custom focus indicator exists.

### 13. Focus Management

Review interfaces such as:

- dialogs
- drawers
- overlays
- menus
- dynamically revealed content
- content removed after interaction

Look for clear problems involving:

- focus disappearing
- focus moving unexpectedly
- focus trapped unintentionally
- closed dialogs failing to return focus reasonably
- hidden interactive elements remaining focusable

Mark findings requiring browser verification accordingly.

### 14. Form Errors and Instructions

Where source code provides enough evidence, review:

- required fields
- validation messaging
- error identification
- relationships between errors and controls
- instructions required to complete inputs
- invalid states

Do not attempt to judge the complete usability of a form through static source alone.

### 15. Tables

Review data tables for:

- proper table markup
- header cells
- meaningful associations where needed
- tables used for layout

Do not require complex ARIA table structures when native table markup provides sufficient semantics.

### 16. Page Title

Check that pages have a meaningful `<title>` structure.

Detailed search optimization belongs in the SEO audit, but missing or unusable document titles are also accessibility concerns because users rely on them for orientation.

### 17. Link and Button Purpose

Beyond empty controls, identify cases where the accessible name is present but likely insufficient to understand the control's purpose.

Prioritize cases where multiple controls expose the same generic name without enough surrounding context.

Do not flag every "Learn more" or "Read more" link automatically.

Evaluate whether its programmatic context makes the destination understandable.

## Priority 3: ARIA Review

Review ARIA carefully.

ARIA should not be treated as inherently good.

The WebAIM Million has repeatedly found that pages containing more ARIA also tend to contain more detected accessibility errors. This does not prove that ARIA causes the errors; complex interfaces frequently use more ARIA. However, incorrect ARIA can create accessibility problems rather than solve them.

Use the principle:

**Native HTML first. ARIA only when necessary.**

Look for:

- invalid roles
- invalid ARIA attributes
- inappropriate role overrides
- `role="button"` on elements that should be buttons
- redundant roles
- incorrect `aria-expanded`
- incorrect `aria-hidden`
- incorrect `aria-controls`
- broken `aria-labelledby`
- broken `aria-describedby`
- IDs referenced by ARIA that do not exist
- interactive descendants hidden from accessibility APIs
- ARIA states that can become inconsistent with visual state
- custom ARIA widgets without the required interaction behavior

Do not recommend adding ARIA merely because an element currently lacks ARIA.

Do not add `aria-label` when visible text or native semantics already provide an appropriate accessible name.

### ARIA Menus

Pay particular attention to:

```html
role="menu" role="menubar" role="menuitem"
```

These roles have specific interaction expectations.

Do not recommend ARIA menu roles for ordinary website navigation.

Ordinary site navigation usually belongs in semantic navigation markup with links.

## Priority 4: Motion, Zoom, and Responsive Accessibility

Review source-level evidence involving:

### Reduced Motion

Identify substantial motion or animation that may need:

```css
@media (prefers-reduced-motion: reduce);
```

Prioritize:

- continuous animation
- parallax
- large movement
- zooming
- spinning
- autoplay motion

Do not require reduced-motion handling for every minor hover transition.

### Zoom and Text Scaling

Identify obvious patterns likely to interfere with:

- browser zoom
- text enlargement
- content reflow

Examples include:

- rigid fixed-height text containers
- large minimum widths
- clipping
- horizontal overflow caused by inflexible layouts

Do not claim zoom failure without runtime verification.

## Priority 5: Issues Requiring Manual Testing

Create a separate section for accessibility concerns that cannot be reliably determined through source review.

Examples include:

- keyboard tab order in the rendered interface
- visible focus quality
- screen-reader announcements
- reading order
- dynamic state announcements
- modal behavior
- responsive reflow at 400% zoom
- actual color contrast after rendered styles are applied
- content meaning and alternative-text quality
- captions and transcripts
- animation behavior
- form error experience
- touch target usability
- timing-dependent interactions

Do not report these as confirmed defects unless source code clearly establishes the problem.

Instead, describe exactly what should be manually tested.

## Avoid Accessibility Overengineering

Do not recommend complex accessibility implementations when a simpler native solution exists.

Do not turn every theoretical WCAG edge case into a required engineering change.

Do not recommend:

- custom ARIA widgets when native HTML works
- excessive focus management
- excessive live regions
- redundant accessible names
- unnecessary `tabindex`
- redundant ARIA
- visually hidden text when visible text would be better
- JavaScript keyboard handlers on native controls that already support keyboard interaction

Accessibility improvements should reduce barriers, not introduce unnecessary implementation complexity.

## Finding Priority

Accessibility severity should reflect likely user impact, not merely whether a WCAG success criterion can be associated with the issue.

### High

A likely barrier that can prevent or substantially impair access to content or functionality.

Examples may include:

- unlabeled required form controls
- inaccessible primary navigation
- keyboard-inaccessible critical functionality
- empty buttons required to operate the interface
- missing accessible names on critical controls
- severe contrast problems affecting essential text

### Medium

A meaningful accessibility barrier that makes content or functionality significantly harder to perceive, understand, navigate, or operate.

### Low

A real accessibility concern with limited scope or impact.

### Informational

A recommendation, manual-testing item, or observation that does not establish a user-facing barrier.

Do not inflate severity because an issue technically maps to WCAG.

## Confidence

Assign confidence independently:

- **High** — the problem is directly established by source
- **Medium** — source strongly suggests a problem but rendered behavior matters
- **Low** — manual testing is required to establish whether a barrier exists

## WCAG References

Where a finding clearly maps to a WCAG 2.2 Level A or AA success criterion, include the relevant criterion.

Example:

**WCAG:** 1.4.3 Contrast (Minimum) — Level AA

Do not force a WCAG citation onto every observation.

Do not claim WCAG failure when the available evidence is insufficient.

## Output

Begin with:

# Accessibility Audit

## Summary

Provide a brief assessment of the accessibility implementation.

Include:

- overall accessibility quality
- major strengths
- highest-impact barriers
- number of findings by severity
- number of findings requiring manual verification

## WebAIM Million Priority Findings

Report the status of all six WebAIM Million categories:

- Low contrast text
- Missing alternative text
- Missing form labels
- Empty links
- Empty buttons
- Missing document language

For each category, state:

- issues found
- no obvious issues found
- unable to determine from source

Do not manufacture findings merely to populate this section.

## Findings

Use this structure:

## Finding: Short descriptive title

**Severity:** High | Medium | Low | Informational
**Confidence:** High | Medium | Low
**Files:** `path/to/file.ext:line`
**WCAG:** Criterion, when applicable

### Problem

Explain the accessibility issue.

### User Impact

Explain who may encounter difficulty and what barrier the implementation creates.

Keep this concrete.

Avoid generic statements such as "This may affect users with disabilities."

### Recommendation

Describe the simplest appropriate solution.

Prefer native HTML whenever possible.

### Example

When useful, provide a small example.

Do not rewrite entire components or templates unless necessary to demonstrate the recommendation.

## Manual Testing Checklist

End with a short, project-specific checklist of things that should be verified manually.

Do not provide a generic accessibility testing checklist unrelated to the source.

Prioritize testing based on the components and behaviors actually present in this project.

Examples might include:

- keyboard navigation through a detected menu
- focus behavior of a detected modal
- screen-reader announcement of detected validation messages
- zoom/reflow of a detected fixed layout

## Positive Findings

Identify meaningful accessibility practices already implemented correctly.

Examples may include:

- strong native HTML semantics
- properly labeled forms
- useful alternative text patterns
- strong focus states
- simple native controls
- appropriate landmarks
- restrained ARIA usage

Only include strengths supported by the source.

## Rules

- Do not modify files.
- Do not claim the website is fully accessible.
- Do not claim the website is WCAG compliant.
- Do not claim legal compliance.
- Prioritize WebAIM Million categories.
- Prioritize real user barriers over theoretical edge cases.
- Distinguish automated/source-detectable issues from manual-testing needs.
- Prefer native HTML over ARIA.
- Do not add ARIA merely to make markup appear more accessible.
- Do not treat every accessibility concern as high severity.
- Do not prescribe complex accessibility patterns without a demonstrated need.
- Do not duplicate findings already better addressed as HTML, CSS, or JavaScript architecture issues unless there is a meaningful accessibility consequence.
- Cite specific files and line numbers whenever possible.
- Prefer concrete findings over generic accessibility advice.
- Base findings on the repository rather than assumptions.
