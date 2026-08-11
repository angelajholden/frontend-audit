# Prompt 05: SEO Audit

Audit the project's frontend for basic technical and on-page SEO issues.

Use the Project Discovery report and previous HTML and accessibility audits to understand how pages are generated and structured.

Do not modify any files.

## Goal

Evaluate whether the website provides search engines with clear, crawlable, well-structured page content.

This is a lightweight technical SEO audit.

The primary question is:

**Does the frontend make it easy for search engines to understand, crawl, and index the site's content without obvious technical barriers?**

Prioritize strong HTML structure, accessibility, useful metadata, crawlable navigation, and sensible page organization.

Do not turn this into a keyword-strategy, backlink, marketing, or content-growth audit.

## Philosophy

Good SEO begins with a well-built website.

Give significant weight to:

- semantic HTML
- meaningful document structure
- accessible markup
- useful headings
- descriptive links
- crawlable navigation
- clear page titles
- appropriate metadata
- indexable page content

Do not recommend SEO techniques that make the markup or user experience worse.

Do not recommend adding unnecessary content solely for search engines.

## Scope

Audit frontend and configuration files that affect:

- rendered page markup
- page titles
- metadata
- canonical URLs
- indexing directives
- structured data
- navigation
- internal linking
- URL generation
- sitemap configuration
- robots configuration

Depending on the project, this may include:

- HTML
- templates
- layouts
- metadata configuration
- route definitions
- CMS templates
- `robots.txt`
- sitemap files
- structured-data scripts
- static-site configuration

Do not perform backlink research, competitor research, keyword research, domain-authority analysis, or traffic analysis.

## Audit Areas

### 1. Page Titles

Review `<title>` elements.

Look for:

- missing titles
- empty titles
- obviously duplicated titles across distinct pages
- titles that contain only the site name
- titles that provide little indication of the page's subject
- excessively templated titles that obscure page meaning

Prefer concise, descriptive page titles.

Do not prescribe exact character limits as hard requirements.

Do not rewrite every title unless a concrete problem exists.

### 2. Meta Descriptions

Review:

```html
<meta name="description" />
```

Look for:

- missing descriptions on important pages
- empty descriptions
- obviously duplicated descriptions
- descriptions unrelated to page content

Treat meta descriptions as useful page summaries, not ranking guarantees.

Do not require every possible page to have a handcrafted description when the site's architecture makes that unreasonable.

Do not recommend keyword stuffing.

### 3. Document Language

Confirm that the root document declares an appropriate language.

For example:

```html
<html lang="en"></html>
```

This may already appear in the accessibility audit.

Do not duplicate it as a major SEO finding unless it is relevant to both audits.

### 4. Heading Structure

Review page headings from an SEO and content-understanding perspective.

Look for:

- missing meaningful page headings
- headings used only for visual styling
- page topics that are difficult to infer from heading structure
- multiple headings that repeat generic phrases without helping users or search engines understand the content

Do not require headings to contain target keywords.

Do not force rigid heading hierarchies merely for SEO.

Semantic and understandable structure matters more than keyword placement.

### 5. Semantic HTML

Consider whether the HTML clearly communicates page structure.

Useful elements may include:

- `<main>`
- `<article>`
- `<section>`
- `<nav>`
- `<header>`
- `<footer>`
- headings
- lists
- tables where appropriate

Do not recommend semantic HTML because of speculative ranking benefits.

Recommend it because meaningful structure improves content understanding, accessibility, maintainability, and machine interpretation.

### 6. Crawlable Navigation

Review primary site navigation and internal navigation.

Look for:

- navigation implemented entirely with JavaScript instead of links
- clickable elements that do not expose crawlable URLs
- important pages only reachable through scripted interactions
- broken link structures
- links missing `href`
- navigation dependent on client-side behavior when ordinary anchors would work

Prefer normal anchor links for navigation.

Do not require JavaScript applications to eliminate legitimate client-side routing if the project is otherwise within scope.

### 7. Internal Links

Review obvious internal-linking problems.

Look for:

- broken relative paths
- important content with no apparent internal route
- repeated links with unclear purpose
- links generated through JavaScript unnecessarily
- malformed URLs

Do not attempt to design a complete internal-linking strategy.

Do not recommend adding large quantities of internal links solely for SEO.

### 8. Link Text

Review link text where there is a clear usability or semantic problem.

Examples include repeated standalone links such as:

- "click here"
- "here"
- "more"

Prefer link text that gives users reasonable context.

Do not over-optimize natural link text for keywords.

Accessibility and clarity come first.

### 9. Canonical URLs

Check for canonical metadata where the project architecture suggests it is relevant.

For example:

```html
<link rel="canonical" href="..." />
```

Look for:

- clearly incorrect canonical URLs
- every page pointing to the same canonical URL unintentionally
- development or staging domains in canonical tags
- malformed URLs
- canonical tags pointing to unrelated pages

Do not require canonical tags when the project has no realistic duplicate-URL concern.

### 10. Robots Directives

Review relevant directives such as:

```html
<meta name="robots" />
```

and `robots.txt`.

Look for obvious problems such as:

- production pages accidentally marked `noindex`
- entire production sites accidentally disallowed
- staging directives carried into production configuration
- contradictory directives

Do not assume that intentionally excluded pages should be indexed.

Flag uncertain cases rather than overriding project intent.

### 11. Sitemap

If a sitemap exists or sitemap generation is configured, review it for obvious structural issues.

Look for:

- development URLs
- malformed URLs
- excluded important routes
- references to pages that no longer exist
- incorrect hostnames

If no sitemap exists, consider whether the site's size and architecture make one useful.

Do not automatically report the absence of a sitemap as a significant SEO defect for a very small, easily crawlable site.

### 12. Images

Review image markup only where it affects discoverability or page structure.

Consider:

- meaningful images with useful alternative text
- descriptive filenames where reasonable
- image dimensions
- appropriate use of responsive image markup

Do not treat `alt` text as a place to insert keywords.

Alternative text should primarily serve the purpose of the image for users who cannot see it.

Accessibility takes precedence over SEO optimization.

### 13. Structured Data

Identify existing structured data such as JSON-LD.

Check for:

- malformed JSON
- obviously incorrect schema types
- missing required values in implemented schema
- URLs or entities that clearly do not match page content
- duplicate or contradictory structured data

Do not recommend structured data simply because a schema type exists for the site's subject.

Only recommend it where the page contains content that legitimately maps to a useful schema.

Do not invent ratings, reviews, authorship, organizations, products, prices, or other structured data not supported by visible content.

### 14. Open Graph and Social Metadata

Review social-sharing metadata where present.

Examples include:

- `og:title`
- `og:description`
- `og:image`
- `og:url`
- Twitter/X card metadata

Look for:

- obviously broken URLs
- missing image references
- stale development domains
- metadata unrelated to the page

Treat this as discoverability and sharing metadata, not core search-ranking SEO.

Do not make missing Open Graph metadata a high-severity SEO issue.

### 15. JavaScript-Dependent Content

Identify important page content that appears to exist only after JavaScript executes.

Consider whether:

- primary content is present in HTML
- navigation is crawlable
- meaningful text depends on client-side insertion
- important links only exist after interaction

Do not assume JavaScript-generated content is automatically invisible to search engines.

Flag it when the implementation introduces unnecessary dependence on JavaScript for basic content discovery.

### 16. URL Structure

Review routes and URLs only for obvious technical problems.

Look for:

- malformed paths
- unstable query-string URLs used for permanent content unnecessarily
- duplicate routes exposing identical content
- inconsistent trailing-slash handling where it clearly creates duplicates
- development URLs committed into production templates

Do not recommend rewriting existing URLs merely to make them shorter or more keyword-rich.

Stable URLs are preferable to theoretically "optimized" URLs.

### 17. Duplicate Content Signals

Identify obvious cases where multiple URLs or templates appear to produce the same content unintentionally.

Possible signals include:

- duplicate route definitions
- multiple canonical variations
- print or alternate versions without clear handling
- query parameters generating identical indexable pages

Do not claim duplicate-content problems without evidence.

### 18. Basic Performance-Relevant Markup

Identify frontend patterns with obvious discoverability or user-experience implications, such as:

- very large blocking assets
- render-blocking scripts that appear unnecessary
- missing image dimensions causing obvious layout instability
- excessively large client-side payloads for simple pages

Do not turn this into a Core Web Vitals or performance audit.

Only report clear frontend implementation concerns.

### 19. Accessibility and SEO Overlap

Recognize that many strong frontend practices support both accessibility and search discoverability.

Examples include:

- semantic HTML
- useful headings
- descriptive links
- meaningful page titles
- image alternative text
- native navigation
- understandable content hierarchy

Do not create duplicate findings solely to place the same defect into another category.

Where an issue was already identified in the accessibility or HTML audit, reference the existing concern and explain any SEO implication briefly.

## What This Audit Should Not Do

Do not perform:

- keyword research
- keyword-density analysis
- backlink analysis
- competitor analysis
- content-gap analysis
- domain-authority scoring
- ranking predictions
- search-volume analysis
- content marketing strategy
- conversion-rate optimization
- local SEO strategy
- paid search recommendations

Do not recommend rewriting natural content around keywords without a user-provided SEO strategy.

Do not claim that a particular technical change will improve rankings.

Search ranking depends on many factors outside this repository.

## Finding Criteria

Report findings when they create a meaningful problem involving:

- crawlability
- indexability
- page understanding
- metadata correctness
- navigation
- duplicate URL signals
- technical page structure
- machine-readable content

Do not report minor SEO preferences as defects.

## Severity

Use a lighter severity model for SEO.

### High

A clear technical issue that may prevent important content from being crawled or indexed correctly.

Examples include:

- production `noindex`
- important content inaccessible through crawlable navigation
- site-wide canonical misconfiguration
- production site blocked by robots directives

### Medium

A meaningful technical or structural issue that reduces clarity, discoverability, or search-engine understanding.

### Low

A concrete improvement with limited likely impact.

### Informational

An optional enhancement or observation.

Do not inflate severity for missing optional metadata.

## Confidence

Assign confidence independently:

- **High** — directly supported by source or configuration
- **Medium** — strongly suggested but depends on deployment or rendered output
- **Low** — requires live-site or search-engine verification

## Output

Begin with:

# SEO Audit

## Summary

Provide a brief assessment of:

- crawlability
- semantic page structure
- metadata quality
- internal navigation
- obvious indexing concerns
- major strengths
- most important technical issues
- total findings by severity

Keep this concise.

## Findings

Use this format:

## Finding: Short descriptive title

**Severity:** High | Medium | Low | Informational
**Confidence:** High | Medium | Low
**Files:** `path/to/file.ext:line`

### Problem

Explain the technical or structural issue.

### Why It Matters

Explain the practical search-discovery consequence without making ranking guarantees.

### Recommendation

Describe the simplest appropriate improvement.

### Example

When useful, provide a small markup or configuration example.

Do not rewrite entire pages or create a content strategy.

## Positive Findings

End with a short section identifying meaningful SEO-friendly frontend practices already present.

Examples may include:

- strong semantic HTML
- useful titles
- crawlable navigation
- descriptive links
- clean page structure
- appropriate canonical handling
- sensible structured data

Only include strengths supported by the repository.

## Rules

- Do not modify files.
- Keep this audit lightweight.
- Prioritize technical and structural SEO.
- Treat semantic HTML and accessibility as foundational.
- Do not perform keyword research.
- Do not recommend keyword stuffing.
- Do not make ranking promises.
- Do not treat every missing optional meta tag as a defect.
- Do not recommend schema markup without a legitimate content use case.
- Do not sacrifice accessibility or natural language for SEO.
- Do not duplicate findings from previous audits unnecessarily.
- Cite specific files and line numbers whenever possible.
- Prefer concrete technical findings over generic SEO advice.
- Base findings on the repository rather than assumptions.
