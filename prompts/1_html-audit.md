# Prompt 01: HTML Audit

Before beginning this audit, locate and read the project's completed discovery report at:

`output/<project-name>/00-project-discovery.md`

Use the **Project Name**, **Output Directory**, source locations, exclusions, and caveats recorded in that report. Do not guess or create a different project name for this audit.

If the discovery report is missing, stop and run Prompt 00 before continuing.

Write the completed HTML audit to the same project-specific output directory as:

`output/<project-name>/01-html-audit.md`

If that report already exists, replace it with the results of the current audit.

Audit the project's HTML and server-rendered HTML templates.

Use the Project Discovery report to identify the correct source files and directories.

Do not modify any project source files.

## Goal

Evaluate the quality, semantics, structure, maintainability, and correctness of the HTML that ultimately reaches the browser.

The primary question is:

**Does this project use HTML elements according to their intended meaning and behavior?**

Prefer native HTML semantics over generic containers, ARIA, CSS, or JavaScript when native HTML already provides the required structure or interaction.

## Scope

Audit editable source files that produce HTML, including as applicable:

- `.html`
- PHP templates
- Laravel Blade templates
- Twig templates
- WordPress theme templates
- CMS templates
- Static-site templates
- Includes
- Partials
- Layouts
- Server-rendered components

Do not primarily audit generated HTML, compiled output, third-party templates, dependencies, or vendor files unless the source implementation cannot otherwise be identified.

## Audit Areas

### 1. Document Structure

Check for appropriate document-level structure, including:

- `<!doctype html>`
- `<html>`
- `lang`
- `<head>`
- `<title>`
- `<meta charset>`
- viewport metadata where appropriate
- `<body>`

Do not treat SEO-specific metadata as part of this audit unless it affects basic document correctness. Detailed metadata review belongs in the SEO audit.

### 2. Semantic Landmarks

Review the use of structural elements such as:

- `<header>`
- `<nav>`
- `<main>`
- `<section>`
- `<article>`
- `<aside>`
- `<footer>`

Identify generic `<div>` containers that represent meaningful document structure and could reasonably use semantic HTML instead.

Do not recommend semantic elements merely to eliminate `<div>` elements. Generic containers are valid when no semantic element applies.

Check that:

- the primary page content is identifiable
- navigation regions are appropriately represented
- repeated site structure is logically organized
- landmarks are not used unnecessarily or misleadingly

### 3. Heading Structure

Review `<h1>` through `<h6>` usage.

Check for:

- meaningful heading hierarchy
- headings used to identify sections
- skipped levels where they indicate poor structure
- headings chosen for visual appearance rather than document structure
- text styled to look like headings without heading markup
- empty headings

Do not require a rigid heading hierarchy when the document structure reasonably supports another choice.

### 4. Native Interactive Elements

Identify interactions implemented with generic elements when native HTML elements would be more appropriate.

Pay particular attention to:

- clickable `<div>` elements
- clickable `<span>` elements
- links being used as buttons
- buttons being used as links
- custom controls duplicating native HTML behavior

Prefer:

- `<button>` for actions
- `<a>` for navigation
- form controls for user input
- `<details>` and `<summary>` where appropriate
- `<dialog>` where appropriate

Do not recommend replacing a custom component with a native element unless the native element actually supports the required behavior.

### 5. Links

Review anchors for correct structural use.

Check for:

- missing `href` attributes when an element is intended to navigate
- placeholder links such as `href="#"`
- JavaScript-only navigation that should be ordinary links
- nested interactive elements
- links containing inappropriate interactive descendants
- malformed URLs where evident
- unnecessary `target="_blank"`

Flag links whose visible purpose is unclear as an HTML concern when the markup itself creates that ambiguity.

Detailed SEO evaluation of link text belongs in the SEO audit.

### 6. Buttons

Review `<button>` elements and button-like controls.

Check for:

- missing `type` attributes where buttons appear inside forms
- inappropriate use of `type="submit"`
- actions implemented as anchors
- generic elements acting as buttons
- nested interactive controls
- buttons containing invalid or inappropriate markup

Do not evaluate JavaScript event handling in depth here. That belongs in the JavaScript audit.

### 7. Forms

Review form structure and native form semantics.

Check:

- `<form>`
- `<label>`
- `<input>`
- `<textarea>`
- `<select>`
- `<option>`
- `<fieldset>`
- `<legend>`
- `<button>`
- relevant native input types

Look for:

- controls without associated labels
- placeholder text being used instead of labels
- incorrect input types
- groups of related controls that should use fieldset/legend
- unnecessary custom form controls
- buttons with unintended default submit behavior
- invalid nesting
- duplicate IDs affecting labels

Accessibility implications may be noted, but detailed accessibility analysis belongs in the accessibility audit.

### 8. Images and Media

Review markup for:

- `<img>`
- `<picture>`
- `<source>`
- `<figure>`
- `<figcaption>`
- `<video>`
- `<audio>`

Check for:

- missing required attributes
- unnecessary wrapper markup
- inappropriate use of background images for meaningful content
- misuse of `<figure>`
- malformed responsive image markup
- width and height attributes where they are useful for reserving layout space

Do not perform a detailed accessibility evaluation of alternative text here. That belongs in the accessibility audit.

### 9. Lists

Check whether groups of related items are appropriately represented using:

- `<ul>`
- `<ol>`
- `<li>`
- `<dl>`
- `<dt>`
- `<dd>`

Identify repeated content implemented as unrelated containers when it clearly represents a list.

Do not require lists for every visually repeated group.

### 10. Tables

Review tables for legitimate tabular data.

Check:

- tables being used for layout
- missing `<th>` elements
- malformed row or cell structure
- improper nesting
- unnecessary tables where CSS layout is more appropriate

Detailed table accessibility should be evaluated in the accessibility audit.

### 11. IDs and Relationships

Look for:

- duplicate IDs
- broken `for` / `id` relationships
- broken fragment references
- invalid references from HTML attributes
- IDs copied repeatedly through templates

Give particular attention to template-generated markup that could create duplicate IDs when repeated.

### 12. HTML Validity and Nesting

Identify clear HTML correctness problems such as:

- invalid nesting
- interactive elements nested inside other interactive elements
- block structure that produces unexpected browser parsing
- missing required parent or child relationships
- duplicate attributes
- malformed elements
- obsolete HTML

Focus on issues that affect browser behavior, semantics, maintainability, or later audits.

Do not produce a long inventory of inconsequential validator warnings.

### 13. Excessive Markup

Identify markup that is unnecessarily complex.

Examples include:

- deeply nested generic wrappers
- repeated containers that serve no structural or styling purpose
- markup generated solely to compensate for poor CSS architecture
- unnecessarily complex structures for simple UI patterns

Do not treat minimal markup as an end in itself. Recommend simplification only when it meaningfully improves clarity, semantics, maintainability, or behavior.

### 14. ARIA and Native HTML

This is not the full accessibility audit, but flag obvious HTML-level misuse of ARIA.

Examples:

- adding `role="button"` to an element that should simply be `<button>`
- adding landmark roles to elements that already provide the same native landmark
- overriding native element semantics without a clear reason
- using ARIA to recreate behavior available through native HTML

Follow this principle:

**No ARIA is better than bad ARIA. Native HTML is preferred when it provides the required semantics and behavior.**

Leave detailed ARIA validation and accessibility impact analysis for the accessibility audit.

## Finding Criteria

Do not report every stylistic preference as a defect.

Report findings when the markup creates or is likely to create problems involving:

- incorrect semantics
- browser behavior
- interaction
- accessibility
- maintainability
- unnecessary implementation complexity
- structural correctness

Distinguish actual problems from optional improvements.

## Severity

Assign one severity to each finding:

### High

The markup is incorrect in a way that substantially affects functionality, semantics, accessibility, browser behavior, or the reliability of the interface.

### Medium

The markup works but creates meaningful structural, semantic, maintainability, or interaction problems.

### Low

The markup is valid or mostly functional but could be improved in a way that provides a concrete benefit.

### Informational

An observation worth noting that does not require a change.

Do not inflate severity.

## Confidence

Assign a confidence level independently from severity:

- **High** — directly supported by the source code
- **Medium** — strongly suggested by the source but may depend on runtime behavior
- **Low** — requires browser behavior, generated output, or additional context to confirm

Do not present low-confidence observations as established defects.

## Output

Begin with:

# HTML Audit

## Summary

Provide a brief assessment of the overall HTML implementation.

Include:

- overall semantic quality
- major patterns observed
- strongest areas
- most important problems
- total findings by severity

Then report individual findings.

Use this format:

## Finding: Short descriptive title

**Severity:** High | Medium | Low | Informational
**Confidence:** High | Medium | Low
**Files:** `path/to/file.ext:line`

### Problem

Explain what the markup is doing.

### Why It Matters

Explain the concrete consequence.

### Recommendation

Describe the preferred HTML structure or approach.

### Example

When useful, provide a small example of improved markup.

Do not rewrite entire templates unless necessary to demonstrate the recommendation.

## Positive Findings

End with a short section identifying HTML patterns the project is already handling well.

Only include meaningful strengths supported by the source.

## Rules

- Do not modify project source files.
- The only file this prompt should create or replace is `output/<project-name>/01-html-audit.md` and any directory needed to contain it.
- Do not refactor the project.
- Do not audit CSS architecture in detail.
- Do not audit JavaScript implementation in detail.
- Do not perform the full accessibility audit.
- Do not perform the SEO audit.
- Do not recommend JavaScript when native HTML solves the problem.
- Do not recommend ARIA when native HTML provides the required semantics.
- Do not criticize `<div>` or `<span>` merely for existing.
- Do not enforce personal markup preferences as standards.
- Cite specific files and line numbers whenever possible.
- Prefer concrete findings over generic best-practice advice.
- Base findings on the repository rather than assumptions about how the site might work.
