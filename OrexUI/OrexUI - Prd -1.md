# Product Requirements Document (PRD)

## Developer-Centric Minimalist Theme – HTML Components Showcase

**Version:** 1.0.0  
**Status:** Draft  
**Last Updated:** 2025-01-15  
**Author:** Platform Architecture Team

---

## Table of Contents

1. [Product Overview](#1-product-overview)
2. [Goals and Non-Goals](#2-goals-and-non-goals)
3. [Success Metrics](#3-success-metrics)
4. [Design Principles](#4-design-principles)
5. [Theming System](#5-theming-system)
6. [Accessibility Requirements](#6-accessibility-requirements)
7. [Component Inventory](#7-component-inventory)
8. [Showcase and Documentation Requirements](#8-showcase-and-documentation-requirements)
9. [Developer Experience](#9-developer-experience)
10. [File and Distribution Structure](#10-file-and-distribution-structure)
11. [Performance Requirements](#11-performance-requirements)
12. [Browser Support](#12-browser-support)
13. [Extensibility and Community](#13-extensibility-and-community)
14. [Risks and Mitigations](#14-risks-and-mitigations)
15. [Future Enhancements](#15-future-enhancements)

---

## 1. Product Overview

### 1.1 Purpose

This project delivers a framework-agnostic, zero-dependency HTML component library designed for developers who require functional, accessible UI elements without the overhead of modern build toolchains. The library serves as both a production-ready component system and a live documentation showcase demonstrating each component's implementation and usage.

The primary deliverable is a collection of semantic HTML patterns enhanced with CSS custom properties and minimal vanilla JavaScript, enabling developers to copy components directly into any web project regardless of stack or infrastructure.

### 1.2 Target Users

**Primary Users:**

- Frontend developers building static sites, prototypes, or lightweight web applications
- Developers working in environments where build tools are impractical or prohibited
- Technical writers and documentation engineers requiring consistent UI patterns
- Backend developers needing functional frontend components without framework expertise
- Educators and students learning web fundamentals

**Secondary Users:**

- Open-source maintainers seeking accessible component references
- Design system architects evaluating minimal implementation patterns
- Developers prototyping interfaces before committing to a framework

### 1.3 Key Value Proposition

| Value | Description |
|-------|-------------|
| Zero Build Requirements | No compilation, transpilation, or bundling. Files run directly in the browser. |
| Copy-Paste Ready | Each component is self-contained and functional when pasted into any HTML document. |
| Framework Agnostic | No dependencies on React, Vue, Angular, or any JavaScript framework. |
| Accessibility First | WCAG 2.1 AA compliant with full keyboard navigation and screen reader support. |
| Themeable by Design | Complete visual customization through CSS custom properties without modifying source files. |
| Lightweight | Minimal file sizes with no external font, icon, or library dependencies. |

---

## 2. Goals and Non-Goals

### 2.1 Primary Goals

1. **Reusability**: Every component must function correctly when copied into an isolated HTML file with only the required CSS and JavaScript includes.

2. **Zero Setup**: Developers must be able to use components by including a single CSS file and a single JavaScript file. No package managers, bundlers, or configuration files.

3. **Accessibility Compliance**: All interactive components must meet WCAG 2.1 AA standards, including keyboard operability, focus management, and appropriate ARIA semantics.

4. **Theming Flexibility**: The visual system must support light and dark themes out of the box, with straightforward customization through CSS variable overrides.

5. **Documentation Completeness**: Every component must include a live demonstration, usage instructions, HTML source, and accessibility notes.

6. **Semantic Correctness**: HTML must be valid, semantic, and progressively enhanced. Components should remain usable when JavaScript is disabled where possible.

7. **Consistency**: All components must share a unified visual language, spacing system, and interaction patterns.

### 2.2 Non-Goals

1. **Framework Integration**: This project does not provide React components, Vue SFCs, Angular modules, or Web Components. Integration guides may be offered as supplementary documentation but are not core deliverables.

2. **Build Tooling**: No Webpack, Vite, Rollup, or similar bundlers. No Sass, Less, PostCSS, or CSS preprocessors. No TypeScript. All source files are the distribution files.

3. **Package Manager Distribution as Primary Channel**: While npm publication may occur for convenience, the canonical distribution method is direct file inclusion via CDN or download.

4. **Animation Library**: Complex animations and transitions are out of scope. Minimal, functional transitions may be included where they serve usability.

5. **Icon Library**: No bundled icon set. Components should function with or without icons, allowing developers to integrate their preferred icon solution.

6. **JavaScript-Dependent Core Functionality**: Where possible, components should provide baseline functionality through CSS alone. JavaScript enhances but should not be required for basic display.

7. **Legacy Browser Support**: Internet Explorer and other non-evergreen browsers are explicitly unsupported.

---

## 3. Success Metrics

### 3.1 Adoption Indicators

| Metric | Target (6 months) | Target (12 months) |
|--------|-------------------|---------------------|
| GitHub Stars | 500 | 2,000 |
| GitHub Forks | 100 | 400 |
| CDN Monthly Requests | 10,000 | 100,000 |
| Documentation Page Views | 5,000/month | 25,000/month |
| Component Snippet Copies (tracked via event) | 1,000 | 10,000 |

### 3.2 Quality Benchmarks

| Metric | Requirement |
|--------|-------------|
| Lighthouse Performance Score | 95+ |
| Lighthouse Accessibility Score | 100 |
| CSS File Size (minified + gzipped) | < 15 KB |
| JS File Size (minified + gzipped) | < 10 KB |
| WCAG 2.1 AA Compliance | 100% of interactive components |
| HTML Validation Errors | 0 |
| Time to First Contentful Paint | < 1.0s on 3G |

### 3.3 Community Health Indicators

| Metric | Target |
|--------|--------|
| Average Issue Response Time | < 48 hours |
| Pull Request Merge Rate | > 70% |
| Documentation Coverage | 100% of components |

---

## 4. Design Principles

### 4.1 Visual Philosophy

The visual system prioritizes clarity, functionality, and developer familiarity over decorative aesthetics. The design language draws from terminal interfaces, code editors, and technical documentation rather than consumer-facing marketing sites.

### 4.2 Core Principles

**4.2.1 Function Over Decoration**

Every visual element must serve a functional purpose. Decorative elements that do not aid comprehension, navigation, or interaction are excluded. Borders define boundaries. Colors communicate state. Spacing establishes hierarchy.

**4.2.2 High Contrast, Low Noise**

Text must be immediately readable against its background. Interactive elements must be clearly distinguishable from static content. The palette is constrained to minimize visual decision-making.

**4.2.3 Consistent Spatial System**

All spacing derives from a base unit scale. Padding, margins, gaps, and component sizing follow predictable increments. This consistency allows components to combine without visual conflict.

**4.2.4 Typography Hierarchy**

A monospace type stack serves as the default, reinforcing the developer-tool aesthetic. A proportional system font stack is available for prose content. Size, weight, and spacing establish hierarchy without reliance on decorative fonts.

**4.2.5 Visible Interactivity**

Interactive elements must appear interactive. Buttons look pressable. Links are underlined or otherwise distinguished. Focus states are prominent. Hover states provide immediate feedback.

### 4.3 Visual Specifications

**Spacing Scale (base unit: 4px):**

```
--space-0: 0
--space-1: 4px
--space-2: 8px
--space-3: 12px
--space-4: 16px
--space-5: 24px
--space-6: 32px
--space-7: 48px
--space-8: 64px
```

**Border Radius:**

```
--radius-none: 0
--radius-sm: 2px
--radius-md: 4px
--radius-lg: 8px
--radius-full: 9999px
```

**Font Stacks:**

```
--font-mono: "SF Mono", "Cascadia Code", "Fira Code", Consolas, monospace
--font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif
```

**Font Sizes:**

```
--text-xs: 0.75rem
--text-sm: 0.875rem
--text-base: 1rem
--text-lg: 1.125rem
--text-xl: 1.25rem
--text-2xl: 1.5rem
--text-3xl: 2rem
```

---

## 5. Theming System

### 5.1 Architecture

The theming system is built entirely on CSS custom properties (CSS variables). All color values, spacing overrides, and visual tokens reference variables defined at the `:root` level. Theme switching occurs by changing these variable values through attribute or class selectors.

### 5.2 Theme Implementation

**5.2.1 Default Theme Application**

```css
:root {
  /* Neutral Scale */
  --color-neutral-0: #ffffff;
  --color-neutral-50: #f9fafb;
  --color-neutral-100: #f3f4f6;
  --color-neutral-200: #e5e7eb;
  --color-neutral-300: #d1d5db;
  --color-neutral-400: #9ca3af;
  --color-neutral-500: #6b7280;
  --color-neutral-600: #4b5563;
  --color-neutral-700: #374151;
  --color-neutral-800: #1f2937;
  --color-neutral-900: #111827;
  --color-neutral-950: #030712;

  /* Semantic Colors */
  --color-primary: #2563eb;
  --color-primary-hover: #1d4ed8;
  --color-success: #16a34a;
  --color-warning: #ca8a04;
  --color-error: #dc2626;
  --color-info: #0891b2;

  /* Surface and Text */
  --surface-background: var(--color-neutral-0);
  --surface-elevated: var(--color-neutral-50);
  --surface-overlay: var(--color-neutral-100);
  --text-primary: var(--color-neutral-900);
  --text-secondary: var(--color-neutral-600);
  --text-muted: var(--color-neutral-400);
  --border-default: var(--color-neutral-200);
  --border-strong: var(--color-neutral-300);
}
```

**5.2.2 Dark Theme**

```css
[data-theme="dark"],
.theme-dark {
  --surface-background: var(--color-neutral-950);
  --surface-elevated: var(--color-neutral-900);
  --surface-overlay: var(--color-neutral-800);
  --text-primary: var(--color-neutral-50);
  --text-secondary: var(--color-neutral-400);
  --text-muted: var(--color-neutral-600);
  --border-default: var(--color-neutral-800);
  --border-strong: var(--color-neutral-700);
}
```

### 5.3 Theme Switching

Theme switching is accomplished through a single attribute on the `<html>` or `<body>` element:

```html
<!-- Light theme (default) -->
<html data-theme="light">

<!-- Dark theme -->
<html data-theme="dark">

<!-- System preference -->
<html data-theme="system">
```

**JavaScript Implementation:**

```javascript
function setTheme(theme) {
  if (theme === 'system') {
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    document.documentElement.setAttribute('data-theme', prefersDark ? 'dark' : 'light');
  } else {
    document.documentElement.setAttribute('data-theme', theme);
  }
  localStorage.setItem('theme', theme);
}

function initTheme() {
  const saved = localStorage.getItem('theme') || 'system';
  setTheme(saved);
}
```

### 5.4 Custom Theming

Developers create custom themes by overriding CSS variables:

```css
[data-theme="custom"] {
  --color-primary: #7c3aed;
  --color-primary-hover: #6d28d9;
  --surface-background: #faf5ff;
  /* Additional overrides */
}
```

---

## 6. Accessibility Requirements

### 6.1 Compliance Standard

All components must meet WCAG 2.1 Level AA success criteria. Select components targeting specialized use cases may implement Level AAA criteria where practical.

### 6.2 Keyboard Navigation

**6.2.1 General Requirements**

- All interactive elements must be reachable via keyboard Tab navigation
- Tab order must follow logical reading order (DOM order unless explicitly modified)
- No keyboard traps except within modal contexts (with documented escape mechanism)
- All actions achievable via mouse must be achievable via keyboard

**6.2.2 Component-Specific Keyboard Patterns**

| Component Type | Required Keys |
|----------------|---------------|
| Buttons | Enter, Space to activate |
| Links | Enter to activate |
| Menus | Arrow keys to navigate, Enter to select, Escape to close |
| Tabs | Arrow keys to navigate, Enter/Space to activate |
| Modals | Escape to close, Tab trapped within |
| Dropdowns | Arrow keys to navigate, Enter to select, Escape to close |
| Accordions | Enter/Space to toggle, Arrow keys between headers |
| Toggle Switches | Space to toggle |

### 6.3 Focus Management

**6.3.1 Visible Focus States**

All focusable elements must display a visible focus indicator with minimum 3:1 contrast ratio against adjacent colors. The default focus style:

```css
:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

**6.3.2 Focus Trapping**

Modal dialogs, slide-out drawers, and other overlay components must trap focus within the component when open. Focus must return to the triggering element when the overlay closes.

**6.3.3 Skip Links**

The documentation site must include a skip navigation link as the first focusable element, allowing keyboard users to bypass repeated navigation.

### 6.4 ARIA Implementation

**6.4.1 Required ARIA Attributes by Component**

| Component | Required ARIA |
|-----------|---------------|
| Modal | `role="dialog"`, `aria-modal="true"`, `aria-labelledby` |
| Alert | `role="alert"` or `role="status"` |
| Tabs | `role="tablist"`, `role="tab"`, `role="tabpanel"`, `aria-selected` |
| Menu | `role="menu"`, `role="menuitem"`, `aria-expanded` |
| Toggle | `role="switch"`, `aria-checked` |
| Progress | `role="progressbar"`, `aria-valuenow`, `aria-valuemin`, `aria-valuemax` |
| Tooltip | `role="tooltip"`, `aria-describedby` on trigger |
| Accordion | `aria-expanded`, `aria-controls` |

**6.4.2 Live Regions**

Dynamic content updates (toasts, validation messages, loading states) must use appropriate ARIA live regions:

- `aria-live="polite"` for non-urgent updates
- `aria-live="assertive"` for critical alerts
- `aria-atomic="true"` when the entire region should be announced

### 6.5 Color and Contrast

- Text contrast ratio: minimum 4.5:1 for normal text, 3:1 for large text
- UI component contrast: minimum 3:1 against adjacent colors
- Information must not be conveyed by color alone
- Focus indicators must maintain 3:1 contrast ratio

### 6.6 Motion and Animation

- Respect `prefers-reduced-motion` media query
- Provide reduced or eliminated animation for users who request it
- No content should flash more than three times per second

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 7. Component Inventory

### 7.1 Layout and Structure

#### 7.1.1 Container

**Purpose:** Constrain content width and center horizontally with consistent padding.

**HTML Structure:**
```html
<div class="container">
  <!-- Content -->
</div>

<!-- Size variants -->
<div class="container container--sm">...</div>
<div class="container container--lg">...</div>
<div class="container container--full">...</div>
```

**Variants:**
- `container--sm`: Maximum width 640px
- `container--md`: Maximum width 768px (default)
- `container--lg`: Maximum width 1024px
- `container--xl`: Maximum width 1280px
- `container--full`: Full width with padding

**CSS Variables:**
```css
--container-padding: var(--space-4);
--container-max-width: 768px;
```

**Accessibility:** None required (presentational).

**JavaScript:** None required.

---

#### 7.1.2 Grid

**Purpose:** Create responsive multi-column layouts.

**HTML Structure:**
```html
<div class="grid">
  <div class="grid__item">...</div>
  <div class="grid__item">...</div>
</div>

<!-- Column variants -->
<div class="grid grid--cols-2">...</div>
<div class="grid grid--cols-3">...</div>
<div class="grid grid--cols-4">...</div>
```

**Variants:**
- `grid--cols-1` through `grid--cols-12`
- `grid--gap-sm`, `grid--gap-md`, `grid--gap-lg`
- Responsive: `grid--cols-2@md`, `grid--cols-3@lg`

**CSS Variables:**
```css
--grid-gap: var(--space-4);
--grid-columns: 1;
```

**Accessibility:** None required (presentational).

**JavaScript:** None required.

---

#### 7.1.3 Stack

**Purpose:** Arrange elements vertically with consistent spacing.

**HTML Structure:**
```html
<div class="stack">
  <div>First item</div>
  <div>Second item</div>
  <div>Third item</div>
</div>

<!-- Gap variants -->
<div class="stack stack--gap-sm">...</div>
<div class="stack stack--gap-lg">...</div>
```

**Variants:**
- `stack--gap-none`, `stack--gap-sm`, `stack--gap-md`, `stack--gap-lg`, `stack--gap-xl`

**Accessibility:** None required (presentational).

**JavaScript:** None required.

---

#### 7.1.4 Cluster

**Purpose:** Arrange elements horizontally with wrapping and consistent spacing.

**HTML Structure:**
```html
<div class="cluster">
  <span>Item one</span>
  <span>Item two</span>
  <span>Item three</span>
</div>
```

**Variants:**
- `cluster--gap-sm`, `cluster--gap-md`, `cluster--gap-lg`
- `cluster--justify-start`, `cluster--justify-center`, `cluster--justify-end`, `cluster--justify-between`
- `cluster--align-start`, `cluster--align-center`, `cluster--align-end`

**Accessibility:** None required (presentational).

**JavaScript:** None required.

---

#### 7.1.5 Sidebar Layout

**Purpose:** Create a two-column layout with a fixed-width sidebar and flexible main content area.

**HTML Structure:**
```html
<div class="sidebar-layout">
  <aside class="sidebar-layout__sidebar">
    <!-- Sidebar content -->
  </aside>
  <main class="sidebar-layout__main">
    <!-- Main content -->
  </main>
</div>

<!-- Right sidebar variant -->
<div class="sidebar-layout sidebar-layout--right">...</div>
```

**Variants:**
- `sidebar-layout--right`: Sidebar on right side
- `sidebar-layout--sticky`: Sidebar sticks on scroll

**CSS Variables:**
```css
--sidebar-width: 280px;
--sidebar-min-content: 60%;
```

**Accessibility:** Use appropriate landmark elements (`<aside>`, `<main>`).

**JavaScript:** None required.

---

#### 7.1.6 Split Layout

**Purpose:** Create equal or proportional horizontal divisions.

**HTML Structure:**
```html
<div class="split">
  <div class="split__item">Left</div>
  <div class="split__item">Right</div>
</div>

<!-- Ratio variants -->
<div class="split split--ratio-1-2">...</div>
<div class="split split--ratio-2-1">...</div>
```

**Variants:**
- `split--ratio-1-1` (default)
- `split--ratio-1-2`, `split--ratio-2-1`
- `split--ratio-1-3`, `split--ratio-3-1`

**Accessibility:** None required (presentational).

**JavaScript:** None required.

---

#### 7.1.7 Section

**Purpose:** Define major page sections with consistent vertical padding.

**HTML Structure:**
```html
<section class="section">
  <div class="section__header">
    <h2 class="section__title">Section Title</h2>
    <p class="section__description">Optional description text.</p>
  </div>
  <div class="section__content">
    <!-- Section content -->
  </div>
</section>
```

**Variants:**
- `section--compact`: Reduced padding
- `section--flush`: No padding

**Accessibility:** Use appropriate heading level within section context.

**JavaScript:** None required.

---

#### 7.1.8 Card

**Purpose:** Contain related content and actions within a bounded container.

**HTML Structure:**
```html
<article class="card">
  <header class="card__header">
    <h3 class="card__title">Card Title</h3>
    <p class="card__subtitle">Optional subtitle</p>
  </header>
  <div class="card__body">
    <p>Card content goes here.</p>
  </div>
  <footer class="card__footer">
    <button class="btn btn--sm">Action</button>
  </footer>
</article>
```

**Variants:**
- `card--bordered`: Visible border
- `card--elevated`: Box shadow elevation
- `card--interactive`: Hover state for clickable cards
- `card--compact`: Reduced padding

**States:**
- Default
- Hover (for interactive variant)
- Focus (for interactive variant)

**Accessibility:**
- Use `<article>` when card represents standalone content
- Interactive cards should be wrapped in `<a>` or use `role="button"` with `tabindex="0"`

**JavaScript:** None required for basic usage. Interactive cards require click handlers.

---

#### 7.1.9 Divider

**Purpose:** Visually separate content sections.

**HTML Structure:**
```html
<hr class="divider">

<!-- With text -->
<div class="divider divider--text">
  <span>Or continue with</span>
</div>

<!-- Vertical -->
<span class="divider divider--vertical"></span>
```

**Variants:**
- `divider--text`: Contains centered text
- `divider--vertical`: Vertical orientation
- `divider--dashed`: Dashed line style
- `divider--thick`: Increased thickness

**Accessibility:** `<hr>` provides implicit separator role. Decorative dividers should use `role="presentation"`.

**JavaScript:** None required.

---

### 7.2 Navigation

#### 7.2.1 Navbar

**Purpose:** Primary site navigation header.

**HTML Structure:**
```html
<header class="navbar">
  <div class="navbar__brand">
    <a href="/" class="navbar__logo">Site Name</a>
  </div>
  <nav class="navbar__nav" aria-label="Main navigation">
    <ul class="navbar__menu">
      <li class="navbar__item">
        <a href="/docs" class="navbar__link">Documentation</a>
      </li>
      <li class="navbar__item">
        <a href="/components" class="navbar__link navbar__link--active" aria-current="page">Components</a>
      </li>
    </ul>
  </nav>
  <div class="navbar__actions">
    <button class="btn btn--sm" data-theme-toggle>Theme</button>
  </div>
  <button class="navbar__toggle" aria-label="Toggle navigation" aria-expanded="false" aria-controls="mobile-menu">
    <span class="navbar__toggle-icon"></span>
  </button>
</header>
```

**States:**
- Default
- Sticky (fixed on scroll)
- Mobile collapsed/expanded

**Keyboard Behavior:**
- Tab through all links and buttons
- Enter activates focused link/button
- Mobile toggle: Enter/Space to expand/collapse

**Accessibility:**
- `<nav>` element with `aria-label`
- `aria-current="page"` on active link
- Mobile toggle: `aria-expanded`, `aria-controls`

**JavaScript Requirements:**
- Mobile menu toggle
- Sticky behavior (optional)
- Escape key closes mobile menu

---

#### 7.2.2 Sidebar Navigation

**Purpose:** Vertical navigation for documentation or application sections.

**HTML Structure:**
```html
<nav class="sidenav" aria-label="Documentation">
  <div class="sidenav__section">
    <h4 class="sidenav__heading">Getting Started</h4>
    <ul class="sidenav__list">
      <li class="sidenav__item">
        <a href="/intro" class="sidenav__link">Introduction</a>
      </li>
      <li class="sidenav__item">
        <a href="/install" class="sidenav__link sidenav__link--active" aria-current="page">Installation</a>
      </li>
    </ul>
  </div>
  <div class="sidenav__section">
    <h4 class="sidenav__heading">Components</h4>
    <ul class="sidenav__list">
      <li class="sidenav__item">
        <a href="/buttons" class="sidenav__link">Buttons</a>
      </li>
    </ul>
  </div>
</nav>
```

**Variants:**
- `sidenav--compact`: Reduced spacing
- `sidenav--bordered`: Right border separator

**States:**
- Active link: `sidenav__link--active`
- Nested expanded/collapsed

**Accessibility:**
- `<nav>` with descriptive `aria-label`
- `aria-current="page"` on active item
- Collapsible sections use `aria-expanded`

**JavaScript Requirements:**
- Collapsible section toggle (optional)
- Scroll spy for active state (optional)

---

#### 7.2.3 Tabs

**Purpose:** Switch between related content panels within a single view.

**HTML Structure:**
```html
<div class="tabs">
  <div class="tabs__list" role="tablist" aria-label="Content sections">
    <button class="tabs__tab" role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">
      Tab One
    </button>
    <button class="tabs__tab" role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2" tabindex="-1">
      Tab Two
    </button>
    <button class="tabs__tab" role="tab" aria-selected="false" aria-controls="panel-3" id="tab-3" tabindex="-1">
      Tab Three
    </button>
  </div>
  <div class="tabs__panel" role="tabpanel" id="panel-1" aria-labelledby="tab-1">
    <p>Content for tab one.</p>
  </div>
  <div class="tabs__panel" role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>
    <p>Content for tab two.</p>
  </div>
  <div class="tabs__panel" role="tabpanel" id="panel-3" aria-labelledby="tab-3" hidden>
    <p>Content for tab three.</p>
  </div>
</div>
```

**Variants:**
- `tabs--bordered`: Bordered panel style
- `tabs--pills`: Pill-shaped tab buttons
- `tabs--vertical`: Vertical tab orientation

**States:**
- Active: `aria-selected="true"`
- Inactive: `aria-selected="false"`, `tabindex="-1"`
- Disabled: `disabled` attribute, `aria-disabled="true"`

**Keyboard Behavior:**
- Arrow Left/Right: Navigate between tabs
- Home: First tab
- End: Last tab
- Enter/Space: Activate focused tab (automatic activation recommended)

**Accessibility:**
- `role="tablist"` on container
- `role="tab"` on each tab button
- `role="tabpanel"` on each panel
- `aria-controls` linking tab to panel
- `aria-labelledby` linking panel to tab
- Only active tab in tab order (`tabindex`)

**JavaScript Requirements:**
- Tab selection and panel visibility
- Keyboard navigation within tablist
- Focus management

---

#### 7.2.4 Breadcrumbs

**Purpose:** Display navigation hierarchy and current location.

**HTML Structure:**
```html
<nav class="breadcrumbs" aria-label="Breadcrumb">
  <ol class="breadcrumbs__list">
    <li class="breadcrumbs__item">
      <a href="/" class="breadcrumbs__link">Home</a>
    </li>
    <li class="breadcrumbs__item">
      <a href="/components" class="breadcrumbs__link">Components</a>
    </li>
    <li class="breadcrumbs__item">
      <span class="breadcrumbs__current" aria-current="page">Buttons</span>
    </li>
  </ol>
</nav>
```

**Accessibility:**
- `<nav>` with `aria-label="Breadcrumb"`
- `<ol>` for ordered hierarchy
- `aria-current="page"` on current item
- Separators via CSS (not in DOM) or `aria-hidden="true"`

**JavaScript:** None required.

---

#### 7.2.5 Pagination

**Purpose:** Navigate between pages of content.

**HTML Structure:**
```html
<nav class="pagination" aria-label="Pagination">
  <a href="?page=1" class="pagination__link pagination__link--prev" aria-label="Previous page">
    Previous
  </a>
  <ul class="pagination__list">
    <li class="pagination__item">
      <a href="?page=1" class="pagination__link" aria-label="Page 1">1</a>
    </li>
    <li class="pagination__item">
      <span class="pagination__link pagination__link--current" aria-current="page" aria-label="Page 2, current page">2</span>
    </li>
    <li class="pagination__item">
      <a href="?page=3" class="pagination__link" aria-label="Page 3">3</a>
    </li>
    <li class="pagination__item">
      <span class="pagination__ellipsis" aria-hidden="true">...</span>
    </li>
    <li class="pagination__item">
      <a href="?page=10" class="pagination__link" aria-label="Page 10">10</a>
    </li>
  </ul>
  <a href="?page=3" class="pagination__link pagination__link--next" aria-label="Next page">
    Next
  </a>
</nav>
```

**States:**
- Current: `pagination__link--current`
- Disabled: `pagination__link--disabled` with `aria-disabled="true"`

**Accessibility:**
- `<nav>` with `aria-label="Pagination"`
- `aria-current="page"` on current page
- Descriptive `aria-label` on each link

**JavaScript:** None required.

---

### 7.3 Basic Controls

#### 7.3.1 Button

**Purpose:** Trigger actions or submit forms.

**HTML Structure:**
```html
<!-- Primary button -->
<button type="button" class="btn btn--primary">Primary Action</button>

<!-- Secondary button -->
<button type="button" class="btn btn--secondary">Secondary</button>

<!-- Outline button -->
<button type="button" class="btn btn--outline">Outline</button>

<!-- Ghost button -->
<button type="button" class="btn btn--ghost">Ghost</button>

<!-- Destructive button -->
<button type="button" class="btn btn--destructive">Delete</button>

<!-- Sizes -->
<button type="button" class="btn btn--sm">Small</button>
<button type="button" class="btn btn--lg">Large</button>

<!-- States -->
<button type="button" class="btn btn--primary" disabled>Disabled</button>
<button type="button" class="btn btn--primary btn--loading" aria-busy="true">
  <span class="btn__spinner" aria-hidden="true"></span>
  Loading...
</button>

<!-- With icon -->
<button type="button" class="btn btn--primary btn--icon-left">
  <svg aria-hidden="true">...</svg>
  With Icon
</button>

<!-- Icon only -->
<button type="button" class="btn btn--icon" aria-label="Close">
  <svg aria-hidden="true">...</svg>
</button>
```

**Variants:**
- `btn--primary`, `btn--secondary`, `btn--outline`, `btn--ghost`, `btn--destructive`
- `btn--sm`, `btn--md` (default), `btn--lg`
- `btn--block`: Full width
- `btn--icon`: Icon only (square)
- `btn--icon-left`, `btn--icon-right`: Icon positioning

**States:**
- Default
- Hover: `:hover`
- Active: `:active`
- Focus: `:focus-visible`
- Disabled: `:disabled`
- Loading: `btn--loading`, `aria-busy="true"`

**Keyboard Behavior:**
- Enter/Space: Activate button

**Accessibility:**
- Use `<button>` element for actions, `<a>` for navigation
- `type="button"` for non-submit buttons
- `aria-label` required for icon-only buttons
- `aria-busy="true"` during loading state
- `disabled` attribute prevents interaction

**JavaScript:** None required for basic usage. Loading state toggle may require JS.

---

#### 7.3.2 Button Group

**Purpose:** Group related buttons together.

**HTML Structure:**
```html
<div class="btn-group" role="group" aria-label="Text formatting">
  <button type="button" class="btn btn--outline">Bold</button>
  <button type="button" class="btn btn--outline btn--active" aria-pressed="true">Italic</button>
  <button type="button" class="btn btn--outline">Underline</button>
</div>
```

**Variants:**
- `btn-group--vertical`: Vertical stacking
- `btn-group--attached`: No gap between buttons

**Accessibility:**
- `role="group"` with `aria-label`
- Toggle buttons use `aria-pressed`

**JavaScript:** Toggle state management for pressed buttons.

---

#### 7.3.3 Link Button

**Purpose:** Button-styled navigation links.

**HTML Structure:**
```html
<a href="/destination" class="btn btn--primary">Navigate</a>
```

**Accessibility:**
- Use `<a>` element for navigation
- Use `<button>` element for actions

**JavaScript:** None required.

---

#### 7.3.4 Toggle Switch

**Purpose:** Binary on/off control.

**HTML Structure:**
```html
<label class="toggle">
  <input type="checkbox" class="toggle__input" role="switch" aria-checked="false">
  <span class="toggle__track">
    <span class="toggle__thumb"></span>
  </span>
  <span class="toggle__label">Enable notifications</span>
</label>

<!-- Checked state -->
<label class="toggle">
  <input type="checkbox" class="toggle__input" role="switch" aria-checked="true" checked>
  <span class="toggle__track">
    <span class="toggle__thumb"></span>
  </span>
  <span class="toggle__label">Notifications enabled</span>
</label>
```

**States:**
- Unchecked (default)
- Checked
- Disabled
- Focus

**Keyboard Behavior:**
- Space: Toggle state
- Tab: Focus toggle

**Accessibility:**
- `role="switch"` on input
- `aria-checked` reflects state
- Associated label text
- Visible focus state

**JavaScript:** Update `aria-checked` on state change (if not using native checkbox binding).

---

#### 7.3.5 Checkbox

**Purpose:** Select one or more options from a set.

**HTML Structure:**
```html
<label class="checkbox">
  <input type="checkbox" class="checkbox__input">
  <span class="checkbox__control" aria-hidden="true"></span>
  <span class="checkbox__label">Accept terms and conditions</span>
</label>

<!-- Indeterminate -->
<label class="checkbox">
  <input type="checkbox" class="checkbox__input" data-indeterminate="true">
  <span class="checkbox__control" aria-hidden="true"></span>
  <span class="checkbox__label">Select all</span>
</label>
```

**States:**
- Unchecked
- Checked
- Indeterminate
- Disabled
- Error

**Keyboard Behavior:**
- Space: Toggle checked state
- Tab: Navigate between checkboxes

**Accessibility:**
- Native `<input type="checkbox">` for full accessibility
- Custom visual control is `aria-hidden`
- Visible focus on input

**JavaScript:** Indeterminate state requires JS: `checkbox.indeterminate = true`.

---

#### 7.3.6 Radio Button

**Purpose:** Select exactly one option from a set.

**HTML Structure:**
```html
<fieldset class="radio-group">
  <legend class="radio-group__legend">Select size</legend>
  <label class="radio">
    <input type="radio" name="size" value="sm" class="radio__input">
    <span class="radio__control" aria-hidden="true"></span>
    <span class="radio__label">Small</span>
  </label>
  <label class="radio">
    <input type="radio" name="size" value="md" class="radio__input" checked>
    <span class="radio__control" aria-hidden="true"></span>
    <span class="radio__label">Medium</span>
  </label>
  <label class="radio">
    <input type="radio" name="size" value="lg" class="radio__input">
    <span class="radio__control" aria-hidden="true"></span>
    <span class="radio__label">Large</span>
  </label>
</fieldset>
```

**States:**
- Unchecked
- Checked
- Disabled
- Error

**Keyboard Behavior:**
- Arrow Up/Down: Navigate within group
- Arrow Left/Right: Navigate within group
- Tab: Move to/from radio group

**Accessibility:**
- `<fieldset>` groups related radios
- `<legend>` provides group label
- Native `<input type="radio">` with shared `name`

**JavaScript:** None required.

---

### 7.4 Form Elements

#### 7.4.1 Text Input

**Purpose:** Single-line text entry.

**HTML Structure:**
```html
<div class="form-field">
  <label for="username" class="form-field__label">Username</label>
  <input type="text" id="username" name="username" class="form-field__input" placeholder="Enter username">
  <span class="form-field__hint">Must be 3-20 characters</span>
</div>

<!-- Required field -->
<div class="form-field">
  <label for="email" class="form-field__label">
    Email
    <span class="form-field__required" aria-hidden="true">*</span>
  </label>
  <input type="email" id="email" name="email" class="form-field__input" required aria-required="true">
</div>

<!-- With prefix/suffix -->
<div class="form-field">
  <label for="price" class="form-field__label">Price</label>
  <div class="form-field__input-group">
    <span class="form-field__prefix">$</span>
    <input type="number" id="price" name="price" class="form-field__input">
    <span class="form-field__suffix">.00</span>
  </div>
</div>
```

**Variants:**
- Size: `form-field__input--sm`, `form-field__input--lg`
- Block: `form-field--block`

**States:**
- Default
- Focus
- Disabled
- Read-only
- Error
- Success

**Accessibility:**
- `<label>` with `for` attribute matching input `id`
- `aria-required="true"` for required fields
- `aria-describedby` linking to hint/error messages
- `aria-invalid="true"` for error state

**JavaScript:** None required for basic usage.

---

#### 7.4.2 Textarea

**Purpose:** Multi-line text entry.

**HTML Structure:**
```html
<div class="form-field">
  <label for="message" class="form-field__label">Message</label>
  <textarea id="message" name="message" class="form-field__textarea" rows="4" placeholder="Enter your message"></textarea>
  <span class="form-field__hint">Maximum 500 characters</span>
</div>

<!-- Auto-resize -->
<div class="form-field">
  <label for="bio" class="form-field__label">Bio</label>
  <textarea id="bio" name="bio" class="form-field__textarea form-field__textarea--auto" data-auto-resize></textarea>
</div>
```

**Variants:**
- `form-field__textarea--auto`: Auto-resize with content

**Accessibility:**
- Same requirements as text input
- Character count updates should be announced: `aria-live="polite"`

**JavaScript:** Auto-resize functionality.

---

#### 7.4.3 Select

**Purpose:** Choose from a predefined list of options.

**HTML Structure:**
```html
<div class="form-field">
  <label for="country" class="form-field__label">Country</label>
  <div class="form-field__select-wrapper">
    <select id="country" name="country" class="form-field__select">
      <option value="">Select a country</option>
      <option value="us">United States</option>
      <option value="ca">Canada</option>
      <option value="uk">United Kingdom</option>
    </select>
  </div>
</div>

<!-- Multiple select -->
<div class="form-field">
  <label for="languages" class="form-field__label">Languages</label>
  <select id="languages" name="languages" class="form-field__select" multiple size="4">
    <option value="en">English</option>
    <option value="es">Spanish</option>
    <option value="fr">French</option>
    <option value="de">German</option>
  </select>
</div>
```

**States:**
- Default
- Focus
- Disabled
- Error

**Accessibility:**
- Native `<select>` for full keyboard and screen reader support
- `<optgroup>` for grouped options

**JavaScript:** None required for native select.

---

#### 7.4.4 Custom Select (Enhanced)

**Purpose:** Stylized select with search and custom rendering.

**HTML Structure:**
```html
<div class="select" data-select>
  <button type="button" class="select__trigger" aria-haspopup="listbox" aria-expanded="false" aria-labelledby="select-label select-value">
    <span id="select-value" class="select__value">Select option</span>
    <span class="select__icon" aria-hidden="true"></span>
  </button>
  <ul class="select__listbox" role="listbox" id="select-options" aria-labelledby="select-label" hidden>
    <li class="select__option" role="option" data-value="opt1">Option One</li>
    <li class="select__option" role="option" data-value="opt2" aria-selected="true">Option Two</li>
    <li class="select__option" role="option" data-value="opt3">Option Three</li>
  </ul>
  <input type="hidden" name="custom-select" value="opt2">
</div>
```

**Keyboard Behavior:**
- Enter/Space: Open listbox
- Arrow Up/Down: Navigate options
- Enter: Select option
- Escape: Close without selecting
- Type-ahead: Jump to matching option

**Accessibility:**
- `role="listbox"` on options container
- `role="option"` on each option
- `aria-selected` on selected option
- `aria-expanded` on trigger
- `aria-activedescendant` for focus indication

**JavaScript Requirements:**
- Open/close listbox
- Keyboard navigation
- Option selection
- Type-ahead search
- Focus management

---

#### 7.4.5 Search Input

**Purpose:** Text input optimized for search queries.

**HTML Structure:**
```html
<div class="search">
  <label for="search" class="visually-hidden">Search</label>
  <span class="search__icon" aria-hidden="true">
    <!-- Search icon SVG -->
  </span>
  <input type="search" id="search" name="q" class="search__input" placeholder="Search..." aria-label="Search">
  <button type="button" class="search__clear" aria-label="Clear search" hidden>
    <!-- Clear icon SVG -->
  </button>
</div>
```

**States:**
- Empty
- Has value (show clear button)
- Loading (show spinner)

**Keyboard Behavior:**
- Escape: Clear input
- Enter: Submit search

**Accessibility:**
- `type="search"` for semantic meaning
- Clear button labeled with `aria-label`
- Results announcement via `aria-live` region

**JavaScript Requirements:**
- Show/hide clear button
- Clear button functionality
- Optional: debounced search, results handling

---

#### 7.4.6 File Input

**Purpose:** Upload file selection.

**HTML Structure:**
```html
<div class="file-input">
  <input type="file" id="file-upload" name="file" class="file-input__input" aria-describedby="file-hint">
  <label for="file-upload" class="file-input__label">
    <span class="file-input__icon" aria-hidden="true">
      <!-- Upload icon SVG -->
    </span>
    <span class="file-input__text">Choose file or drag here</span>
    <span class="file-input__selected"></span>
  </label>
  <span id="file-hint" class="file-input__hint">Maximum file size: 10MB</span>
</div>
```

**States:**
- Default
- Drag over
- File selected
- Error (invalid file)

**Accessibility:**
- Native `<input type="file">` remains accessible
- Label associated via `for` attribute
- Selected file name announced

**JavaScript Requirements:**
- Drag and drop handling
- File validation
- Selected file display

---

#### 7.4.7 Range Slider

**Purpose:** Select a value within a range.

**HTML Structure:**
```html
<div class="range">
  <label for="volume" class="range__label">
    Volume: <output id="volume-output" for="volume">50</output>%
  </label>
  <input type="range" id="volume" name="volume" class="range__input" min="0" max="100" value="50" aria-describedby="volume-output">
</div>
```

**Accessibility:**
- `<output>` for live value display
- `min`, `max`, `value` attributes
- `aria-valuemin`, `aria-valuemax`, `aria-valuenow` (native range provides these)

**JavaScript Requirements:**
- Update output on input change

---

#### 7.4.8 Date Picker

**Purpose:** Date selection input.

**HTML Structure:**
```html
<div class="form-field">
  <label for="date" class="form-field__label">Select date</label>
  <input type="date" id="date" name="date" class="form-field__input">
</div>
```

**Note:** This library uses native date input. A custom date picker may be provided as an optional enhancement but is not required for MVP.

**Accessibility:**
- Native `<input type="date">` provides built-in accessibility

**JavaScript:** None required for native input.

---

### 7.5 Form States and Validation

#### 7.5.1 Error State

**Purpose:** Indicate invalid form input.

**HTML Structure:**
```html
<div class="form-field form-field--error">
  <label for="email-error" class="form-field__label">Email</label>
  <input type="email" id="email-error" name="email" class="form-field__input" aria-invalid="true" aria-describedby="email-error-msg" value="invalid-email">
  <span id="email-error-msg" class="form-field__error" role="alert">
    Please enter a valid email address.
  </span>
</div>
```

**Accessibility:**
- `aria-invalid="true"` on input
- `aria-describedby` links to error message
- Error message uses `role="alert"` for immediate announcement

**JavaScript:** Validation logic to set/clear error state.

---

#### 7.5.2 Success State

**Purpose:** Confirm valid input.

**HTML Structure:**
```html
<div class="form-field form-field--success">
  <label for="email-success" class="form-field__label">Email</label>
  <input type="email" id="email-success" name="email" class="form-field__input" aria-describedby="email-success-msg" value="valid@email.com">
  <span id="email-success-msg" class="form-field__success">
    Email is available.
  </span>
</div>
```

**Accessibility:**
- Success message linked via `aria-describedby`
- Optional: `aria-live="polite"` for dynamic updates

---

#### 7.5.3 Disabled State

**Purpose:** Prevent interaction with form elements.

**HTML Structure:**
```html
<div class="form-field form-field--disabled">
  <label for="disabled-input" class="form-field__label">Disabled field</label>
  <input type="text" id="disabled-input" name="disabled" class="form-field__input" disabled value="Cannot edit">
</div>
```

**Accessibility:**
- `disabled` attribute on input
- Disabled elements are excluded from tab order

---

#### 7.5.4 Read-Only State

**Purpose:** Display non-editable form values.

**HTML Structure:**
```html
<div class="form-field form-field--readonly">
  <label for="readonly-input" class="form-field__label">Read-only field</label>
  <input type="text" id="readonly-input" name="readonly" class="form-field__input" readonly value="Fixed value">
</div>
```

**Accessibility:**
- `readonly` attribute on input
- Read-only elements remain in tab order

---

#### 7.5.5 Form Field Group

**Purpose:** Group related form fields with shared validation.

**HTML Structure:**
```html
<fieldset class="form-group">
  <legend class="form-group__legend">Contact Information</legend>
  <div class="form-field">
    <label for="phone" class="form-field__label">Phone</label>
    <input type="tel" id="phone" name="phone" class="form-field__input">
  </div>
  <div class="form-field">
    <label for="email" class="form-field__label">Email</label>
    <input type="email" id="email" name="email" class="form-field__input">
  </div>
</fieldset>
```

**Accessibility:**
- `<fieldset>` groups related fields
- `<legend>` provides group label

---

#### 7.5.6 Inline Validation

**Purpose:** Validate input as user types.

**HTML Structure:**
```html
<div class="form-field" data-validate="email">
  <label for="inline-email" class="form-field__label">Email</label>
  <input type="email" id="inline-email" name="email" class="form-field__input" aria-describedby="inline-email-feedback">
  <span id="inline-email-feedback" class="form-field__feedback" aria-live="polite"></span>
</div>
```

**JavaScript Requirements:**
- Debounced input validation
- Update feedback element
- Set `aria-invalid` appropriately

---

### 7.6 Feedback Components

#### 7.6.1 Alert

**Purpose:** Display important messages to users.

**HTML Structure:**
```html
<!-- Information alert -->
<div class="alert alert--info" role="alert">
  <span class="alert__icon" aria-hidden="true">
    <!-- Info icon SVG -->
  </span>
  <div class="alert__content">
    <strong class="alert__title">Information</strong>
    <p class="alert__message">This is an informational message.</p>
  </div>
</div>

<!-- Success alert -->
<div class="alert alert--success" role="status">
  <span class="alert__icon" aria-hidden="true">
    <!-- Success icon SVG -->
  </span>
  <div class="alert__content">
    <p class="alert__message">Your changes have been saved.</p>
  </div>
</div>

<!-- Warning alert -->
<div class="alert alert--warning" role="alert">
  <span class="alert__icon" aria-hidden="true">
    <!-- Warning icon SVG -->
  </span>
  <div class="alert__content">
    <p class="alert__message">Your session will expire in 5 minutes.</p>
  </div>
</div>

<!-- Error alert -->
<div class="alert alert--error" role="alert">
  <span class="alert__icon" aria-hidden="true">
    <!-- Error icon SVG -->
  </span>
  <div class="alert__content">
    <p class="alert__message">Failed to save changes. Please try again.</p>
  </div>
</div>

<!-- Dismissible alert -->
<div class="alert alert--info alert--dismissible" role="alert">
  <div class="alert__content">
    <p class="alert__message">This alert can be dismissed.</p>
  </div>
  <button type="button" class="alert__close" aria-label="Dismiss alert">
    <!-- Close icon SVG -->
  </button>
</div>
```

**Variants:**
- `alert--info`, `alert--success`, `alert--warning`, `alert--error`
- `alert--dismissible`: Includes close button
- `alert--compact`: Reduced padding

**Accessibility:**
- `role="alert"` for important messages (assertive)
- `role="status"` for status updates (polite)
- Close button has `aria-label`

**JavaScript Requirements:**
- Dismiss functionality for dismissible alerts

---

#### 7.6.2 Toast

**Purpose:** Temporary notifications that appear and auto-dismiss.

**HTML Structure:**
```html
<!-- Toast container (positioned fixed) -->
<div class="toast-container" aria-live="polite" aria-atomic="true">
  <!-- Individual toast -->
  <div class="toast toast--success" role="status">
    <span class="toast__icon" aria-hidden="true">
      <!-- Success icon SVG -->
    </span>
    <div class="toast__content">
      <p class="toast__message">Settings saved successfully.</p>
    </div>
    <button type="button" class="toast__close" aria-label="Dismiss">
      <!-- Close icon SVG -->
    </button>
  </div>
</div>
```

**Variants:**
- `toast--info`, `toast--success`, `toast--warning`, `toast--error`

**Positions:**
- `toast-container--top-right` (default)
- `toast-container--top-left`
- `toast-container--bottom-right`
- `toast-container--bottom-left`
- `toast-container--top-center`
- `toast-container--bottom-center`

**Accessibility:**
- Container: `aria-live="polite"` for non-critical, `aria-live="assertive"` for critical
- `aria-atomic="true"` announces entire toast
- Sufficient display time (minimum 5 seconds or user-controlled)
- Pause auto-dismiss on hover/focus

**JavaScript Requirements:**
- Toast creation and insertion
- Auto-dismiss timer
- Dismiss on close button click
- Pause timer on interaction
- Queue management for multiple toasts

**JavaScript API:**
```javascript
// Programmatic usage
toast.show({
  message: 'Settings saved successfully.',
  type: 'success',
  duration: 5000
});
```

---

#### 7.6.3 Spinner (Loading Indicator)

**Purpose:** Indicate loading or processing state.

**HTML Structure:**
```html
<!-- Basic spinner -->
<div class="spinner" role="status" aria-label="Loading">
  <span class="spinner__circle"></span>
</div>

<!-- Spinner with text -->
<div class="spinner spinner--text" role="status">
  <span class="spinner__circle" aria-hidden="true"></span>
  <span class="spinner__label">Loading...</span>
</div>

<!-- Inline spinner -->
<span class="spinner spinner--inline" role="status" aria-label="Loading"></span>
```

**Variants:**
- Size: `spinner--sm`, `spinner--md` (default), `spinner--lg`
- `spinner--inline`: For use within text/buttons
- `spinner--text`: Includes visible label

**Accessibility:**
- `role="status"` for non-blocking indicator
- `aria-label` when no visible text

**JavaScript:** None required (CSS animation).

---

#### 7.6.4 Skeleton Loader

**Purpose:** Placeholder content during loading.

**HTML Structure:**
```html
<div class="skeleton-container" aria-busy="true" aria-label="Loading content">
  <div class="skeleton skeleton--text"></div>
  <div class="skeleton skeleton--text skeleton--text-sm"></div>
  <div class="skeleton skeleton--text skeleton--text-lg"></div>
  <div class="skeleton skeleton--circle"></div>
  <div class="skeleton skeleton--rect"></div>
</div>
```

**Variants:**
- `skeleton--text`: Line of text (various widths)
- `skeleton--circle`: Avatar/icon placeholder
- `skeleton--rect`: Rectangle (image/card placeholder)

**Accessibility:**
- Container: `aria-busy="true"` while loading
- `aria-label` describes what is loading
- Remove skeleton and `aria-busy` when content loads

**JavaScript:** None for skeleton display. Content replacement requires JS.

---

#### 7.6.5 Progress Bar

**Purpose:** Show progress of a task.

**HTML Structure:**
```html
<!-- Determinate progress -->
<div class="progress" role="progressbar" aria-valuenow="45" aria-valuemin="0" aria-valuemax="100" aria-label="Upload progress">
  <div class="progress__bar" style="width: 45%"></div>
  <span class="progress__label">45%</span>
</div>

<!-- Indeterminate progress -->
<div class="progress progress--indeterminate" role="progressbar" aria-label="Loading">
  <div class="progress__bar"></div>
</div>

<!-- With steps -->
<div class="progress progress--steps" role="progressbar" aria-valuenow="2" aria-valuemin="1" aria-valuemax="4" aria-label="Step 2 of 4">
  <div class="progress__step progress__step--complete">1</div>
  <div class="progress__step progress__step--current" aria-current="step">2</div>
  <div class="progress__step">3</div>
  <div class="progress__step">4</div>
</div>
```

**Variants:**
- `progress--sm`, `progress--md` (default), `progress--lg`
- `progress--indeterminate`: Animated loading without known progress
- `progress--steps`: Step indicator

**Accessibility:**
- `role="progressbar"`
- `aria-valuenow`, `aria-valuemin`, `aria-valuemax` for determinate
- `aria-label` or visible label
- Announce significant progress changes

**JavaScript:** Update `aria-valuenow` and width on progress change.

---

### 7.7 Overlays

#### 7.7.1 Modal Dialog

**Purpose:** Focus user attention on a specific task or message.

**HTML Structure:**
```html
<!-- Modal trigger -->
<button type="button" class="btn btn--primary" data-modal-open="example-modal">
  Open Modal
</button>

<!-- Modal -->
<div class="modal" id="example-modal" role="dialog" aria-modal="true" aria-labelledby="modal-title" hidden>
  <div class="modal__backdrop" data-modal-close></div>
  <div class="modal__container">
    <header class="modal__header">
      <h2 id="modal-title" class="modal__title">Modal Title</h2>
      <button type="button" class="modal__close" data-modal-close aria-label="Close modal">
        <!-- Close icon SVG -->
      </button>
    </header>
    <div class="modal__body">
      <p>Modal content goes here.</p>
    </div>
    <footer class="modal__footer">
      <button type="button" class="btn btn--secondary" data-modal-close>Cancel</button>
      <button type="button" class="btn btn--primary">Confirm</button>
    </footer>
  </div>
</div>
```

**Variants:**
- Size: `modal--sm`, `modal--md` (default), `modal--lg`, `modal--full`
- `modal--centered`: Vertically centered

**States:**
- Hidden (default)
- Open

**Keyboard Behavior:**
- Escape: Close modal
- Tab: Cycle through focusable elements within modal (trapped)
- Shift+Tab: Reverse cycle

**Accessibility:**
- `role="dialog"` and `aria-modal="true"`
- `aria-labelledby` referencing modal title
- Focus trapped within modal when open
- Focus returns to trigger element on close
- Background content is `aria-hidden="true"` and `inert` when modal is open

**JavaScript Requirements:**
- Open/close modal
- Focus trap implementation
- Return focus to trigger
- Set `inert` on background content
- Prevent body scroll when open
- Escape key handling

---

#### 7.7.2 Confirmation Dialog

**Purpose:** Confirm destructive or important actions.

**HTML Structure:**
```html
<div class="modal modal--sm" id="confirm-dialog" role="alertdialog" aria-modal="true" aria-labelledby="confirm-title" aria-describedby="confirm-desc" hidden>
  <div class="modal__backdrop" data-modal-close></div>
  <div class="modal__container">
    <header class="modal__header">
      <h2 id="confirm-title" class="modal__title">Delete Item?</h2>
    </header>
    <div class="modal__body">
      <p id="confirm-desc">This action cannot be undone. Are you sure you want to delete this item?</p>
    </div>
    <footer class="modal__footer">
      <button type="button" class="btn btn--secondary" data-modal-close>Cancel</button>
      <button type="button" class="btn btn--destructive" data-confirm-action>Delete</button>
    </footer>
  </div>
</div>
```

**Accessibility:**
- `role="alertdialog"` for confirmations
- `aria-describedby` for descriptive message
- Focus should go to least destructive action (Cancel) by default

---

#### 7.7.3 Drawer (Slide-out Panel)

**Purpose:** Supplementary content that slides in from screen edge.

**HTML Structure:**
```html
<button type="button" class="btn btn--primary" data-drawer-open="settings-drawer">
  Open Settings
</button>

<div class="drawer drawer--right" id="settings-drawer" role="dialog" aria-modal="true" aria-labelledby="drawer-title" hidden>
  <div class="drawer__backdrop" data-drawer-close></div>
  <div class="drawer__panel">
    <header class="drawer__header">
      <h2 id="drawer-title" class="drawer__title">Settings</h2>
      <button type="button" class="drawer__close" data-drawer-close aria-label="Close settings">
        <!-- Close icon SVG -->
      </button>
    </header>
    <div class="drawer__body">
      <!-- Drawer content -->
    </div>
    <footer class="drawer__footer">
      <button type="button" class="btn btn--primary">Save Changes</button>
    </footer>
  </div>
</div>
```

**Variants:**
- Position: `drawer--left`, `drawer--right`, `drawer--top`, `drawer--bottom`
- Size: `drawer--sm`, `drawer--md` (default), `drawer--lg`

**Accessibility:**
- Same requirements as modal
- Focus trap within drawer
- Escape closes drawer

**JavaScript Requirements:**
- Same as modal
- Slide animation handling

---

#### 7.7.4 Dropdown Menu

**Purpose:** Display a list of actions or options.

**HTML Structure:**
```html
<div class="dropdown">
  <button type="button" class="dropdown__trigger btn btn--secondary" aria-haspopup="menu" aria-expanded="false" aria-controls="dropdown-menu">
    Actions
    <span class="dropdown__icon" aria-hidden="true"></span>
  </button>
  <div class="dropdown__menu" id="dropdown-menu" role="menu" hidden>
    <button type="button" class="dropdown__item" role="menuitem">Edit</button>
    <button type="button" class="dropdown__item" role="menuitem">Duplicate</button>
    <hr class="dropdown__divider" role="separator">
    <button type="button" class="dropdown__item dropdown__item--destructive" role="menuitem">Delete</button>
  </div>
</div>
```

**Variants:**
- Alignment: `dropdown--left` (default), `dropdown--right`
- `dropdown--up`: Opens upward

**States:**
- Closed (default)
- Open

**Keyboard Behavior:**
- Enter/Space on trigger: Toggle menu
- Arrow Down: Open menu, move to first item
- Arrow Up: Move to last item
- Arrow keys within menu: Navigate items
- Enter: Activate item
- Escape: Close menu
- Tab: Close menu, move focus to next element

**Accessibility:**
- Trigger: `aria-haspopup="menu"`, `aria-expanded`, `aria-controls`
- Menu: `role="menu"`
- Items: `role="menuitem"`
- Dividers: `role="separator"`

**JavaScript Requirements:**
- Toggle menu visibility
- Keyboard navigation
- Click outside to close
- Focus management

---

#### 7.7.5 Tooltip

**Purpose:** Provide supplementary information on hover/focus.

**HTML Structure:**
```html
<button type="button" class="btn btn--icon" aria-describedby="tooltip-1">
  <span aria-hidden="true">?</span>
</button>
<div class="tooltip" id="tooltip-1" role="tooltip" hidden>
  Click for more information
</div>
```

**Variants:**
- Position: `tooltip--top` (default), `tooltip--bottom`, `tooltip--left`, `tooltip--right`

**States:**
- Hidden (default)
- Visible

**Accessibility:**
- `role="tooltip"` on tooltip element
- `aria-describedby` on trigger referencing tooltip `id`
- Appear on focus, not just hover
- Sufficient delay before hiding (allow mouse to reach tooltip)

**JavaScript Requirements:**
- Show on hover/focus
- Hide on mouse leave/blur
- Position calculation
- Delay handling

---

#### 7.7.6 Popover

**Purpose:** Rich content display on interaction (more complex than tooltip).

**HTML Structure:**
```html
<button type="button" class="btn btn--secondary" data-popover-trigger aria-expanded="false" aria-controls="popover-1">
  More Info
</button>
<div class="popover" id="popover-1" hidden>
  <div class="popover__header">
    <h4 class="popover__title">Popover Title</h4>
    <button type="button" class="popover__close" aria-label="Close popover">
      <!-- Close icon -->
    </button>
  </div>
  <div class="popover__body">
    <p>Rich content including links, buttons, etc.</p>
    <a href="#">Learn more</a>
  </div>
</div>
```

**Variants:**
- Position: `popover--top`, `popover--bottom`, `popover--left`, `popover--right`
- `popover--interactive`: Remains open when content is interacted with

**Keyboard Behavior:**
- Enter/Space on trigger: Toggle popover
- Escape: Close popover
- Tab through popover content when open
- Tab out closes popover

**Accessibility:**
- `aria-expanded` on trigger
- `aria-controls` linking trigger to popover
- Focus moves into popover on open (first focusable element)

**JavaScript Requirements:**
- Toggle visibility
- Position calculation
- Click outside to close
- Focus management

---

### 7.8 Data Display

#### 7.8.1 Table

**Purpose:** Display structured tabular data.

**HTML Structure:**
```html
<div class="table-container">
  <table class="table">
    <caption class="table__caption">User accounts</caption>
    <thead class="table__head">
      <tr>
        <th scope="col" class="table__th">Name</th>
        <th scope="col" class="table__th">Email</th>
        <th scope="col" class="table__th table__th--numeric">Balance</th>
        <th scope="col" class="table__th">Status</th>
        <th scope="col" class="table__th">
          <span class="visually-hidden">Actions</span>
        </th>
      </tr>
    </thead>
    <tbody class="table__body">
      <tr class="table__row">
        <td class="table__td">John Doe</td>
        <td class="table__td">john@example.com</td>
        <td class="table__td table__td--numeric">$1,234.56</td>
        <td class="table__td"><span class="badge badge--success">Active</span></td>
        <td class="table__td table__td--actions">
          <button class="btn btn--sm btn--ghost">Edit</button>
        </td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Sortable table -->
<table class="table table--sortable">
  <thead>
    <tr>
      <th scope="col" class="table__th" aria-sort="ascending">
        <button class="table__sort">
          Name
          <span class="table__sort-icon" aria-hidden="true"></span>
        </button>
      </th>
    </tr>
  </thead>
</table>
```

**Variants:**
- `table--striped`: Alternating row colors
- `table--bordered`: Visible cell borders
- `table--hover`: Row hover highlighting
- `table--compact`: Reduced padding
- `table--sortable`: Sortable columns

**Accessibility:**
- `<caption>` or `aria-label` for table description
- `scope="col"` on column headers
- `scope="row"` on row headers
- `aria-sort` on sorted columns
- Actions column header needs accessible name

**JavaScript Requirements:**
- Sorting functionality for sortable tables

---

#### 7.8.2 Description List

**Purpose:** Display key-value pairs.

**HTML Structure:**
```html
<dl class="dl">
  <div class="dl__item">
    <dt class="dl__term">Full Name</dt>
    <dd class="dl__description">John Doe</dd>
  </div>
  <div class="dl__item">
    <dt class="dl__term">Email</dt>
    <dd class="dl__description">john@example.com</dd>
  </div>
</dl>

<!-- Horizontal variant -->
<dl class="dl dl--horizontal">
  <div class="dl__item">
    <dt class="dl__term">Status</dt>
    <dd class="dl__description">Active</dd>
  </div>
</dl>
```

**Variants:**
- `dl--horizontal`: Term and description on same line
- `dl--bordered`: Separating borders

**Accessibility:** Native `<dl>`, `<dt>`, `<dd>` provide semantic structure.

**JavaScript:** None required.

---

#### 7.8.3 List

**Purpose:** Display sequential items.

**HTML Structure:**
```html
<!-- Unordered list -->
<ul class="list">
  <li class="list__item">First item</li>
  <li class="list__item">Second item</li>
  <li class="list__item">Third item</li>
</ul>

<!-- Ordered list -->
<ol class="list list--ordered">
  <li class="list__item">Step one</li>
  <li class="list__item">Step two</li>
  <li class="list__item">Step three</li>
</ol>

<!-- Interactive list -->
<ul class="list list--interactive" role="listbox" aria-label="Select an option">
  <li class="list__item" role="option" tabindex="0">Option one</li>
  <li class="list__item list__item--selected" role="option" tabindex="0" aria-selected="true">Option two</li>
  <li class="list__item" role="option" tabindex="0">Option three</li>
</ul>
```

**Variants:**
- `list--unstyled`: No bullets or numbers
- `list--ordered`: Numbered list
- `list--horizontal`: Inline items
- `list--interactive`: Selectable items
- `list--divided`: Dividers between items

**Accessibility:**
- Use semantic `<ul>` or `<ol>`
- Interactive lists need `role="listbox"` and `role="option"`

**JavaScript:** Selection handling for interactive lists.

---

#### 7.8.4 Badge

**Purpose:** Label or status indicator.

**HTML Structure:**
```html
<!-- Status badges -->
<span class="badge">Default</span>
<span class="badge badge--primary">Primary</span>
<span class="badge badge--success">Active</span>
<span class="badge badge--warning">Pending</span>
<span class="badge badge--error">Failed</span>
<span class="badge badge--info">New</span>

<!-- Outline badges -->
<span class="badge badge--outline">Outline</span>
<span class="badge badge--outline badge--success">Success</span>

<!-- With remove button -->
<span class="badge badge--removable">
  Tag
  <button type="button" class="badge__remove" aria-label="Remove tag">
    <!-- Close icon -->
  </button>
</span>

<!-- Notification dot -->
<span class="badge badge--dot" aria-label="New notifications"></span>

<!-- Badge with count -->
<span class="badge badge--count">42</span>
```

**Variants:**
- `badge--primary`, `badge--success`, `badge--warning`, `badge--error`, `badge--info`
- `badge--outline`: Outline style
- `badge--dot`: Small indicator dot
- `badge--count`: Numeric badge
- `badge--removable`: Includes remove button

**Accessibility:**
- Decorative badges may need `aria-hidden="true"`
- Functional badges need accessible labels
- Remove buttons need `aria-label`

**JavaScript:** Remove button functionality.

---

#### 7.8.5 Avatar

**Purpose:** Represent users visually.

**HTML Structure:**
```html
<!-- Image avatar -->
<img class="avatar" src="user.jpg" alt="John Doe">

<!-- Initials avatar -->
<span class="avatar avatar--initials" aria-label="John Doe">JD</span>

<!-- Placeholder avatar -->
<span class="avatar avatar--placeholder" aria-label="Unknown user">
  <!-- User icon SVG -->
</span>

<!-- Avatar with status -->
<div class="avatar-wrapper">
  <img class="avatar" src="user.jpg" alt="John Doe">
  <span class="avatar__status avatar__status--online" aria-label="Online"></span>
</div>

<!-- Avatar group -->
<div class="avatar-group">
  <img class="avatar" src="user1.jpg" alt="User 1">
  <img class="avatar" src="user2.jpg" alt="User 2">
  <img class="avatar" src="user3.jpg" alt="User 3">
  <span class="avatar avatar--count">+5</span>
</div>
```

**Variants:**
- Size: `avatar--xs`, `avatar--sm`, `avatar--md` (default), `avatar--lg`, `avatar--xl`
- Shape: `avatar--rounded` (default), `avatar--square`
- `avatar--initials`: Text initials
- `avatar--placeholder`: Icon placeholder

**Accessibility:**
- `alt` text on images
- `aria-label` on non-image avatars
- Status indicators need accessible labels

**JavaScript:** Fallback to initials on image load error.

---

#### 7.8.6 Tag / Chip

**Purpose:** Categorize or label items.

**HTML Structure:**
```html
<!-- Simple tag -->
<span class="tag">Category</span>

<!-- Linked tag -->
<a href="/category/tech" class="tag">Technology</a>

<!-- Removable tag -->
<span class="tag tag--removable">
  JavaScript
  <button type="button" class="tag__remove" aria-label="Remove JavaScript tag">
    <!-- Close icon -->
  </button>
</span>

<!-- Tag with icon -->
<span class="tag tag--icon">
  <!-- Icon SVG -->
  Label
</span>
```

**Variants:**
- `tag--primary`, `tag--secondary`
- `tag--removable`: Includes remove button
- Size: `tag--sm`, `tag--lg`

**Accessibility:**
- Linked tags use `<a>` element
- Remove buttons need `aria-label`
- Tag groups should use list semantics when appropriate

**JavaScript:** Remove button functionality.

---

#### 7.8.7 Key-Value Pair

**Purpose:** Display single key-value data inline.

**HTML Structure:**
```html
<div class="kv">
  <span class="kv__key">Status:</span>
  <span class="kv__value">Active</span>
</div>

<!-- Stacked variant -->
<div class="kv kv--stacked">
  <span class="kv__key">Total Revenue</span>
  <span class="kv__value">$12,345.67</span>
</div>
```

**Variants:**
- `kv--inline` (default): Horizontal layout
- `kv--stacked`: Vertical layout

**Accessibility:** Use `<dl>`, `<dt>`, `<dd>` when in a list context.

**JavaScript:** None required.

---

#### 7.8.8 Stat Card

**Purpose:** Display prominent metrics or statistics.

**HTML Structure:**
```html
<div class="stat">
  <span class="stat__label">Total Users</span>
  <span class="stat__value">12,345</span>
  <span class="stat__change stat__change--positive">
    +12.5%
  </span>
</div>

<!-- With icon -->
<div class="stat stat--icon">
  <span class="stat__icon" aria-hidden="true">
    <!-- Icon SVG -->
  </span>
  <div class="stat__content">
    <span class="stat__label">Revenue</span>
    <span class="stat__value">$98,765</span>
  </div>
</div>
```

**Variants:**
- `stat--icon`: Includes icon
- `stat--compact`: Reduced size
- Change indicators: `stat__change--positive`, `stat__change--negative`

**Accessibility:**
- Values should be readable by screen readers
- Change direction should not rely on color alone (include +/- or text)

**JavaScript:** None required.

---

### 7.9 Code and Developer UI

#### 7.9.1 Code Block

**Purpose:** Display formatted source code.

**HTML Structure:**
```html
<div class="code-block">
  <header class="code-block__header">
    <span class="code-block__lang">javascript</span>
    <button type="button" class="code-block__copy" data-copy-code aria-label="Copy code to clipboard">
      Copy
    </button>
  </header>
  <pre class="code-block__pre"><code class="code-block__code">function greet(name) {
  console.log(`Hello, ${name}!`);
}</code></pre>
</div>

<!-- With line numbers -->
<div class="code-block code-block--numbered">
  <pre class="code-block__pre"><code class="code-block__code"><span class="code-block__line">const x = 1;</span>
<span class="code-block__line">const y = 2;</span>
<span class="code-block__line">console.log(x + y);</span></code></pre>
</div>

<!-- Highlighted lines -->
<div class="code-block">
  <pre class="code-block__pre"><code class="code-block__code"><span class="code-block__line">import { Component } from 'react';</span>
<span class="code-block__line code-block__line--highlight">export default function App() {</span>
<span class="code-block__line code-block__line--highlight">  return &lt;div&gt;Hello&lt;/div&gt;;</span>
<span class="code-block__line code-block__line--highlight">}</span></code></pre>
</div>
```

**Variants:**
- `code-block--numbered`: Show line numbers
- `code-block--wrap`: Wrap long lines
- Line highlighting: `code-block__line--highlight`, `code-block__line--added`, `code-block__line--removed`

**States:**
- Default
- Copy success (temporary feedback)

**Accessibility:**
- `<pre>` and `<code>` for semantic markup
- Copy button has accessible label
- Copy confirmation announced to screen readers

**JavaScript Requirements:**
- Copy to clipboard functionality
- Copy success feedback

---

#### 7.9.2 Inline Code

**Purpose:** Mark code within prose text.

**HTML Structure:**
```html
<p>Use the <code class="code">console.log()</code> function to debug.</p>
```

**Accessibility:** Native `<code>` element provides semantic meaning.

**JavaScript:** None required.

---

#### 7.9.3 Keyboard Shortcut (Kbd)

**Purpose:** Display keyboard key combinations.

**HTML Structure:**
```html
<p>Press <kbd class="kbd">Ctrl</kbd> + <kbd class="kbd">C</kbd> to copy.</p>

<!-- Combined keys -->
<kbd class="kbd-group">
  <kbd class="kbd">Cmd</kbd>
  <kbd class="kbd">Shift</kbd>
  <kbd class="kbd">P</kbd>
</kbd>
```

**Accessibility:** Native `<kbd>` element provides semantic meaning.

**JavaScript:** None required.

---

#### 7.9.4 Copy Button

**Purpose:** Copy associated content to clipboard.

**HTML Structure:**
```html
<div class="copy-container">
  <code class="code" id="copy-target">npm install package-name</code>
  <button type="button" class="btn-copy" data-copy-target="copy-target" aria-label="Copy to clipboard">
    <span class="btn-copy__icon btn-copy__icon--copy" aria-hidden="true">
      <!-- Copy icon -->
    </span>
    <span class="btn-copy__icon btn-copy__icon--success" aria-hidden="true" hidden>
      <!-- Check icon -->
    </span>
  </button>
</div>
```

**States:**
- Default: Copy icon visible
- Success: Check icon visible (temporary, ~2 seconds)

**Accessibility:**
- `aria-label` describes action
- Success state announced via `aria-live` region or updated label

**JavaScript Requirements:**
- Copy to clipboard
- Visual feedback toggle
- Announcement of success

---

#### 7.9.5 Terminal Output

**Purpose:** Display command-line interface output.

**HTML Structure:**
```html
<div class="terminal">
  <header class="terminal__header">
    <span class="terminal__title">Terminal</span>
    <button type="button" class="terminal__copy" aria-label="Copy terminal output">
      Copy
    </button>
  </header>
  <div class="terminal__body">
    <pre class="terminal__output"><span class="terminal__prompt">$</span> npm install
<span class="terminal__stdout">added 150 packages in 3.2s</span>
<span class="terminal__prompt">$</span> npm run build
<span class="terminal__stdout">Build complete.</span></pre>
  </div>
</div>
```

**Variants:**
- `terminal--dark` (default)
- `terminal--light`

**Accessibility:**
- Content in `<pre>` for preservation
- Copy button accessible

**JavaScript:** Copy functionality.

---

### 7.10 Navigation Aids

#### 7.10.1 Back to Top

**Purpose:** Quick navigation to page top.

**HTML Structure:**
```html
<button type="button" class="back-to-top" aria-label="Back to top" hidden>
  <span aria-hidden="true">
    <!-- Up arrow icon -->
  </span>
</button>
```

**States:**
- Hidden (at page top)
- Visible (after scrolling threshold)

**Keyboard Behavior:**
- Enter/Space: Scroll to top, focus moves to top of page

**Accessibility:**
- `aria-label` describes action
- Focus management after scroll

**JavaScript Requirements:**
- Show/hide based on scroll position
- Smooth scroll to top
- Focus management

---

#### 7.10.2 Scroll Progress Indicator

**Purpose:** Show reading/scroll progress.

**HTML Structure:**
```html
<div class="scroll-progress" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100" aria-label="Reading progress">
  <div class="scroll-progress__bar"></div>
</div>
```

**Accessibility:**
- `role="progressbar"` with value attributes
- May be `aria-hidden="true"` if purely decorative

**JavaScript Requirements:**
- Calculate scroll percentage
- Update bar width and ARIA values

---

#### 7.10.3 Table of Contents

**Purpose:** Navigate within long documents.

**HTML Structure:**
```html
<nav class="toc" aria-label="Table of Contents">
  <h4 class="toc__heading">On this page</h4>
  <ul class="toc__list">
    <li class="toc__item">
      <a href="#section-1" class="toc__link toc__link--active" aria-current="location">Introduction</a>
    </li>
    <li class="toc__item">
      <a href="#section-2" class="toc__link">Getting Started</a>
      <ul class="toc__list toc__list--nested">
        <li class="toc__item">
          <a href="#section-2-1" class="toc__link">Installation</a>
        </li>
        <li class="toc__item">
          <a href="#section-2-2" class="toc__link">Configuration</a>
        </li>
      </ul>
    </li>
  </ul>
</nav>
```

**States:**
- Active section: `toc__link--active`

**Accessibility:**
- `<nav>` with `aria-label`
- `aria-current="location"` on active item
- Semantic nested lists

**JavaScript Requirements:**
- Scroll spy to update active state
- Smooth scroll to section

---

#### 7.10.4 Anchor Link

**Purpose:** Deep link to specific page sections.

**HTML Structure:**
```html
<h2 id="section-title" class="anchor-target">
  Section Title
  <a href="#section-title" class="anchor-link" aria-label="Link to Section Title">
    #
  </a>
</h2>
```

**States:**
- Hidden by default
- Visible on heading hover/focus

**Accessibility:**
- `aria-label` describes the link purpose
- Heading `id` for fragment navigation

**JavaScript:** None required (CSS hover state).

---

### 7.11 Interactive Utilities

#### 7.11.1 Accordion

**Purpose:** Expandable/collapsible content sections.

**HTML Structure:**
```html
<div class="accordion">
  <div class="accordion__item">
    <h3 class="accordion__header">
      <button type="button" class="accordion__trigger" aria-expanded="false" aria-controls="accordion-panel-1">
        <span class="accordion__title">Section One</span>
        <span class="accordion__icon" aria-hidden="true"></span>
      </button>
    </h3>
    <div class="accordion__panel" id="accordion-panel-1" hidden>
      <div class="accordion__content">
        <p>Content for section one.</p>
      </div>
    </div>
  </div>
  <div class="accordion__item">
    <h3 class="accordion__header">
      <button type="button" class="accordion__trigger" aria-expanded="true" aria-controls="accordion-panel-2">
        <span class="accordion__title">Section Two</span>
        <span class="accordion__icon" aria-hidden="true"></span>
      </button>
    </h3>
    <div class="accordion__panel" id="accordion-panel-2">
      <div class="accordion__content">
        <p>Content for section two.</p>
      </div>
    </div>
  </div>
</div>
```

**Variants:**
- `accordion--bordered`: Visible borders
- `accordion--flush`: No outer borders
- `accordion--single`: Only one panel open at a time

**States:**
- Collapsed: `aria-expanded="false"`, panel `hidden`
- Expanded: `aria-expanded="true"`, panel visible

**Keyboard Behavior:**
- Enter/Space: Toggle panel
- Arrow Up/Down: Navigate between accordion headers
- Home: First header
- End: Last header

**Accessibility:**
- Headers wrapped in appropriate heading level
- `aria-expanded` on trigger
- `aria-controls` linking trigger to panel
- Panel uses `hidden` attribute when collapsed

**JavaScript Requirements:**
- Toggle panel visibility
- Update `aria-expanded`
- Optional: Single-panel-open enforcement
- Keyboard navigation

---

#### 7.11.2 Disclosure (Show/Hide)

**Purpose:** Simple expandable content toggle.

**HTML Structure:**
```html
<div class="disclosure">
  <button type="button" class="disclosure__trigger" aria-expanded="false" aria-controls="disclosure-content">
    <span class="disclosure__icon" aria-hidden="true"></span>
    Show more details
  </button>
  <div class="disclosure__content" id="disclosure-content" hidden>
    <p>Additional details that were hidden.</p>
  </div>
</div>

<!-- Using native details/summary -->
<details class="disclosure">
  <summary class="disclosure__summary">Show more details</summary>
  <div class="disclosure__content">
    <p>Additional details that were hidden.</p>
  </div>
</details>
```

**Accessibility:**
- Native `<details>`/`<summary>` provides built-in accessibility
- Custom implementation needs `aria-expanded` and `aria-controls`

**JavaScript:** None required for native `<details>`. Custom needs toggle logic.

---

#### 7.11.3 Filter Controls

**Purpose:** Filter displayed content.

**HTML Structure:**
```html
<div class="filters" role="group" aria-label="Filter components">
  <button type="button" class="filters__btn filters__btn--active" aria-pressed="true" data-filter="all">
    All
  </button>
  <button type="button" class="filters__btn" aria-pressed="false" data-filter="layout">
    Layout
  </button>
  <button type="button" class="filters__btn" aria-pressed="false" data-filter="forms">
    Forms
  </button>
  <button type="button" class="filters__btn" aria-pressed="false" data-filter="feedback">
    Feedback
  </button>
</div>

<div class="filter-results" aria-live="polite">
  <div class="filter-item" data-category="layout">Grid component</div>
  <div class="filter-item" data-category="forms">Input component</div>
  <div class="filter-item" data-category="feedback">Alert component</div>
</div>
```

**Accessibility:**
- `role="group"` with `aria-label`
- `aria-pressed` on toggle buttons
- Results region uses `aria-live="polite"`
- Announce filter results count

**JavaScript Requirements:**
- Filter content visibility
- Update `aria-pressed` states
- Announce result count changes

---

#### 7.11.4 Collapsible Section

**Purpose:** Toggle visibility of page sections.

**HTML Structure:**
```html
<section class="collapsible">
  <header class="collapsible__header">
    <h2 class="collapsible__title">Advanced Settings</h2>
    <button type="button" class="collapsible__toggle" aria-expanded="true" aria-controls="collapsible-content">
      <span class="visually-hidden">Toggle section</span>
      <span class="collapsible__icon" aria-hidden="true"></span>
    </button>
  </header>
  <div class="collapsible__content" id="collapsible-content">
    <!-- Section content -->
  </div>
</section>
```

**Accessibility:**
- `aria-expanded` on toggle button
- `aria-controls` linking to content

**JavaScript:** Toggle visibility and ARIA state.

---

### 7.12 Media and Content Helpers

#### 7.12.1 Figure

**Purpose:** Image with optional caption.

**HTML Structure:**
```html
<figure class="figure">
  <img class="figure__image" src="image.jpg" alt="Description of image">
  <figcaption class="figure__caption">Figure 1: Image caption text.</figcaption>
</figure>

<!-- Full width -->
<figure class="figure figure--full">
  <img class="figure__image" src="wide-image.jpg" alt="Description">
  <figcaption class="figure__caption">A full-width image.</figcaption>
</figure>
```

**Variants:**
- `figure--full`: Full container width
- `figure--bordered`: Border around image

**Accessibility:**
- `alt` attribute required on images
- `<figcaption>` associated via `<figure>`

**JavaScript:** None required.

---

#### 7.12.2 Empty State

**Purpose:** Placeholder for empty content areas.

**HTML Structure:**
```html
<div class="empty-state">
  <span class="empty-state__icon" aria-hidden="true">
    <!-- Icon SVG -->
  </span>
  <h3 class="empty-state__title">No results found</h3>
  <p class="empty-state__description">Try adjusting your search or filter criteria.</p>
  <button type="button" class="btn btn--primary">Clear filters</button>
</div>
```

**Variants:**
- `empty-state--compact`: Reduced padding
- `empty-state--inline`: Inline layout

**Accessibility:** Ensure actionable elements are keyboard accessible.

**JavaScript:** None required.

---

#### 7.12.3 Text Highlight / Mark

**Purpose:** Emphasize specific text.

**HTML Structure:**
```html
<p>Search results for "<mark class="highlight">component</mark>" found in 5 documents.</p>

<!-- Variants -->
<mark class="highlight highlight--warning">Warning text</mark>
<mark class="highlight highlight--info">Informational text</mark>
```

**Variants:**
- `highlight` (default): Standard highlight
- `highlight--warning`, `highlight--info`, `highlight--success`

**Accessibility:** Native `<mark>` element provides semantic emphasis.

**JavaScript:** None required.

---

#### 7.12.4 Blockquote

**Purpose:** Display quoted content.

**HTML Structure:**
```html
<blockquote class="blockquote">
  <p class="blockquote__text">The best way to predict the future is to invent it.</p>
  <footer class="blockquote__footer">
    <cite class="blockquote__cite">Alan Kay</cite>
  </footer>
</blockquote>
```

**Variants:**
- `blockquote--bordered`: Left border accent
- `blockquote--centered`: Centered text

**Accessibility:** Native `<blockquote>` and `<cite>` provide semantic structure.

**JavaScript:** None required.

---

#### 7.12.5 Callout / Admonition

**Purpose:** Highlight important information within content.

**HTML Structure:**
```html
<aside class="callout callout--info" role="note">
  <span class="callout__icon" aria-hidden="true">
    <!-- Info icon -->
  </span>
  <div class="callout__content">
    <strong class="callout__title">Note</strong>
    <p class="callout__text">This is an informational callout.</p>
  </div>
</aside>

<aside class="callout callout--warning" role="note">
  <span class="callout__icon" aria-hidden="true">
    <!-- Warning icon -->
  </span>
  <div class="callout__content">
    <strong class="callout__title">Warning</strong>
    <p class="callout__text">This action cannot be undone.</p>
  </div>
</aside>

<aside class="callout callout--tip" role="note">
  <span class="callout__icon" aria-hidden="true">
    <!-- Lightbulb icon -->
  </span>
  <div class="callout__content">
    <strong class="callout__title">Tip</strong>
    <p class="callout__text">You can also use keyboard shortcuts.</p>
  </div>
</aside>
```

**Variants:**
- `callout--info`, `callout--warning`, `callout--tip`, `callout--error`
- `callout--compact`: Reduced padding

**Accessibility:**
- `role="note"` for supplementary information
- Title distinguishes callout type

**JavaScript:** None required.

---

### 7.13 User Action Blocks

#### 7.13.1 Retry Block

**Purpose:** Display error state with retry action.

**HTML Structure:**
```html
<div class="retry-block" role="alert">
  <span class="retry-block__icon" aria-hidden="true">
    <!-- Error icon -->
  </span>
  <div class="retry-block__content">
    <h4 class="retry-block__title">Failed to load data</h4>
    <p class="retry-block__message">There was a problem connecting to the server.</p>
  </div>
  <button type="button" class="btn btn--primary" data-retry>
    Try again
  </button>
</div>
```

**Accessibility:**
- `role="alert"` for error announcement
- Retry button is keyboard accessible

**JavaScript:** Retry button triggers data reload.

---

#### 7.13.2 Dismissible Panel

**Purpose:** Content block that can be permanently or temporarily dismissed.

**HTML Structure:**
```html
<div class="dismissible" data-dismissible>
  <div class="dismissible__content">
    <p>Welcome to the new dashboard. Take a tour to learn about new features.</p>
  </div>
  <button type="button" class="dismissible__close" data-dismiss aria-label="Dismiss message">
    <!-- Close icon -->
  </button>
</div>

<!-- With don't show again -->
<div class="dismissible" data-dismissible="welcome-banner">
  <div class="dismissible__content">
    <p>Welcome message content.</p>
    <label class="checkbox checkbox--sm">
      <input type="checkbox" class="checkbox__input" data-dismiss-permanent>
      <span class="checkbox__control" aria-hidden="true"></span>
      <span class="checkbox__label">Don't show again</span>
    </label>
  </div>
  <button type="button" class="dismissible__close" data-dismiss aria-label="Dismiss">
    <!-- Close icon -->
  </button>
</div>
```

**Accessibility:**
- Close button has `aria-label`
- Focus moves appropriately after dismissal

**JavaScript Requirements:**
- Dismiss functionality
- Optional: localStorage persistence for "don't show again"

---

#### 7.13.3 Confirmation Block

**Purpose:** Inline confirmation for actions.

**HTML Structure:**
```html
<div class="confirm-block">
  <p class="confirm-block__message">Are you sure you want to delete this item?</p>
  <div class="confirm-block__actions">
    <button type="button" class="btn btn--secondary btn--sm" data-confirm-cancel>Cancel</button>
    <button type="button" class="btn btn--destructive btn--sm" data-confirm-action>Delete</button>
  </div>
</div>
```

**Accessibility:**
- Focus should be managed appropriately
- Destructive action clearly labeled

**JavaScript:** Action and cancel handling.

---

### 7.14 Footer and Meta Components

#### 7.14.1 Page Footer

**Purpose:** Site-wide footer with navigation and meta information.

**HTML Structure:**
```html
<footer class="footer">
  <div class="footer__container">
    <div class="footer__brand">
      <span class="footer__logo">Site Name</span>
      <p class="footer__tagline">A brief description.</p>
    </div>
    <nav class="footer__nav" aria-label="Footer navigation">
      <div class="footer__nav-group">
        <h4 class="footer__nav-title">Documentation</h4>
        <ul class="footer__nav-list">
          <li><a href="/docs" class="footer__link">Getting Started</a></li>
          <li><a href="/components" class="footer__link">Components</a></li>
        </ul>
      </div>
      <div class="footer__nav-group">
        <h4 class="footer__nav-title">Community</h4>
        <ul class="footer__nav-list">
          <li><a href="/github" class="footer__link">GitHub</a></li>
          <li><a href="/discord" class="footer__link">Discord</a></li>
        </ul>
      </div>
    </nav>
  </div>
  <div class="footer__bottom">
    <p class="footer__copyright">Copyright 2025. All rights reserved.</p>
  </div>
</footer>
```

**Variants:**
- `footer--compact`: Single-line footer
- `footer--centered`: Centered content

**Accessibility:**
- `<footer>` landmark
- Navigation sections with `aria-label`

**JavaScript:** None required.

---

#### 7.14.2 Meta Information

**Purpose:** Display document or item metadata.

**HTML Structure:**
```html
<div class="meta">
  <span class="meta__item">
    <span class="meta__label">Author:</span>
    <span class="meta__value">John Doe</span>
  </span>
  <span class="meta__separator" aria-hidden="true">|</span>
  <span class="meta__item">
    <span class="meta__label">Published:</span>
    <time class="meta__value" datetime="2025-01-15">January 15, 2025</time>
  </span>
  <span class="meta__separator" aria-hidden="true">|</span>
  <span class="meta__item">
    <span class="meta__value">5 min read</span>
  </span>
</div>
```

**Accessibility:**
- Use `<time>` element with `datetime` attribute for dates
- Separators are `aria-hidden`

**JavaScript:** None required.

---

#### 7.14.3 Version Badge

**Purpose:** Display software version number.

**HTML Structure:**
```html
<span class="version-badge">v1.2.3</span>

<!-- With status -->
<span class="version-badge version-badge--stable">v1.2.3 Stable</span>
<span class="version-badge version-badge--beta">v2.0.0-beta.1</span>
```

**Variants:**
- `version-badge--stable`
- `version-badge--beta`
- `version-badge--deprecated`

**Accessibility:** Purely informational, no special requirements.

**JavaScript:** None required.

---

### 7.15 Accessibility Utilities

#### 7.15.1 Skip Link

**Purpose:** Allow keyboard users to skip navigation.

**HTML Structure:**
```html
<a href="#main-content" class="skip-link">Skip to main content</a>

<!-- Target -->
<main id="main-content" tabindex="-1">
  <!-- Page content -->
</main>
```

**States:**
- Visually hidden by default
- Visible on focus

**CSS:**
```css
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  z-index: 9999;
  padding: var(--space-2) var(--space-4);
  background: var(--surface-background);
  color: var(--text-primary);
}

.skip-link:focus {
  top: 0;
}
```

**Accessibility:**
- Must be first focusable element on page
- Target element should have `tabindex="-1"` to receive focus

**JavaScript:** None required.

---

#### 7.15.2 Visually Hidden

**Purpose:** Hide content visually while keeping it accessible to screen readers.

**HTML Structure:**
```html
<span class="visually-hidden">Additional context for screen readers</span>

<!-- Example: Icon button -->
<button type="button" class="btn btn--icon">
  <svg aria-hidden="true">...</svg>
  <span class="visually-hidden">Close dialog</span>
</button>
```

**CSS:**
```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

**Usage Rules:**
- Use for accessible labels on icon-only buttons
- Use for additional context in links ("Read more" becomes "Read more about topic")
- Do not use for decorative content (use `aria-hidden` instead)

**JavaScript:** None required.

---

#### 7.15.3 Focus Trap

**Purpose:** Contain keyboard focus within a component.

**HTML Structure:**
```html
<div class="focus-trap" data-focus-trap>
  <button>First focusable</button>
  <input type="text">
  <button>Last focusable</button>
</div>
```

**JavaScript Implementation:**
```javascript
function createFocusTrap(container) {
  const focusableSelectors = [
    'a[href]',
    'button:not([disabled])',
    'input:not([disabled])',
    'select:not([disabled])',
    'textarea:not([disabled])',
    '[tabindex]:not([tabindex="-1"])'
  ].join(', ');

  const focusableElements = container.querySelectorAll(focusableSelectors);
  const firstElement = focusableElements[0];
  const lastElement = focusableElements[focusableElements.length - 1];

  function handleKeydown(event) {
    if (event.key !== 'Tab') return;

    if (event.shiftKey && document.activeElement === firstElement) {
      event.preventDefault();
      lastElement.focus();
    } else if (!event.shiftKey && document.activeElement === lastElement) {
      event.preventDefault();
      firstElement.focus();
    }
  }

  container.addEventListener('keydown', handleKeydown);

  return {
    activate() {
      firstElement?.focus();
    },
    deactivate() {
      container.removeEventListener('keydown', handleKeydown);
    }
  };
}
```

**Accessibility:**
- Required for modal dialogs, drawers, and overlays
- Must provide escape mechanism (Escape key)
- Focus returns to trigger element on close

---

#### 7.15.4 Live Region

**Purpose:** Announce dynamic content updates to screen readers.

**HTML Structure:**
```html
<!-- Polite announcements (wait for user to finish) -->
<div class="live-region" aria-live="polite" aria-atomic="true"></div>

<!-- Assertive announcements (interrupt immediately) -->
<div class="live-region" aria-live="assertive" aria-atomic="true"></div>

<!-- Status region for form validation -->
<div role="status" class="live-region"></div>

<!-- Alert region for errors -->
<div role="alert" class="live-region"></div>
```

**CSS:**
```css
.live-region {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

**JavaScript:**
```javascript
function announce(message, priority = 'polite') {
  const region = document.querySelector(`[aria-live="${priority}"]`);
  if (region) {
    region.textContent = '';
    setTimeout(() => {
      region.textContent = message;
    }, 100);
  }
}
```

---

### 7.16 Utility Classes

#### 7.16.1 Spacing Utilities

**Purpose:** Apply consistent spacing without custom CSS.

**Classes:**

```css
/* Margin */
.m-0 { margin: 0; }
.m-1 { margin: var(--space-1); }
.m-2 { margin: var(--space-2); }
.m-3 { margin: var(--space-3); }
.m-4 { margin: var(--space-4); }
.m-5 { margin: var(--space-5); }
.m-6 { margin: var(--space-6); }

/* Margin top */
.mt-0 { margin-top: 0; }
.mt-1 { margin-top: var(--space-1); }
/* ... through mt-8 */

/* Margin right */
.mr-0 { margin-right: 0; }
/* ... */

/* Margin bottom */
.mb-0 { margin-bottom: 0; }
/* ... */

/* Margin left */
.ml-0 { margin-left: 0; }
/* ... */

/* Margin horizontal */
.mx-0 { margin-left: 0; margin-right: 0; }
.mx-auto { margin-left: auto; margin-right: auto; }
/* ... */

/* Margin vertical */
.my-0 { margin-top: 0; margin-bottom: 0; }
/* ... */

/* Padding (same pattern) */
.p-0 { padding: 0; }
.p-1 { padding: var(--space-1); }
/* ... */

.pt-0 { padding-top: 0; }
/* ... */

/* Gap (for flex/grid) */
.gap-0 { gap: 0; }
.gap-1 { gap: var(--space-1); }
/* ... */
```

---

#### 7.16.2 Border Utilities

**Purpose:** Apply border styles without custom CSS.

**Classes:**

```css
/* Border all sides */
.border { border: 1px solid var(--border-default); }
.border-0 { border: 0; }
.border-2 { border-width: 2px; }

/* Border sides */
.border-t { border-top: 1px solid var(--border-default); }
.border-r { border-right: 1px solid var(--border-default); }
.border-b { border-bottom: 1px solid var(--border-default); }
.border-l { border-left: 1px solid var(--border-default); }

/* Border colors */
.border-strong { border-color: var(--border-strong); }
.border-primary { border-color: var(--color-primary); }
.border-error { border-color: var(--color-error); }

/* Border radius */
.rounded-none { border-radius: 0; }
.rounded-sm { border-radius: var(--radius-sm); }
.rounded { border-radius: var(--radius-md); }
.rounded-lg { border-radius: var(--radius-lg); }
.rounded-full { border-radius: var(--radius-full); }
```

---

#### 7.16.3 Text Utilities

**Purpose:** Apply text styles without custom CSS.

**Classes:**

```css
/* Font size */
.text-xs { font-size: var(--text-xs); }
.text-sm { font-size: var(--text-sm); }
.text-base { font-size: var(--text-base); }
.text-lg { font-size: var(--text-lg); }
.text-xl { font-size: var(--text-xl); }
.text-2xl { font-size: var(--text-2xl); }
.text-3xl { font-size: var(--text-3xl); }

/* Font weight */
.font-normal { font-weight: 400; }
.font-medium { font-weight: 500; }
.font-semibold { font-weight: 600; }
.font-bold { font-weight: 700; }

/* Font family */
.font-mono { font-family: var(--font-mono); }
.font-sans { font-family: var(--font-sans); }

/* Text color */
.text-primary { color: var(--text-primary); }
.text-secondary { color: var(--text-secondary); }
.text-muted { color: var(--text-muted); }
.text-success { color: var(--color-success); }
.text-warning { color: var(--color-warning); }
.text-error { color: var(--color-error); }

/* Text alignment */
.text-left { text-align: left; }
.text-center { text-align: center; }
.text-right { text-align: right; }

/* Text transform */
.uppercase { text-transform: uppercase; }
.lowercase { text-transform: lowercase; }
.capitalize { text-transform: capitalize; }

/* Text decoration */
.underline { text-decoration: underline; }
.no-underline { text-decoration: none; }

/* Text overflow */
.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

---

#### 7.16.4 Surface Utilities

**Purpose:** Apply background colors and elevation.

**Classes:**

```css
/* Background colors */
.bg-surface { background-color: var(--surface-background); }
.bg-elevated { background-color: var(--surface-elevated); }
.bg-overlay { background-color: var(--surface-overlay); }
.bg-primary { background-color: var(--color-primary); }
.bg-success { background-color: var(--color-success); }
.bg-warning { background-color: var(--color-warning); }
.bg-error { background-color: var(--color-error); }

/* Elevation / Shadow */
.shadow-none { box-shadow: none; }
.shadow-sm { box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05); }
.shadow { box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1), 0 1px 2px rgba(0, 0, 0, 0.06); }
.shadow-md { box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 2px 4px rgba(0, 0, 0, 0.06); }
.shadow-lg { box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1), 0 4px 6px rgba(0, 0, 0, 0.05); }
```

---

#### 7.16.5 Display and Layout Utilities

**Purpose:** Control element display and positioning.

**Classes:**

```css
/* Display */
.block { display: block; }
.inline-block { display: inline-block; }
.inline { display: inline; }
.flex { display: flex; }
.inline-flex { display: inline-flex; }
.grid { display: grid; }
.hidden { display: none; }

/* Flexbox */
.flex-row { flex-direction: row; }
.flex-col { flex-direction: column; }
.flex-wrap { flex-wrap: wrap; }
.flex-nowrap { flex-wrap: nowrap; }
.items-start { align-items: flex-start; }
.items-center { align-items: center; }
.items-end { align-items: flex-end; }
.justify-start { justify-content: flex-start; }
.justify-center { justify-content: center; }
.justify-end { justify-content: flex-end; }
.justify-between { justify-content: space-between; }
.flex-1 { flex: 1 1 0%; }
.flex-auto { flex: 1 1 auto; }
.flex-none { flex: none; }

/* Width */
.w-full { width: 100%; }
.w-auto { width: auto; }
.max-w-none { max-width: none; }
.max-w-full { max-width: 100%; }

/* Position */
.relative { position: relative; }
.absolute { position: absolute; }
.fixed { position: fixed; }
.sticky { position: sticky; }
```

---

#### 7.16.6 Interactive State Utilities

**Purpose:** Apply interactive state styles.

**Classes:**

```css
/* Cursor */
.cursor-pointer { cursor: pointer; }
.cursor-not-allowed { cursor: not-allowed; }
.cursor-default { cursor: default; }

/* Pointer events */
.pointer-events-none { pointer-events: none; }
.pointer-events-auto { pointer-events: auto; }

/* User select */
.select-none { user-select: none; }
.select-text { user-select: text; }
.select-all { user-select: all; }

/* Opacity */
.opacity-0 { opacity: 0; }
.opacity-50 { opacity: 0.5; }
.opacity-100 { opacity: 1; }

/* Disabled state */
.disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}
```

---

## 8. Showcase and Documentation Requirements

### 8.1 Documentation Site Structure

The showcase site serves as both documentation and live demonstration. It must be a single-page application or simple multi-page static site that runs without a build process.

**Site Structure:**

```
/
├── index.html           # Landing page with overview
├── /docs/
│   ├── getting-started.html
│   ├── theming.html
│   ├── accessibility.html
│   └── contributing.html
├── /components/
│   ├── index.html       # Component overview/catalog
│   ├── layout.html
│   ├── navigation.html
│   ├── buttons.html
│   ├── forms.html
│   ├── feedback.html
│   ├── overlays.html
│   ├── data-display.html
│   └── utilities.html
├── /examples/
│   ├── dashboard.html
│   ├── form-page.html
│   ├── settings.html
│   └── documentation.html
├── /css/
│   └── styles.css
└── /js/
    └── main.js
```

### 8.2 Component Documentation Format

Each component section must include:

**8.2.1 Component Header**
- Component name
- Brief description (one to two sentences)
- Category/group tag

**8.2.2 Live Preview**
- Interactive demonstration of the component
- All variants displayed
- All states demonstrable (hover, focus, active, disabled)
- Theme toggle to preview in light/dark modes

**8.2.3 Usage Guidelines**
- When to use this component
- When not to use this component
- Best practices
- Common mistakes to avoid

**8.2.4 Code Examples**

```html
<div class="code-example">
  <div class="code-example__preview">
    <!-- Live rendered component -->
  </div>
  <div class="code-example__source">
    <div class="code-example__tabs">
      <button class="code-example__tab code-example__tab--active" data-tab="html">HTML</button>
      <button class="code-example__tab" data-tab="css">CSS</button>
      <button class="code-example__tab" data-tab="js">JS</button>
    </div>
    <div class="code-example__panel" data-panel="html">
      <div class="code-block">
        <button class="code-block__copy" data-copy>Copy</button>
        <pre><code><!-- HTML source --></code></pre>
      </div>
    </div>
  </div>
</div>
```

**8.2.5 Variants Table**
- List of all available variants
- Class names
- Visual example or description

**8.2.6 Properties/Attributes**
- Data attributes
- CSS custom properties for customization
- JavaScript options (if applicable)

**8.2.7 Accessibility Notes**
- ARIA requirements
- Keyboard interactions
- Screen reader behavior
- Focus management details

**8.2.8 Related Components**
- Links to similar or complementary components

### 8.3 Copy-Paste Functionality

**Requirements:**

1. Every code block includes a copy button
2. Copying includes only the relevant code (no extra whitespace or line numbers)
3. Visual feedback confirms successful copy
4. Copied code is self-contained and functional when pasted into any HTML file

**Copy Behavior:**

```javascript
async function copyCode(button) {
  const codeBlock = button.closest('.code-block');
  const code = codeBlock.querySelector('code').textContent;
  
  try {
    await navigator.clipboard.writeText(code);
    button.textContent = 'Copied!';
    button.classList.add('code-block__copy--success');
    
    setTimeout(() => {
      button.textContent = 'Copy';
      button.classList.remove('code-block__copy--success');
    }, 2000);
  } catch (err) {
    button.textContent = 'Failed';
  }
}
```

### 8.4 Standalone Snippet Requirements

Each component snippet must:

1. **Include all necessary HTML** - Complete structure, not partial
2. **Reference required CSS classes** - Document which classes are needed
3. **Note JavaScript dependencies** - List required JS functions
4. **Provide minimal working example** - Functional when pasted alone

**Snippet Template:**

```html
<!--
  Component: Button
  Required CSS: .btn, .btn--primary
  Required JS: None
-->

<button type="button" class="btn btn--primary">
  Click Me
</button>
```

### 8.5 Example Layouts

**8.5.1 Dashboard Layout**

Full-page example demonstrating:
- Navbar with navigation
- Sidebar navigation
- Grid of stat cards
- Data table
- Charts placeholder
- Alert/notification area

**8.5.2 Form Page**

Full-page example demonstrating:
- Form layout with sections
- All input types
- Validation states
- Form actions (submit, cancel)
- Progress indicator

**8.5.3 Settings Page**

Full-page example demonstrating:
- Tabbed navigation
- Form groups
- Toggle switches
- Save confirmation
- Danger zone section

**8.5.4 Documentation Page**

Full-page example demonstrating:
- Sidebar table of contents
- Prose content with headings
- Code blocks
- Callouts and admonitions
- Previous/next navigation

---

## 9. Developer Experience

### 9.1 Zero Setup Integration

**Minimum viable integration:**

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Page</title>
  <link rel="stylesheet" href="https://cdn.example.com/dcmt/1.0.0/styles.css">
</head>
<body>
  <button class="btn btn--primary">Hello World</button>
  
  <script src="https://cdn.example.com/dcmt/1.0.0/main.js"></script>
</body>
</html>
```

### 9.2 CDN Distribution

**Hosted assets:**

```
https://cdn.example.com/dcmt/
├── latest/
│   ├── styles.css
│   ├── styles.min.css
│   ├── main.js
│   └── main.min.js
├── 1.0.0/
│   ├── styles.css
│   ├── styles.min.css
│   ├── main.js
│   └── main.min.js
└── 1.0.1/
    └── ...
```

**Usage options:**

```html
<!-- Latest version (use for development only) -->
<link rel="stylesheet" href="https://cdn.example.com/dcmt/latest/styles.min.css">

<!-- Pinned version (recommended for production) -->
<link rel="stylesheet" href="https://cdn.example.com/dcmt/1.0.0/styles.min.css">
```

### 9.3 Local Development

**Download and include:**

```html
<link rel="stylesheet" href="./css/styles.css">
<script src="./js/main.js"></script>
```

### 9.4 Class Naming Conventions

**BEM Methodology:**

- Block: `.component`
- Element: `.component__element`
- Modifier: `.component--modifier`

**Examples:**

```css
.btn { }                      /* Block */
.btn__icon { }                /* Element */
.btn--primary { }             /* Modifier */
.btn--lg { }                  /* Size modifier */
.btn__icon--left { }          /* Element modifier */
```

**Naming Rules:**

1. Use lowercase letters only
2. Use hyphens for multi-word names: `.form-field`
3. Use double underscore for elements: `__`
4. Use double hyphen for modifiers: `--`
5. Prefix state classes with `is-` or `has-`: `.is-active`, `.has-error`
6. Keep names semantic and descriptive

### 9.5 CSS Custom Property Naming

**Convention:**

```css
--{category}-{property}: value;
--{category}-{property}-{variant}: value;
```

**Categories:**

- `--color-*`: Color values
- `--space-*`: Spacing values
- `--font-*`: Font properties
- `--text-*`: Text sizes
- `--radius-*`: Border radius
- `--shadow-*`: Box shadows
- `--surface-*`: Surface colors
- `--border-*`: Border colors

**Examples:**

```css
--color-primary: #2563eb;
--color-primary-hover: #1d4ed8;
--space-4: 16px;
--font-mono: "SF Mono", monospace;
--text-lg: 1.125rem;
--radius-md: 4px;
--surface-background: #ffffff;
```

### 9.6 JavaScript API

**Global namespace:**

```javascript
window.DCMT = {
  // Theme
  theme: {
    get: () => string,
    set: (theme: 'light' | 'dark' | 'system') => void,
    toggle: () => void
  },
  
  // Toast notifications
  toast: {
    show: (options: ToastOptions) => void,
    dismiss: (id: string) => void,
    dismissAll: () => void
  },
  
  // Modal
  modal: {
    open: (id: string) => void,
    close: (id?: string) => void
  },
  
  // Drawer
  drawer: {
    open: (id: string) => void,
    close: (id?: string) => void
  },
  
  // Utilities
  utils: {
    copyToClipboard: (text: string) => Promise<boolean>,
    announce: (message: string, priority?: 'polite' | 'assertive') => void
  }
};
```

**Initialization:**

```javascript
// Auto-initialize all components
document.addEventListener('DOMContentLoaded', () => {
  DCMT.init();
});

// Or initialize specific components
DCMT.init({
  components: ['modal', 'dropdown', 'tabs']
});
```

---

## 10. File and Distribution Structure

### 10.1 Single-File Distribution

For maximum simplicity, provide a single HTML file containing all styles and scripts inline:

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DCMT Components</title>
  <style>
    /* All CSS here */
  </style>
</head>
<body>
  <!-- Component examples -->
  
  <script>
    /* All JavaScript here */
  </script>
</body>
</html>
```

### 10.2 Multi-File Structure

**Repository structure:**

```
dcmt/
├── index.html                 # Documentation home
├── css/
│   ├── styles.css             # Complete unminified CSS
│   ├── styles.min.css         # Minified CSS
│   └── components/            # Individual component CSS (optional)
│       ├── buttons.css
│       ├── forms.css
│       └── ...
├── js/
│   ├── main.js                # Complete unminified JS
│   ├── main.min.js            # Minified JS
│   └── components/            # Individual component JS (optional)
│       ├── modal.js
│       ├── dropdown.js
│       └── ...
├── docs/
│   ├── getting-started.html
│   ├── theming.html
│   └── ...
├── components/
│   ├── index.html
│   ├── buttons.html
│   └── ...
├── examples/
│   ├── dashboard.html
│   └── ...
├── LICENSE
├── README.md
└── CONTRIBUTING.md
```

### 10.3 CSS Architecture

**styles.css structure:**

```css
/*! DCMT v1.0.0 | MIT License */

/* ==========================================================================
   1. CSS Custom Properties (Design Tokens)
   ========================================================================== */

:root {
  /* Colors */
  /* Spacing */
  /* Typography */
  /* Borders */
  /* Shadows */
}

/* Dark theme overrides */
[data-theme="dark"] { }

/* ==========================================================================
   2. Reset / Normalize
   ========================================================================== */

/* ==========================================================================
   3. Base Styles
   ========================================================================== */

/* ==========================================================================
   4. Layout Components
   ========================================================================== */

/* ==========================================================================
   5. Navigation Components
   ========================================================================== */

/* ==========================================================================
   6. Form Components
   ========================================================================== */

/* ==========================================================================
   7. Feedback Components
   ========================================================================== */

/* ==========================================================================
   8. Overlay Components
   ========================================================================== */

/* ==========================================================================
   9. Data Display Components
   ========================================================================== */

/* ==========================================================================
   10. Utility Classes
   ========================================================================== */

/* ==========================================================================
   11. Print Styles
   ========================================================================== */

@media print { }
```

### 10.4 JavaScript Architecture

**main.js structure:**

```javascript
/*! DCMT v1.0.0 | MIT License */

(function(global) {
  'use strict';

  /* ==========================================================================
     Utility Functions
     ========================================================================== */

  /* ==========================================================================
     Theme Management
     ========================================================================== */

  /* ==========================================================================
     Component: Modal
     ========================================================================== */

  /* ==========================================================================
     Component: Dropdown
     ========================================================================== */

  /* ==========================================================================
     Component: Tabs
     ========================================================================== */

  /* ==========================================================================
     Component: Accordion
     ========================================================================== */

  /* ==========================================================================
     Component: Toast
     ========================================================================== */

  /* ==========================================================================
     Auto-initialization
     ========================================================================== */

  /* ==========================================================================
     Public API
     ========================================================================== */

  global.DCMT = {
    version: '1.0.0',
    init: init,
    theme: themeAPI,
    modal: modalAPI,
    dropdown: dropdownAPI,
    tabs: tabsAPI,
    accordion: accordionAPI,
    toast: toastAPI,
    utils: utilsAPI
  };

})(typeof window !== 'undefined' ? window : this);
```

### 10.5 GitHub Pages Compatibility

All files must function when served from GitHub Pages:

1. No server-side processing required
2. Relative paths for all asset references
3. No build step required
4. Works with default GitHub Pages configuration

---

## 11. Performance Requirements

### 11.1 File Size Budgets

| Asset | Unminified | Minified | Gzipped |
|-------|------------|----------|---------|
| styles.css | < 80 KB | < 50 KB | < 15 KB |
| main.js | < 40 KB | < 25 KB | < 10 KB |
| **Total** | < 120 KB | < 75 KB | < 25 KB |

### 11.2 Loading Strategy

**CSS Loading:**

```html
<!-- Critical CSS inline for above-the-fold content -->
<style>
  /* Minimal reset and layout styles */
</style>

<!-- Full stylesheet loaded normally -->
<link rel="stylesheet" href="styles.css">
```

**JavaScript Loading:**

```html
<!-- Non-blocking script loading -->
<script src="main.js" defer></script>
```

### 11.3 Runtime Performance

**Event Handling:**

- Use event delegation for repeated elements
- Debounce scroll and resize handlers (150ms minimum)
- Throttle high-frequency events

```javascript
// Event delegation example
document.addEventListener('click', (event) => {
  const trigger = event.target.closest('[data-modal-open]');
  if (trigger) {
    const modalId = trigger.getAttribute('data-modal-open');
    DCMT.modal.open(modalId);
  }
});
```

**DOM Operations:**

- Batch DOM reads and writes
- Use `requestAnimationFrame` for visual updates
- Minimize forced reflows

**Memory Management:**

- Remove event listeners when components are destroyed
- Avoid storing unnecessary references
- Use WeakMap for component instance storage

### 11.4 CSS Performance

**Selector Efficiency:**

- Avoid deep nesting (max 3 levels)
- Avoid universal selectors
- Use class selectors over element selectors

```css
/* Good */
.btn { }
.btn__icon { }
.card .btn { }

/* Avoid */
.sidebar .nav ul li a span { }
* { }
div.btn { }
```

**Animation Performance:**

- Animate only `transform` and `opacity`
- Use `will-change` sparingly and remove after animation
- Respect `prefers-reduced-motion`

```css
.modal {
  transform: translateY(20px);
  opacity: 0;
  transition: transform 0.2s ease, opacity 0.2s ease;
}

.modal.is-open {
  transform: translateY(0);
  opacity: 1;
}

@media (prefers-reduced-motion: reduce) {
  .modal {
    transition: none;
  }
}
```

---

## 12. Browser Support

### 12.1 Supported Browsers

| Browser | Minimum Version | Release Date |
|---------|-----------------|--------------|
| Chrome | 90+ | April 2021 |
| Firefox | 88+ | April 2021 |
| Safari | 14+ | September 2020 |
| Edge | 90+ | April 2021 |

### 12.2 Required Features

The following browser features are required:

| Feature | Usage |
|---------|-------|
| CSS Custom Properties | Theming system |
| CSS Grid | Layout components |
| CSS Flexbox | Component layouts |
| CSS `gap` property | Spacing in flex/grid |
| `clip-path` | Visual effects |
| `:focus-visible` | Focus styling |
| `ResizeObserver` | Responsive components |
| `IntersectionObserver` | Scroll spy, lazy loading |
| Clipboard API | Copy functionality |
| `<dialog>` element | Modal (optional, polyfilled) |

### 12.3 Graceful Degradation

For browsers missing specific features:

1. **CSS Custom Properties**: Provide fallback values inline

```css
.btn {
  background-color: #2563eb; /* Fallback */
  background-color: var(--color-primary);
}
```

2. **JavaScript APIs**: Check for feature support

```javascript
function copyToClipboard(text) {
  if (navigator.clipboard && navigator.clipboard.writeText) {
    return navigator.clipboard.writeText(text);
  }
  // Fallback for older browsers
  const textarea = document.createElement('textarea');
  textarea.value = text;
  document.body.appendChild(textarea);
  textarea.select();
  document.execCommand('copy');
  document.body.removeChild(textarea);
  return Promise.resolve();
}
```

### 12.4 Explicitly Unsupported

- Internet Explorer (all versions)
- Legacy Edge (EdgeHTML)
- Opera Mini
- UC Browser

---

## 13. Extensibility and Community

### 13.1 Adding New Components

**Component Checklist:**

1. [ ] HTML structure follows BEM naming conventions
2. [ ] CSS uses only existing design tokens
3. [ ] Component works without JavaScript where possible
4. [ ] All interactive elements are keyboard accessible
5. [ ] ARIA attributes applied correctly
6. [ ] Works in both light and dark themes
7. [ ] Responsive at all breakpoints
8. [ ] Documentation page created
9. [ ] All variants documented
10. [ ] Code examples are copy-paste ready

**CSS Template:**

```css
/* ==========================================================================
   Component: Component Name
   ========================================================================== */

/**
 * 1. Description of the component
 * 2. Usage notes
 */

.component {
  /* Base styles */
}

.component__element {
  /* Element styles */
}

.component--modifier {
  /* Modifier styles */
}

/* States */
.component.is-active { }
.component:hover { }
.component:focus-visible { }
.component:disabled,
.component.is-disabled { }

/* Responsive */
@media (min-width: 768px) {
  .component { }
}
```

**JavaScript Template:**

```javascript
/* ==========================================================================
   Component: Component Name
   ========================================================================== */

/**
 * Component description
 * @param {HTMLElement} element - The component root element
 * @param {Object} options - Configuration options
 */
function ComponentName(element, options = {}) {
  // Default options
  const defaults = {
    option1: true,
    option2: 'value'
  };
  
  const settings = { ...defaults, ...options };
  
  // Private state
  let isInitialized = false;
  
  // Private methods
  function init() {
    if (isInitialized) return;
    bindEvents();
    isInitialized = true;
  }
  
  function bindEvents() {
    element.addEventListener('click', handleClick);
  }
  
  function handleClick(event) {
    // Handle click
  }
  
  function destroy() {
    element.removeEventListener('click', handleClick);
    isInitialized = false;
  }
  
  // Public API
  return {
    init,
    destroy,
    // Other public methods
  };
}
```

### 13.2 Contribution Guidelines

**CONTRIBUTING.md contents:**

1. **Code of Conduct** - Expected behavior in the community
2. **Development Setup** - How to set up local development
3. **Pull Request Process** - Steps for submitting changes
4. **Coding Standards** - Style guide and conventions
5. **Testing Requirements** - Manual and automated testing
6. **Documentation Requirements** - What documentation is needed
7. **Accessibility Requirements** - A11y checklist for all changes

**Pull Request Template:**

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New component
- [ ] Component enhancement
- [ ] Documentation update
- [ ] Other (describe)

## Checklist
- [ ] Code follows project conventions
- [ ] Changes work in all supported browsers
- [ ] Light and dark themes tested
- [ ] Keyboard navigation works correctly
- [ ] Screen reader testing completed
- [ ] Documentation updated
- [ ] No accessibility regressions

## Screenshots
(if applicable)
```

### 13.3 Issue Templates

**Bug Report:**

```markdown
## Bug Description
Clear description of the bug

## Steps to Reproduce
1. Step one
2. Step two
3. Step three

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Environment
- Browser: 
- OS: 
- Version: 

## Screenshots
(if applicable)
```

**Feature Request:**

```markdown
## Feature Description
Clear description of the feature

## Use Case
Why this feature is needed

## Proposed Solution
How you think it should work

## Alternatives Considered
Other approaches considered

## Additional Context
Any other relevant information
```

### 13.4 Versioning

**Semantic Versioning (SemVer):**

- **Major (X.0.0)**: Breaking changes
- **Minor (0.X.0)**: New features, backwards compatible
- **Patch (0.0.X)**: Bug fixes, backwards compatible

**Breaking Changes:**

- Removing a component
- Removing a CSS class
- Changing HTML structure requirements
- Changing JavaScript API signatures
- Removing CSS custom properties

**Non-Breaking Changes:**

- Adding new components
- Adding new variants/modifiers
- Adding new CSS custom properties
- Adding new JavaScript methods
- Bug fixes
- Performance improvements

---

## 14. Risks and Mitigations

### 14.1 Scope Creep

**Risk:** Adding too many components or features beyond the core requirement.

**Mitigation:**
- Define clear MVP component list
- Defer advanced features to future versions
- Require justification for new components
- Community voting on feature priority

**Acceptance Criteria for New Components:**
1. Must solve a common, documented use case
2. Must not duplicate existing component functionality
3. Must be achievable without external dependencies
4. Must not increase CSS by more than 5KB
5. Must not increase JS by more than 3KB

### 14.2 Accessibility Regressions

**Risk:** Changes introducing accessibility issues.

**Mitigation:**
- Mandatory accessibility checklist in PR template
- Automated accessibility linting (axe-core)
- Manual screen reader testing for interactive components
- Keyboard navigation testing requirement
- Documented accessibility testing procedure

**Testing Procedure:**
1. Run automated accessibility scan
2. Tab through all interactive elements
3. Test with screen reader (VoiceOver, NVDA)
4. Verify focus management
5. Test with high contrast mode
6. Test with reduced motion preference

### 14.3 Style Inconsistency

**Risk:** Visual inconsistency between components.

**Mitigation:**
- Strict adherence to design tokens
- No magic numbers in CSS
- Visual regression testing
- Component style review in PRs
- Living style guide documentation

**CSS Review Checklist:**
- [ ] Uses only defined spacing scale
- [ ] Uses only defined color variables
- [ ] Uses only defined typography scale
- [ ] Uses only defined border radius values
- [ ] Consistent hover/focus states
- [ ] Consistent transition timing

### 14.4 Browser Compatibility Issues

**Risk:** Components breaking in supported browsers.

**Mitigation:**
- Cross-browser testing requirement
- Feature detection over browser detection
- Documented fallback strategies
- CI testing in multiple browsers

### 14.5 Performance Degradation

**Risk:** Growing file sizes and slow runtime performance.

**Mitigation:**
- File size budgets enforced in CI
- Performance benchmarking
- Regular audits
- Lazy loading for documentation site

### 14.6 Documentation Staleness

**Risk:** Documentation becoming out of sync with code.

**Mitigation:**
- Documentation updates required with code changes
- Component examples are live (not static images)
- Automated extraction of code examples where possible
- Regular documentation review

---

## 15. Future Enhancements

### 15.1 Version 1.1 (Short-term)

**Additional Components:**
- Color picker (native + enhanced)
- Date range picker
- Multi-select with search
- Tree view
- Timeline component

**Enhancements:**
- Additional button variants
- More form layout options
- Enhanced table features (sorting, filtering)
- Additional utility classes

### 15.2 Version 1.2 (Medium-term)

**Features:**
- Print stylesheet optimization
- RTL (Right-to-Left) language support
- High contrast theme variant
- Component animation library (opt-in)

**Templates:**
- Landing page template
- Blog template
- E-commerce product page
- Admin dashboard

### 15.3 Version 2.0 (Long-term)

**Major Features:**
- Web Components wrappers (optional)
- CSS-only component variants (no JS required)
- Figma design kit
- VS Code extension for snippets

**Framework Integration Guides:**
- React usage guide
- Vue usage guide
- Svelte usage guide
- Alpine.js integration
- HTMX integration

**Tooling:**
- Component generator CLI
- Theme generator tool
- Accessibility auditor integration

---

## Appendix A: Complete Component Checklist

### Layout and Structure
- [ ] Container
- [ ] Grid
- [ ] Stack
- [ ] Cluster
- [ ] Sidebar Layout
- [ ] Split Layout
- [ ] Section
- [ ] Card
- [ ] Divider

### Navigation
- [ ] Navbar
- [ ] Sidebar Navigation
- [ ] Tabs
- [ ] Breadcrumbs
- [ ] Pagination

### Basic Controls
- [ ] Button
- [ ] Button Group
- [ ] Link Button
- [ ] Toggle Switch
- [ ] Checkbox
- [ ] Radio Button

### Form Elements
- [ ] Text Input
- [ ] Textarea
- [ ] Select (Native)
- [ ] Select (Enhanced)
- [ ] Search Input
- [ ] File Input
- [ ] Range Slider
- [ ] Date Picker (Native)

### Form States
- [ ] Error State
- [ ] Success State
- [ ] Disabled State
- [ ] Read-Only State
- [ ] Form Field Group
- [ ] Inline Validation

### Feedback
- [ ] Alert
- [ ] Toast
- [ ] Spinner
- [ ] Skeleton Loader
- [ ] Progress Bar

### Overlays
- [ ] Modal Dialog
- [ ] Confirmation Dialog
- [ ] Drawer
- [ ] Dropdown Menu
- [ ] Tooltip
- [ ] Popover

### Data Display
- [ ] Table
- [ ] Description List
- [ ] List
- [ ] Badge
- [ ] Avatar
- [ ] Tag/Chip
- [ ] Key-Value Pair
- [ ] Stat Card

### Code and Developer UI
- [ ] Code Block
- [ ] Inline Code
- [ ] Keyboard Shortcut
- [ ] Copy Button
- [ ] Terminal Output

### Navigation Aids
- [ ] Back to Top
- [ ] Scroll Progress
- [ ] Table of Contents
- [ ] Anchor Link

### Interactive Utilities
- [ ] Accordion
- [ ] Disclosure
- [ ] Filter Controls
- [ ] Collapsible Section

### Media and Content
- [ ] Figure
- [ ] Empty State
- [ ] Text Highlight
- [ ] Blockquote
- [ ] Callout/Admonition

### User Action Blocks
- [ ] Retry Block
- [ ] Dismissible Panel
- [ ] Confirmation Block

### Footer and Meta
- [ ] Page Footer
- [ ] Meta Information
- [ ] Version Badge

### Accessibility Utilities
- [ ] Skip Link
- [ ] Visually Hidden
- [ ] Focus Trap
- [ ] Live Region

### Utility Classes
- [ ] Spacing
- [ ] Border
- [ ] Text
- [ ] Surface
- [ ] Display/Layout
- [ ] Interactive States

---

## Appendix B: Design Token Reference

### Color Tokens

```css
:root {
  /* Neutral Scale */
  --color-neutral-0: #ffffff;
  --color-neutral-50: #f9fafb;
  --color-neutral-100: #f3f4f6;
  --color-neutral-200: #e5e7eb;
  --color-neutral-300: #d1d5db;
  --color-neutral-400: #9ca3af;
  --color-neutral-500: #6b7280;
  --color-neutral-600: #4b5563;
  --color-neutral-700: #374151;
  --color-neutral-800: #1f2937;
  --color-neutral-900: #111827;
  --color-neutral-950: #030712;

  /* Brand */
  --color-primary: #2563eb;
  --color-primary-hover: #1d4ed8;
  --color-primary-active: #1e40af;

  /* Semantic */
  --color-success: #16a34a;
  --color-success-hover: #15803d;
  --color-warning: #ca8a04;
  --color-warning-hover: #a16207;
  --color-error: #dc2626;
  --color-error-hover: #b91c1c;
  --color-info: #0891b2;
  --color-info-hover: #0e7490;
}
```

### Spacing Tokens

```css
:root {
  --space-0: 0;
  --space-px: 1px;
  --space-0-5: 0.125rem;  /* 2px */
  --space-1: 0.25rem;     /* 4px */
  --space-1-5: 0.375rem;  /* 6px */
  --space-2: 0.5rem;      /* 8px */
  --space-2-5: 0.625rem;  /* 10px */
  --space-3: 0.75rem;     /* 12px */
  --space-3-5: 0.875rem;  /* 14px */
  --space-4: 1rem;        /* 16px */
  --space-5: 1.25rem;     /* 20px */
  --space-6: 1.5rem;      /* 24px */
  --space-7: 1.75rem;     /* 28px */
  --space-8: 2rem;        /* 32px */
  --space-9: 2.25rem;     /* 36px */
  --space-10: 2.5rem;     /* 40px */
  --space-12: 3rem;       /* 48px */
  --space-14: 3.5rem;     /* 56px */
  --space-16: 4rem;       /* 64px */
  --space-20: 5rem;       /* 80px */
  --space-24: 6rem;       /* 96px */
}
```

### Typography Tokens

```css
:root {
  /* Font Families */
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  --font-mono: "SF Mono", "Cascadia Code", "Fira Code", Consolas, "Liberation Mono", Menlo, monospace;

  /* Font Sizes */
  --text-xs: 0.75rem;     /* 12px */
  --text-sm: 0.875rem;    /* 14px */
  --text-base: 1rem;      /* 16px */
  --text-lg: 1.125rem;    /* 18px */
  --text-xl: 1.25rem;     /* 20px */
  --text-2xl: 1.5rem;     /* 24px */
  --text-3xl: 1.875rem;   /* 30px */
  --text-4xl: 2.25rem;    /* 36px */

  /* Line Heights */
  --leading-none: 1;
  --leading-tight: 1.25;
  --leading-snug: 1.375;
  --leading-normal: 1.5;
  --leading-relaxed: 1.625;
  --leading-loose: 2;

  /* Font Weights */
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;
}
```

---

## Appendix C: Keyboard Shortcut Reference

| Component | Key | Action |
|-----------|-----|--------|
| **General** | | |
| All interactive | Tab | Move focus forward |
| All interactive | Shift + Tab | Move focus backward |
| Buttons, Links | Enter | Activate |
| Buttons | Space | Activate |
| **Modal/Drawer** | | |
| Modal | Escape | Close modal |
| Modal | Tab | Cycle focus within |
| **Dropdown** | | |
| Dropdown trigger | Enter/Space | Open menu |
| Dropdown | Arrow Down | Next item |
| Dropdown | Arrow Up | Previous item |
| Dropdown | Enter | Select item |
| Dropdown | Escape | Close menu |
| **Tabs** | | |
| Tab list | Arrow Left | Previous tab |
| Tab list | Arrow Right | Next tab |
| Tab list | Home | First tab |
| Tab list | End | Last tab |
| **Accordion** | | |
| Accordion header | Enter/Space | Toggle panel |
| Accordion header | Arrow Down | Next header |
| Accordion header | Arrow Up | Previous header |
| **Select** | | |
| Select trigger | Enter/Space | Open listbox |
| Listbox | Arrow Down | Next option |
| Listbox | Arrow Up | Previous option |
| Listbox | Enter | Select option |
| Listbox | Escape | Close listbox |
| Listbox | Type character | Jump to matching option |
| **Toggle/Checkbox** | | |
| Toggle | Space | Toggle state |
| Checkbox | Space | Toggle checked |
| **Radio Group** | | |
| Radio group | Arrow Down/Right | Next option |
| Radio group | Arrow Up/Left | Previous option |

---

*Document Version: 1.0.0*
*Last Updated: 2025-01-15*
*Status: Approved for Development*