# Prompt 02: CSS Audit

Audit the project's CSS and related styling source files.

Use the Project Discovery report to identify the correct source files and directories.

Do not modify any files.

## Goal

Evaluate the CSS for correctness, maintainability, responsiveness, accessibility-related styling concerns, and unnecessary complexity.

The primary question is:

**Does the CSS provide a stable, understandable, maintainable presentation layer without fighting the HTML or creating avoidable problems?**

The audit should respect the project's existing styling approach.

If the project uses Tailwind, Bootstrap, Sass, PostCSS, CSS Modules, or another styling system, evaluate the source within that system rather than assuming plain CSS is preferable.

## Scope

Audit editable styling source files such as:

- `.css`
- `.scss`
- `.sass`
- `.less`
- Tailwind source and configuration
- Bootstrap overrides
- component or template styles
- PostCSS source
- inline styles when they are part of the project's authored code

Do not primarily audit:

- compiled CSS
- minified CSS
- generated utility output
- vendor stylesheets
- third-party dependencies

when editable source files are available.

## Audit Areas

### 1. CSS Organization

Evaluate how styles are organized.

Look for:

- logical grouping
- predictable naming
- duplicated style rules
- scattered definitions for the same component
- overly large files with unrelated concerns
- styles that are difficult to trace back to the markup
- unnecessary fragmentation

Do not require a particular CSS methodology such as BEM, OOCSS, ITCSS, utility-first CSS, or CSS Modules.

Evaluate whether the current approach is internally coherent and maintainable.

### 2. Specificity

Review selector specificity.

Look for:

- unnecessarily specific selectors
- long selector chains
- IDs used for styling
- escalating specificity
- selectors written primarily to override other selectors
- repeated override patterns
- specificity conflicts caused by source order or nesting

Prefer the least specificity reasonably needed for the design.

Do not recommend weakening selectors simply to achieve lower specificity when the existing selector clearly and safely expresses the intended scope.

### 3. `!important`

Identify uses of `!important`.

Determine whether each use is:

- necessary
- compensating for a specificity problem
- overriding a framework intentionally
- part of a utility convention
- unnecessary

Do not automatically treat every `!important` declaration as a defect.

Framework utilities or intentional overrides may use it appropriately.

Flag it when it indicates a maintainability problem or unnecessary cascade conflict.

### 4. Duplication

Identify meaningful duplication such as:

- repeated declaration blocks
- near-identical selectors
- repeated values that create inconsistency
- duplicate media-query rules
- multiple implementations of the same component style

Do not recommend abstraction merely because two declarations happen to share a value.

Recommend consolidation only where it meaningfully improves consistency or maintainability.

### 5. Unused and Dead CSS

Identify styles that appear unused based on the repository.

Examples include:

- selectors with no matching markup
- obsolete component styles
- unused utility classes
- duplicate legacy rules
- unused animation definitions
- unused custom properties

Be cautious when templates, CMS content, JavaScript-generated classes, or runtime states may use selectors dynamically.

Mark uncertain findings with lower confidence.

Do not assume a selector is unused solely because it cannot be found in a static HTML file.

### 6. Layout

Review use of:

- normal document flow
- Flexbox
- Grid
- positioning
- floats
- margins
- padding
- gaps
- width and height

Look for:

- unnecessarily complicated layout systems
- absolute positioning used for primary page layout
- brittle coordinate-based placement
- floats used where modern layout systems would be more appropriate
- unnecessary wrappers created only to support difficult CSS
- layout rules that depend on fragile element order
- excessive negative margins
- avoidable overlap

Do not recommend rewriting working layouts simply because another layout technique also exists.

### 7. Responsive Design

Evaluate whether the styling adapts reasonably across viewport sizes.

Look for:

- fixed widths that cause horizontal scrolling
- rigid layouts
- excessive dependence on specific viewport dimensions
- missing wrapping behavior
- large minimum widths
- content clipping
- controls that become unusable at smaller sizes
- media-query conflicts
- repeated breakpoint overrides
- unnecessary device-specific breakpoints

Prefer content-driven responsive behavior when practical.

Do not require a specific mobile-first or desktop-first methodology if the implementation is coherent.

### 8. Sizing and Units

Review the use of:

- `px`
- `rem`
- `em`
- `%`
- viewport units
- intrinsic sizing
- `min()`
- `max()`
- `clamp()`

Do not treat pixels as inherently wrong.

Flag sizing choices when they create actual problems such as:

- text that cannot scale reasonably
- rigid containers
- clipping
- inaccessible zoom behavior
- unnecessary dependence on viewport dimensions

### 9. Fixed Heights

Pay particular attention to fixed heights on containers containing text or dynamic content.

Look for:

- clipped text
- overflow at larger font sizes
- translated content problems
- content that cannot grow
- components that depend on exact copy length

Prefer `min-height`, intrinsic sizing, or content-driven sizing when appropriate.

Do not flag fixed dimensions when the content is genuinely fixed, such as icons, avatars, thumbnails, or decorative elements.

### 10. Overflow

Review:

- `overflow`
- `overflow-x`
- `overflow-y`
- `hidden`
- clipping
- scroll containers

Identify cases where overflow rules may hide meaningful content or keyboard focus.

Do not treat intentional scroll containers as defects.

### 11. Typography

Review typography-related CSS for structural problems.

Look for:

- unreadably small text
- excessively tight line-height
- fixed text containers that prevent scaling
- font declarations that create unnecessary complexity
- inconsistent typography caused by duplicated rules
- text transformed solely through CSS in ways that alter meaning or readability

Do not perform subjective visual design critique.

### 12. Color and Contrast

Identify obvious styling patterns likely to create contrast problems.

Inspect:

- foreground/background color combinations
- muted text
- disabled states
- links
- controls
- placeholders
- focus indicators

Do not claim WCAG contrast compliance solely from casual visual inspection.

If contrast cannot be reliably determined from source values or depends on runtime layering, identify it for accessibility review rather than presenting it as a confirmed failure.

Detailed contrast analysis belongs in the accessibility audit.

### 13. Focus Styles

Look for styling that:

- removes outlines
- suppresses focus indicators
- makes keyboard focus difficult to see
- applies hover styling without an equivalent focus treatment where relevant

Examples include:

```css
outline: none;
```

or:

```css
outline: 0;
```

Do not flag outline removal if the project provides an adequate custom focus indicator.

### 14. Hover and Pointer Assumptions

Identify important interactions or information that appear to depend exclusively on:

- `:hover`
- pointer precision
- mouse position

Look for cases where hover styles reveal content or affordances that may not be available to keyboard or touch users.

Do not treat decorative hover effects as accessibility problems.

### 15. Motion and Animation

Review:

- transitions
- animations
- transforms
- scrolling effects
- parallax
- autoplay-style visual motion

Check whether substantial non-essential motion accounts for:

```css
@media (prefers-reduced-motion: reduce);
```

Do not require reduced-motion overrides for every minor transition.

Prioritize motion that is large, continuous, disorienting, or functionally significant.

### 16. Visibility Techniques

Review techniques such as:

- `display: none`
- `visibility: hidden`
- opacity
- off-screen positioning
- clipping
- visually-hidden utility classes

Look for:

- content hidden visually but still interactive
- content intended for assistive technology that is implemented incorrectly
- focusable elements hidden off-screen
- invisible overlays

Do not perform a full accessibility audit here. Flag styling patterns that need accessibility review.

### 17. CSS and HTML Semantics

Identify CSS complexity that appears to exist because the underlying HTML structure is poor.

Examples include:

- selectors relying on deeply nested generic elements
- visual reordering that significantly differs from DOM order
- excessive pseudo-elements being used to generate meaningful content
- CSS simulating controls that should use native HTML
- difficult selectors compensating for missing classes or semantics

When the root problem is HTML, recommend addressing the markup instead of adding more CSS.

### 18. Visual Reordering

Review:

- `order`
- grid placement
- flex direction
- absolute positioning

Flag significant cases where the visual reading order may differ from the DOM order.

Do not assume every use of `order` is a problem.

Focus on cases likely to affect understanding, keyboard navigation, or responsive behavior.

### 19. Pseudo-Elements

Review `::before` and `::after`.

Identify pseudo-elements used for:

- meaningful text
- required labels
- critical icons without an accessible equivalent
- content users need to understand the page

Decorative pseudo-elements are appropriate.

Meaningful content should generally exist in the DOM.

### 20. Custom Properties and Repeated Values

Review CSS custom properties where used.

Look for:

- inconsistent repeated values
- variables whose names obscure their purpose
- unnecessary abstraction
- important design values repeated widely without a clear system

Do not require every repeated value to become a custom property.

Recommend variables where they genuinely improve consistency or maintainability.

### 21. Framework-Specific Styling

If the project uses a CSS framework or utility system, evaluate it within that system.

#### Tailwind

Do not report long utility class lists merely because they are long.

Look instead for:

- unnecessary arbitrary values
- repeated complex utility combinations
- inconsistent design-token usage
- excessive overrides
- CSS added primarily to fight Tailwind
- inaccessible state styling
- unnecessary duplication

#### Bootstrap

Look for:

- excessive framework overrides
- specificity battles
- misuse of layout utilities
- unnecessary custom replacements for existing framework behavior
- reliance on outdated Bootstrap patterns where project version is known

Do not recommend removing a framework simply because vanilla CSS could implement the design.

## Finding Criteria

Report issues when they materially affect:

- maintainability
- responsiveness
- accessibility
- rendering stability
- browser behavior
- layout reliability
- code clarity
- unnecessary complexity

Do not turn subjective styling preferences into defects.

Do not critique aesthetics unless the CSS creates a functional or accessibility problem.

## Severity

Assign one severity to each finding:

### High

The CSS causes or is very likely to cause significant usability, accessibility, responsiveness, or rendering problems.

### Medium

The CSS works but creates meaningful maintainability, layout, responsiveness, or accessibility concerns.

### Low

The CSS could be improved in a way that provides a concrete but limited benefit.

### Informational

An observation worth documenting that does not require a change.

Do not inflate severity.

## Confidence

Assign confidence independently:

- **High** — directly supported by the source
- **Medium** — strongly suggested but depends partly on runtime behavior
- **Low** — requires browser inspection or additional context

## Output

Begin with:

# CSS Audit

## Summary

Provide a brief assessment of:

- overall CSS organization
- specificity health
- responsiveness
- maintainability
- accessibility-related styling
- major strengths
- most important problems
- total findings by severity

Then report individual findings.

Use this structure:

## Finding: Short descriptive title

**Severity:** High | Medium | Low | Informational
**Confidence:** High | Medium | Low
**Files:** `path/to/file.ext:line`

### Problem

Explain what the CSS is doing.

### Why It Matters

Explain the concrete consequence.

### Recommendation

Describe the preferred approach.

### Example

When useful, provide a small CSS example.

Do not rewrite entire stylesheets unless necessary to demonstrate the recommendation.

## Positive Findings

End with a short section describing meaningful CSS practices the project is already handling well.

Only include strengths supported by the source.

## Rules

- Do not modify files.
- Do not refactor the project.
- Respect the existing CSS architecture and framework.
- Do not recommend removing Tailwind, Bootstrap, Sass, or another tool merely because a different approach is possible.
- Do not treat every `!important` as a defect.
- Do not treat every pixel value as a defect.
- Do not treat every fixed dimension as a defect.
- Do not treat every `<div>`-dependent selector as a CSS problem if the real issue belongs in the HTML.
- Do not perform subjective visual design criticism.
- Do not perform the full accessibility audit.
- Do not claim WCAG compliance or failure based solely on this CSS review.
- Cite specific files and line numbers whenever possible.
- Prefer concrete findings over generic best-practice advice.
- Base findings on the repository rather than assumptions.
