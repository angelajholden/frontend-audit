# Project Discovery

### Project Name

`scenic`

### Output Directory

`output/scenic/`

### Audit Suitability

**In Scope.** Scenic is a static, multi-page website whose rendered frontend is authored directly in HTML, CSS, and vanilla JavaScript. It does not use a component-driven JavaScript UI framework or client-side rendering framework.

### Frontend Summary

The site consists of four hand-authored static documents: a root homepage plus blog, contact, and generic page documents in subdirectories. Every page loads the same root-level stylesheets and deferred JavaScript file using relative URLs. Pages are served as-is; there is no templating, generation, bundling, package manager, or build step. Header, navigation, newsletter, social, and footer markup is duplicated across documents rather than produced from shared includes.

### HTML Source

- `index.html` — homepage and root entry point
- `blog/index.html` — blog article page
- `contact/index.html` — contact page
- `page/index.html` — generic content page

These four editable files are the complete source of browser HTML. There are no layouts, partials, templates, or generated pages.

### CSS Source

- `style.css` — primary authored site styles and responsive rules
- `normalize.css` — vendored normalization library; note integration effects but do not audit it as authored source
- `animate.css` — vendored animation library; note integration and accessibility effects but do not audit its internals as authored source
- Inline styles in the HTML, notably newsletter honeypot positioning

The project uses neither a CSS framework nor a preprocessor. Google Fonts supplies Open Sans at runtime.

### JavaScript Source

- `script.js` — the only authored browser script, loaded with `defer` on all four pages

It implements the copyright year, responsive menu open/close behavior, Escape handling, contact-form page metadata, a print action, and social-sharing URLs. The browser APIs are used directly; there is no module system, bundler, jQuery, or other JavaScript library.

### Build and Tooling

There is no `package.json`, build configuration, lint configuration, or formatter configuration. The site can be opened directly or served with a simple static server; the README recommends VS Code Live Server. `.vscode/settings.json` is editor configuration only.

### Third-Party Dependencies

- **CSS:** `normalize.css` and `animate.css` are committed third-party stylesheets.
- **Fonts:** Open Sans is loaded from Google Fonts.
- **Forms/services:** the newsletter posts to Mailchimp and the contact form posts to Formspree.
- **Content/services:** photographs are credited to Unsplash; social-sharing endpoints are constructed by `script.js`.

### Audit Exclusions

- `.git/` and `.vscode/`
- `design/` reference screenshots and PSD source
- `images/`, including `images/optimized/`, except to validate referenced assets
- `svg/` standalone icon assets except to validate referenced assets
- `normalize.css` and `animate.css` internals as vendored code
- `.DS_Store`, `README.md`, `LICENSE.md`, and `QA.md` as non-runtime documentation

There are no dependency directories, compiled bundles, or generated build outputs.

### Important Caveats

- Shared page chrome is duplicated, so repeated findings may affect all four HTML files and line numbers can differ.
- Relative asset and navigation paths differ between the root and nested pages.
- Several behaviors are conditional on page-specific elements; JavaScript must be evaluated on every page, not only the homepage.
- Source inspection can confirm markup and deterministic script issues, but responsive behavior, focus management, contrast over images, animation, form-service behavior, and popup/share behavior still require browser or assistive-technology verification.
