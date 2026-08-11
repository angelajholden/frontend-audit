# Prompt 03: JavaScript Audit

Audit the project's frontend JavaScript and related browser-side scripting.

Use the Project Discovery report to identify the correct source files and directories.

Do not modify any files.

## Goal

Evaluate the JavaScript for correctness, maintainability, resilience, unnecessary complexity, accessibility-related interaction concerns, and whether JavaScript is being used where native HTML or CSS would be more appropriate.

The primary question is:

**Is the JavaScript doing only what JavaScript needs to do, and is it doing that work clearly and reliably?**

Prefer native HTML and CSS behavior when they can provide the required functionality without JavaScript.

## Scope

Audit editable frontend source files such as:

- `.js`
- `.mjs`
- `.ts` when compiled for browser use
- inline scripts
- browser modules
- frontend utility files
- DOM interaction code
- API/fetch code used in the browser

Do not primarily audit:

- minified JavaScript
- compiled bundles
- vendor libraries
- dependency source
- server-side JavaScript that does not affect the browser
- generated files

when editable source files are available.

## Audit Areas

### 1. Unnecessary JavaScript

Identify JavaScript that recreates functionality already available through HTML or CSS.

Examples include:

- clickable generic elements instead of buttons or links
- custom accordions where `<details>` / `<summary>` would meet the requirements
- JavaScript-only show/hide behavior that CSS can handle cleanly
- custom form validation duplicating native browser behavior without a clear reason
- script-driven navigation that should be ordinary links
- visual state changes that belong in CSS
- JavaScript that compensates for incorrect HTML semantics

When the root issue is HTML, recommend fixing the HTML rather than adding more JavaScript.

Do not recommend replacing JavaScript with native HTML or CSS when the native behavior does not actually meet the functional requirements.

### 2. JavaScript Architecture

Review how frontend logic is organized.

Look for:

- very large files with unrelated responsibilities
- monolithic functions
- duplicated logic
- unclear module boundaries
- tightly coupled DOM and data logic
- repeated utility functions
- hidden shared state
- code that is difficult to trace

Do not require a specific architecture or design pattern.

Evaluate whether the current organization is understandable and maintainable for the size of the project.

### 3. Global Scope

Identify:

- unnecessary global variables
- functions attached to `window`
- shared mutable state
- accidental globals
- global event handlers

Prefer module or local scope where appropriate.

Do not flag intentional global integration points without a concrete maintainability or collision risk.

### 4. DOM Queries

Review DOM selection such as:

- `querySelector`
- `querySelectorAll`
- `getElementById`
- `getElementsByClassName`

Look for:

- repeated queries for the same element
- queries inside loops when they can be avoided
- brittle selectors
- selectors tightly coupled to presentation classes
- assumptions that queried elements always exist
- overly broad document-level queries

Recommend caching or narrowing DOM queries where it provides a concrete benefit.

Do not optimize trivial queries without evidence of complexity or maintainability concerns.

### 5. DOM Existence and Defensive Code

Check whether scripts safely handle pages where expected elements may not exist.

Look for:

- null dereferences
- scripts loaded globally but assuming page-specific markup
- event listeners attached to missing elements
- methods called on potentially null values

Prefer guards when scripts may run across multiple page types.

Do not encourage excessive defensive programming where the markup contract is guaranteed and clear.

### 6. Event Listeners

Review event handling.

Look for:

- duplicate listeners
- listeners attached repeatedly
- unnecessary listeners on many child elements
- missing cleanup where components are created and destroyed dynamically
- anonymous callbacks that make lifecycle management difficult
- inappropriate event types
- event handling tightly coupled to DOM structure

Consider event delegation when it meaningfully reduces complexity.

Do not recommend delegation merely because it is possible.

### 7. Keyboard Interaction

Review JavaScript-driven interactions for obvious keyboard concerns.

Look for:

- click-only controls
- custom keyboard handling that conflicts with native behavior
- keyboard events added to generic elements that should be native controls
- focus moved unexpectedly
- focus not restored after temporary UI closes
- Escape handling where modal or temporary UI reasonably requires it

Do not perform the full accessibility audit here.

When native HTML would provide the required keyboard semantics automatically, recommend the native element first.

### 8. Focus Management

Review JavaScript that changes:

- focus
- visibility
- modal state
- menus
- drawers
- dialogs
- overlays
- dynamically inserted interfaces

Look for:

- focus sent to arbitrary locations
- focus lost after content removal
- focus trapped unintentionally
- modal interfaces without appropriate focus handling
- focus management implemented where native `<dialog>` could reduce custom code

Do not assume every UI state change requires programmatic focus movement.

Detailed accessibility impact belongs in the accessibility audit.

### 9. State Management

Review application state stored in:

- variables
- DOM attributes
- classes
- `data-*` attributes
- localStorage
- sessionStorage
- URL parameters
- history state

Look for:

- duplicated sources of truth
- DOM state and JavaScript state becoming unsynchronized
- state inferred from presentation classes unnecessarily
- unclear state transitions
- booleans or flags that can contradict each other

Prefer one clear source of truth where practical.

### 10. CSS Class Manipulation

Review uses of:

- `classList.add`
- `classList.remove`
- `classList.toggle`
- `className`

Look for:

- JavaScript controlling detailed presentation rather than state
- many classes toggled independently to represent one logical state
- presentation-specific logic embedded in JavaScript
- fragile class-name strings duplicated throughout code

Prefer JavaScript to communicate state and CSS to determine presentation.

### 11. Data Attributes

Review `data-*` attributes used for JavaScript hooks or state.

Look for:

- overloaded attributes
- values duplicated in JavaScript constants
- inconsistent naming
- presentation classes being used as JS hooks where a stable data attribute would be clearer

Do not require `data-*` attributes when semantic IDs or stable structural selectors are already appropriate.

### 12. `innerHTML` and DOM Injection

Review uses of:

- `innerHTML`
- `outerHTML`
- `insertAdjacentHTML`
- template strings inserted into the DOM

Consider:

- whether content includes untrusted or external data
- cross-site scripting risk
- unnecessary HTML parsing
- difficulty maintaining generated markup
- event-handler loss when replacing DOM trees

Prefer safer DOM APIs or explicit sanitization when content is not fully trusted.

Do not flag static, controlled markup insertion as a security vulnerability without evidence.

Distinguish actual risk from general caution.

### 13. DOM Creation

Review extensive use of:

- `createElement`
- `append`
- `appendChild`
- string-built markup
- client-side rendering

Look for situations where large amounts of static interface markup are being generated unnecessarily in JavaScript.

If markup is known before runtime, consider whether it belongs in HTML or a server-side template.

Do not criticize legitimate dynamic UI generation.

### 14. Fetch and API Requests

Review browser-side network requests.

Look for:

- missing error handling
- assumptions that every response succeeds
- failure to check `response.ok`
- parsing errors
- duplicate requests
- race conditions
- stale results
- loading states that can become stuck
- user actions enabled while requests are already in flight
- failure states that leave the interface unusable

Do not require elaborate retry or state-management systems for simple requests.

### 15. Async Code

Review:

- `async`
- `await`
- Promises
- timers
- callbacks

Look for:

- unhandled promise rejections
- missing `try/catch` where failure is expected
- forgotten `await`
- sequential requests that unintentionally depend on each other
- race conditions
- async callbacks used incorrectly
- errors swallowed without useful handling

Do not wrap every async operation in `try/catch` if errors are intentionally handled elsewhere.

### 16. Timers

Review:

- `setTimeout`
- `setInterval`
- `requestAnimationFrame`

Look for:

- timers used to solve synchronization problems
- arbitrary delays
- intervals that are never cleared
- repeated polling without a clear need
- animation timers that should use CSS

Flag timing-based code when it is brittle or unnecessary.

### 17. Form Handling

Review JavaScript used with forms.

Look for:

- preventing native submission unnecessarily
- custom validation duplicating native validation
- failure to handle failed submissions
- controls disabled permanently after an error
- values read incorrectly
- serialization issues
- manually simulated form behavior

Prefer native browser behavior unless there is a concrete reason to replace it.

### 18. Navigation and URLs

Review JavaScript that handles:

- navigation
- URL parameters
- hashes
- history
- routing-like behavior

Look for:

- ordinary navigation implemented via JavaScript
- broken back/forward behavior
- state not represented in the URL where users reasonably expect it
- URL state and interface state becoming inconsistent
- links disabled and replaced with click handlers

Do not require client-side routing.

### 19. Progressive Enhancement

Determine whether core content and functionality reasonably survive if JavaScript:

- fails to load
- loads slowly
- encounters an exception
- is blocked

Not every website needs to function completely without JavaScript.

Prioritize basic content access, navigation, forms, and core interactions where native behavior can provide a useful baseline.

### 20. Error Handling

Look for errors that:

- fail silently
- only appear in the console
- leave the interface in an inconsistent state
- expose raw technical messages to users
- prevent later scripts from running
- originate from assumptions about DOM or API data

Recommendations should be proportional to the application's complexity.

### 21. Third-Party JavaScript

Identify major browser-side third-party scripts.

Examples include:

- analytics
- embedded widgets
- social embeds
- maps
- chat widgets
- advertising
- consent tools
- libraries loaded from CDNs

Look for:

- duplicate library loading
- unnecessary dependencies
- large libraries used for trivial functionality
- blocking scripts
- third-party scripts loaded on pages that do not need them
- unmanaged global dependencies

Do not recommend removing a third-party service simply because it is third-party.

### 22. Libraries and Dependencies

Evaluate frontend libraries according to their actual use.

Look for:

- libraries imported but barely used
- duplicate libraries solving the same problem
- legacy libraries retained for one small feature
- dependency code recreated manually elsewhere
- multiple versions of the same library

Do not recommend replacing a working library with custom JavaScript without a clear benefit.

### 23. jQuery

If jQuery is present, evaluate how it is used.

Do not automatically treat jQuery as a defect.

Look for:

- jQuery included solely for trivial selectors or class toggling
- modern JavaScript and jQuery mixed inconsistently
- plugins that require jQuery
- deprecated patterns
- duplicate native and jQuery implementations
- jQuery loaded globally when only a small isolated feature requires it

Recommend removal only when the project can reasonably eliminate it without unnecessary risk or rewrite cost.

### 24. Dead and Unused JavaScript

Identify:

- unused functions
- unreachable code
- obsolete handlers
- variables never read
- imports never used
- abandoned feature code
- duplicate implementations

Be cautious when functions may be referenced by inline HTML, CMS-generated content, third-party integrations, or global hooks.

Lower confidence when usage cannot be confirmed from static analysis.

### 25. Comments

Review comments only when they materially affect maintainability.

Flag:

- comments describing behavior that no longer matches the code
- large blocks of commented-out code
- TODOs that indicate unfinished production behavior
- comments compensating for unnecessarily confusing implementation

Do not require comments on self-explanatory code.

### 26. Browser APIs

Review browser APIs where relevant.

Examples include:

- localStorage
- sessionStorage
- IntersectionObserver
- MutationObserver
- ResizeObserver
- History API
- Clipboard API
- Geolocation
- media APIs

Check for:

- feature assumptions
- cleanup requirements
- failure handling
- unnecessary use of expensive observers
- APIs used where simpler native behavior exists

Do not flag modern browser APIs simply because older alternatives exist.

### 27. Performance-Relevant Patterns

Identify obvious JavaScript patterns that may materially affect frontend performance.

Examples include:

- expensive DOM work inside loops
- layout reads/writes repeatedly interleaved
- large operations on every scroll event
- unthrottled high-frequency handlers
- repeated unnecessary network requests
- huge client-side rendering operations

Do not perform speculative micro-optimization.

Only report performance concerns where the code pattern is meaningfully likely to matter.

### 28. Security-Relevant Frontend Patterns

Identify clear browser-side risks such as:

- unsanitized external data inserted as HTML
- secrets or private credentials embedded in frontend source
- unsafe URL construction
- misuse of `eval`
- dynamic function construction
- trust placed in client-side validation for security

Do not treat frontend-visible API identifiers as secrets unless they actually grant privileged access.

Do not turn this into a full security audit.

## Finding Criteria

Report findings when the JavaScript creates or is likely to create problems involving:

- functionality
- reliability
- maintainability
- accessibility
- browser behavior
- performance
- security
- unnecessary complexity

Distinguish actual defects from optional refactoring opportunities.

Do not penalize simple code for being simple.

## Severity

Assign one severity to each finding:

### High

The JavaScript causes or is likely to cause major functionality, accessibility, security, data integrity, or reliability problems.

### Medium

The JavaScript works but creates meaningful maintainability, interaction, performance, or resilience concerns.

### Low

The implementation could be improved in a concrete but limited way.

### Informational

An observation worth documenting that does not require a change.

Do not inflate severity.

## Confidence

Assign confidence independently:

- **High** — directly supported by source
- **Medium** — strongly suggested but partly dependent on runtime behavior
- **Low** — requires browser testing, API behavior, or additional context

## Output

Begin with:

# JavaScript Audit

## Summary

Provide a brief assessment of:

- overall JavaScript organization
- amount of unnecessary JavaScript
- DOM interaction quality
- state management
- async and API handling
- accessibility-related interaction concerns
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

Explain what the JavaScript is doing.

### Why It Matters

Explain the concrete consequence.

### Recommendation

Describe the preferred approach.

When the real solution belongs in HTML or CSS, say so explicitly.

### Example

When useful, provide a small example.

Do not rewrite entire files unless necessary to demonstrate the recommendation.

## Positive Findings

End with a short section describing meaningful JavaScript practices the project is already handling well.

Only include strengths supported by the source.

## Rules

- Do not modify files.
- Do not refactor the project.
- Do not install or remove dependencies.
- Do not recommend JavaScript when native HTML or CSS already solves the problem adequately.
- Do not recommend replacing working libraries without a concrete benefit.
- Do not automatically treat jQuery as a defect.
- Do not perform framework-specific architecture review outside the project's established scope.
- Do not perform the full accessibility audit.
- Do not perform a full security audit.
- Do not perform speculative micro-optimization.
- Do not turn code-style preferences into defects.
- Cite specific files and line numbers whenever possible.
- Prefer concrete findings over generic best-practice advice.
- Base findings on the repository rather than assumptions.
