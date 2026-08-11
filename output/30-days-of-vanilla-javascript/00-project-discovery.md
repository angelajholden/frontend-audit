# Project Discovery

### Audit Suitability

**In Scope.** “30 Days of Vanilla JavaScript” is a static educational site built directly with HTML, one shared authored CSS file, and vanilla JavaScript ES modules. It does not use a component-driven JavaScript framework.

### Frontend Summary

The repository contains a landing/day-01 page, day-02 through day-30 pages, a UI-components collection, privacy policy, and custom 404 page. Pages are authored as standalone HTML documents with substantially repeated header, navigation, sidebar, author, and footer markup. Each loads shared `style.css`, `normalize.css`, and `script.js`; challenge pages additionally load one file from `components/`. `script.js` imports shared behaviors from `global/` and the lazy YouTube component.

### HTML Source

- `index.html`, `404.html`
- `day-02/index.html` through `day-30/index.html`
- `ui-components/index.html`
- `privacy-policy/index.html`

These are direct, editable page sources; there is no template or generated HTML layer.

### CSS Source

- `style.css` — authored global, layout, component, responsive, theme, and embedded Prism theme styles
- `normalize.css` — third-party reset; account for it but exclude it from authored-source findings

There is no CSS framework, preprocessor, modules system, or build processing.

### JavaScript Source

- `script.js` — shared ES-module entry point
- `global/*.js` — navigation, filtering, breadcrumbs, consent, sharing, copyright, and Prism
- `components/*.js` — individual challenge implementations

Important browser features include native dialog, Drag and Drop, Clipboard, History, Fetch, localStorage/cookies, and ES modules. Local JSON under `data/` feeds several demos. External runtime services include Google Fonts, YouTube thumbnails/embeds, and Formspree. `global/prism.js` is vendored PrismJS.

### Build and Tooling

There is no package manifest, dependency manager, bundler, transpiler, or automated lint/test configuration. The site is served as static files. ES modules and local `fetch()` demos require HTTP serving rather than unrestricted `file:` execution.

### Audit Exclusions

- `.git/`, `.DS_Store`, `LICENSE.md`, `QA.md`
- `assets/`, mockup PNGs, and editable Affinity design source
- `images/` and `svg/` except when validating references and accessible usage
- `normalize.css` and vendored `global/prism.js` as third-party source
- JSON data except where its values affect generated UI or links
- Code examples inside `<pre><code>` should not be confused with executing DOM, though inaccurate examples remain a documentation concern

### Important Caveats

- Shared page chrome is manually duplicated across more than 30 documents, so one source pattern often has site-wide reach.
- Each challenge intentionally demonstrates a focused implementation, but its interactive demo is still evaluated as user-facing functionality.
- The published origin is declared as `https://30daysofvanillajavascript.com/`; live hosting headers, redirects, robots, sitemap, external services, and network failures require deployment testing.
- Audits should prefer authored module source over matching code printed inside tutorial blocks.
