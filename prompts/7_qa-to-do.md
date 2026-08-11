# Prompt 07: QA To-Do List

Read:

`output/<project-name>/06-final-report.md`

Also consult the earlier audit reports when needed to preserve exact file references or clarify a finding.

Do not modify project source files.

Write the completed checklist to:

`output/<project-name>/07-qa-todo.md`

## Goal

Convert the completed frontend audit into a practical pre-launch QA checklist.

This document is not another audit.

Do not re-explain every finding in detail.

Create a list of concrete tasks a developer can work through, check off, and verify before the project is considered complete.

## Include

### Fix Before Launch

Confirmed issues that should be resolved before release.

Use Markdown checkboxes:

- [ ] Replace placeholder social links with real destinations or remove them.
- [ ] Add a skip link to the main content.
- [ ] Correct inconsistent business location copy.

Include file references where useful.

### Verify Manually

Items that require browser, device, keyboard, screen reader, responsive, deployment, or other manual verification.

Examples:

- [ ] Keyboard-test the mobile navigation.
- [ ] Verify focus returns to the menu trigger after closing.
- [ ] Test the site at 200% and 400% zoom.
- [ ] Verify rendered color contrast.
- [ ] Confirm production robots and indexing behavior.

### Optional Cleanup

Low-priority or informational improvements that are not launch blockers.

Do not mix these with required fixes.

## Rules

- Base the checklist on the completed audit reports.
- Do not invent new findings.
- Deduplicate overlapping items.
- Write each item as a concrete action.
- Keep tasks small enough to check off individually.
- Preserve relevant file references.
- Do not include long explanations.
- Do not modify source files.
- Do not treat informational findings as required work.
