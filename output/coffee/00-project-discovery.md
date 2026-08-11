# Project Discovery

### Audit Suitability

**In Scope.** The `coffee` project is a static, multi-page site whose browser output is authored directly in HTML, CSS, and a small vanilla JavaScript file. It does not use a component-driven JavaScript framework.

### Frontend Summary

Four static documents (`index.html` plus `about/`, `contact/`, and `menu/`) are served directly. Each page loads a shared reset, a shared authored stylesheet, Google Fonts, and one deferred script. There is no templating or build step; repeated header/footer markup is copied into each page.

### HTML Source

- `index.html`
- `about/index.html`
- `contact/index.html`
- `menu/index.html`

These files are the rendered page sources and the primary HTML audit scope.

### CSS Source

- `style.css` — all authored site and responsive styling
- `normalize.css` — third-party Normalize.css; note its effect but do not audit it as authored source

There is no preprocessor, CSS framework, module system, or build-time CSS processing.

### JavaScript Source

- `script.js` — deferred vanilla JavaScript for the responsive menu, copyright year, and contact-form setup

There are no browser package dependencies, modules, bundlers, inline scripts, or jQuery. The contact form submits to Formspree; the About page embeds YouTube; Google Fonts is loaded from Google.

### Build and Tooling

No `package.json`, package manager, bundler, compiler, linter, formatter, or static-site generator is present. Files are intended to be served as-is.

### Audit Exclusions

- `.git/`, `.DS_Store`
- `designs/` source artwork and PDF
- `images/` and `svg/` binary/vector assets except when validating references and accessible usage
- `normalize.css` as third-party source
- `README.md`, `QA.md`, and `LICENSE.md`

### Important Caveats

- Shared markup is duplicated across all four pages, so one defect may be site-wide.
- Runtime behavior involving Formspree, YouTube, Google Fonts, and Google Maps cannot be fully established from source.
- No deployment URL or server configuration is provided, limiting canonical, redirect, header, and live indexing checks.
- The audit should cite editable source, not design artifacts.
