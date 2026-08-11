# Frontend Audit

Frontend Audit is a reusable agentic workflow for auditing websites built primarily with **HTML, CSS, and JavaScript**.

It is designed for use with coding agents such as Codex or Claude Code.

The workflow reviews a project's frontend, produces individual audit reports, and finishes with a consolidated QA report. It does **not** modify the website being audited.

## Run the Audit

Add both this repository and the website you want to audit to your coding-agent workspace.

Then use:

```text
Use the `frontend-audit` project to audit the website project I added to this workspace.

Run all prompts in the `frontend-audit/prompts` directory in order, starting with project discovery.

Follow the instructions in each prompt exactly.

Do not modify the website project's source code.

Use Prompt 00 to determine the project name and create the project-specific folder under `frontend-audit/output/`. Write each audit report to that folder as instructed by the prompts.

If Project Discovery determines that the project is out of scope for this audit, stop after the discovery report.
```

## Workflow

The prompts run in order:

```text
0_project-discovery.md
1_html-audit.md
2_css-audit.md
3_javascript-audit.md
4_a11y-audit.md
5_seo-audit.md
6_final-report.md
7_qa-todo.md
```

Prompt 00 examines the project architecture and creates:

```text
output/<project-name>/
```

Each later prompt reads the discovery report before continuing and writes its report into the same project folder.

Example:

```text
output/
└── coffee/
    ├── 00-project-discovery.md
    ├── 01-html-audit.md
    ├── 02-css-audit.md
    ├── 03-javascript-audit.md
    ├── 04-accessibility-audit.md
    ├── 05-seo-audit.md
    ├── 06-final-report.md
    └── 07-qa-todo.md
```

This allows the same Frontend Audit repository to contain reports for multiple projects.

## What It Audits

Frontend Audit focuses on:

- HTML semantics and structure
- CSS maintainability and responsiveness
- browser-side JavaScript
- practical accessibility issues
- basic technical SEO
- pre-launch QA items

The workflow favors **native HTML, simple implementations, and concrete problems over theoretical best practices**.

A valid implementation is not considered a defect merely because another approach is also commonly recommended.

## Intended Projects

This audit is intended for projects whose rendered frontend is fundamentally HTML, CSS, and JavaScript.

Examples include:

- static websites
- vanilla JavaScript projects
- Vite projects without a JavaScript UI framework
- Tailwind or Bootstrap projects
- Sass/SCSS projects
- PHP or Laravel Blade templates
- Twig or Django templates
- WordPress themes
- CMS and other server-rendered templates

The presence of Node.js, npm, TypeScript, Vite, or other tooling does not automatically place a project outside the audit's scope.

## Out of Scope

This is not a framework-specific audit for applications primarily built with component-driven JavaScript frameworks such as:

- React / Next.js
- Vue / Nuxt
- Angular
- Svelte / SvelteKit
- Solid
- Ember

Prompt 00 determines whether the project is appropriate for the workflow.

## Audit Philosophy

The purpose of Frontend Audit is to provide a second set of eyes near the end of a project.

It is particularly useful for catching things that are easy to miss while designing, developing, and preparing a site for deployment: incomplete links, forgotten accessibility details, placeholder content, fragile interactions, responsive issues, and other QA items.

The audit:

- does not modify project source
- distinguishes severity from confidence
- prefers native HTML over unnecessary JavaScript or ARIA
- avoids treating preferences as defects
- avoids accessibility overengineering
- keeps SEO focused on technical structure rather than keyword strategy
- consolidates overlapping findings
- finishes with an actionable QA to-do list

The developer decides which recommendations should be implemented.
