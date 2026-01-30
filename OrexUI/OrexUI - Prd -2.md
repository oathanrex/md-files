## 16. Versioning and Release Strategy

### 16.1 Semantic Versioning Rules

This project adheres strictly to Semantic Versioning 2.0.0 (SemVer). Version numbers follow the format `MAJOR.MINOR.PATCH` where:

**MAJOR version** increments indicate incompatible API changes that require user action to upgrade. Users must review migration notes before upgrading.

**MINOR version** increments indicate new functionality added in a backwards-compatible manner. Users can upgrade without code changes.

**PATCH version** increments indicate backwards-compatible bug fixes, security patches, and documentation corrections. Users should upgrade promptly.

**Pre-release versions** use the format `X.Y.Z-alpha.N`, `X.Y.Z-beta.N`, or `X.Y.Z-rc.N` for testing before stable releases.

**Version Examples:**

| Version | Description |
|---------|-------------|
| 1.0.0 | Initial stable release |
| 1.0.1 | Bug fix release |
| 1.1.0 | New component or feature added |
| 1.2.0 | Multiple new features |
| 2.0.0 | Breaking changes introduced |
| 2.0.0-beta.1 | Beta preview of version 2.0.0 |

### 16.2 Breaking vs Non-Breaking Changes

**Breaking Changes (require MAJOR version increment):**

- Removal of any CSS class from the public API
- Removal of any CSS custom property
- Renaming of any CSS class or custom property
- Changes to required HTML structure that break existing implementations
- Removal of any JavaScript function or method from the public API
- Changes to JavaScript function signatures (parameters, return types)
- Removal of a component entirely
- Changes to default behavior that alter existing functionality
- Removal of support for a previously supported browser
- Changes to the theming system that break existing custom themes

**Non-Breaking Changes (require MINOR or PATCH version increment):**

- Addition of new components
- Addition of new CSS classes
- Addition of new CSS custom properties
- Addition of new component variants or modifiers
- Addition of new JavaScript functions or methods
- Addition of optional parameters to existing functions
- Bug fixes that correct behavior to match documentation
- Performance improvements with no API changes
- Accessibility improvements that do not change markup requirements
- Documentation updates and corrections
- Addition of new utility classes
- Visual refinements that do not change component dimensions significantly

### 16.3 Deprecation Policy

**Deprecation Timeline:**

1. **Announcement**: Deprecated features are announced in the CHANGELOG and marked in documentation at least one MINOR version before removal.

2. **Warning Period**: Deprecated features remain functional for a minimum of two MINOR versions or six months, whichever is longer.

3. **Removal**: Deprecated features are removed only in MAJOR version releases.

**Deprecation Marking:**

CSS classes marked for deprecation:

```css
/* @deprecated since 1.3.0 - Use .btn--secondary instead. Will be removed in 2.0.0 */
.btn--alt {
  /* styles */
}
```

JavaScript functions marked for deprecation:

```javascript
/**
 * @deprecated since 1.3.0 - Use DCMT.modal.open() instead. Will be removed in 2.0.0
 */
function openModal(id) {
  console.warn('openModal() is deprecated. Use DCMT.modal.open() instead.');
  return DCMT.modal.open(id);
}
```

Documentation marking:

```html
<div class="callout callout--warning">
  <strong>Deprecated</strong>
  <p>The <code>.btn--alt</code> class is deprecated since version 1.3.0. 
  Use <code>.btn--secondary</code> instead. This class will be removed in version 2.0.0.</p>
</div>
```

**Deprecation Documentation Requirements:**

- Reason for deprecation
- Version when deprecation was introduced
- Recommended replacement or migration path
- Version when the feature will be removed
- Code example showing the migration

### 16.4 Changelog Requirements

Every release must include a CHANGELOG.md entry following the Keep a Changelog format.

**CHANGELOG.md Structure:**

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
### Changed
### Deprecated
### Removed
### Fixed
### Security

## [1.2.0] - 2025-02-15

### Added
- New `Drawer` component for slide-out panels (#142)
- `toast--info` variant for informational toast notifications (#138)
- CSS custom property `--toast-duration` for configurable auto-dismiss timing (#138)

### Changed
- Improved keyboard navigation in `Tabs` component (#145)
- Updated focus ring color for better visibility in dark theme (#147)

### Deprecated
- `.alert--notice` class deprecated in favor of `.alert--info` (#138)

### Fixed
- Fixed focus trap not releasing correctly when modal closes (#140)
- Corrected ARIA attributes on pagination component (#143)
- Fixed tooltip positioning when near viewport edge (#144)

## [1.1.0] - 2025-01-20

### Added
...
```

**Changelog Entry Requirements:**

1. Every merged pull request must have a corresponding changelog entry
2. Entries must reference the pull request or issue number
3. Breaking changes must be clearly labeled with `**BREAKING:**` prefix
4. Security fixes must be in the Security section with CVE numbers if applicable
5. Entries must be written in imperative mood ("Add feature" not "Added feature")
6. Each entry must be understandable without reading the full PR

**Release Notes:**

For MAJOR and MINOR releases, a separate release notes document is required containing:

- Summary of the release
- Highlight of significant new features
- List of breaking changes with migration instructions
- List of deprecations
- Known issues
- Acknowledgment of contributors

---

## 17. Testing and Quality Assurance

### 17.1 Accessibility Testing Requirements

**Keyboard Testing Protocol:**

For every interactive component, the following keyboard tests must pass:

| Test | Requirement |
|------|-------------|
| Tab navigation | Component can be reached via Tab key |
| Tab order | Focus order matches visual/logical order |
| Focus visibility | Focus indicator is clearly visible (3:1 contrast minimum) |
| Activation | Enter and/or Space activates the component appropriately |
| Escape | Escape closes overlays/popups where applicable |
| Arrow keys | Arrow navigation works for menus, tabs, radio groups, sliders |
| No trap | Focus is not trapped except in modal contexts |
| Trap escape | Modal focus traps can be exited via Escape |

**Screen Reader Testing Protocol:**

Testing must be performed with at least two of the following screen reader combinations:

| Screen Reader | Browser | Platform |
|---------------|---------|----------|
| VoiceOver | Safari | macOS |
| NVDA | Firefox | Windows |
| NVDA | Chrome | Windows |
| JAWS | Chrome | Windows |
| TalkBack | Chrome | Android |
| VoiceOver | Safari | iOS |

**Screen Reader Test Checklist:**

- [ ] Component name/role is announced correctly
- [ ] Component state is announced (expanded, selected, checked, etc.)
- [ ] State changes are announced appropriately
- [ ] Instructions for interaction are clear or implied by role
- [ ] Related labels and descriptions are announced
- [ ] Error messages are announced when they appear
- [ ] Dynamic content updates are announced via live regions
- [ ] Decorative elements are not announced

**Color Contrast Testing:**

| Element Type | Minimum Ratio | Tool |
|--------------|---------------|------|
| Normal text (< 18pt) | 4.5:1 | WebAIM Contrast Checker |
| Large text (>= 18pt or 14pt bold) | 3:1 | WebAIM Contrast Checker |
| UI components and graphics | 3:1 | WebAIM Contrast Checker |
| Focus indicators | 3:1 | Manual verification |

All color combinations must be tested in both light and dark themes.

### 17.2 Manual Testing Checklist

**Component Testing Checklist:**

Every component must pass the following manual tests before release:

```markdown
## Component: [Component Name]

### Visual Testing
- [ ] Renders correctly in light theme
- [ ] Renders correctly in dark theme
- [ ] Renders correctly at 320px viewport width
- [ ] Renders correctly at 768px viewport width
- [ ] Renders correctly at 1440px viewport width
- [ ] All variants display correctly
- [ ] All states display correctly (hover, focus, active, disabled)
- [ ] No visual overflow or clipping issues
- [ ] Spacing matches design system tokens
- [ ] Typography matches design system tokens

### Interaction Testing
- [ ] Click/tap interactions work correctly
- [ ] Hover states appear on mouse hover
- [ ] Focus states appear on keyboard focus
- [ ] Disabled state prevents interaction
- [ ] Loading state displays correctly (if applicable)
- [ ] Animations respect prefers-reduced-motion

### Keyboard Testing
- [ ] Component is focusable via Tab
- [ ] Focus order is logical
- [ ] Focus indicator is visible
- [ ] All functionality accessible via keyboard
- [ ] Escape closes component (if applicable)

### Screen Reader Testing
- [ ] Component role is announced
- [ ] Component label is announced
- [ ] State changes are announced
- [ ] Error messages are announced (if applicable)

### Code Quality
- [ ] HTML validates (no errors)
- [ ] No console errors in browser
- [ ] No accessibility errors in automated scan
- [ ] Code matches documented examples
```

### 17.3 Automated Testing

**HTML Validation:**

All HTML must pass W3C validation with zero errors. Warnings should be reviewed and resolved where practical.

```bash
# Validation command (using html-validate)
npx html-validate "**/*.html"
```

**CSS Linting:**

CSS must pass Stylelint with the project configuration:

```json
{
  "extends": ["stylelint-config-standard"],
  "rules": {
    "selector-class-pattern": "^[a-z][a-z0-9]*(-[a-z0-9]+)*(__[a-z0-9]+(-[a-z0-9]+)*)?(--[a-z0-9]+(-[a-z0-9]+)*)?$",
    "custom-property-pattern": "^[a-z]+(-[a-z0-9]+)*$",
    "declaration-block-no-duplicate-properties": true,
    "no-descending-specificity": true,
    "color-no-invalid-hex": true
  }
}
```

**JavaScript Linting:**

JavaScript must pass ESLint with the project configuration:

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": ["eslint:recommended"],
  "rules": {
    "no-unused-vars": "error",
    "no-undef": "error",
    "eqeqeq": "error",
    "no-console": ["warn", { "allow": ["warn", "error"] }]
  }
}
```

**Accessibility Auditing:**

Automated accessibility testing using axe-core:

```javascript
// Run axe-core on each component page
const { AxeBuilder } = require('@axe-core/playwright');

test('component page has no accessibility violations', async ({ page }) => {
  await page.goto('/components/buttons.html');
  const results = await new AxeBuilder({ page }).analyze();
  expect(results.violations).toEqual([]);
});
```

**Automated Testing Requirements:**

| Test Type | Tool | Frequency |
|-----------|------|-----------|
| HTML Validation | html-validate | Every commit |
| CSS Linting | Stylelint | Every commit |
| JS Linting | ESLint | Every commit |
| Accessibility Audit | axe-core | Every commit |
| Link Checking | linkinator | Weekly |
| Performance Audit | Lighthouse CI | Every release |

### 17.4 Cross-Browser Testing

**Required Browser Testing Matrix:**

| Browser | Versions | Platforms |
|---------|----------|-----------|
| Chrome | Latest, Latest-1 | Windows, macOS, Linux |
| Firefox | Latest, Latest-1 | Windows, macOS, Linux |
| Safari | Latest, Latest-1 | macOS, iOS |
| Edge | Latest, Latest-1 | Windows |
| Chrome Mobile | Latest | Android |
| Safari Mobile | Latest | iOS |

**Cross-Browser Test Checklist:**

- [ ] Visual rendering matches reference
- [ ] All interactions function correctly
- [ ] JavaScript executes without errors
- [ ] CSS custom properties apply correctly
- [ ] Focus states display correctly
- [ ] Animations perform smoothly
- [ ] No console errors or warnings

**Testing Methodology:**

1. Primary testing performed in Chrome (development browser)
2. Secondary testing in Firefox and Safari before PR merge
3. Full browser matrix testing before each release
4. Critical bug fixes tested in all supported browsers

### 17.5 Definition of Done

A component is considered "done" and ready for release when all of the following criteria are met:

**Code Completeness:**
- [ ] All planned variants are implemented
- [ ] All states are implemented (default, hover, focus, active, disabled)
- [ ] Light and dark theme styles are complete
- [ ] Responsive behavior is implemented
- [ ] JavaScript functionality is complete (if applicable)

**Quality Assurance:**
- [ ] HTML validates with zero errors
- [ ] CSS passes linting with zero errors
- [ ] JavaScript passes linting with zero errors
- [ ] axe-core reports zero accessibility violations
- [ ] Manual accessibility testing completed
- [ ] Cross-browser testing completed (Chrome, Firefox, Safari)
- [ ] Manual testing checklist completed

**Documentation:**
- [ ] Component documentation page created
- [ ] All variants documented with examples
- [ ] All states documented
- [ ] Accessibility notes documented
- [ ] Keyboard interactions documented
- [ ] Copy-paste snippets provided
- [ ] Related components linked

**Code Review:**
- [ ] Pull request reviewed by at least one other contributor
- [ ] All review feedback addressed
- [ ] CHANGELOG entry added

---

## 18. Documentation and Component Authoring Workflow

### 18.1 Component Documentation Template

Every component documentation page must follow this standard template:

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Component Name] - DCMT Components</title>
  <link rel="stylesheet" href="../css/styles.css">
</head>
<body>
  <!-- Skip Link -->
  <a href="#main-content" class="skip-link">Skip to main content</a>

  <!-- Navigation -->
  <!-- ... -->

  <main id="main-content" class="container">
    <!-- Component Header -->
    <header class="component-header">
      <h1 class="component-header__title">[Component Name]</h1>
      <p class="component-header__description">
        [One to two sentence description of the component's purpose]
      </p>
      <div class="component-header__meta">
        <span class="badge">Category: [Category]</span>
        <span class="badge badge--outline">Since: v[X.Y.Z]</span>
      </div>
    </header>

    <!-- Table of Contents -->
    <nav class="toc" aria-label="Table of Contents">
      <h2 class="toc__heading">On this page</h2>
      <ul class="toc__list">
        <li><a href="#overview">Overview</a></li>
        <li><a href="#anatomy">Anatomy</a></li>
        <li><a href="#variants">Variants</a></li>
        <li><a href="#states">States</a></li>
        <li><a href="#accessibility">Accessibility</a></li>
        <li><a href="#usage">Usage Guidelines</a></li>
        <li><a href="#examples">Examples</a></li>
        <li><a href="#api">API Reference</a></li>
      </ul>
    </nav>

    <!-- Overview Section -->
    <section id="overview" class="doc-section">
      <h2>Overview</h2>
      <p>[Detailed description of when and why to use this component]</p>
      
      <!-- Basic Example -->
      <div class="example">
        <div class="example__preview">
          <!-- Live component -->
        </div>
        <div class="example__code">
          <div class="code-block">
            <header class="code-block__header">
              <span class="code-block__lang">html</span>
              <button type="button" class="code-block__copy" data-copy>Copy</button>
            </header>
            <pre><code><!-- HTML code --></code></pre>
          </div>
        </div>
      </div>
    </section>

    <!-- Anatomy Section -->
    <section id="anatomy" class="doc-section">
      <h2>Anatomy</h2>
      <p>[Description of the component's structural parts]</p>
      
      <!-- Anatomy diagram or annotated example -->
      <figure class="figure">
        <div class="example__preview">
          <!-- Annotated component showing parts -->
        </div>
        <figcaption class="figure__caption">
          Component anatomy showing key structural elements
        </figcaption>
      </figure>

      <!-- Parts table -->
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Part</th>
            <th scope="col">Class</th>
            <th scope="col">Description</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Container</td>
            <td><code>[component]</code></td>
            <td>[Description]</td>
          </tr>
          <!-- Additional parts -->
        </tbody>
      </table>
    </section>

    <!-- Variants Section -->
    <section id="variants" class="doc-section">
      <h2>Variants</h2>
      
      <!-- For each variant -->
      <h3>[Variant Name]</h3>
      <p>[When to use this variant]</p>
      
      <div class="example">
        <div class="example__preview">
          <!-- Variant example -->
        </div>
        <div class="example__code">
          <div class="code-block">
            <pre><code><!-- Variant HTML --></code></pre>
          </div>
        </div>
      </div>
    </section>

    <!-- States Section -->
    <section id="states" class="doc-section">
      <h2>States</h2>
      
      <div class="states-grid">
        <!-- Default State -->
        <div class="state-example">
          <h4>Default</h4>
          <!-- Component in default state -->
        </div>
        
        <!-- Hover State -->
        <div class="state-example">
          <h4>Hover</h4>
          <!-- Component with hover class forced -->
        </div>
        
        <!-- Focus State -->
        <div class="state-example">
          <h4>Focus</h4>
          <!-- Component with focus class forced -->
        </div>
        
        <!-- Active State -->
        <div class="state-example">
          <h4>Active</h4>
          <!-- Component with active class forced -->
        </div>
        
        <!-- Disabled State -->
        <div class="state-example">
          <h4>Disabled</h4>
          <!-- Component in disabled state -->
        </div>
      </div>
    </section>

    <!-- Accessibility Section -->
    <section id="accessibility" class="doc-section">
      <h2>Accessibility</h2>
      
      <h3>ARIA Attributes</h3>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Attribute</th>
            <th scope="col">Element</th>
            <th scope="col">Usage</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><code>[attribute]</code></td>
            <td>[element]</td>
            <td>[description]</td>
          </tr>
        </tbody>
      </table>

      <h3>Keyboard Interactions</h3>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Key</th>
            <th scope="col">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><kbd>Tab</kbd></td>
            <td>[action description]</td>
          </tr>
        </tbody>
      </table>

      <h3>Screen Reader Behavior</h3>
      <p>[Description of how the component is announced and interacted with via screen reader]</p>
    </section>

    <!-- Usage Guidelines Section -->
    <section id="usage" class="doc-section">
      <h2>Usage Guidelines</h2>
      
      <h3>When to use</h3>
      <ul class="list">
        <li>[Use case 1]</li>
        <li>[Use case 2]</li>
      </ul>

      <h3>When not to use</h3>
      <ul class="list">
        <li>[Anti-pattern 1]</li>
        <li>[Anti-pattern 2]</li>
      </ul>

      <h3>Best Practices</h3>
      <div class="callout callout--tip">
        <p>[Best practice recommendation]</p>
      </div>

      <h3>Common Mistakes</h3>
      <div class="callout callout--warning">
        <p>[Common mistake to avoid]</p>
      </div>
    </section>

    <!-- Examples Section -->
    <section id="examples" class="doc-section">
      <h2>Examples</h2>
      
      <h3>[Example Name]</h3>
      <p>[Example description and context]</p>
      
      <div class="example">
        <div class="example__preview">
          <!-- Full example -->
        </div>
        <div class="example__code">
          <div class="code-block">
            <pre><code><!-- Example HTML --></code></pre>
          </div>
        </div>
      </div>
    </section>

    <!-- API Reference Section -->
    <section id="api" class="doc-section">
      <h2>API Reference</h2>
      
      <h3>CSS Classes</h3>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Class</th>
            <th scope="col">Description</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><code>.[class-name]</code></td>
            <td>[description]</td>
          </tr>
        </tbody>
      </table>

      <h3>CSS Custom Properties</h3>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Property</th>
            <th scope="col">Default</th>
            <th scope="col">Description</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><code>--[property-name]</code></td>
            <td><code>[default value]</code></td>
            <td>[description]</td>
          </tr>
        </tbody>
      </table>

      <h3>JavaScript API</h3>
      <!-- If applicable -->
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Method</th>
            <th scope="col">Parameters</th>
            <th scope="col">Description</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><code>DCMT.[component].[method]()</code></td>
            <td><code>[params]</code></td>
            <td>[description]</td>
          </tr>
        </tbody>
      </table>
    </section>

    <!-- Related Components -->
    <section class="doc-section">
      <h2>Related Components</h2>
      <ul class="list">
        <li><a href="[url]">[Related Component 1]</a> - [relationship]</li>
        <li><a href="[url]">[Related Component 2]</a> - [relationship]</li>
      </ul>
    </section>
  </main>

  <!-- Footer -->
  <!-- ... -->

  <script src="../js/main.js"></script>
</body>
</html>
```

### 18.2 Live Demo and Copy Button Rules

**Live Demo Requirements:**

1. Every code example must have a corresponding live preview
2. The live preview must render the exact code shown in the code block
3. The preview must be interactive where applicable (hover states, click actions)
4. A theme toggle must be available to preview components in both themes
5. Viewport width controls should be available for responsive components

**Copy Button Implementation:**

```html
<div class="example">
  <div class="example__preview" data-preview>
    <!-- Live rendered component here -->
    <button class="btn btn--primary">Example Button</button>
  </div>
  
  <div class="example__toolbar">
    <button type="button" class="example__theme-toggle" data-toggle-preview-theme aria-label="Toggle preview theme">
      Toggle Theme
    </button>
  </div>
  
  <div class="example__source">
    <div class="example__tabs" role="tablist">
      <button role="tab" aria-selected="true" aria-controls="html-panel" id="html-tab">HTML</button>
      <button role="tab" aria-selected="false" aria-controls="css-panel" id="css-tab" tabindex="-1">CSS</button>
    </div>
    
    <div class="example__panel" role="tabpanel" id="html-panel" aria-labelledby="html-tab">
      <div class="code-block">
        <header class="code-block__header">
          <button type="button" class="code-block__copy" data-copy aria-label="Copy HTML code">
            <span class="code-block__copy-text">Copy</span>
            <span class="code-block__copy-success" hidden>Copied</span>
          </button>
        </header>
        <pre><code class="language-html">&lt;button class="btn btn--primary"&gt;Example Button&lt;/button&gt;</code></pre>
      </div>
    </div>
    
    <div class="example__panel" role="tabpanel" id="css-panel" aria-labelledby="css-tab" hidden>
      <!-- CSS code if component-specific CSS is needed -->
    </div>
  </div>
</div>
```

**Copy Button Behavior Rules:**

1. Copy button must copy only the relevant code, no extra whitespace
2. Copy must preserve indentation appropriate for pasting
3. Visual feedback ("Copied") must display for 2 seconds
4. Success state must be announced to screen readers
5. Copy button must be keyboard accessible
6. Failed copy attempts must show error state

**Copy Snippet Format:**

Copied code must be self-contained and include a comment header:

```html
<!-- DCMT Component: Button (Primary) -->
<!-- Requires: styles.css -->
<button type="button" class="btn btn--primary">
  Button Text
</button>
```

### 18.3 Naming and File Organization

**Documentation File Naming:**

```
/docs/
├── getting-started.html     # Lowercase, hyphen-separated
├── theming.html
├── accessibility.html
└── customization.html

/components/
├── index.html               # Component catalog
├── buttons.html             # Plural for component categories
├── forms.html
├── modals.html
├── navigation.html
├── tables.html
└── utilities.html
```

**Naming Conventions:**

| Item | Convention | Example |
|------|------------|---------|
| HTML files | lowercase, hyphen-separated | `getting-started.html` |
| Section IDs | lowercase, hyphen-separated | `id="button-variants"` |
| Component headings | Title Case | "Primary Button" |
| Code examples | lowercase class names | `class="btn btn--primary"` |

**Documentation Page URL Structure:**

```
/                           # Home
/docs/getting-started       # Getting started guide
/docs/theming               # Theming documentation
/components/                # Component index
/components/buttons         # Button component docs
/examples/dashboard         # Example layouts
```

**Section ID Conventions:**

- Use descriptive, unique IDs
- Prefix with component name for component-specific sections
- Use consistent patterns across all documentation

```html
<!-- Good -->
<section id="button-variants">
<section id="button-states">
<section id="button-accessibility">

<!-- Avoid -->
<section id="variants">        <!-- Too generic -->
<section id="section-3">       <!-- Not descriptive -->
<section id="buttonVariants">  <!-- Not hyphenated -->
```

### 18.4 Documentation Requirement for New Components

**Mandatory Documentation Rule:**

No component may be merged into the main branch without complete documentation. The documentation must be included in the same pull request as the component code.

**Documentation Checklist for New Components:**

```markdown
## Documentation Checklist

### Page Structure
- [ ] Documentation page created at correct path
- [ ] Page follows standard template structure
- [ ] Page includes skip link and navigation
- [ ] Table of contents links to all sections

### Content Sections
- [ ] Overview with basic example
- [ ] Anatomy diagram/description
- [ ] All variants documented with examples
- [ ] All states documented with examples
- [ ] Accessibility section complete
- [ ] Usage guidelines (when to use, when not to use)
- [ ] At least two real-world examples
- [ ] API reference (classes, custom properties, JS methods)
- [ ] Related components linked

### Code Examples
- [ ] Every example has live preview
- [ ] Every example has copy button
- [ ] HTML code is complete and copy-paste ready
- [ ] CSS shown when component-specific styles needed
- [ ] JavaScript shown when required

### Quality
- [ ] Documentation page validates (HTML)
- [ ] All links work
- [ ] All examples render correctly
- [ ] Copy buttons function correctly
- [ ] Theme toggle works on examples
```

---

## 19. Contribution and Code Review Process

### 19.1 Branch Naming Conventions

All branches must follow this naming pattern:

```
[type]/[issue-number]-[short-description]
```

**Branch Types:**

| Type | Usage |
|------|-------|
| `feature/` | New components or features |
| `fix/` | Bug fixes |
| `docs/` | Documentation changes only |
| `refactor/` | Code refactoring without behavior change |
| `chore/` | Build, tooling, or maintenance tasks |
| `a11y/` | Accessibility improvements |

**Examples:**

```
feature/142-drawer-component
fix/156-modal-focus-trap
docs/160-button-accessibility-notes
refactor/163-consolidate-spacing-tokens
chore/165-update-linting-config
a11y/168-improve-toast-announcements
```

**Rules:**

1. Branch names must be lowercase
2. Use hyphens to separate words
3. Include issue number when applicable
4. Keep descriptions concise (3-5 words)
5. No special characters except hyphens

### 19.2 Pull Request Checklist

Every pull request must include the following completed checklist in the PR description:

```markdown
## Pull Request

### Description
[Clear description of what this PR does]

### Related Issues
Closes #[issue-number]

### Type of Change
- [ ] New component
- [ ] Component enhancement
- [ ] Bug fix
- [ ] Documentation update
- [ ] Accessibility improvement
- [ ] Refactoring
- [ ] Build/tooling

### Checklist

#### Code Quality
- [ ] Code follows BEM naming conventions
- [ ] CSS uses only design system tokens (no magic numbers)
- [ ] JavaScript follows project ESLint rules
- [ ] No console.log statements (except warnings/errors)
- [ ] Code is commented where necessary

#### Testing
- [ ] HTML validates with zero errors
- [ ] CSS passes Stylelint with zero errors
- [ ] JavaScript passes ESLint with zero errors
- [ ] axe-core reports zero accessibility violations
- [ ] Tested in Chrome (latest)
- [ ] Tested in Firefox (latest)
- [ ] Tested in Safari (latest)
- [ ] Keyboard navigation tested
- [ ] Screen reader tested

#### Theming
- [ ] Works correctly in light theme
- [ ] Works correctly in dark theme
- [ ] Respects prefers-reduced-motion

#### Responsive
- [ ] Works at 320px viewport width
- [ ] Works at 768px viewport width
- [ ] Works at 1440px viewport width

#### Documentation
- [ ] Documentation page created/updated
- [ ] All variants documented with examples
- [ ] Accessibility notes included
- [ ] Copy-paste snippets work standalone
- [ ] CHANGELOG.md updated

#### Breaking Changes
- [ ] No breaking changes
- [ ] Breaking changes documented in PR description
- [ ] Migration path documented

### Screenshots
[Include before/after screenshots for visual changes]

### Testing Instructions
[Steps for reviewers to test the changes]
```

### 19.3 Code Style and Naming Consistency

**CSS Style Rules:**

```css
/* Component block comment */
/* ==========================================================================
   Component: Component Name
   ========================================================================== */

/**
 * Description of the component
 *
 * 1. Explanation for non-obvious property
 * 2. Another explanation
 */

/* Block */
.component {
  /* Positioning */
  position: relative;
  
  /* Box model */
  display: flex;
  padding: var(--space-4);
  
  /* Typography */
  font-family: var(--font-mono);
  font-size: var(--text-base);
  
  /* Visual */
  background-color: var(--surface-background);
  border: 1px solid var(--border-default);
  border-radius: var(--radius-md);
  
  /* Animation */
  transition: background-color 0.15s ease;
}

/* Element */
.component__element {
  /* styles */
}

/* Modifier */
.component--modifier {
  /* styles */
}

/* State (using is- prefix) */
.component.is-active {
  /* styles */
}

/* Pseudo-states */
.component:hover {
  /* styles */
}

.component:focus-visible {
  /* styles */
}

.component:disabled,
.component.is-disabled {
  /* styles */
}

/* Responsive */
@media (min-width: 768px) {
  .component {
    /* responsive overrides */
  }
}
```

**JavaScript Style Rules:**

```javascript
/* ==========================================================================
   Component: Component Name
   ========================================================================== */

/**
 * Initialize component functionality
 * @param {HTMLElement} element - Root element
 * @param {Object} options - Configuration options
 * @returns {Object} Component instance
 */
function initComponent(element, options = {}) {
  // Early return if invalid
  if (!element) {
    console.warn('Component: Element not found');
    return null;
  }

  // Default options
  const defaults = {
    option1: true,
    option2: 'value'
  };
  
  const settings = Object.assign({}, defaults, options);
  
  // Private variables
  let isInitialized = false;
  let isOpen = false;
  
  // DOM references
  const trigger = element.querySelector('[data-trigger]');
  const content = element.querySelector('[data-content]');
  
  // Private functions
  function handleClick(event) {
    event.preventDefault();
    toggle();
  }
  
  function handleKeydown(event) {
    if (event.key === 'Escape') {
      close();
    }
  }
  
  function open() {
    if (isOpen) return;
    
    isOpen = true;
    element.classList.add('is-open');
    trigger.setAttribute('aria-expanded', 'true');
    content.hidden = false;
  }
  
  function close() {
    if (!isOpen) return;
    
    isOpen = false;
    element.classList.remove('is-open');
    trigger.setAttribute('aria-expanded', 'false');
    content.hidden = true;
  }
  
  function toggle() {
    isOpen ? close() : open();
  }
  
  function bindEvents() {
    trigger.addEventListener('click', handleClick);
    document.addEventListener('keydown', handleKeydown);
  }
  
  function unbindEvents() {
    trigger.removeEventListener('click', handleClick);
    document.removeEventListener('keydown', handleKeydown);
  }
  
  function init() {
    if (isInitialized) return;
    
    bindEvents();
    isInitialized = true;
  }
  
  function destroy() {
    unbindEvents();
    isInitialized = false;
  }
  
  // Initialize
  init();
  
  // Public API
  return {
    open,
    close,
    toggle,
    destroy
  };
}
```

**Naming Consistency Requirements:**

| Item | Convention | Example |
|------|------------|---------|
| CSS block class | lowercase, hyphen-separated | `.modal-dialog` |
| CSS element class | BEM element notation | `.modal-dialog__header` |
| CSS modifier class | BEM modifier notation | `.modal-dialog--large` |
| CSS state class | `is-` or `has-` prefix | `.is-open`, `.has-error` |
| CSS custom property | `--category-property` | `--color-primary` |
| JS function | camelCase | `initModal()` |
| JS constant | SCREAMING_SNAKE_CASE | `DEFAULT_OPTIONS` |
| JS private variable | camelCase | `isInitialized` |
| Data attributes | `data-component-action` | `data-modal-open` |
| ID attributes | lowercase, hyphen-separated | `id="main-content"` |

### 19.4 Review and Approval Flow

**Review Requirements:**

| Change Type | Minimum Reviewers | Required Expertise |
|-------------|-------------------|-------------------|
| New component | 2 | 1 CSS/HTML, 1 Accessibility |
| Component modification | 1 | CSS/HTML or JS as applicable |
| Bug fix | 1 | Area of expertise |
| Documentation only | 1 | Any maintainer |
| Breaking change | 2 | 1 Maintainer, 1 Core contributor |

**Review Process:**

1. **Submission**: Author creates PR with completed checklist
2. **Automated Checks**: CI runs linting, validation, accessibility audits
3. **Initial Review**: Reviewer verifies checklist completion
4. **Code Review**: Detailed review of code quality and conventions
5. **Testing Review**: Reviewer tests functionality in browsers
6. **Accessibility Review**: Keyboard and screen reader testing
7. **Documentation Review**: Verify documentation completeness
8. **Approval**: Required approvals obtained
9. **Merge**: Squash and merge to main branch

**Review Criteria:**

| Aspect | Passing | Needs Work |
|--------|---------|------------|
| Naming | Follows BEM, consistent with codebase | Deviates from conventions |
| Tokens | Uses only design system tokens | Magic numbers or hardcoded values |
| Accessibility | Passes automated checks, keyboard works | Violations or keyboard issues |
| Documentation | Complete per template | Missing sections or examples |
| Testing | All checklist items verified | Incomplete testing |
| Code Quality | Clean, commented, maintainable | Complex, unclear, redundant |

**Feedback Guidelines:**

- Be specific and actionable
- Reference documentation or examples
- Distinguish between required changes and suggestions
- Use conventional comments: `nit:`, `suggestion:`, `required:`

```markdown
required: This button is missing `type="button"` which may cause unintended form submission.

suggestion: Consider using `aria-describedby` here to associate the helper text with the input.

nit: Extra whitespace on line 45.
```

### 19.5 Documentation and Examples Update Requirement

**Rule**: Every pull request that modifies component behavior, appearance, or API must include corresponding documentation updates in the same PR.

**Specific Requirements:**

**For New Components:**
- Complete documentation page following template
- All variants and states documented
- Accessibility section complete
- At least two usage examples
- Entry in component index page
- CHANGELOG entry

**For Component Modifications:**
- Updated documentation reflecting changes
- Updated code examples if syntax changed
- Updated accessibility notes if ARIA changed
- Migration notes if behavior changed
- CHANGELOG entry

**For Bug Fixes:**
- Documentation correction if fix reveals documentation error
- CHANGELOG entry

**For Deprecations:**
- Deprecation notice added to documentation
- Migration guide provided
- CHANGELOG entry in "Deprecated" section

**Enforcement:**

Pull requests will not be merged if:
- New component lacks documentation page
- Modified component has outdated documentation
- Code examples don't match current implementation
- CHANGELOG entry is missing

---

## 20. Starter Templates and Reference Layouts

### 20.1 Template Requirements

All starter templates must adhere to the following requirements:

1. **Self-Contained**: Each template must be a single HTML file that works when opened directly in a browser
2. **Component-Only**: Templates must use only documented component classes; no custom one-off styles
3. **Copy-Paste Ready**: Templates must be functional when copied into any project with the CSS/JS files
4. **Fully Responsive**: Templates must work correctly from 320px to 1440px viewport widths
5. **Accessible**: Templates must pass accessibility audits with zero violations
6. **Theme-Compatible**: Templates must work correctly in both light and dark themes
7. **Documented**: Each template must include inline comments explaining the structure

### 20.2 Basic Page Layout Template

**Purpose**: Foundation template for simple pages with header, main content, and footer.

**File**: `/examples/basic-page.html`

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Basic Page Layout - DCMT Template</title>
  <link rel="stylesheet" href="../css/styles.css">
</head>
<body>
  <!--
    DCMT Template: Basic Page Layout
    
    Structure:
    - Skip link for accessibility
    - Navbar with navigation and theme toggle
    - Main content area with container
    - Page footer
    
    Customize by replacing placeholder content.
  -->

  <!-- Skip Link -->
  <a href="#main-content" class="skip-link">Skip to main content</a>

  <!-- Navbar -->
  <header class="navbar">
    <div class="container">
      <div class="navbar__brand">
        <a href="/" class="navbar__logo">Site Name</a>
      </div>
      <nav class="navbar__nav" aria-label="Main navigation">
        <ul class="navbar__menu">
          <li class="navbar__item">
            <a href="#" class="navbar__link navbar__link--active" aria-current="page">Home</a>
          </li>
          <li class="navbar__item">
            <a href="#" class="navbar__link">About</a>
          </li>
          <li class="navbar__item">
            <a href="#" class="navbar__link">Contact</a>
          </li>
        </ul>
      </nav>
      <div class="navbar__actions">
        <button type="button" class="btn btn--ghost btn--sm" data-theme-toggle aria-label="Toggle theme">
          Theme
        </button>
      </div>
    </div>
  </header>

  <!-- Main Content -->
  <main id="main-content" tabindex="-1">
    <div class="container py-8">
      <!-- Page Header -->
      <header class="section section--compact">
        <h1>Page Title</h1>
        <p class="text-secondary">
          Brief description of this page's content and purpose.
        </p>
      </header>

      <!-- Content Section -->
      <section class="section">
        <h2>Section Heading</h2>
        <p>
          This is a basic page layout template. Replace this content with your 
          actual page content. The layout uses the container component for 
          consistent max-width and padding.
        </p>
        
        <div class="stack stack--gap-md mt-6">
          <div class="card">
            <div class="card__body">
              <h3 class="card__title">Card Title</h3>
              <p>Card content goes here. Cards are useful for grouping related content.</p>
            </div>
            <footer class="card__footer">
              <button type="button" class="btn btn--primary btn--sm">Action</button>
            </footer>
          </div>
          
          <div class="card">
            <div class="card__body">
              <h3 class="card__title">Another Card</h3>
              <p>More content in another card component.</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Call to Action -->
      <section class="section">
        <div class="card bg-elevated">
          <div class="card__body text-center">
            <h2>Ready to get started?</h2>
            <p class="text-secondary mb-4">
              Join thousands of developers using these components.
            </p>
            <div class="cluster cluster--justify-center">
              <button type="button" class="btn btn--primary">Get Started</button>
              <button type="button" class="btn btn--outline">Learn More</button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>

  <!-- Footer -->
  <footer class="footer">
    <div class="container">
      <div class="footer__content">
        <p class="footer__copyright">
          Copyright 2025. All rights reserved.
        </p>
        <nav aria-label="Footer navigation">
          <ul class="cluster cluster--gap-md">
            <li><a href="#" class="footer__link">Privacy</a></li>
            <li><a href="#" class="footer__link">Terms</a></li>
            <li><a href="#" class="footer__link">Contact</a></li>
          </ul>
        </nav>
      </div>
    </div>
  </footer>

  <script src="../js/main.js"></script>
</body>
</html>
```

### 20.3 Dashboard Layout Template

**Purpose**: Admin dashboard layout with sidebar navigation, header, and content panels.

**File**: `/examples/dashboard.html`

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard Layout - DCMT Template</title>
  <link rel="stylesheet" href="../css/styles.css">
</head>
<body>
  <!--
    DCMT Template: Dashboard Layout
    
    Structure:
    - Sidebar navigation (collapsible on mobile)
    - Top header with search and user menu
    - Main content area with stats and data table
    - Toast container for notifications
    
    This template demonstrates:
    - Sidebar layout component
    - Stats cards
    - Data tables
    - Dropdown menus
    - Alert components
  -->

  <!-- Skip Link -->
  <a href="#main-content" class="skip-link">Skip to main content</a>

  <!-- Dashboard Layout -->
  <div class="sidebar-layout">
    <!-- Sidebar -->
    <aside class="sidebar-layout__sidebar">
      <div class="sidenav">
        <!-- Logo -->
        <div class="sidenav__header">
          <a href="/" class="sidenav__logo">Dashboard</a>
        </div>

        <!-- Navigation -->
        <nav aria-label="Dashboard navigation">
          <div class="sidenav__section">
            <h4 class="sidenav__heading">Main</h4>
            <ul class="sidenav__list">
              <li class="sidenav__item">
                <a href="#" class="sidenav__link sidenav__link--active" aria-current="page">
                  Overview
                </a>
              </li>
              <li class="sidenav__item">
                <a href="#" class="sidenav__link">Analytics</a>
              </li>
              <li class="sidenav__item">
                <a href="#" class="sidenav__link">Reports</a>
              </li>
            </ul>
          </div>

          <div class="sidenav__section">
            <h4 class="sidenav__heading">Management</h4>
            <ul class="sidenav__list">
              <li class="sidenav__item">
                <a href="#" class="sidenav__link">Users</a>
              </li>
              <li class="sidenav__item">
                <a href="#" class="sidenav__link">Settings</a>
              </li>
            </ul>
          </div>
        </nav>

        <!-- Sidebar Footer -->
        <div class="sidenav__footer">
          <button type="button" class="btn btn--ghost btn--sm w-full" data-theme-toggle>
            Toggle Theme
          </button>
        </div>
      </div>
    </aside>

    <!-- Main Content Area -->
    <div class="sidebar-layout__main">
      <!-- Top Header -->
      <header class="navbar navbar--bordered">
        <div class="navbar__content">
          <!-- Mobile Menu Toggle -->
          <button type="button" class="btn btn--ghost btn--icon navbar__toggle" aria-label="Toggle sidebar" data-sidebar-toggle>
            <span aria-hidden="true">☰</span>
          </button>

          <!-- Search -->
          <div class="search search--compact">
            <label for="dashboard-search" class="visually-hidden">Search</label>
            <input type="search" id="dashboard-search" class="search__input" placeholder="Search...">
          </div>

          <!-- User Menu -->
          <div class="dropdown">
            <button type="button" class="btn btn--ghost" aria-haspopup="menu" aria-expanded="false">
              <span class="avatar avatar--sm avatar--initials">JD</span>
              <span class="ml-2">John Doe</span>
            </button>
            <div class="dropdown__menu" role="menu" hidden>
              <a href="#" class="dropdown__item" role="menuitem">Profile</a>
              <a href="#" class="dropdown__item" role="menuitem">Settings</a>
              <hr class="dropdown__divider" role="separator">
              <button type="button" class="dropdown__item" role="menuitem">Sign Out</button>
            </div>
          </div>
        </div>
      </header>

      <!-- Page Content -->
      <main id="main-content" tabindex="-1" class="p-6">
        <!-- Page Header -->
        <header class="mb-6">
          <h1>Dashboard Overview</h1>
          <p class="text-secondary">Welcome back. Here's what's happening today.</p>
        </header>

        <!-- Alert -->
        <div class="alert alert--info alert--dismissible mb-6" role="alert">
          <div class="alert__content">
            <p class="alert__message">
              New features are available. 
              <a href="#">Learn more about the latest updates</a>.
            </p>
          </div>
          <button type="button" class="alert__close" aria-label="Dismiss">×</button>
        </div>

        <!-- Stats Grid -->
        <div class="grid grid--cols-4 gap-4 mb-6">
          <div class="stat card">
            <span class="stat__label">Total Users</span>
            <span class="stat__value">12,345</span>
            <span class="stat__change stat__change--positive">+12.5%</span>
          </div>
          <div class="stat card">
            <span class="stat__label">Revenue</span>
            <span class="stat__value">$98,765</span>
            <span class="stat__change stat__change--positive">+8.2%</span>
          </div>
          <div class="stat card">
            <span class="stat__label">Orders</span>
            <span class="stat__value">1,234</span>
            <span class="stat__change stat__change--negative">-3.1%</span>
          </div>
          <div class="stat card">
            <span class="stat__label">Conversion</span>
            <span class="stat__value">3.24%</span>
            <span class="stat__change stat__change--positive">+0.5%</span>
          </div>
        </div>

        <!-- Recent Activity Table -->
        <section class="card">
          <header class="card__header">
            <h2 class="card__title">Recent Activity</h2>
            <div class="dropdown">
              <button type="button" class="btn btn--ghost btn--sm" aria-haspopup="menu" aria-expanded="false">
                Actions
              </button>
              <div class="dropdown__menu" role="menu" hidden>
                <button type="button" class="dropdown__item" role="menuitem">Export CSV</button>
                <button type="button" class="dropdown__item" role="menuitem">Print</button>
              </div>
            </div>
          </header>
          <div class="card__body p-0">
            <div class="table-container">
              <table class="table table--hover">
                <thead>
                  <tr>
                    <th scope="col">User</th>
                    <th scope="col">Action</th>
                    <th scope="col">Date</th>
                    <th scope="col">Status</th>
                    <th scope="col"><span class="visually-hidden">Actions</span></th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>
                      <div class="cluster cluster--gap-sm">
                        <span class="avatar avatar--sm avatar--initials">AS</span>
                        <span>Alice Smith</span>
                      </div>
                    </td>
                    <td>Created new project</td>
                    <td><time datetime="2025-01-15">Jan 15, 2025</time></td>
                    <td><span class="badge badge--success">Complete</span></td>
                    <td>
                      <button type="button" class="btn btn--ghost btn--sm">View</button>
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <div class="cluster cluster--gap-sm">
                        <span class="avatar avatar--sm avatar--initials">BJ</span>
                        <span>Bob Johnson</span>
                      </div>
                    </td>
                    <td>Updated settings</td>
                    <td><time datetime="2025-01-14">Jan 14, 2025</time></td>
                    <td><span class="badge badge--info">In Progress</span></td>
                    <td>
                      <button type="button" class="btn btn--ghost btn--sm">View</button>
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <div class="cluster cluster--gap-sm">
                        <span class="avatar avatar--sm avatar--initials">CW</span>
                        <span>Carol White</span>
                      </div>
                    </td>
                    <td>Deleted file</td>
                    <td><time datetime="2025-01-14">Jan 14, 2025</time></td>
                    <td><span class="badge badge--warning">Pending</span></td>
                    <td>
                      <button type="button" class="btn btn--ghost btn--sm">View</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
          <footer class="card__footer">
            <nav class="pagination" aria-label="Table pagination">
              <button type="button" class="pagination__link pagination__link--prev" disabled aria-disabled="true">
                Previous
              </button>
              <span class="pagination__info">Showing 1-3 of 50 results</span>
              <a href="#" class="pagination__link pagination__link--next">Next</a>
            </nav>
          </footer>
        </section>
      </main>
    </div>
  </div>

  <!-- Toast Container -->
  <div class="toast-container toast-container--bottom-right" aria-live="polite" aria-atomic="true">
    <!-- Toasts inserted here dynamically -->
  </div>

  <script src="../js/main.js"></script>
</body>
</html>
```

### 20.4 Form-Heavy Page Template

**Purpose**: Page layout optimized for forms with validation, field groups, and multi-step patterns.

**File**: `/examples/form-page.html`

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Form Page Layout - DCMT Template</title>
  <link rel="stylesheet" href="../css/styles.css">
</head>
<body>
  <!--
    DCMT Template: Form-Heavy Page
    
    Structure:
    - Navbar with minimal navigation
    - Centered form container
    - Form with multiple sections/fieldsets
    - Validation states
    - Progress indicator
    
    This template demonstrates:
    - All form input types
    - Field grouping with fieldsets
    - Inline validation
    - Form actions
    - Progress/step indicator
  -->

  <!-- Skip Link -->
  <a href="#main-content" class="skip-link">Skip to main content</a>

  <!-- Navbar -->
  <header class="navbar">
    <div class="container">
      <div class="navbar__brand">
        <a href="/" class="navbar__logo">FormApp</a>
      </div>
      <div class="navbar__actions">
        <button type="button" class="btn btn--ghost btn--sm" data-theme-toggle>
          Theme
        </button>
      </div>
    </div>
  </header>

  <!-- Main Content -->
  <main id="main-content" tabindex="-1">
    <div class="container container--sm py-8">
      <!-- Page Header -->
      <header class="text-center mb-8">
        <h1>Create Account</h1>
        <p class="text-secondary">Fill out the form below to create your account.</p>
      </header>

      <!-- Progress Indicator -->
      <div class="progress progress--steps mb-8" role="progressbar" aria-valuenow="1" aria-valuemin="1" aria-valuemax="3" aria-label="Form progress">
        <div class="progress__step progress__step--current" aria-current="step">
          <span class="progress__step-number">1</span>
          <span class="progress__step-label">Account</span>
        </div>
        <div class="progress__step">
          <span class="progress__step-number">2</span>
          <span class="progress__step-label">Profile</span>
        </div>
        <div class="progress__step">
          <span class="progress__step-number">3</span>
          <span class="progress__step-label">Confirm</span>
        </div>
      </div>

      <!-- Form -->
      <form class="card" novalidate>
        <div class="card__body">
          <!-- Account Information -->
          <fieldset class="form-group">
            <legend class="form-group__legend">Account Information</legend>
            
            <div class="stack stack--gap-md">
              <!-- Email -->
              <div class="form-field">
                <label for="email" class="form-field__label">
                  Email Address
                  <span class="form-field__required" aria-hidden="true">*</span>
                </label>
                <input 
                  type="email" 
                  id="email" 
                  name="email" 
                  class="form-field__input" 
                  placeholder="you@example.com"
                  required
                  aria-required="true"
                  autocomplete="email"
                >
                <span class="form-field__hint">We'll never share your email.</span>
              </div>

              <!-- Password -->
              <div class="form-field">
                <label for="password" class="form-field__label">
                  Password
                  <span class="form-field__required" aria-hidden="true">*</span>
                </label>
                <input 
                  type="password" 
                  id="password" 
                  name="password" 
                  class="form-field__input" 
                  required
                  aria-required="true"
                  aria-describedby="password-requirements"
                  autocomplete="new-password"
                  minlength="8"
                >
                <span id="password-requirements" class="form-field__hint">
                  Minimum 8 characters with at least one number.
                </span>
              </div>

              <!-- Confirm Password -->
              <div class="form-field">
                <label for="confirm-password" class="form-field__label">
                  Confirm Password
                  <span class="form-field__required" aria-hidden="true">*</span>
                </label>
                <input 
                  type="password" 
                  id="confirm-password" 
                  name="confirm-password" 
                  class="form-field__input" 
                  required
                  aria-required="true"
                  autocomplete="new-password"
                >
              </div>
            </div>
          </fieldset>

          <hr class="divider my-6">

          <!-- Profile Information -->
          <fieldset class="form-group">
            <legend class="form-group__legend">Profile Information</legend>
            
            <div class="stack stack--gap-md">
              <!-- Name Fields -->
              <div class="grid grid--cols-2 gap-4">
                <div class="form-field">
                  <label for="first-name" class="form-field__label">First Name</label>
                  <input 
                    type="text" 
                    id="first-name" 
                    name="first-name" 
                    class="form-field__input"
                    autocomplete="given-name"
                  >
                </div>
                <div class="form-field">
                  <label for="last-name" class="form-field__label">Last Name</label>
                  <input 
                    type="text" 
                    id="last-name" 
                    name="last-name" 
                    class="form-field__input"
                    autocomplete="family-name"
                  >
                </div>
              </div>

              <!-- Phone -->
              <div class="form-field">
                <label for="phone" class="form-field__label">Phone Number</label>
                <input 
                  type="tel" 
                  id="phone" 
                  name="phone" 
                  class="form-field__input" 
                  placeholder="(555) 555-5555"
                  autocomplete="tel"
                >
              </div>

              <!-- Country Select -->
              <div class="form-field">
                <label for="country" class="form-field__label">Country</label>
                <div class="form-field__select-wrapper">
                  <select id="country" name="country" class="form-field__select">
                    <option value="">Select a country</option>
                    <option value="us">United States</option>
                    <option value="ca">Canada</option>
                    <option value="uk">United Kingdom</option>
                    <option value="au">Australia</option>
                  </select>
                </div>
              </div>

              <!-- Bio -->
              <div class="form-field">
                <label for="bio" class="form-field__label">Bio</label>
                <textarea 
                  id="bio" 
                  name="bio" 
                  class="form-field__textarea" 
                  rows="3"
                  placeholder="Tell us about yourself..."
                ></textarea>
                <span class="form-field__hint">Optional. Maximum 500 characters.</span>
              </div>
            </div>
          </fieldset>

          <hr class="divider my-6">

          <!-- Preferences -->
          <fieldset class="form-group">
            <legend class="form-group__legend">Preferences</legend>
            
            <div class="stack stack--gap-md">
              <!-- Radio Group -->
              <fieldset class="form-field">
                <legend class="form-field__label">Account Type</legend>
                <div class="stack stack--gap-sm">
                  <label class="radio">
                    <input type="radio" name="account-type" value="personal" class="radio__input" checked>
                    <span class="radio__control" aria-hidden="true"></span>
                    <span class="radio__label">Personal</span>
                  </label>
                  <label class="radio">
                    <input type="radio" name="account-type" value="business" class="radio__input">
                    <span class="radio__control" aria-hidden="true"></span>
                    <span class="radio__label">Business</span>
                  </label>
                </div>
              </fieldset>

              <!-- Checkbox -->
              <label class="checkbox">
                <input type="checkbox" name="newsletter" class="checkbox__input">
                <span class="checkbox__control" aria-hidden="true"></span>
                <span class="checkbox__label">Subscribe to our newsletter</span>
              </label>

              <!-- Toggle -->
              <label class="toggle">
                <input type="checkbox" class="toggle__input" role="switch" aria-checked="false">
                <span class="toggle__track">
                  <span class="toggle__thumb"></span>
                </span>
                <span class="toggle__label">Enable two-factor authentication</span>
              </label>
            </div>
          </fieldset>

          <hr class="divider my-6">

          <!-- Terms -->
          <div class="form-field">
            <label class="checkbox">
              <input type="checkbox" name="terms" class="checkbox__input" required aria-required="true">
              <span class="checkbox__control" aria-hidden="true"></span>
              <span class="checkbox__label">
                I agree to the <a href="#">Terms of Service</a> and <a href="#">Privacy Policy</a>
              </span>
            </label>
          </div>
        </div>

        <!-- Form Actions -->
        <footer class="card__footer">
          <div class="cluster cluster--justify-between w-full">
            <button type="button" class="btn btn--ghost">Back</button>
            <div class="cluster">
              <button type="button" class="btn btn--secondary">Save Draft</button>
              <button type="submit" class="btn btn--primary">Create Account</button>
            </div>
          </div>
        </footer>
      </form>

      <!-- Error Example (hidden by default) -->
      <div class="form-field form-field--error mt-6" hidden>
        <label for="error-example" class="form-field__label">Field with Error</label>
        <input 
          type="email" 
          id="error-example" 
          class="form-field__input" 
          value="invalid-email"
          aria-invalid="true"
          aria-describedby="error-example-msg"
        >
        <span id="error-example-msg" class="form-field__error" role="alert">
          Please enter a valid email address.
        </span>
      </div>
    </div>
  </main>

  <!-- Footer -->
  <footer class="footer">
    <div class="container text-center">
      <p class="text-secondary text-sm">
        Need help? <a href="#">Contact support</a>
      </p>
    </div>
  </footer>

  <script src="../js/main.js"></script>
</body>
</html>
```

### 20.5 Data Table with Sidebar Template

**Purpose**: Layout for data-heavy interfaces with filterable sidebar and full-width table.

**File**: `/examples/data-table.html`

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Data Table Layout - DCMT Template</title>
  <link rel="stylesheet" href="../css/styles.css">
</head>
<body>
  <!--
    DCMT Template: Data Table with Sidebar
    
    Structure:
    - Navbar with breadcrumbs
    - Sidebar with filters
    - Main area with search, table, and pagination
    
    This template demonstrates:
    - Sidebar layout with filters
    - Search input
    - Sortable data table
    - Pagination
    - Bulk action toolbar
    - Empty state
  -->

  <!-- Skip Link -->
  <a href="#main-content" class="skip-link">Skip to main content</a>

  <!-- Navbar -->
  <header class="navbar">
    <div class="container container--full">
      <div class="navbar__brand">
        <a href="/" class="navbar__logo">DataApp</a>
      </div>
      <nav class="navbar__nav" aria-label="Main navigation">
        <ul class="navbar__menu">
          <li class="navbar__item">
            <a href="#" class="navbar__link">Dashboard</a>
          </li>
          <li class="navbar__item">
            <a href="#" class="navbar__link navbar__link--active" aria-current="page">Records</a>
          </li>
          <li class="navbar__item">
            <a href="#" class="navbar__link">Reports</a>
          </li>
        </ul>
      </nav>
      <div class="navbar__actions">
        <button type="button" class="btn btn--ghost btn--sm" data-theme-toggle>Theme</button>
      </div>
    </div>
  </header>

  <!-- Breadcrumbs -->
  <div class="bg-elevated border-b">
    <div class="container container--full py-2">
      <nav class="breadcrumbs" aria-label="Breadcrumb">
        <ol class="breadcrumbs__list">
          <li class="breadcrumbs__item">
            <a href="#" class="breadcrumbs__link">Home</a>
          </li>
          <li class="breadcrumbs__item">
            <a href="#" class="breadcrumbs__link">Records</a>
          </li>
          <li class="breadcrumbs__item">
            <span class="breadcrumbs__current" aria-current="page">All Records</span>
          </li>
        </ol>
      </nav>
    </div>
  </div>

  <!-- Main Layout -->
  <div class="sidebar-layout">
    <!-- Filters Sidebar -->
    <aside class="sidebar-layout__sidebar bg-elevated">
      <div class="p-4">
        <h2 class="text-lg font-semibold mb-4">Filters</h2>
        
        <!-- Filter Form -->
        <form class="stack stack--gap-md">
          <!-- Status Filter -->
          <fieldset class="form-field">
            <legend class="form-field__label">Status</legend>
            <div class="stack stack--gap-sm">
              <label class="checkbox">
                <input type="checkbox" name="status" value="active" class="checkbox__input" checked>
                <span class="checkbox__control" aria-hidden="true"></span>
                <span class="checkbox__label">Active</span>
              </label>
              <label class="checkbox">
                <input type="checkbox" name="status" value="pending" class="checkbox__input" checked>
                <span class="checkbox__control" aria-hidden="true"></span>
                <span class="checkbox__label">Pending</span>
              </label>
              <label class="checkbox">
                <input type="checkbox" name="status" value="inactive" class="checkbox__input">
                <span class="checkbox__control" aria-hidden="true"></span>
                <span class="checkbox__label">Inactive</span>
              </label>
            </div>
          </fieldset>

          <hr class="divider">

          <!-- Category Filter -->
          <div class="form-field">
            <label for="category" class="form-field__label">Category</label>
            <div class="form-field__select-wrapper">
              <select id="category" name="category" class="form-field__select">
                <option value="">All Categories</option>
                <option value="a">Category A</option>
                <option value="b">Category B</option>
                <option value="c">Category C</option>
              </select>
            </div>
          </div>

          <!-- Date Range -->
          <div class="form-field">
            <label for="date-from" class="form-field__label">Date From</label>
            <input type="date" id="date-from" name="date-from" class="form-field__input">
          </div>

          <div class="form-field">
            <label for="date-to" class="form-field__label">Date To</label>
            <input type="date" id="date-to" name="date-to" class="form-field__input">
          </div>

          <hr class="divider">

          <!-- Filter Actions -->
          <div class="cluster">
            <button type="submit" class="btn btn--primary btn--sm">Apply Filters</button>
            <button type="reset" class="btn btn--ghost btn--sm">Reset</button>
          </div>
        </form>
      </div>
    </aside>

    <!-- Main Content -->
    <main id="main-content" tabindex="-1" class="sidebar-layout__main">
      <div class="p-6">
        <!-- Page Header -->
        <header class="cluster cluster--justify-between mb-6">
          <div>
            <h1>Records</h1>
            <p class="text-secondary">Manage all records in the system.</p>
          </div>
          <button type="button" class="btn btn--primary">
            Add Record
          </button>
        </header>

        <!-- Toolbar -->
        <div class="card mb-4">
          <div class="card__body">
            <div class="cluster cluster--justify-between">
              <!-- Search -->
              <div class="search" style="max-width: 320px;">
                <label for="table-search" class="visually-hidden">Search records</label>
                <input type="search" id="table-search" class="search__input" placeholder="Search records...">
              </div>

              <!-- View Options -->
              <div class="cluster">
                <div class="btn-group" role="group" aria-label="View options">
                  <button type="button" class="btn btn--outline btn--sm btn--active" aria-pressed="true">Table</button>
                  <button type="button" class="btn btn--outline btn--sm" aria-pressed="false">Cards</button>
                </div>
                
                <div class="dropdown">
                  <button type="button" class="btn btn--outline btn--sm" aria-haspopup="menu" aria-expanded="false">
                    Export
                  </button>
                  <div class="dropdown__menu" role="menu" hidden>
                    <button type="button" class="dropdown__item" role="menuitem">Export CSV</button>
                    <button type="button" class="dropdown__item" role="menuitem">Export PDF</button>
                    <button type="button" class="dropdown__item" role="menuitem">Print</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Bulk Actions (shown when items selected) -->
        <div class="alert alert--info mb-4" role="status" hidden>
          <div class="cluster cluster--justify-between w-full">
            <span><strong>3 items selected</strong></span>
            <div class="cluster">
              <button type="button" class="btn btn--sm btn--ghost">Deselect All</button>
              <button type="button" class="btn btn--sm btn--secondary">Archive</button>
              <button type="button" class="btn btn--sm btn--destructive">Delete</button>
            </div>
          </div>
        </div>

        <!-- Data Table -->
        <div class="card">
          <div class="table-container">
            <table class="table table--hover table--sortable">
              <caption class="visually-hidden">Records data table</caption>
              <thead>
                <tr>
                  <th scope="col" class="table__th--checkbox">
                    <label class="checkbox">
                      <input type="checkbox" class="checkbox__input" aria-label="Select all records">
                      <span class="checkbox__control" aria-hidden="true"></span>
                    </label>
                  </th>
                  <th scope="col" aria-sort="ascending">
                    <button class="table__sort">
                      ID
                      <span class="table__sort-icon" aria-hidden="true"></span>
                    </button>
                  </th>
                  <th scope="col">
                    <button class="table__sort">
                      Name
                      <span class="table__sort-icon" aria-hidden="true"></span>
                    </button>
                  </th>
                  <th scope="col">Category</th>
                  <th scope="col">
                    <button class="table__sort">
                      Date Created
                      <span class="table__sort-icon" aria-hidden="true"></span>
                    </button>
                  </th>
                  <th scope="col">Status</th>
                  <th scope="col"><span class="visually-hidden">Actions</span></th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>
                    <label class="checkbox">
                      <input type="checkbox" class="checkbox__input" aria-label="Select record 001">
                      <span class="checkbox__control" aria-hidden="true"></span>
                    </label>
                  </td>
                  <td><code>REC-001</code></td>
                  <td>First Record</td>
                  <td>Category A</td>
                  <td><time datetime="2025-01-15">Jan 15, 2025</time></td>
                  <td><span class="badge badge--success">Active</span></td>
                  <td>
                    <div class="dropdown dropdown--right">
                      <button type="button" class="btn btn--ghost btn--sm btn--icon" aria-haspopup="menu" aria-expanded="false" aria-label="Record actions">
                        ⋮
                      </button>
                      <div class="dropdown__menu" role="menu" hidden>
                        <button type="button" class="dropdown__item" role="menuitem">View</button>
                        <button type="button" class="dropdown__item" role="menuitem">Edit</button>
                        <button type="button" class="dropdown__item" role="menuitem">Duplicate</button>
                        <hr class="dropdown__divider" role="separator">
                        <button type="button" class="dropdown__item dropdown__item--destructive" role="menuitem">Delete</button>
                      </div>
                    </div>
                  </td>
                </tr>
                <tr>
                  <td>
                    <label class="checkbox">
                      <input type="checkbox" class="checkbox__input" aria-label="Select record 002">
                      <span class="checkbox__control" aria-hidden="true"></span>
                    </label>
                  </td>
                  <td><code>REC-002</code></td>
                  <td>Second Record</td>
                  <td>Category B</td>
                  <td><time datetime="2025-01-14">Jan 14, 2025</time></td>
                  <td><span class="badge badge--warning">Pending</span></td>
                  <td>
                    <div class="dropdown dropdown--right">
                      <button type="button" class="btn btn--ghost btn--sm btn--icon" aria-haspopup="menu" aria-expanded="false" aria-label="Record actions">
                        ⋮
                      </button>
                      <div class="dropdown__menu" role="menu" hidden>
                        <button type="button" class="dropdown__item" role="menuitem">View</button>
                        <button type="button" class="dropdown__item" role="menuitem">Edit</button>
                        <button type="button" class="dropdown__item" role="menuitem">Duplicate</button>
                        <hr class="dropdown__divider" role="separator">
                        <button type="button" class="dropdown__item dropdown__item--destructive" role="menuitem">Delete</button>
                      </div>
                    </div>
                  </td>
                </tr>
                <tr>
                  <td>
                    <label class="checkbox">
                      <input type="checkbox" class="checkbox__input" aria-label="Select record 003">
                      <span class="checkbox__control" aria-hidden="true"></span>
                    </label>
                  </td>
                  <td><code>REC-003</code></td>
                  <td>Third Record</td>
                  <td>Category A</td>
                  <td><time datetime="2025-01-13">Jan 13, 2025</time></td>
                  <td><span class="badge badge--error">Inactive</span></td>
                  <td>
                    <div class="dropdown dropdown--right">
                      <button type="button" class="btn btn--ghost btn--sm btn--icon" aria-haspopup="menu" aria-expanded="false" aria-label="Record actions">
                        ⋮
                      </button>
                      <div class="dropdown__menu" role="menu" hidden>
                        <button type="button" class="dropdown__item" role="menuitem">View</button>
                        <button type="button" class="dropdown__item" role="menuitem">Edit</button>
                        <button type="button" class="dropdown__item" role="menuitem">Duplicate</button>
                        <hr class="dropdown__divider" role="separator">
                        <button type="button" class="dropdown__item dropdown__item--destructive" role="menuitem">Delete</button>
                      </div>
                    </div>
                  </td>
                </tr>
                <tr>
                  <td>
                    <label class="checkbox">
                      <input type="checkbox" class="checkbox__input" aria-label="Select record 004">
                      <span class="checkbox__control" aria-hidden="true"></span>
                    </label>
                  </td>
                  <td><code>REC-004</code></td>
                  <td>Fourth Record</td>
                  <td>Category C</td>
                  <td><time datetime="2025-01-12">Jan 12, 2025</time></td>
                  <td><span class="badge badge--success">Active</span></td>
                  <td>
                    <div class="dropdown dropdown--right">
                      <button type="button" class="btn btn--ghost btn--sm btn--icon" aria-haspopup="menu" aria-expanded="false" aria-label="Record actions">
                        ⋮
                      </button>
                      <div class="dropdown__menu" role="menu" hidden>
                        <button type="button" class="dropdown__item" role="menuitem">View</button>
                        <button type="button" class="dropdown__item" role="menuitem">Edit</button>
                        <button type="button" class="dropdown__item" role="menuitem">Duplicate</button>
                        <hr class="dropdown__divider" role="separator">
                        <button type="button" class="dropdown__item dropdown__item--destructive" role="menuitem">Delete</button>
                      </div>
                    </div>
                  </td>
                </tr>
                <tr>
                  <td>
                    <label class="checkbox">
                      <input type="checkbox" class="checkbox__input" aria-label="Select record 005">
                      <span class="checkbox__control" aria-hidden="true"></span>
                    </label>
                  </td>
                  <td><code>REC-005</code></td>
                  <td>Fifth Record</td>
                  <td>Category B</td>
                  <td><time datetime="2025-01-11">Jan 11, 2025</time></td>
                  <td><span class="badge badge--success">Active</span></td>
                  <td>
                    <div class="dropdown dropdown--right">
                      <button type="button" class="btn btn--ghost btn--sm btn--icon" aria-haspopup="menu" aria-expanded="false" aria-label="Record actions">
                        ⋮
                      </button>
                      <div class="dropdown__menu" role="menu" hidden>
                        <button type="button" class="dropdown__item" role="menuitem">View</button>
                        <button type="button" class="dropdown__item" role="menuitem">Edit</button>
                        <button type="button" class="dropdown__item" role="menuitem">Duplicate</button>
                        <hr class="dropdown__divider" role="separator">
                        <button type="button" class="dropdown__item dropdown__item--destructive" role="menuitem">Delete</button>
                      </div>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- Pagination -->
          <footer class="card__footer">
            <div class="cluster cluster--justify-between w-full">
              <div class="cluster cluster--gap-sm">
                <label for="per-page" class="text-sm text-secondary">Rows per page:</label>
                <select id="per-page" class="form-field__select form-field__select--sm" style="width: auto;">
                  <option value="10">10</option>
                  <option value="25">25</option>
                  <option value="50">50</option>
                  <option value="100">100</option>
                </select>
              </div>
              
              <nav class="pagination" aria-label="Table pagination">
                <a href="#" class="pagination__link pagination__link--prev" aria-label="Previous page">Previous</a>
                <ul class="pagination__list">
                  <li class="pagination__item">
                    <span class="pagination__link pagination__link--current" aria-current="page" aria-label="Page 1, current page">1</span>
                  </li>
                  <li class="pagination__item">
                    <a href="#" class="pagination__link" aria-label="Page 2">2</a>
                  </li>
                  <li class="pagination__item">
                    <a href="#" class="pagination__link" aria-label="Page 3">3</a>
                  </li>
                  <li class="pagination__item">
                    <span class="pagination__ellipsis" aria-hidden="true">...</span>
                  </li>
                  <li class="pagination__item">
                    <a href="#" class="pagination__link" aria-label="Page 10">10</a>
                  </li>
                </ul>
                <a href="#" class="pagination__link pagination__link--next" aria-label="Next page">Next</a>
              </nav>

              <span class="text-sm text-secondary">Showing 1-5 of 50 records</span>
            </div>
          </footer>
        </div>

        <!-- Empty State (shown when no results) -->
        <div class="card" hidden>
          <div class="empty-state">
            <span class="empty-state__icon" aria-hidden="true">
              <!-- Empty icon placeholder -->
              <svg width="64" height="64" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <path d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/>
              </svg>
            </span>
            <h3 class="empty-state__title">No records found</h3>
            <p class="empty-state__description">
              Try adjusting your search or filter criteria to find what you're looking for.
            </p>
            <div class="cluster cluster--justify-center mt-4">
              <button type="button" class="btn btn--secondary">Clear Filters</button>
              <button type="button" class="btn btn--primary">Add Record</button>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>

  <script src="../js/main.js"></script>
</body>
</html>
```

### 20.6 Template Validation Checklist

Before a starter template can be included in the repository, it must pass the following validation:

```markdown
## Template Validation Checklist

### File Requirements
- [ ] Single HTML file, no external dependencies beyond styles.css and main.js
- [ ] Valid HTML5 document structure
- [ ] Proper meta tags (charset, viewport)
- [ ] Descriptive page title

### Component Usage
- [ ] Uses only documented component classes
- [ ] No inline styles (except for demonstration purposes with comment)
- [ ] No custom CSS classes not defined in styles.css
- [ ] No custom JavaScript not defined in main.js

### Accessibility
- [ ] Skip link present and functional
- [ ] All images have alt text
- [ ] All form inputs have labels
- [ ] All interactive elements are keyboard accessible
- [ ] Proper heading hierarchy (h1, h2, h3...)
- [ ] ARIA attributes used correctly
- [ ] Passes axe-core with zero violations

### Responsiveness
- [ ] Renders correctly at 320px width
- [ ] Renders correctly at 768px width
- [ ] Renders correctly at 1024px width
- [ ] Renders correctly at 1440px width
- [ ] No horizontal scrolling at any width

### Theming
- [ ] Works correctly in light theme
- [ ] Works correctly in dark theme
- [ ] Theme toggle functional

### Documentation
- [ ] Contains inline HTML comments explaining structure
- [ ] Comments identify which components are used
- [ ] Comments provide customization guidance

### Functionality
- [ ] All buttons and links have appropriate actions or placeholders
- [ ] All forms have proper structure
- [ ] All interactive components initialize correctly
- [ ] No JavaScript console errors
```

---

## 21. Migration and Upgrade Guidelines

### 21.1 Change Introduction Process

All changes to the public API follow a structured introduction process to minimize disruption for users.

**For Additive Changes (new classes, properties, components):**

1. Feature is implemented and documented
2. Feature is announced in release notes
3. Feature is available immediately upon release
4. No action required from existing users

**For Modifications (changes to existing behavior):**

1. Change is proposed via GitHub issue with rationale
2. Change is discussed with community feedback period (minimum 2 weeks)
3. If approved, change is implemented with backwards compatibility where possible
4. Change is documented with migration notes
5. Change is announced prominently in release notes

**For Removals (deprecations leading to removal):**

1. Feature is marked as deprecated with console warnings (if JS) or documentation notices
2. Deprecation is announced in CHANGELOG under "Deprecated" section
3. Minimum two MINOR versions or six months before removal
4. Feature is removed only in MAJOR version release
5. Migration path documented before deprecation begins

### 21.2 Deprecation Announcement Format

When a class, property, or component is deprecated, the following announcement format is used:

**In CHANGELOG.md:**

```markdown
### Deprecated

- **`.btn--alt`** - Deprecated in favor of `.btn--secondary`. This class will be removed in v2.0.0. 
  See [migration guide](#migrating-btn-alt) for details.
  
- **`DCMT.openModal()`** - Deprecated in favor of `DCMT.modal.open()`. This function will be removed in v2.0.0.
  See [migration guide](#migrating-modal-api) for details.
```

**In Documentation:**

```html
<aside class="callout callout--warning" role="note">
  <strong>Deprecated since v1.3.0</strong>
  <p>
    The <code>.btn--alt</code> class is deprecated and will be removed in v2.0.0.
    Use <code>.btn--secondary</code> instead.
  </p>
  <details>
    <summary>Migration steps</summary>
    <ol>
      <li>Find all instances of <code>class="btn btn--alt"</code> in your HTML</li>
      <li>Replace with <code>class="btn btn--secondary"</code></li>
      <li>Verify visual appearance matches expectations</li>
    </ol>
  </details>
</aside>
```

**In CSS (as comments):**

```css
/**
 * @deprecated since 1.3.0
 * @removal 2.0.0
 * @migration Use .btn--secondary instead
 * @see https://docs.example.com/migration/btn-alt
 */
.btn--alt {
  /* Alias to --secondary for backwards compatibility */
  /* Styles here */
}
```

**In JavaScript (runtime warning):**

```javascript
/**
 * @deprecated since 1.3.0 - Use DCMT.modal.open() instead
 */
function openModal(id) {
  if (typeof console !== 'undefined' && console.warn) {
    console.warn(
      'DCMT: openModal() is deprecated since v1.3.0 and will be removed in v2.0.0. ' +
      'Use DCMT.modal.open() instead. ' +
      'See https://docs.example.com/migration/modal-api'
    );
  }
  return DCMT.modal.open(id);
}
```

### 21.3 Minimum Support Windows

| Change Type | Minimum Support Period | Removal Allowed In |
|-------------|----------------------|-------------------|
| CSS class deprecation | 6 months or 2 MINOR versions | Next MAJOR version |
| CSS custom property deprecation | 6 months or 2 MINOR versions | Next MAJOR version |
| JavaScript function deprecation | 6 months or 2 MINOR versions | Next MAJOR version |
| Component deprecation | 12 months or 3 MINOR versions | Next MAJOR version |
| HTML structure change | 6 months or 2 MINOR versions | Next MAJOR version |
| Browser support removal | Announced 6 months in advance | Next MAJOR version |

### 21.4 Upgrade Notes Format

For each MAJOR version release, a comprehensive upgrade guide is published following this format:

**File**: `/docs/upgrade/v1-to-v2.md`

```markdown
# Upgrading from v1.x to v2.0

This guide covers all breaking changes introduced in DCMT v2.0 and provides 
step-by-step migration instructions.

## Overview

Version 2.0 introduces the following breaking changes:

1. [Removed deprecated button classes](#removed-button-classes)
2. [Updated modal JavaScript API](#updated-modal-api)
3. [Changed form field HTML structure](#changed-form-field-structure)
4. [Renamed CSS custom properties](#renamed-css-properties)
5. [Dropped support for Safari 14](#browser-support-changes)

## Before You Begin

1. Ensure you are on the latest v1.x release (v1.5.2)
2. Address all deprecation warnings in your console
3. Review your usage of deprecated classes using the search patterns below
4. Back up your project or create a new branch for the migration

## Step-by-Step Migration

### 1. Removed Button Classes {#removed-button-classes}

**What changed:** The following button classes have been removed:

| Removed Class | Replacement |
|---------------|-------------|
| `.btn--alt` | `.btn--secondary` |
| `.btn--danger` | `.btn--destructive` |
| `.btn--xs` | `.btn--sm` |

**How to migrate:**

Search your codebase for the removed classes:

```bash
# Find all occurrences
grep -r "btn--alt\|btn--danger\|btn--xs" --include="*.html" .
```

Replace each occurrence:

```html
<!-- Before -->
<button class="btn btn--alt">Cancel</button>
<button class="btn btn--danger">Delete</button>

<!-- After -->
<button class="btn btn--secondary">Cancel</button>
<button class="btn btn--destructive">Delete</button>
```

### 2. Updated Modal JavaScript API {#updated-modal-api}

**What changed:** The global modal functions have been moved to a namespaced API.

| Removed Function | Replacement |
|------------------|-------------|
| `openModal(id)` | `DCMT.modal.open(id)` |
| `closeModal(id)` | `DCMT.modal.close(id)` |
| `closeAllModals()` | `DCMT.modal.closeAll()` |

**How to migrate:**

Search your codebase for the removed functions:

```bash
grep -r "openModal\|closeModal\|closeAllModals" --include="*.js" --include="*.html" .
```

Update each occurrence:

```javascript
// Before
openModal('my-modal');
closeModal('my-modal');
closeAllModals();

// After
DCMT.modal.open('my-modal');
DCMT.modal.close('my-modal');
DCMT.modal.closeAll();
```

For inline event handlers:

```html
<!-- Before -->
<button onclick="openModal('confirm')">Open</button>

<!-- After -->
<button onclick="DCMT.modal.open('confirm')">Open</button>
```

### 3. Changed Form Field HTML Structure {#changed-form-field-structure}

**What changed:** The `.form-field__hint` and `.form-field__error` elements now 
require explicit association with the input via `aria-describedby`.

**Before (v1.x):**

```html
<div class="form-field">
  <label for="email" class="form-field__label">Email</label>
  <input type="email" id="email" class="form-field__input">
  <span class="form-field__hint">We'll never share your email.</span>
</div>
```

**After (v2.0):**

```html
<div class="form-field">
  <label for="email" class="form-field__label">Email</label>
  <input type="email" id="email" class="form-field__input" aria-describedby="email-hint">
  <span id="email-hint" class="form-field__hint">We'll never share your email.</span>
</div>
```

**How to migrate:**

1. Find all form fields with hints or error messages
2. Add unique `id` attributes to hint/error elements
3. Add `aria-describedby` to the corresponding input

```bash
# Find form fields with hints
grep -r "form-field__hint\|form-field__error" --include="*.html" .
```

### 4. Renamed CSS Custom Properties {#renamed-css-properties}

**What changed:** Several CSS custom properties have been renamed for consistency.

| Old Property | New Property |
|--------------|--------------|
| `--color-bg` | `--surface-background` |
| `--color-bg-elevated` | `--surface-elevated` |
| `--color-text` | `--text-primary` |
| `--color-text-muted` | `--text-muted` |
| `--border-color` | `--border-default` |

**How to migrate:**

If you have custom themes or CSS that references these properties:

```css
/* Before */
.my-custom-element {
  background: var(--color-bg);
  color: var(--color-text);
  border-color: var(--border-color);
}

/* After */
.my-custom-element {
  background: var(--surface-background);
  color: var(--text-primary);
  border-color: var(--border-default);
}
```

Search and replace in your custom CSS:

```bash
grep -r "color-bg\|color-text\|border-color" --include="*.css" .
```

### 5. Browser Support Changes {#browser-support-changes}

**What changed:** Safari 14 is no longer supported. The minimum Safari version is now 15.

**Impact:** If you need to support Safari 14, remain on DCMT v1.x.

**How to check:** Review your analytics to determine Safari 14 usage among your users.

## Automated Migration Script

A migration helper script is available for common replacements:

```bash
# Download the migration script
curl -O https://raw.githubusercontent.com/example/dcmt/main/scripts/migrate-v2.sh

# Run in dry-run mode first
./migrate-v2.sh --dry-run ./src

# Run the actual migration
./migrate-v2.sh ./src
```

The script handles:
- CSS class replacements
- JavaScript API updates
- CSS custom property renames

Manual review is still required for:
- Form field `aria-describedby` additions
- Custom theme updates
- Complex JavaScript integrations

## Verification Checklist

After migration, verify the following:

- [ ] No console warnings about deprecated functions
- [ ] All buttons render correctly
- [ ] All modals open and close correctly
- [ ] All form validation works correctly
- [ ] Custom themes apply correctly
- [ ] No visual regressions in light theme
- [ ] No visual regressions in dark theme
- [ ] All automated tests pass

## Getting Help

If you encounter issues during migration:

1. Check the [FAQ section](#faq) below
2. Search [existing issues](https://github.com/example/dcmt/issues)
3. Open a [new issue](https://github.com/example/dcmt/issues/new) with the "migration" label

## FAQ

### Q: Can I use v1 and v2 classes together during migration?

A: No. Each page should use only one version. Migrate page by page if needed.

### Q: Will v1.x receive security updates?

A: Yes. Version 1.x will receive critical security updates for 12 months after v2.0 release.

### Q: How do I pin to v1.x via CDN?

A: Use the versioned CDN URL:

```html
<link rel="stylesheet" href="https://cdn.example.com/dcmt/1.5.2/styles.min.css">
```
```

### 21.5 Version Support Policy

| Version | Status | Security Updates | Bug Fixes | New Features |
|---------|--------|------------------|-----------|--------------|
| Latest MAJOR (e.g., 2.x) | Active | Yes | Yes | Yes |
| Previous MAJOR (e.g., 1.x) | Maintenance | Yes (12 months) | Critical only (6 months) | No |
| Older MAJOR versions | End of Life | No | No | No |

**Maintenance Timeline Example:**

```
v1.0.0 released: January 2025
v2.0.0 released: January 2026

v1.x support timeline:
- January 2026 - July 2026: Critical bug fixes
- January 2026 - January 2027: Security updates only
- January 2027: v1.x reaches end of life
```

### 21.6 Communication Channels for Updates

**Deprecation and Breaking Change Announcements:**

1. **CHANGELOG.md**: All changes documented per release
2. **GitHub Releases**: Release notes with highlights
3. **GitHub Discussions**: Announcements category for major changes
4. **Documentation Site**: Banner notifications for upcoming breaking changes
5. **Console Warnings**: Runtime warnings for deprecated JavaScript APIs

**Upgrade Notification Timeline:**

| Timeframe | Action |
|-----------|--------|
| 3 months before MAJOR | Announce planned breaking changes |
| 2 months before MAJOR | Release beta version for testing |
| 1 month before MAJOR | Release RC version, publish migration guide |
| MAJOR release | Full release with final migration documentation |
| 1 month after MAJOR | Reminder to upgrade, address common migration issues |

---

*Document Extension Version: 1.1.0*
*Last Updated: 2025-01-15*
*Status: Approved for Implementation*