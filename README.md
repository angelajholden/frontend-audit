# Frontend Audit

Frontend Audit is a collection of reusable prompts for auditing websites built primarily with **HTML, CSS, and JavaScript**.

The prompts are designed to be used with coding agents such as Codex, Claude Code, or similar tools that can inspect a repository.

The goal is not to automatically rewrite a project. The goal is to help an agent examine the frontend, identify meaningful problems, and produce a practical report that a developer can review before making changes.

## What This Audit Covers

Frontend Audit focuses on:

- HTML
- CSS
- JavaScript
- Accessibility
- Technical SEO

It is intended for projects where the browser-facing frontend is fundamentally HTML, CSS, and JavaScript.

That can include:

- Static websites
- Vanilla JavaScript projects
- Vite projects without a JavaScript UI framework
- Tailwind CSS
- Bootstrap
- Sass or other CSS preprocessors
- PHP templates
- Laravel Blade templates
- Twig templates
- Django templates
- Rails templates
- WordPress themes
- CMS templates
- Other server-rendered websites

The backend technology does not matter as long as the frontend can reasonably be evaluated as HTML, CSS, and JavaScript.

## What This Audit Does Not Cover

Frontend Audit is not intended as a framework-specific audit for applications primarily built with:

- React
- Next.js
- Vue
- Nuxt
- Angular
- Svelte
- SvelteKit
- Solid
- Ember
- Similar component-driven JavaScript frameworks

The first prompt performs project discovery and determines whether the project is within scope.

The presence of Node.js, npm, Vite, TypeScript, Tailwind, or another build tool does **not** automatically mean a project is out of scope.

## How It Works

Run the prompts in order:

```text
0_project-discovery.md
1_html-audit.md
2_css-audit.md
3_javascript-audit.md
4_a11y-audit.md
5_seo-audit.md
6_final-report.md
```

## Running the Audit

Run the seven prompts in order.

Prompt 00 establishes a filesystem-safe project name, preferably from the repository name, and creates a project-specific directory under `output/`:

```text
output/<project-name>/
```

Every later prompt reads `output/<project-name>/00-project-discovery.md` before beginning. The discovery report is the source of truth for the project name, output directory, source locations, exclusions, and architectural caveats.

Each prompt writes its Markdown report into that same project-specific directory. This allows one copy or fork of Frontend Audit to store audits for multiple projects without their reports colliding.

For example:

```text
output/
├── project-one/
│   ├── 00-project-discovery.md
│   ├── 01-html-audit.md
│   ├── 02-css-audit.md
│   ├── 03-javascript-audit.md
│   ├── 04-accessibility-audit.md
│   ├── 05-seo-audit.md
│   └── 06-final-report.md
└── project-two/
    ├── 00-project-discovery.md
    ├── 01-html-audit.md
    ├── 02-css-audit.md
    ├── 03-javascript-audit.md
    ├── 04-accessibility-audit.md
    ├── 05-seo-audit.md
    └── 06-final-report.md
```

The audit prompts must not modify the website being reviewed. They may only create or replace their own report files in the appropriate project output directory.

If an audit is rerun for the same project, the existing project directory is reused and the corresponding report is replaced with the current results.

### 00 — Project Discovery

Examines the repository before any audit begins.

It identifies:

- the filesystem-safe project name and output directory
- how HTML is produced
- how CSS is organized
- how JavaScript is loaded
- build tooling
- frontend dependencies
- generated and vendor files
- whether the project is within scope

This prevents later prompts from making assumptions about the architecture and gives every later audit a shared source of truth.

### 01 — HTML Audit

Reviews the HTML and templates for:

- semantic markup
- document structure
- headings
- landmarks
- buttons and links
- forms
- images and media
- lists and tables
- invalid nesting
- duplicate IDs
- unnecessary ARIA
- excessive markup

The audit favors native HTML when the platform already provides the required semantics or behavior.

### 02 — CSS Audit

Reviews the project's authored styles for:

- organization
- specificity
- `!important`
- duplication
- unused CSS
- layout
- responsiveness
- sizing
- overflow
- typography
- focus styles
- motion
- visual reordering
- maintainability

The audit respects the styling system already in use.

Tailwind, Bootstrap, Sass, and similar tools are evaluated within their own conventions rather than treated as problems simply because they are not vanilla CSS.

### 03 — JavaScript Audit

Reviews browser-side JavaScript for:

- unnecessary JavaScript
- organization
- global state
- DOM queries
- event handling
- keyboard interaction
- focus management
- state management
- DOM injection
- API requests
- async behavior
- forms
- navigation
- progressive enhancement
- third-party scripts
- dependencies
- dead code
- obvious frontend security risks

A major principle of this audit is:

> Do not use JavaScript to solve a problem that HTML or CSS already solves well.

### 04 — Accessibility Audit

The accessibility audit takes a practical, high-impact approach.

It gives particular attention to the recurring issues identified by the **WebAIM Million** project, including:

- low contrast text
- missing image alternative text
- missing form labels
- empty links
- empty buttons
- missing document language

It also reviews semantic HTML, keyboard accessibility, focus behavior, forms, ARIA, motion, zoom, and other common accessibility concerns.

This is a source-code audit, not a complete accessibility certification.

The prompts intentionally distinguish between:

- issues that can be established from source
- issues that are likely but require browser verification
- issues that require manual accessibility testing

The audit does **not** claim that a website is WCAG compliant or legally compliant.

### 05 — SEO Audit

The SEO audit uses a lightweight, organic approach focused on good frontend engineering.

It reviews:

- page titles
- meta descriptions
- semantic HTML
- heading structure
- crawlable navigation
- internal links
- canonical URLs
- robots directives
- sitemaps
- structured data
- social metadata
- JavaScript-dependent content
- obvious indexing problems

It does not perform:

- keyword research
- backlink analysis
- competitor analysis
- ranking predictions
- keyword-density analysis
- content marketing strategy

The underlying philosophy is simple:

> A well-built, accessible, understandable website gives both users and search engines a strong foundation.

### 06 — Final Report

The final prompt combines the previous audits into one implementation-focused report.

It:

- consolidates duplicate findings
- identifies root causes
- normalizes severity
- prioritizes fixes
- identifies quick wins
- separates manual testing from confirmed issues
- summarizes accessibility and SEO concerns
- recommends a practical order of work

The final report should be considerably more useful than simply concatenating five separate audit reports.

## Audit Philosophy

Frontend Audit favors practical engineering over theoretical perfection.

### Native HTML First

If HTML already provides the appropriate element and behavior, use it.

For example:

```html
<button type="button">Open menu</button>
```

is generally preferable to recreating a button with:

```html
<div role="button" tabindex="0">Open menu</div>
```

plus custom JavaScript keyboard handling.

### Fix Root Causes

The prompts are designed to identify underlying problems rather than stack patches on top of them.

A single semantic HTML change may solve problems previously appearing in:

- HTML
- CSS
- JavaScript
- accessibility
- SEO

The final report consolidates these into one finding whenever possible.

### Accessibility Without Overengineering

Accessibility matters, but complexity does not automatically improve accessibility.

The audit prefers:

- semantic HTML
- native controls
- visible labels
- predictable keyboard behavior
- clear focus states
- restrained ARIA

over elaborate custom accessibility implementations when the platform already provides an appropriate solution.

### SEO Without Gaming Search Engines

The SEO audit focuses on making the site understandable and crawlable.

It does not attempt to manufacture search rankings through keyword stuffing, artificial content, or speculative optimization.

### Respect Existing Architecture

The audit does not recommend changing technologies simply because another approach could also work.

It does not automatically recommend removing:

- Tailwind
- Bootstrap
- Sass
- jQuery
- build tooling
- third-party libraries

Recommendations should be based on a concrete problem in the project.

## Findings

Audit findings use two separate measurements:

### Severity

- **High**
- **Medium**
- **Low**
- **Informational**

Severity represents the likely impact of the issue.

### Confidence

- **High**
- **Medium**
- **Low**

Confidence represents how certain the agent can be based on source code alone.

A high-severity issue may still have low confidence if browser testing is required to confirm it.

Separating severity from confidence helps prevent speculative findings from being presented as established defects.

## Using the Prompts

Give the coding agent access to the project you want to audit and to this Frontend Audit prompt repository.

Begin with:

```text
prompts/0_project-discovery.md
```

Prompt 00 creates `output/<project-name>/00-project-discovery.md`. Review that report before continuing.

If the project is within scope, run the remaining prompts in order. Each later prompt reads the discovery report first and writes its report into the same project-specific directory.

The prompts intentionally instruct the agent not to modify project source files. Frontend Audit is designed as an **audit-first workflow**:

```text
Understand
    ↓
Audit
    ↓
Review findings
    ↓
Prioritize
    ↓
Make approved changes
```

The developer remains responsible for deciding which recommendations should be implemented.

## Why This Exists

AI tools can generate functioning websites very quickly.

Functioning, however, does not necessarily mean:

- semantically correct
- maintainable
- accessible
- responsive
- crawlable
- appropriately structured
- using the web platform well

Frontend Audit provides a repeatable way to have an agent inspect that frontend through the lens of established frontend engineering practices.

The purpose is not to replace frontend expertise.

The purpose is to make that expertise easier to apply consistently when working with AI coding agents.
