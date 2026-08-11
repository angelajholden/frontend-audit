# Prompt 06: Final Frontend Audit Report

Create a final frontend audit report using the completed:

- Project Discovery
- HTML Audit
- CSS Audit
- JavaScript Audit
- Accessibility Audit
- SEO Audit

Do not modify any files.

## Goal

Produce one concise, prioritized report that tells the developer:

- what is working well
- what should be fixed
- what matters most
- where findings overlap
- what requires manual verification
- what can reasonably wait

The final report should synthesize the previous audits rather than simply concatenate them.

## Primary Principle

Prioritize findings by practical impact.

Give the greatest weight to issues involving:

1. Broken or unreliable functionality
2. Significant accessibility barriers
3. Incorrect or fragile HTML semantics
4. Maintainability problems likely to create bugs
5. Responsive or layout failures
6. Security-relevant frontend issues
7. Crawlability or indexing problems
8. Lower-impact code quality improvements

Do not prioritize cosmetic preferences or theoretical edge cases over real user-facing problems.

## Deduplicate Findings

Many findings may appear in more than one audit.

Examples:

- a clickable `<div>` may appear in the HTML, JavaScript, and accessibility audits
- missing focus styles may appear in the CSS and accessibility audits
- poor heading structure may appear in the HTML, accessibility, and SEO audits
- JavaScript navigation may appear in JavaScript and SEO findings

Do not report the same root problem multiple times.

Combine related findings into one consolidated finding.

When consolidating, preserve all meaningful consequences.

For example:

A generic clickable element may simultaneously create:

- incorrect HTML semantics
- unnecessary JavaScript
- keyboard accessibility problems
- maintenance complexity

Report this as one root issue with those consequences rather than four separate defects.

## Resolve Conflicting Recommendations

If previous audits recommend different approaches for the same issue, evaluate the underlying implementation and choose the simplest appropriate solution.

Prefer, in order:

1. Correct native HTML
2. CSS
3. Minimal JavaScript
4. ARIA where native semantics are insufficient
5. Additional libraries or abstractions only when clearly justified

Do not preserve a recommendation merely because an earlier audit made it.

The final report should represent the best consolidated recommendation.

## Severity

Normalize all findings to:

- **High**
- **Medium**
- **Low**
- **Informational**

### High

A significant issue affecting:

- essential functionality
- major accessibility barriers
- security
- important content access
- serious rendering or responsive failures
- crawlability or indexing

### Medium

A meaningful issue affecting:

- maintainability
- semantic correctness
- accessibility
- reliability
- responsive behavior
- performance
- technical SEO

but not preventing most users from using the site.

### Low

A concrete improvement with limited impact.

### Informational

An optional improvement, observation, or manual-review note.

Do not inflate severity.

## Confidence

Preserve confidence independently from severity:

- **High**
- **Medium**
- **Low**

Prefer high-confidence findings in the primary action plan.

Low-confidence findings should generally appear in manual verification or follow-up sections unless their potential impact is significant.

## Prioritization

Within each severity level, prioritize:

1. User impact
2. Breadth of impact
3. Likelihood of causing failures
4. Ease of resolving the root cause
5. Whether one fix resolves multiple findings

Prefer fixes that simplify the codebase while resolving several concerns at once.

Example:

Replacing a custom clickable `<div>` with a native `<button>` may simultaneously improve:

- semantics
- keyboard behavior
- accessibility
- JavaScript complexity
- maintainability

Treat that as a higher-value fix than several unrelated low-impact cleanup items.

## Output Structure

# Frontend Audit Report

## Executive Summary

Provide a concise overview of the frontend implementation.

Include:

- overall frontend health
- strongest areas
- most important risks
- number of consolidated findings by severity
- whether manual testing is still required

Keep this section short.

## Project Architecture

Summarize the Project Discovery findings in a few paragraphs.

Include:

- how HTML is produced
- styling approach
- JavaScript approach
- major frontend tools or libraries
- relevant framework or CMS context
- important audit caveats

Do not repeat the complete discovery report.

## What the Project Does Well

Identify meaningful strengths across the audits.

Examples may include:

- semantic HTML
- simple frontend architecture
- good CSS organization
- minimal JavaScript
- strong native controls
- accessible forms
- restrained ARIA usage
- responsive layout
- crawlable navigation
- sensible metadata

Only include strengths supported by the audits.

Do not add generic praise.

## Priority Findings

Report consolidated findings in priority order.

Use this format:

## Finding: Short descriptive title

**Severity:** High | Medium | Low | Informational
**Confidence:** High | Medium | Low
**Categories:** HTML | CSS | JavaScript | Accessibility | SEO
**Files:** `path/to/file.ext:line`

### Problem

Explain the root problem.

Do not list every audit's wording separately.

### Impact

Explain the practical consequences.

Include relevant effects such as:

- usability
- accessibility
- reliability
- maintainability
- responsive behavior
- performance
- search discoverability

Only include consequences actually supported by the audits.

### Recommendation

Describe the simplest appropriate fix.

Prefer root-cause fixes over patches.

### Example

When useful, provide a small example.

Do not rewrite entire files.

## Recommended Fix Order

Create a practical implementation sequence.

Group work into:

### Fix First

Issues that should be addressed before other cleanup.

Typically:

- broken functionality
- major accessibility barriers
- severe semantic problems
- serious security concerns
- crawl/index blocking issues
- major responsive failures

### Fix Next

Important but non-blocking issues.

Typically:

- maintainability problems
- repeated implementation problems
- moderate accessibility concerns
- fragile JavaScript
- CSS architecture problems

### Nice to Improve

Low-impact or optional improvements.

Do not include Informational findings as mandatory work.

## Quick Wins

Identify a short list of changes that:

- are relatively small
- have high value
- resolve multiple findings
- reduce future complexity

Do not label large refactors as quick wins.

## Accessibility Summary

Provide a short accessibility-specific summary.

Include:

- WebAIM Million priority issues found
- major user barriers
- areas that appear strong
- items requiring manual testing

Do not claim WCAG compliance.

Do not claim legal compliance.

## Manual Testing Required

Consolidate all manual verification items from previous audits.

Only include tests relevant to this specific project.

Examples may include:

- keyboard navigation
- focus order
- dialogs
- responsive reflow
- actual rendered color contrast
- screen-reader announcements
- form errors
- dynamic content
- touch interactions

For each test, briefly state what needs verification.

Do not produce a generic accessibility or QA checklist.

## SEO Summary

Provide a brief technical SEO summary.

Focus on:

- crawlability
- page structure
- metadata
- internal navigation
- indexing directives
- obvious technical issues

Do not include keyword strategy, backlink recommendations, or ranking predictions.

## Lower-Priority Cleanup

Summarize lower-impact maintainability or code-quality opportunities.

Keep this section concise.

Do not overwhelm the report with trivial cleanup.

## Final Recommendation

End with a short assessment of the project.

Answer:

- Is the frontend fundamentally sound?
- Are the problems mostly isolated or systemic?
- What should the developer address first?
- Does the project need targeted fixes or broader frontend restructuring?

Do not recommend a rewrite unless the audits demonstrate that the existing architecture is fundamentally unmaintainable or unreliable.

## Rules

- Do not modify files.
- Do not concatenate the previous audits.
- Deduplicate overlapping findings.
- Identify root causes.
- Prefer native HTML solutions where appropriate.
- Prefer simple fixes over additional abstractions.
- Do not inflate severity.
- Do not turn preferences into defects.
- Do not claim WCAG compliance.
- Do not claim legal accessibility compliance.
- Do not make SEO ranking promises.
- Do not recommend framework rewrites merely because another architecture is possible.
- Do not recommend removing Tailwind, Bootstrap, jQuery, or another library without a concrete reason.
- Preserve important file and line references.
- Prefer actionable findings over generic advice.
- Keep the final report readable enough that a developer can actually use it as an implementation plan.
