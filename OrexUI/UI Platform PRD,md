---

**PROMPT FOR CLAUDE**

You are designing a production-grade, long-term stable, evergreen UI component system.

This is NOT a typical CSS framework.
This is a “drop-in, no-build, standards-only component platform”.

Core goals:

1. No build tools required. Ever.
2. Works with plain HTML + CSS via CDN links.
3. JavaScript is optional and only for progressive enhancement.
4. Old code must continue to work unchanged for years.
5. Updates must never require migrations inside the same major version.
6. Stability is enforced by rules and process, not by promises.

Design the project as if it must still work unchanged in 10 years.

### Fundamental Principles

* Add, never invalidate.
* Additive changes allowed, breaking changes blocked.
* Every component must work without JavaScript (degraded UX acceptable).
* JavaScript only enhances behaviour, never provides core functionality.
* Use only web standards (HTML, CSS, minimal vanilla JS).
* No framework, no bundler, no transpilation, no CSS-in-JS.
* Copy-paste components must work in a standalone HTML file.

### File Distribution Model (CDN based)

```html
<link rel="stylesheet" href="core.css">
<link rel="stylesheet" href="components/button.css">
<link rel="stylesheet" href="components/modal.css">
<script src="helpers.js" defer></script>
```

* `core.css` = tokens, reset, base (very stable, rarely changes)
* each component has its own CSS file
* optional components are loaded individually
* tiny shared JS helpers file for interactions only

### CSS Architecture

* Class-only selectors (no element selectors like `button {}`).
* No deep selector nesting (max depth 3).
* Use CSS variables (design tokens) as public API.
* Theming done by overriding variables, not editing components.
* Dark mode via `prefers-color-scheme`.

### Component Contract

Every component must include:

* standalone `component.html` that works by itself
* `component.css`
* full README documentation
* accessibility built in by default:

  * keyboard support
  * correct ARIA attributes
  * focus management
  * WCAG AA contrast

No inline JS (`onclick` etc).
All behaviour via data-attributes + shared helpers.js.

### Progressive Enhancement Rule

Without JS:

* component is still usable.

With JS:

* nicer interactions (animation, focus trap, toggle etc).

JS must never be required for basic usage.

### Size Budgets (hard limits)

* core.css < 20KB gzipped
* common bundle < 30KB gzipped
* each individual optional component < 5KB gzipped

If a component exceeds limit:

* it must be simplified or split into base + advanced addon.

Budgets are never increased to fit a feature.
Features must adapt to budgets.

### Stability & Versioning

Inside one major version:

* class names never removed
* tokens never removed
* behaviour never silently changed

Instead:

1. Add new name/variant
2. Mark old as deprecated
3. Remove only in next major version with long warning + migration guide

Updates should be:

> change CDN version → refresh page → everything still works

### No Exceptions Policy

These are always rejected:

* inline JavaScript
* requirement of build tools
* breaking rename without deprecation
* components that only work with JS
* undocumented features
* partially finished components

Incomplete work never merges.

### Required Deliverables from You

Design this project and output:

1. Complete folder/file structure
2. Core CSS tokens system
3. Base styles and reset approach
4. Example implementation of at least:

   * Button
   * Modal (using native `<dialog>`)
   * Card
5. Minimal `helpers.js` API design (very small, generic utilities)
6. Theming system using CSS variables
7. Accessibility rules and patterns for all components
8. Rules for adding new components (a checklist/gates)
9. Release and versioning strategy
10. Documentation structure for each component
11. Examples of how old code stays valid across versions

All examples must be pure HTML/CSS (no framework code).

Design it so that:

* any commit that follows the rules is safe to release immediately
* any old HTML snippet copied years ago still renders correctly in future versions

The system must prioritize:
stability > features
boring > clever
add beside > rewrite

Your output should read like a production PRD + technical specification + governance model for an evergreen, no-build, copy-paste UI component platform.

Nothing should rely on future refactors.
Nothing should require later cleanup.
Everything must be safe at the moment it is merged.

---
