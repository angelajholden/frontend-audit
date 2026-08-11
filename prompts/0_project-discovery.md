# Prompt 00: Project Discovery

You are auditing an existing website or web application repository.

Your first task is to understand how the frontend is built before performing any code audit.

Do not modify any files.

## Goal

Determine whether this project is appropriate for a frontend audit focused on HTML, CSS, JavaScript, accessibility, and technical SEO.

This audit is intended for projects whose rendered frontend is fundamentally built with HTML, CSS, and JavaScript.

The project may use:

- Vanilla HTML, CSS, and JavaScript
- A CSS framework or library such as Tailwind CSS or Bootstrap
- A build tool such as Vite, Webpack, Parcel, Rollup, or similar
- Server-side templates that ultimately render HTML, such as:
    - PHP
    - Laravel Blade
    - Twig
    - Django templates
    - Ruby on Rails templates
    - WordPress themes
    - CMS templates
    - Other server-rendered template systems

Server-side technology does not automatically place the project outside the scope of this audit.

## Framework Boundary

Determine whether the frontend is primarily implemented using a JavaScript UI framework or application framework such as:

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

If the frontend is primarily built with one of these frameworks, stop the audit after completing the discovery report.

Clearly state that the project is outside the intended scope of this audit.

Do not attempt to convert the application to vanilla HTML, CSS, or JavaScript.

Do not assume that the presence of Node.js, npm, TypeScript, Vite, or another build tool means the project is framework-based. Determine how the actual frontend is implemented.

## Inspect the Repository

Examine the repository structure and relevant configuration files.

Identify:

### 1. Frontend Architecture

Explain how pages are produced.

Determine whether the project uses:

- Static HTML files
- Server-rendered templates
- Generated static pages
- Client-side rendering
- A combination of these approaches

Identify the primary frontend entry points.

### 2. HTML

Determine where the HTML comes from.

Identify:

- HTML files
- Template files
- Layouts
- Includes
- Partials
- Components, if they are server-rendered
- CMS template structures, if applicable

Explain which files or directories should be examined during the HTML audit.

### 3. CSS

Determine how styles are created and loaded.

Identify:

- CSS files
- Sass, SCSS, Less, or other preprocessors
- CSS modules or other organizational systems
- Tailwind CSS
- Bootstrap
- Other CSS libraries or frameworks
- Reset or normalization libraries
- Build-time CSS processing

Explain which source files should be examined during the CSS audit.

Do not treat framework-generated or compiled CSS as the primary audit source when editable source files exist.

### 4. JavaScript

Determine how frontend JavaScript is organized and loaded.

Identify:

- JavaScript entry points
- ES modules
- Bundled scripts
- Inline scripts
- Third-party scripts
- TypeScript that compiles to frontend JavaScript
- jQuery
- JavaScript libraries
- Package dependencies used by the browser

Explain which source files should be examined during the JavaScript audit.

### 5. Build and Development Tooling

Identify relevant tooling such as:

- package.json
- npm, pnpm, Yarn, or Bun
- Vite
- Webpack
- Parcel
- Rollup
- PostCSS
- Sass
- Tailwind
- Babel
- TypeScript
- Linters
- Formatters

Briefly explain what the tooling does in this particular project.

Do not provide a generic explanation of each tool.

### 6. Third-Party Dependencies

Identify major frontend dependencies that materially affect the audit.

Separate them into useful categories such as:

- CSS frameworks
- JavaScript libraries
- UI libraries
- Utilities
- Analytics
- Third-party widgets
- Embedded services

Do not produce an exhaustive package inventory unless a dependency affects the frontend architecture or audit.

### 7. Generated and Vendor Files

Identify files or directories that should normally be excluded from direct source-code auditing, including things such as:

- `node_modules`
- build output
- compiled assets
- minified vendor files
- generated CSS
- generated JavaScript
- cache directories
- third-party source code

Prefer auditing the project's editable source files.

## Discovery Report

Return a concise report using this structure:

### Audit Suitability

State one of:

- **In Scope**
- **In Scope with Caveats**
- **Out of Scope**

Explain why.

### Frontend Summary

Briefly describe how this project's frontend works.

### HTML Source

List the primary files or directories that should be included in the HTML audit.

### CSS Source

List the primary files or directories that should be included in the CSS audit.

Identify any CSS framework or library.

### JavaScript Source

List the primary files or directories that should be included in the JavaScript audit.

Identify important libraries or browser-side dependencies.

### Build and Tooling

Summarize relevant frontend tooling.

### Audit Exclusions

List generated, vendor, dependency, or irrelevant directories that later audit prompts should ignore.

### Important Caveats

Identify anything later audit prompts need to understand about this particular architecture.

## Rules

- Do not modify any files.
- Do not refactor code.
- Do not install dependencies.
- Do not upgrade packages.
- Do not make architectural recommendations yet.
- Do not perform the HTML, CSS, JavaScript, accessibility, or SEO audits yet.
- Do not infer a framework solely from package names or build tooling.
- Base conclusions on the repository contents.
- Keep the report focused on information needed for the later frontend audits.
