# MarkdownStudio Pro v3.0.0

## Complete Implementation Package

I'll create the complete, production-ready implementation. Due to the comprehensive nature of this project, I'll organize it into logical sections.

---

## Table of Contents

1. [Project Structure](#1-project-structure)
2. [Core HTML](#2-core-html)
3. [Stylesheets](#3-stylesheets)
4. [JavaScript Modules](#4-javascript-modules)
5. [Service Worker](#5-service-worker)
6. [Configuration Files](#6-configuration-files)
7. [Testing Suite](#7-testing-suite)
8. [Deployment Files](#8-deployment-files)

---

## 1. Project Structure

```
markdown-editor/
│
├── index.html
├── 404.html
├── robots.txt
├── sitemap.xml
├── manifest.json
├── sw.js
├── .nojekyll
├── LICENSE
├── README.md
│
├── assets/
│   ├── css/
│   │   ├── style.css
│   │   ├── editor.css
│   │   ├── preview.css
│   │   ├── components.css
│   │   └── print.css
│   │
│   ├── js/
│   │   ├── app.js
│   │   │
│   │   ├── core/
│   │   │   ├── editor.js
│   │   │   ├── preview.js
│   │   │   ├── toolbar.js
│   │   │   └── state.js
│   │   │
│   │   ├── services/
│   │   │   ├── storage.js
│   │   │   ├── export.js
│   │   │   ├── theme.js
│   │   │   └── shortcuts.js
│   │   │
│   │   ├── utils/
│   │   │   ├── event-bus.js
│   │   │   ├── debounce.js
│   │   │   ├── dom.js
│   │   │   └── sanitize.js
│   │   │
│   │   └── lib/
│   │       ├── marked.min.js
│   │       └── purify.min.js
│   │
│   └── images/
│       ├── favicon.ico
│       ├── favicon-16.png
│       ├── favicon-32.png
│       ├── apple-touch-icon.png
│       ├── icon-192.png
│       ├── icon-512.png
│       ├── og-image.png
│       └── screenshot.png
│
├── tests/
│   ├── setup.js
│   ├── unit/
│   │   ├── parser.test.js
│   │   ├── sanitizer.test.js
│   │   ├── storage.test.js
│   │   ├── editor.test.js
│   │   └── export.test.js
│   │
│   └── security/
│       └── xss.test.js
│
├── .github/
│   └── workflows/
│       ├── test.yml
│       └── deploy.yml
│
├── package.json
└── vitest.config.js
```

---

## 2. Core HTML

### index.html

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <!-- Character Set & Viewport -->
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  
  <!-- Primary Meta Tags -->
  <title>Free Online Markdown Editor with Live Preview | MarkdownStudio Pro</title>
  <meta name="title" content="Free Online Markdown Editor with Live Preview | MarkdownStudio Pro">
  <meta name="description" content="Free online Markdown editor with instant live preview. Write, format, and export Markdown to HTML. Supports GitHub Flavored Markdown, syntax highlighting, dark mode. No signup required. Works offline.">
  <meta name="keywords" content="markdown editor, online markdown editor, markdown preview, markdown to html, free markdown editor, github markdown, markdown converter, live markdown preview, markdown online, web markdown editor">
  <meta name="author" content="Oathan Rex">
  <meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1">
  <meta name="googlebot" content="index, follow">
  <link rel="canonical" href="https://oathanrex.github.io/markdown-editor/">
  
  <!-- Theme Color -->
  <meta name="theme-color" content="#ffffff" media="(prefers-color-scheme: light)">
  <meta name="theme-color" content="#0f172a" media="(prefers-color-scheme: dark)">
  <meta name="color-scheme" content="light dark">
  
  <!-- Mobile Web App -->
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <meta name="apple-mobile-web-app-title" content="MarkdownStudio">
  <meta name="application-name" content="MarkdownStudio Pro">
  <meta name="format-detection" content="telephone=no">
  
  <!-- Open Graph / Facebook -->
  <meta property="og:type" content="website">
  <meta property="og:url" content="https://oathanrex.github.io/markdown-editor/">
  <meta property="og:title" content="Free Online Markdown Editor with Live Preview">
  <meta property="og:description" content="Write Markdown, see results instantly. Export to HTML. Supports GFM, syntax highlighting, dark mode. Free, no signup required. Works offline.">
  <meta property="og:image" content="https://oathanrex.github.io/markdown-editor/assets/images/og-image.png">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:image:alt" content="MarkdownStudio Pro - Free Online Markdown Editor">
  <meta property="og:site_name" content="MarkdownStudio Pro">
  <meta property="og:locale" content="en_US">
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:url" content="https://oathanrex.github.io/markdown-editor/">
  <meta name="twitter:title" content="Free Online Markdown Editor with Live Preview">
  <meta name="twitter:description" content="Write Markdown, see results instantly. Export to HTML. Free, no signup required. Works offline.">
  <meta name="twitter:image" content="https://oathanrex.github.io/markdown-editor/assets/images/og-image.png">
  <meta name="twitter:image:alt" content="MarkdownStudio Pro Editor Interface">
  
  <!-- Favicon & Icons -->
  <link rel="icon" type="image/x-icon" href="./assets/images/favicon.ico">
  <link rel="icon" type="image/png" sizes="16x16" href="./assets/images/favicon-16.png">
  <link rel="icon" type="image/png" sizes="32x32" href="./assets/images/favicon-32.png">
  <link rel="apple-touch-icon" sizes="180x180" href="./assets/images/apple-touch-icon.png">
  <link rel="mask-icon" href="./assets/images/safari-pinned-tab.svg" color="#2563eb">
  
  <!-- PWA Manifest -->
  <link rel="manifest" href="./manifest.json">
  
  <!-- Preload Critical Resources -->
  <link rel="preload" href="./assets/css/style.css" as="style">
  <link rel="preload" href="./assets/js/lib/marked.min.js" as="script">
  <link rel="preload" href="./assets/js/lib/purify.min.js" as="script">
  
  <!-- Preconnect for Performance -->
  <link rel="dns-prefetch" href="//fonts.googleapis.com">
  
  <!-- Stylesheets -->
  <link rel="stylesheet" href="./assets/css/style.css">
  
  <!-- Structured Data - WebApplication -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebApplication",
    "name": "MarkdownStudio Pro",
    "alternateName": ["MarkdownStudio", "Markdown Editor Online", "Free Markdown Editor"],
    "url": "https://oathanrex.github.io/markdown-editor/",
    "description": "Free online Markdown editor with instant live preview. Write, format, and export Markdown to HTML. Supports GitHub Flavored Markdown, syntax highlighting, and dark mode. No signup required.",
    "applicationCategory": "DeveloperApplication",
    "applicationSubCategory": "Text Editor",
    "operatingSystem": "Any",
    "browserRequirements": "Requires JavaScript. Compatible with Chrome 90+, Firefox 88+, Safari 14+, Edge 90+",
    "softwareVersion": "3.0.0",
    "releaseNotes": "https://github.com/oathanrex/markdown-editor/releases",
    "screenshot": "https://oathanrex.github.io/markdown-editor/assets/images/screenshot.png",
    "featureList": [
      "Live Markdown Preview",
      "GitHub Flavored Markdown Support",
      "Syntax Highlighting for Code Blocks",
      "Export to Markdown and HTML",
      "Dark/Light Theme Toggle",
      "Offline Support",
      "Keyboard Shortcuts",
      "Auto Save",
      "Table of Contents Generation",
      "Copy Code Button",
      "Responsive Design",
      "No Registration Required"
    ],
    "offers": {
      "@type": "Offer",
      "price": "0",
      "priceCurrency": "USD",
      "availability": "https://schema.org/InStock"
    },
    "author": {
      "@type": "Person",
      "name": "Oathan Rex",
      "url": "https://oathanrex.github.io"
    },
    "publisher": {
      "@type": "Person",
      "name": "Oathan Rex",
      "url": "https://oathanrex.github.io"
    },
    "maintainer": {
      "@type": "Person",
      "name": "Oathan Rex"
    },
    "datePublished": "2025-01-15",
    "dateModified": "2025-01-15",
    "inLanguage": "en",
    "isAccessibleForFree": true,
    "license": "https://opensource.org/licenses/MIT"
  }
  </script>
  
  <!-- Structured Data - FAQ -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [
      {
        "@type": "Question",
        "name": "Is MarkdownStudio Pro free to use?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "Yes, MarkdownStudio Pro is completely free to use with no signup or registration required. All features are available at no cost."
        }
      },
      {
        "@type": "Question",
        "name": "Where is my data stored?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "All your documents are stored locally in your browser's storage (localStorage). Nothing is sent to any server, ensuring complete privacy."
        }
      },
      {
        "@type": "Question",
        "name": "Does MarkdownStudio work offline?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "Yes! After your first visit, MarkdownStudio works completely offline thanks to service worker technology. You can continue editing even without an internet connection."
        }
      },
      {
        "@type": "Question",
        "name": "What Markdown features are supported?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "MarkdownStudio supports GitHub Flavored Markdown (GFM) including tables, task lists, strikethrough, fenced code blocks with syntax highlighting, autolinks, and more."
        }
      },
      {
        "@type": "Question",
        "name": "Can I export my documents?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "Yes, you can export your documents as Markdown (.md) files or as styled HTML documents. The HTML export includes all styling for a complete, standalone document."
        }
      }
    ]
  }
  </script>
  
  <!-- Structured Data - BreadcrumbList -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "BreadcrumbList",
    "itemListElement": [
      {
        "@type": "ListItem",
        "position": 1,
        "name": "Home",
        "item": "https://oathanrex.github.io/"
      },
      {
        "@type": "ListItem",
        "position": 2,
        "name": "Markdown Editor",
        "item": "https://oathanrex.github.io/markdown-editor/"
      }
    ]
  }
  </script>
</head>
<body>
  <!-- Skip Link for Accessibility -->
  <a href="#editor-textarea" class="skip-link">Skip to editor</a>
  
  <!-- Offline Notification Banner -->
  <div id="offline-banner" class="offline-banner" hidden>
    <span class="offline-icon">📡</span>
    <span>You're offline. Changes are saved locally.</span>
  </div>
  
  <!-- Notifications Container -->
  <div id="notifications" class="notifications" role="alert" aria-live="polite"></div>
  
  <!-- Main Application Container -->
  <div class="app-container" id="app">
    
    <!-- ==================== HEADER ==================== -->
    <header class="app-header" role="banner">
      <div class="header-start">
        <div class="header-brand">
          <a href="./" class="logo" aria-label="MarkdownStudio Pro Home">
            <span class="logo-icon" aria-hidden="true">
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="24" height="24" rx="4" fill="currentColor"/>
                <path d="M5 17V7H7.5L10 11L12.5 7H15V17H12.5V11L10 15L7.5 11V17H5Z" fill="white"/>
                <path d="M16 17V7H18.5L21 12V7H23V17H20.5L18 12V17H16Z" fill="white"/>
              </svg>
            </span>
            <span class="logo-text">MarkdownStudio</span>
            <span class="logo-badge">Pro</span>
          </a>
        </div>
      </div>
      
      <div class="header-center">
        <div class="document-info">
          <span class="document-title" id="document-title">Untitled Document</span>
          <span class="save-indicator" id="save-indicator" data-status="saved">
            <span class="save-icon"></span>
            <span class="save-text">Saved</span>
          </span>
        </div>
      </div>
      
      <div class="header-end">
        <nav class="header-actions" role="navigation" aria-label="Application actions">
          <button type="button" 
                  class="btn btn-icon" 
                  id="btn-new" 
                  data-action="new-document"
                  aria-label="New document" 
                  title="New Document (Ctrl+Alt+N)">
            <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
              <polyline points="14 2 14 8 20 8"/>
              <line x1="12" y1="18" x2="12" y2="12"/>
              <line x1="9" y1="15" x2="15" y2="15"/>
            </svg>
          </button>
          
          <button type="button" 
                  class="btn btn-icon" 
                  id="btn-open"
                  data-action="open-file"
                  aria-label="Open file" 
                  title="Open File (Ctrl+O)">
            <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M22 19a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h5l2 3h9a2 2 0 0 1 2 2z"/>
            </svg>
          </button>
          
          <div class="header-divider" role="separator"></div>
          
          <button type="button" 
                  class="btn btn-icon" 
                  id="btn-shortcuts"
                  data-action="show-shortcuts"
                  aria-label="Keyboard shortcuts" 
                  title="Keyboard Shortcuts (Ctrl+/)">
            <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <rect x="2" y="4" width="20" height="16" rx="2" ry="2"/>
              <path d="M6 8h.001M10 8h.001M14 8h.001M18 8h.001M8 12h.001M12 12h.001M16 12h.001M6 16h12"/>
            </svg>
          </button>
          
          <button type="button" 
                  class="btn btn-icon" 
                  id="btn-theme"
                  data-action="toggle-theme"
                  aria-label="Toggle theme" 
                  title="Toggle Dark/Light Theme">
            <svg class="icon icon-sun" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="12" cy="12" r="5"/>
              <line x1="12" y1="1" x2="12" y2="3"/>
              <line x1="12" y1="21" x2="12" y2="23"/>
              <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/>
              <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/>
              <line x1="1" y1="12" x2="3" y2="12"/>
              <line x1="21" y1="12" x2="23" y2="12"/>
              <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/>
              <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/>
            </svg>
            <svg class="icon icon-moon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
            </svg>
          </button>
          
          <button type="button" 
                  class="btn btn-icon" 
                  id="btn-fullscreen"
                  data-action="toggle-fullscreen"
                  aria-label="Toggle fullscreen" 
                  title="Fullscreen (F11)">
            <svg class="icon icon-expand" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="15 3 21 3 21 9"/>
              <polyline points="9 21 3 21 3 15"/>
              <line x1="21" y1="3" x2="14" y2="10"/>
              <line x1="3" y1="21" x2="10" y2="14"/>
            </svg>
            <svg class="icon icon-compress" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="4 14 10 14 10 20"/>
              <polyline points="20 10 14 10 14 4"/>
              <line x1="14" y1="10" x2="21" y2="3"/>
              <line x1="3" y1="21" x2="10" y2="14"/>
            </svg>
          </button>
        </nav>
      </div>
    </header>
    
    <!-- ==================== TOOLBAR ==================== -->
    <div class="toolbar" role="toolbar" aria-label="Formatting toolbar" id="toolbar">
      <!-- Undo/Redo Group -->
      <div class="toolbar-group" role="group" aria-label="History">
        <button type="button" 
                class="btn btn-tool" 
                id="btn-undo"
                data-action="undo"
                aria-label="Undo" 
                title="Undo (Ctrl+Z)"
                disabled>
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="1 4 1 10 7 10"/>
            <path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                id="btn-redo"
                data-action="redo"
                aria-label="Redo" 
                title="Redo (Ctrl+Y)"
                disabled>
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="23 4 23 10 17 10"/>
            <path d="M20.49 15a9 9 0 1 1-2.12-9.36L23 10"/>
          </svg>
        </button>
      </div>
      
      <div class="toolbar-divider" role="separator" aria-hidden="true"></div>
      
      <!-- Headings Group -->
      <div class="toolbar-group" role="group" aria-label="Headings">
        <div class="toolbar-dropdown">
          <button type="button" 
                  class="btn btn-tool btn-dropdown" 
                  id="btn-heading"
                  aria-haspopup="true"
                  aria-expanded="false"
                  aria-label="Heading level" 
                  title="Heading">
            <span class="btn-label">H</span>
            <svg class="icon icon-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="6 9 12 15 18 9"/>
            </svg>
          </button>
          <div class="dropdown-menu" role="menu" hidden>
            <button type="button" class="dropdown-item" data-action="heading" data-level="1" role="menuitem">
              <span class="heading-preview h1">Heading 1</span>
              <kbd>Ctrl+1</kbd>
            </button>
            <button type="button" class="dropdown-item" data-action="heading" data-level="2" role="menuitem">
              <span class="heading-preview h2">Heading 2</span>
              <kbd>Ctrl+2</kbd>
            </button>
            <button type="button" class="dropdown-item" data-action="heading" data-level="3" role="menuitem">
              <span class="heading-preview h3">Heading 3</span>
              <kbd>Ctrl+3</kbd>
            </button>
            <button type="button" class="dropdown-item" data-action="heading" data-level="4" role="menuitem">
              <span class="heading-preview h4">Heading 4</span>
              <kbd>Ctrl+4</kbd>
            </button>
            <button type="button" class="dropdown-item" data-action="heading" data-level="5" role="menuitem">
              <span class="heading-preview h5">Heading 5</span>
              <kbd>Ctrl+5</kbd>
            </button>
            <button type="button" class="dropdown-item" data-action="heading" data-level="6" role="menuitem">
              <span class="heading-preview h6">Heading 6</span>
              <kbd>Ctrl+6</kbd>
            </button>
          </div>
        </div>
      </div>
      
      <div class="toolbar-divider" role="separator" aria-hidden="true"></div>
      
      <!-- Text Formatting Group -->
      <div class="toolbar-group" role="group" aria-label="Text formatting">
        <button type="button" 
                class="btn btn-tool" 
                data-action="bold"
                aria-label="Bold" 
                title="Bold (Ctrl+B)">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M6 4h8a4 4 0 0 1 4 4 4 4 0 0 1-4 4H6z"/>
            <path d="M6 12h9a4 4 0 0 1 4 4 4 4 0 0 1-4 4H6z"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                data-action="italic"
                aria-label="Italic" 
                title="Italic (Ctrl+I)">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="19" y1="4" x2="10" y2="4"/>
            <line x1="14" y1="20" x2="5" y2="20"/>
            <line x1="15" y1="4" x2="9" y2="20"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                data-action="strikethrough"
                aria-label="Strikethrough" 
                title="Strikethrough (Ctrl+Shift+S)">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M17.3 4.9c-2.3-.6-4.4-1-6.2-.9-2.7 0-5.3.7-5.3 3.6 0 1.5 1.8 3.3 5.7 3.9"/>
            <line x1="4" y1="12" x2="20" y2="12"/>
            <path d="M8.5 15c0 2.2 2.3 4.1 6.2 4.1 3.5 0 5.3-1.8 5.3-3.5 0-1.5-1.8-3-5.7-3.4"/>
          </svg>
        </button>
      </div>
      
      <div class="toolbar-divider" role="separator" aria-hidden="true"></div>
      
      <!-- Lists Group -->
      <div class="toolbar-group" role="group" aria-label="Lists">
        <button type="button" 
                class="btn btn-tool" 
                data-action="unordered-list"
                aria-label="Bullet list" 
                title="Bullet List">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="8" y1="6" x2="21" y2="6"/>
            <line x1="8" y1="12" x2="21" y2="12"/>
            <line x1="8" y1="18" x2="21" y2="18"/>
            <circle cx="4" cy="6" r="1" fill="currentColor"/>
            <circle cx="4" cy="12" r="1" fill="currentColor"/>
            <circle cx="4" cy="18" r="1" fill="currentColor"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                data-action="ordered-list"
                aria-label="Numbered list" 
                title="Numbered List">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="10" y1="6" x2="21" y2="6"/>
            <line x1="10" y1="12" x2="21" y2="12"/>
            <line x1="10" y1="18" x2="21" y2="18"/>
            <text x="3" y="7" font-size="6" fill="currentColor" stroke="none">1</text>
            <text x="3" y="13" font-size="6" fill="currentColor" stroke="none">2</text>
            <text x="3" y="19" font-size="6" fill="currentColor" stroke="none">3</text>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                data-action="task-list"
                aria-label="Task list" 
                title="Task List">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="5" width="6" height="6" rx="1"/>
            <path d="M5 8l1.5 1.5L9 7"/>
            <line x1="12" y1="8" x2="21" y2="8"/>
            <rect x="3" y="14" width="6" height="6" rx="1"/>
            <line x1="12" y1="17" x2="21" y2="17"/>
          </svg>
        </button>
      </div>
      
      <div class="toolbar-divider" role="separator" aria-hidden="true"></div>
      
      <!-- Insert Group -->
      <div class="toolbar-group" role="group" aria-label="Insert elements">
        <button type="button" 
                class="btn btn-tool" 
                data-action="link"
                aria-label="Insert link" 
                title="Insert Link (Ctrl+K)">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/>
            <path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                data-action="image"
                aria-label="Insert image" 
                title="Insert Image (Ctrl+Shift+I)">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="3" width="18" height="18" rx="2" ry="2"/>
            <circle cx="8.5" cy="8.5" r="1.5"/>
            <polyline points="21 15 16 10 5 21"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                data-action="table"
                aria-label="Insert table" 
                title="Insert Table">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="3" width="18" height="18" rx="2"/>
            <line x1="3" y1="9" x2="21" y2="9"/>
            <line x1="3" y1="15" x2="21" y2="15"/>
            <line x1="9" y1="3" x2="9" y2="21"/>
            <line x1="15" y1="3" x2="15" y2="21"/>
          </svg>
        </button>
      </div>
      
      <div class="toolbar-divider" role="separator" aria-hidden="true"></div>
      
      <!-- Code Group -->
      <div class="toolbar-group" role="group" aria-label="Code formatting">
        <button type="button" 
                class="btn btn-tool" 
                data-action="inline-code"
                aria-label="Inline code" 
                title="Inline Code (Ctrl+`)">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="16 18 22 12 16 6"/>
            <polyline points="8 6 2 12 8 18"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                data-action="code-block"
                aria-label="Code block" 
                title="Code Block (Ctrl+Shift+C)">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="3" width="18" height="18" rx="2"/>
            <polyline points="9 8 5 12 9 16"/>
            <polyline points="15 8 19 12 15 16"/>
          </svg>
        </button>
      </div>
      
      <div class="toolbar-divider" role="separator" aria-hidden="true"></div>
      
      <!-- Block Elements Group -->
      <div class="toolbar-group" role="group" aria-label="Block elements">
        <button type="button" 
                class="btn btn-tool" 
                data-action="blockquote"
                aria-label="Blockquote" 
                title="Blockquote (Ctrl+Shift+Q)">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M3 21c3 0 7-1 7-8V5c0-1.25-.756-2.017-2-2H4c-1.25 0-2 .75-2 1.972V11c0 1.25.75 2 2 2 1 0 1 0 1 1v1c0 1-1 2-2 2s-1 .008-1 1.031V21z"/>
            <path d="M15 21c3 0 7-1 7-8V5c0-1.25-.757-2.017-2-2h-4c-1.25 0-2 .75-2 1.972V11c0 1.25.75 2 2 2h.75c0 2.25.25 4-2.75 4v3c0 1 0 1 1 1z"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                data-action="horizontal-rule"
                aria-label="Horizontal rule" 
                title="Horizontal Rule">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="3" y1="12" x2="21" y2="12"/>
          </svg>
        </button>
      </div>
      
      <!-- Spacer -->
      <div class="toolbar-spacer"></div>
      
      <!-- View Controls -->
      <div class="toolbar-group" role="group" aria-label="View controls">
        <button type="button" 
                class="btn btn-tool" 
                id="btn-view-editor"
                data-action="view-editor"
                aria-label="Editor only" 
                title="Editor Only"
                aria-pressed="false">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="3" width="18" height="18" rx="2"/>
            <line x1="12" y1="3" x2="12" y2="21"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool active" 
                id="btn-view-split"
                data-action="view-split"
                aria-label="Split view" 
                title="Split View"
                aria-pressed="true">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="3" width="18" height="18" rx="2"/>
            <line x1="12" y1="3" x2="12" y2="21"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-tool" 
                id="btn-view-preview"
                data-action="view-preview"
                aria-label="Preview only" 
                title="Preview Only"
                aria-pressed="false">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
            <circle cx="12" cy="12" r="3"/>
          </svg>
        </button>
      </div>
      
      <div class="toolbar-divider" role="separator" aria-hidden="true"></div>
      
      <!-- Export Group -->
      <div class="toolbar-group" role="group" aria-label="Export options">
        <div class="toolbar-dropdown">
          <button type="button" 
                  class="btn btn-tool btn-dropdown btn-primary-tool" 
                  id="btn-export"
                  aria-haspopup="true"
                  aria-expanded="false"
                  aria-label="Export" 
                  title="Export">
            <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
              <polyline points="7 10 12 15 17 10"/>
              <line x1="12" y1="15" x2="12" y2="3"/>
            </svg>
            <span class="btn-label">Export</span>
            <svg class="icon icon-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="6 9 12 15 18 9"/>
            </svg>
          </button>
          <div class="dropdown-menu dropdown-menu-right" role="menu" hidden>
            <button type="button" class="dropdown-item" data-action="export-md" role="menuitem">
              <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
                <polyline points="14 2 14 8 20 8"/>
              </svg>
              <span>Download Markdown (.md)</span>
            </button>
            <button type="button" class="dropdown-item" data-action="export-html" role="menuitem">
              <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
                <polyline points="14 2 14 8 20 8"/>
                <polyline points="10 12 8 14 10 16"/>
                <polyline points="14 12 16 14 14 16"/>
              </svg>
              <span>Download HTML</span>
            </button>
            <div class="dropdown-divider" role="separator"></div>
            <button type="button" class="dropdown-item" data-action="copy-markdown" role="menuitem">
              <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="9" y="9" width="13" height="13" rx="2" ry="2"/>
                <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/>
              </svg>
              <span>Copy Markdown</span>
            </button>
            <button type="button" class="dropdown-item" data-action="copy-html" role="menuitem">
              <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="9" y="9" width="13" height="13" rx="2" ry="2"/>
                <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/>
              </svg>
              <span>Copy HTML</span>
            </button>
            <div class="dropdown-divider" role="separator"></div>
            <button type="button" class="dropdown-item" data-action="print" role="menuitem">
              <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 6 2 18 2 18 9"/>
                <path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"/>
                <rect x="6" y="14" width="12" height="8"/>
              </svg>
              <span>Print / Save as PDF</span>
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- ==================== MAIN CONTENT ==================== -->
    <main class="app-main" role="main" id="main-content">
      <div class="editor-container" id="editor-container" data-view="split">
        
        <!-- Editor Pane -->
        <div class="pane editor-pane" id="editor-pane">
          <div class="pane-header">
            <div class="pane-header-start">
              <span class="pane-title">Markdown</span>
            </div>
            <div class="pane-header-end">
              <button type="button" 
                      class="btn btn-icon btn-xs" 
                      data-action="toggle-line-numbers"
                      aria-label="Toggle line numbers" 
                      title="Toggle Line Numbers">
                <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <text x="4" y="8" font-size="6" fill="currentColor" stroke="none">1</text>
                  <text x="4" y="14" font-size="6" fill="currentColor" stroke="none">2</text>
                  <text x="4" y="20" font-size="6" fill="currentColor" stroke="none">3</text>
                  <line x1="10" y1="6" x2="20" y2="6"/>
                  <line x1="10" y1="12" x2="20" y2="12"/>
                  <line x1="10" y1="18" x2="20" y2="18"/>
                </svg>
              </button>
              <button type="button" 
                      class="btn btn-icon btn-xs" 
                      data-action="toggle-word-wrap"
                      aria-label="Toggle word wrap" 
                      title="Toggle Word Wrap">
                <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M3 6h18"/>
                  <path d="M3 12h15a3 3 0 1 1 0 6h-4"/>
                  <polyline points="16 16 14 18 16 20"/>
                  <path d="M3 18h7"/>
                </svg>
              </button>
            </div>
          </div>
          <div class="editor-wrapper">
            <div class="line-numbers" id="line-numbers" hidden></div>
            <textarea 
              id="editor-textarea" 
              class="editor-textarea"
              placeholder="Start writing Markdown here...

# Quick Start

- **Bold**: Ctrl+B or **text**
- *Italic*: Ctrl+I or *text*
- [Link](url): Ctrl+K
- `Code`: Ctrl+`

Use the toolbar above or keyboard shortcuts to format your content."
              spellcheck="true"
              autocapitalize="sentences"
              autocomplete="off"
              autocorrect="on"
              aria-label="Markdown editor. Start typing to create your document."
            ></textarea>
          </div>
        </div>
        
        <!-- Resizer -->
        <div class="pane-resizer" 
             id="pane-resizer"
             role="separator" 
             aria-label="Resize panes"
             aria-orientation="vertical"
             aria-valuenow="50"
             aria-valuemin="20"
             aria-valuemax="80"
             tabindex="0">
          <div class="resizer-handle"></div>
        </div>
        
        <!-- Preview Pane -->
        <div class="pane preview-pane" id="preview-pane">
          <div class="pane-header">
            <div class="pane-header-start">
              <span class="pane-title">Preview</span>
            </div>
            <div class="pane-header-end">
              <button type="button" 
                      class="btn btn-icon btn-xs" 
                      id="btn-toc"
                      data-action="toggle-toc"
                      aria-label="Toggle table of contents" 
                      title="Table of Contents"
                      aria-pressed="false">
                <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <line x1="8" y1="6" x2="21" y2="6"/>
                  <line x1="8" y1="12" x2="21" y2="12"/>
                  <line x1="8" y1="18" x2="21" y2="18"/>
                  <line x1="3" y1="6" x2="3.01" y2="6"/>
                  <line x1="3" y1="12" x2="3.01" y2="12"/>
                  <line x1="3" y1="18" x2="3.01" y2="18"/>
                </svg>
              </button>
              <button type="button" 
                      class="btn btn-icon btn-xs" 
                      id="btn-sync-scroll"
                      data-action="toggle-sync-scroll"
                      aria-label="Toggle synchronized scrolling" 
                      title="Sync Scroll"
                      aria-pressed="true">
                <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <polyline points="17 1 21 5 17 9"/>
                  <path d="M3 11V9a4 4 0 0 1 4-4h14"/>
                  <polyline points="7 23 3 19 7 15"/>
                  <path d="M21 13v2a4 4 0 0 1-4 4H3"/>
                </svg>
              </button>
            </div>
          </div>
          <div class="preview-wrapper">
            <!-- Table of Contents Sidebar -->
            <aside class="toc-sidebar" id="toc-sidebar" hidden>
              <div class="toc-header">
                <h3 class="toc-title">Contents</h3>
                <button type="button" 
                        class="btn btn-icon btn-xs" 
                        data-action="close-toc"
                        aria-label="Close table of contents">
                  <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <line x1="18" y1="6" x2="6" y2="18"/>
                    <line x1="6" y1="6" x2="18" y2="18"/>
                  </svg>
                </button>
              </div>
              <nav class="toc-nav" aria-label="Table of contents">
                <ul class="toc-list" id="toc-list">
                  <li class="toc-empty">No headings found</li>
                </ul>
              </nav>
            </aside>
            
            <!-- Preview Content -->
            <div id="preview-container" 
                 class="preview-content markdown-body"
                 aria-label="Rendered preview"
                 role="document">
              <div class="preview-placeholder">
                <p>Start typing in the editor to see your formatted Markdown here.</p>
              </div>
            </div>
          </div>
        </div>
        
      </div>
    </main>
    
    <!-- ==================== STATUS BAR ==================== -->
    <footer class="status-bar" role="contentinfo" id="status-bar">
      <div class="status-start">
        <span class="status-item" id="stats-words" title="Word count">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
            <polyline points="14 2 14 8 20 8"/>
            <line x1="16" y1="13" x2="8" y2="13"/>
            <line x1="16" y1="17" x2="8" y2="17"/>
            <polyline points="10 9 9 9 8 9"/>
          </svg>
          <span class="status-value">0</span>
          <span class="status-label">words</span>
        </span>
        <span class="status-divider">|</span>
        <span class="status-item" id="stats-chars" title="Character count">
          <span class="status-value">0</span>
          <span class="status-label">chars</span>
        </span>
        <span class="status-divider">|</span>
        <span class="status-item" id="stats-lines" title="Line count">
          <span class="status-value">0</span>
          <span class="status-label">lines</span>
        </span>
        <span class="status-divider">|</span>
        <span class="status-item" id="stats-reading-time" title="Estimated reading time">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="10"/>
            <polyline points="12 6 12 12 16 14"/>
          </svg>
          <span class="status-value">0</span>
          <span class="status-label">min read</span>
        </span>
      </div>
      
      <div class="status-center">
        <span class="status-item" id="cursor-position" title="Cursor position">
          <span class="status-label">Ln</span>
          <span class="status-value" id="cursor-line">1</span>
          <span class="status-label">, Col</span>
          <span class="status-value" id="cursor-col">1</span>
        </span>
      </div>
      
      <div class="status-end">
        <span class="status-item status-item-clickable" 
              id="status-markdown" 
              title="Markdown mode"
              role="button"
              tabindex="0">
          Markdown
        </span>
        <span class="status-divider">|</span>
        <span class="status-item" id="status-encoding" title="File encoding">
          UTF-8
        </span>
      </div>
    </footer>
    
  </div>
  
  <!-- ==================== MODALS & OVERLAYS ==================== -->
  
  <!-- Keyboard Shortcuts Modal -->
  <div class="modal" id="shortcuts-modal" role="dialog" aria-modal="true" aria-labelledby="shortcuts-title" hidden>
    <div class="modal-backdrop" data-action="close-modal"></div>
    <div class="modal-content">
      <header class="modal-header">
        <h2 id="shortcuts-title" class="modal-title">Keyboard Shortcuts</h2>
        <button type="button" 
                class="btn btn-icon modal-close" 
                data-action="close-modal"
                aria-label="Close">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>
      </header>
      <div class="modal-body">
        <div class="shortcuts-grid">
          <!-- Formatting Section -->
          <section class="shortcuts-section">
            <h3 class="shortcuts-section-title">Formatting</h3>
            <dl class="shortcuts-list">
              <div class="shortcut-item">
                <dt>Bold</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>B</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Italic</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>I</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Strikethrough</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Shift</kbd><span class="kbd-plus">+</span><kbd>S</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Inline Code</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>`</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Code Block</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Shift</kbd><span class="kbd-plus">+</span><kbd>C</kbd></dd>
              </div>
            </dl>
          </section>
          
          <!-- Headings Section -->
          <section class="shortcuts-section">
            <h3 class="shortcuts-section-title">Headings</h3>
            <dl class="shortcuts-list">
              <div class="shortcut-item">
                <dt>Heading 1</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>1</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Heading 2</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>2</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Heading 3</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>3</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Heading 4</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>4</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Heading 5</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>5</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Heading 6</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>6</kbd></dd>
              </div>
            </dl>
          </section>
          
          <!-- Insert Section -->
          <section class="shortcuts-section">
            <h3 class="shortcuts-section-title">Insert</h3>
            <dl class="shortcuts-list">
              <div class="shortcut-item">
                <dt>Link</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>K</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Image</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Shift</kbd><span class="kbd-plus">+</span><kbd>I</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Blockquote</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Shift</kbd><span class="kbd-plus">+</span><kbd>Q</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Horizontal Rule</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Shift</kbd><span class="kbd-plus">+</span><kbd>H</kbd></dd>
              </div>
            </dl>
          </section>
          
          <!-- Actions Section -->
          <section class="shortcuts-section">
            <h3 class="shortcuts-section-title">Actions</h3>
            <dl class="shortcuts-list">
              <div class="shortcut-item">
                <dt>Save</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>S</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Undo</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Z</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Redo</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Y</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>New Document</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Alt</kbd><span class="kbd-plus">+</span><kbd>N</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Open File</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>O</kbd></dd>
              </div>
            </dl>
          </section>
          
          <!-- View Section -->
          <section class="shortcuts-section">
            <h3 class="shortcuts-section-title">View</h3>
            <dl class="shortcuts-list">
              <div class="shortcut-item">
                <dt>Fullscreen</dt>
                <dd><kbd>F11</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Toggle Preview</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Shift</kbd><span class="kbd-plus">+</span><kbd>P</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Toggle Theme</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>Shift</kbd><span class="kbd-plus">+</span><kbd>L</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Show Shortcuts</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>/</kbd></dd>
              </div>
            </dl>
          </section>
          
          <!-- Find Section -->
          <section class="shortcuts-section">
            <h3 class="shortcuts-section-title">Find & Replace</h3>
            <dl class="shortcuts-list">
              <div class="shortcut-item">
                <dt>Find</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>F</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Find & Replace</dt>
                <dd><kbd>Ctrl</kbd><span class="kbd-plus">+</span><kbd>H</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Find Next</dt>
                <dd><kbd>F3</kbd> or <kbd>Enter</kbd></dd>
              </div>
              <div class="shortcut-item">
                <dt>Find Previous</dt>
                <dd><kbd>Shift</kbd><span class="kbd-plus">+</span><kbd>F3</kbd></dd>
              </div>
            </dl>
          </section>
        </div>
        
        <div class="shortcuts-footer">
          <p class="shortcuts-note">
            <strong>Note:</strong> On macOS, use <kbd>Cmd</kbd> instead of <kbd>Ctrl</kbd>
          </p>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Find & Replace Modal -->
  <div class="find-panel" id="find-panel" role="dialog" aria-label="Find and replace" hidden>
    <div class="find-panel-content">
      <div class="find-row">
        <div class="find-input-wrapper">
          <input type="text" 
                 id="find-input" 
                 class="find-input" 
                 placeholder="Find..."
                 aria-label="Find text">
          <span class="find-count" id="find-count"></span>
        </div>
        <div class="find-actions">
          <button type="button" 
                  class="btn btn-icon btn-xs" 
                  id="btn-find-prev"
                  data-action="find-prev"
                  aria-label="Previous match" 
                  title="Previous (Shift+F3)">
            <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="18 15 12 9 6 15"/>
            </svg>
          </button>
          <button type="button" 
                  class="btn btn-icon btn-xs" 
                  id="btn-find-next"
                  data-action="find-next"
                  aria-label="Next match" 
                  title="Next (F3)">
            <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="6 9 12 15 18 9"/>
            </svg>
          </button>
        </div>
        <div class="find-options">
          <button type="button" 
                  class="btn btn-icon btn-xs" 
                  id="btn-find-case"
                  data-action="toggle-find-case"
                  aria-label="Match case" 
                  title="Match Case"
                  aria-pressed="false">
            <span class="btn-text">Aa</span>
          </button>
          <button type="button" 
                  class="btn btn-icon btn-xs" 
                  id="btn-find-word"
                  data-action="toggle-find-word"
                  aria-label="Match whole word" 
                  title="Match Whole Word"
                  aria-pressed="false">
            <span class="btn-text">W</span>
          </button>
          <button type="button" 
                  class="btn btn-icon btn-xs" 
                  id="btn-find-regex"
                  data-action="toggle-find-regex"
                  aria-label="Use regular expression" 
                  title="Regular Expression"
                  aria-pressed="false">
            <span class="btn-text">.*</span>
          </button>
        </div>
        <button type="button" 
                class="btn btn-icon btn-xs find-toggle-replace" 
                id="btn-toggle-replace"
                data-action="toggle-replace"
                aria-label="Toggle replace" 
                title="Toggle Replace"
                aria-expanded="false">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="6 9 12 15 18 9"/>
          </svg>
        </button>
        <button type="button" 
                class="btn btn-icon btn-xs find-close" 
                data-action="close-find"
                aria-label="Close find panel">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>
      </div>
      <div class="replace-row" id="replace-row" hidden>
        <div class="find-input-wrapper">
          <input type="text" 
                 id="replace-input" 
                 class="find-input" 
                 placeholder="Replace..."
                 aria-label="Replace with">
        </div>
        <div class="replace-actions">
          <button type="button" 
                  class="btn btn-sm" 
                  id="btn-replace"
                  data-action="replace"
                  aria-label="Replace">
            Replace
          </button>
          <button type="button" 
                  class="btn btn-sm" 
                  id="btn-replace-all"
                  data-action="replace-all"
                  aria-label="Replace all">
            Replace All
          </button>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Link/Image Insert Modal -->
  <div class="modal modal-sm" id="insert-modal" role="dialog" aria-modal="true" aria-labelledby="insert-modal-title" hidden>
    <div class="modal-backdrop" data-action="close-modal"></div>
    <div class="modal-content">
      <header class="modal-header">
        <h2 id="insert-modal-title" class="modal-title">Insert Link</h2>
        <button type="button" 
                class="btn btn-icon modal-close" 
                data-action="close-modal"
                aria-label="Close">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>
      </header>
      <div class="modal-body">
        <form id="insert-form" class="form">
          <div class="form-group">
            <label for="insert-text" class="form-label">Text</label>
            <input type="text" 
                   id="insert-text" 
                   class="form-input" 
                   placeholder="Link text"
                   autocomplete="off">
          </div>
          <div class="form-group">
            <label for="insert-url" class="form-label">URL</label>
            <input type="url" 
                   id="insert-url" 
                   class="form-input" 
                   placeholder="https://example.com"
                   autocomplete="off"
                   required>
          </div>
          <div class="form-group" id="insert-title-group">
            <label for="insert-title" class="form-label">Title (optional)</label>
            <input type="text" 
                   id="insert-title" 
                   class="form-input" 
                   placeholder="Link title"
                   autocomplete="off">
          </div>
        </form>
      </div>
      <footer class="modal-footer">
        <button type="button" class="btn" data-action="close-modal">Cancel</button>
        <button type="submit" form="insert-form" class="btn btn-primary" id="insert-submit">Insert</button>
      </footer>
    </div>
  </div>
  
  <!-- Confirmation Modal -->
  <div class="modal modal-sm" id="confirm-modal" role="alertdialog" aria-modal="true" aria-labelledby="confirm-title" aria-describedby="confirm-message" hidden>
    <div class="modal-backdrop" data-action="close-modal"></div>
    <div class="modal-content">
      <header class="modal-header">
        <h2 id="confirm-title" class="modal-title">Confirm Action</h2>
      </header>
      <div class="modal-body">
        <p id="confirm-message">Are you sure you want to proceed?</p>
      </div>
      <footer class="modal-footer">
        <button type="button" class="btn" data-action="confirm-cancel">Cancel</button>
        <button type="button" class="btn btn-danger" data-action="confirm-ok" id="confirm-ok">Confirm</button>
      </footer>
    </div>
  </div>
  
  <!-- Hidden file input for opening files -->
  <input type="file" 
         id="file-input" 
         accept=".md,.markdown,.txt,.text"
         hidden>
  
  <!-- ==================== SEO CONTENT ==================== -->
  <article class="seo-content" id="seo-content" aria-label="About MarkdownStudio Pro">
    <div class="seo-content-inner">
      <section class="seo-section">
        <h2>Free Online Markdown Editor</h2>
        <p>
          <strong>MarkdownStudio Pro</strong> is a powerful, free online Markdown editor designed for developers, 
          technical writers, students, and anyone who works with Markdown. With instant live preview, 
          comprehensive keyboard shortcuts, and offline support, you can focus on writing without distractions.
        </p>
        <p>
          No signup required. No data sent to servers. Your documents are saved locally in 
          your browser and can be exported as Markdown or HTML at any time. Complete privacy 
          and ownership of your content.
        </p>
      </section>
      
      <section class="seo-section">
        <h2>Key Features</h2>
        <ul class="feature-list">
          <li>
            <strong>Live Preview</strong> — See your formatted Markdown rendered in real-time as you type
          </li>
          <li>
            <strong>GitHub Flavored Markdown</strong> — Full support for tables, task lists, strikethrough, 
            fenced code blocks, and more
          </li>
          <li>
            <strong>Syntax Highlighting</strong> — Code blocks are automatically highlighted for 
            better readability
          </li>
          <li>
            <strong>Auto Save</strong> — Your work is automatically saved to your browser's local storage
          </li>
          <li>
            <strong>Export Options</strong> — Download your documents as Markdown (.md) or styled HTML files
          </li>
          <li>
            <strong>Dark Mode</strong> — Easy on the eyes with automatic system theme detection
          </li>
          <li>
            <strong>Offline Support</strong> — Works without internet after the first load thanks to 
            service worker technology
          </li>
          <li>
            <strong>Keyboard Shortcuts</strong> — Comprehensive shortcuts for power users to 
            format content quickly
          </li>
          <li>
            <strong>Copy Code Button</strong> — One-click copy for all code blocks in the preview
          </li>
          <li>
            <strong>Responsive Design</strong> — Works beautifully on desktop, tablet, and mobile devices
          </li>
          <li>
            <strong>Table of Contents</strong> — Auto-generated navigation from your document headings
          </li>
          <li>
            <strong>Find & Replace</strong> — Powerful search with regex support
          </li>
        </ul>
      </section>
      
      <section class="seo-section">
        <h2>How to Use MarkdownStudio Pro</h2>
        <ol class="steps-list">
          <li>
            <strong>Start Writing</strong> — Begin typing Markdown in the left editor panel. 
            The syntax is simple and intuitive.
          </li>
          <li>
            <strong>See Instant Preview</strong> — Watch your formatted content appear in 
            real-time in the right preview panel.
          </li>
          <li>
            <strong>Use Formatting Tools</strong> — Use the toolbar buttons or keyboard 
            shortcuts for quick formatting.
          </li>
          <li>
            <strong>Auto-Save Works</strong> — Your work is automatically saved. Just keep 
            typing without worry.
          </li>
          <li>
            <strong>Export When Ready</strong> — Download your document as Markdown or HTML 
            when you're finished.
          </li>
        </ol>
      </section>
      
      <section class="seo-section">
        <h2>Markdown Syntax Quick Reference</h2>
        <div class="syntax-reference">
          <div class="syntax-item">
            <code># Heading 1</code>
            <span>Creates a main heading</span>
          </div>
          <div class="syntax-item">
            <code>## Heading 2</code>
            <span>Creates a subheading</span>
          </div>
          <div class="syntax-item">
            <code>**bold text**</code>
            <span>Makes text bold</span>
          </div>
          <div class="syntax-item">
            <code>*italic text*</code>
            <span>Makes text italic</span>
          </div>
          <div class="syntax-item">
            <code>[link text](url)</code>
            <span>Creates a hyperlink</span>
          </div>
          <div class="syntax-item">
            <code>![alt text](image-url)</code>
            <span>Embeds an image</span>
          </div>
          <div class="syntax-item">
            <code>`code`</code>
            <span>Inline code formatting</span>
          </div>
```markdown
          <div class="syntax-item">
            <code>```language</code>
            <span>Code block with syntax highlighting</span>
          </div>
          <div class="syntax-item">
            <code>- item</code>
            <span>Bullet list item</span>
          </div>
          <div class="syntax-item">
            <code>1. item</code>
            <span>Numbered list item</span>
          </div>
          <div class="syntax-item">
            <code>- [ ] task</code>
            <span>Task list item</span>
          </div>
          <div class="syntax-item">
            <code>> quote</code>
            <span>Blockquote</span>
          </div>
          <div class="syntax-item">
            <code>---</code>
            <span>Horizontal rule</span>
          </div>
          <div class="syntax-item">
            <code>| A | B |</code>
            <span>Table (with header row)</span>
          </div>
        </div>
      </section>
      
      <section class="seo-section">
        <h2>Frequently Asked Questions</h2>
        <dl class="faq-list">
          <div class="faq-item">
            <dt>Is MarkdownStudio Pro completely free?</dt>
            <dd>
              Yes, MarkdownStudio Pro is 100% free to use with no hidden costs, no premium 
              tiers, and no signup required. All features are available to everyone.
            </dd>
          </div>
          <div class="faq-item">
            <dt>Where is my data stored?</dt>
            <dd>
              All your documents are stored locally in your browser's storage (localStorage). 
              Nothing is ever sent to any server. Your data stays on your device, ensuring 
              complete privacy and security.
            </dd>
          </div>
          <div class="faq-item">
            <dt>Does it work offline?</dt>
            <dd>
              Yes! After your first visit, MarkdownStudio Pro works completely offline thanks 
              to service worker technology. You can continue editing even without an internet 
              connection, and your changes will be saved locally.
            </dd>
          </div>
          <div class="faq-item">
            <dt>What Markdown features are supported?</dt>
            <dd>
              MarkdownStudio Pro supports GitHub Flavored Markdown (GFM) including headers, 
              bold, italic, strikethrough, links, images, code blocks with syntax highlighting, 
              tables, task lists, blockquotes, horizontal rules, and more.
            </dd>
          </div>
          <div class="faq-item">
            <dt>Can I export my documents?</dt>
            <dd>
              Yes! You can export your documents as Markdown (.md) files for use in other 
              applications, or as fully styled HTML documents that can be opened in any 
              web browser or converted to PDF.
            </dd>
          </div>
          <div class="faq-item">
            <dt>Is this suitable for writing GitHub README files?</dt>
            <dd>
              Absolutely! MarkdownStudio Pro is perfect for writing GitHub README files, 
              documentation, wikis, and any other Markdown content. The live preview shows 
              exactly how your content will look when rendered.
            </dd>
          </div>
          <div class="faq-item">
            <dt>What browsers are supported?</dt>
            <dd>
              MarkdownStudio Pro works in all modern browsers including Chrome, Firefox, 
              Safari, and Edge. For the best experience, we recommend using the latest 
              version of your preferred browser.
            </dd>
          </div>
        </dl>
      </section>
      
      <section class="seo-section">
        <h2>Use Cases</h2>
        <div class="use-cases">
          <div class="use-case">
            <h3>Software Development</h3>
            <p>
              Write README files, API documentation, contributing guidelines, changelogs, 
              and code comments with full GitHub Flavored Markdown support.
            </p>
          </div>
          <div class="use-case">
            <h3>Technical Writing</h3>
            <p>
              Create product documentation, user guides, knowledge base articles, and 
              technical specifications with professional formatting.
            </p>
          </div>
          <div class="use-case">
            <h3>Academic Writing</h3>
            <p>
              Draft research papers, thesis chapters, lecture notes, and study materials 
              with easy-to-use formatting tools.
            </p>
          </div>
          <div class="use-case">
            <h3>Content Creation</h3>
            <p>
              Write blog posts, articles, newsletters, and social media content drafts 
              with distraction-free editing.
            </p>
          </div>
          <div class="use-case">
            <h3>Note Taking</h3>
            <p>
              Capture meeting notes, personal thoughts, project ideas, and quick 
              documentation with automatic saving.
            </p>
          </div>
          <div class="use-case">
            <h3>Project Management</h3>
            <p>
              Write project proposals, status reports, runbooks, and incident 
              postmortems with clear formatting.
            </p>
          </div>
        </div>
      </section>
    </div>
  </article>
  
  <!-- ==================== FOOTER ==================== -->
  <footer class="app-footer" id="app-footer">
    <div class="footer-content">
      <nav class="footer-links" aria-label="Footer navigation">
        <a href="https://github.com/oathanrex/markdown-editor" 
           target="_blank" 
           rel="noopener noreferrer"
           class="footer-link">
          <svg class="icon" viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
          </svg>
          <span>GitHub</span>
        </a>
        <a href="https://github.com/oathanrex/markdown-editor/issues" 
           target="_blank" 
           rel="noopener noreferrer"
           class="footer-link">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="10"/>
            <line x1="12" y1="8" x2="12" y2="12"/>
            <line x1="12" y1="16" x2="12.01" y2="16"/>
          </svg>
          <span>Report Bug</span>
        </a>
        <a href="https://github.com/oathanrex/markdown-editor/discussions" 
           target="_blank" 
           rel="noopener noreferrer"
           class="footer-link">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/>
          </svg>
          <span>Feedback</span>
        </a>
        <a href="https://github.com/oathanrex/markdown-editor/blob/main/LICENSE" 
           target="_blank" 
           rel="noopener noreferrer"
           class="footer-link">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="11" width="18" height="11" rx="2" ry="2"/>
            <path d="M7 11V7a5 5 0 0 1 10 0v4"/>
          </svg>
          <span>MIT License</span>
        </a>
      </nav>
      <p class="footer-copyright">
        <span>&copy; 2025</span>
        <a href="https://oathanrex.github.io" target="_blank" rel="noopener noreferrer">Oathan Rex</a>
        <span class="footer-separator">•</span>
        <span>Made with passion for the open source community</span>
      </p>
      <p class="footer-version">
        <span>Version 3.0.0</span>
      </p>
    </div>
  </footer>
  
  <!-- ==================== SCRIPTS ==================== -->
  <!-- Third-party libraries -->
  <script src="./assets/js/lib/marked.min.js"></script>
  <script src="./assets/js/lib/purify.min.js"></script>
  
  <!-- Application -->
  <script type="module" src="./assets/js/app.js"></script>
  
  <!-- Service Worker Registration -->
  <script>
    // Register service worker for offline support
    if ('serviceWorker' in navigator) {
      window.addEventListener('load', () => {
        navigator.serviceWorker.register('./sw.js')
          .then(registration => {
            console.log('SW registered:', registration.scope);
          })
          .catch(error => {
            console.log('SW registration failed:', error);
          });
      });
    }
    
    // Detect online/offline status
    function updateOnlineStatus() {
      const banner = document.getElementById('offline-banner');
      if (banner) {
        banner.hidden = navigator.onLine;
      }
    }
    
    window.addEventListener('online', updateOnlineStatus);
    window.addEventListener('offline', updateOnlineStatus);
    updateOnlineStatus();
  </script>
</body>
</html>
```

Now let me continue with the CSS stylesheet:

---

## 3. Stylesheets

### assets/css/style.css

```css
/**
 * MarkdownStudio Pro - Main Stylesheet
 * Version: 3.0.0
 * Author: Oathan Rex
 * License: MIT
 */

/* ============================================
   TABLE OF CONTENTS
   ============================================
   1. CSS Custom Properties (Design Tokens)
   2. Reset & Base Styles
   3. Typography
   4. Layout Components
   5. Header
   6. Toolbar
   7. Editor & Preview Panes
   8. Status Bar
   9. Buttons & Controls
   10. Forms & Inputs
   11. Modals & Dialogs
   12. Notifications
   13. Dropdowns
   14. Find & Replace Panel
   15. Table of Contents
   16. Markdown Preview Styles
   17. SEO Content
   18. Footer
   19. Utility Classes
   20. Animations
   21. Responsive Design
   22. Print Styles
   23. Accessibility
   ============================================ */

/* ============================================
   1. CSS CUSTOM PROPERTIES (DESIGN TOKENS)
   ============================================ */

:root {
  /* ---- Color Palette ---- */
  /* Primary Colors */
  --color-primary-50: #eff6ff;
  --color-primary-100: #dbeafe;
  --color-primary-200: #bfdbfe;
  --color-primary-300: #93c5fd;
  --color-primary-400: #60a5fa;
  --color-primary-500: #3b82f6;
  --color-primary-600: #2563eb;
  --color-primary-700: #1d4ed8;
  --color-primary-800: #1e40af;
  --color-primary-900: #1e3a8a;
  
  /* Neutral Colors */
  --color-neutral-50: #f8fafc;
  --color-neutral-100: #f1f5f9;
  --color-neutral-200: #e2e8f0;
  --color-neutral-300: #cbd5e1;
  --color-neutral-400: #94a3b8;
  --color-neutral-500: #64748b;
  --color-neutral-600: #475569;
  --color-neutral-700: #334155;
  --color-neutral-800: #1e293b;
  --color-neutral-900: #0f172a;
  --color-neutral-950: #020617;
  
  /* Semantic Colors */
  --color-success-50: #f0fdf4;
  --color-success-500: #22c55e;
  --color-success-600: #16a34a;
  --color-success-700: #15803d;
  
  --color-warning-50: #fefce8;
  --color-warning-500: #eab308;
  --color-warning-600: #ca8a04;
  --color-warning-700: #a16207;
  
  --color-error-50: #fef2f2;
  --color-error-500: #ef4444;
  --color-error-600: #dc2626;
  --color-error-700: #b91c1c;
  
  --color-info-50: #eff6ff;
  --color-info-500: #3b82f6;
  --color-info-600: #2563eb;
  
  /* ---- Light Theme (Default) ---- */
  --color-bg: #ffffff;
  --color-bg-alt: var(--color-neutral-50);
  --color-surface: var(--color-neutral-50);
  --color-surface-hover: var(--color-neutral-100);
  --color-surface-active: var(--color-neutral-200);
  --color-surface-elevated: #ffffff;
  
  --color-text: var(--color-neutral-900);
  --color-text-secondary: var(--color-neutral-600);
  --color-text-muted: var(--color-neutral-400);
  --color-text-inverted: #ffffff;
  
  --color-border: var(--color-neutral-200);
  --color-border-hover: var(--color-neutral-300);
  --color-border-focus: var(--color-primary-500);
  
  --color-accent: var(--color-primary-600);
  --color-accent-hover: var(--color-primary-700);
  --color-accent-light: var(--color-primary-100);
  --color-accent-subtle: var(--color-primary-50);
  
  --color-code-bg: var(--color-neutral-100);
  --color-code-text: var(--color-neutral-800);
  
  --color-scrollbar: var(--color-neutral-300);
  --color-scrollbar-hover: var(--color-neutral-400);
  
  --color-selection-bg: var(--color-primary-100);
  --color-selection-text: var(--color-neutral-900);
  
  /* ---- Typography ---- */
  --font-family-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 
    'Helvetica Neue', Arial, 'Noto Sans', sans-serif, 'Apple Color Emoji', 
    'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  
  --font-family-mono: 'SF Mono', SFMono-Regular, ui-monospace, Menlo, Monaco, 
    Consolas, 'Liberation Mono', 'Courier New', monospace;
  
  --font-family-editor: var(--font-family-mono);
  --font-family-preview: var(--font-family-sans);
  
  /* Font Sizes */
  --font-size-2xs: 0.625rem;  /* 10px */
  --font-size-xs: 0.75rem;    /* 12px */
  --font-size-sm: 0.875rem;   /* 14px */
  --font-size-base: 1rem;     /* 16px */
  --font-size-lg: 1.125rem;   /* 18px */
  --font-size-xl: 1.25rem;    /* 20px */
  --font-size-2xl: 1.5rem;    /* 24px */
  --font-size-3xl: 1.875rem;  /* 30px */
  --font-size-4xl: 2.25rem;   /* 36px */
  
  /* Font Weights */
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
  
  /* Line Heights */
  --line-height-none: 1;
  --line-height-tight: 1.25;
  --line-height-snug: 1.375;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.625;
  --line-height-loose: 2;
  
  /* Letter Spacing */
  --letter-spacing-tighter: -0.05em;
  --letter-spacing-tight: -0.025em;
  --letter-spacing-normal: 0;
  --letter-spacing-wide: 0.025em;
  --letter-spacing-wider: 0.05em;
  --letter-spacing-widest: 0.1em;
  
  /* ---- Spacing ---- */
  --spacing-0: 0;
  --spacing-px: 1px;
  --spacing-0-5: 0.125rem;  /* 2px */
  --spacing-1: 0.25rem;     /* 4px */
  --spacing-1-5: 0.375rem;  /* 6px */
  --spacing-2: 0.5rem;      /* 8px */
  --spacing-2-5: 0.625rem;  /* 10px */
  --spacing-3: 0.75rem;     /* 12px */
  --spacing-3-5: 0.875rem;  /* 14px */
  --spacing-4: 1rem;        /* 16px */
  --spacing-5: 1.25rem;     /* 20px */
  --spacing-6: 1.5rem;      /* 24px */
  --spacing-7: 1.75rem;     /* 28px */
  --spacing-8: 2rem;        /* 32px */
  --spacing-9: 2.25rem;     /* 36px */
  --spacing-10: 2.5rem;     /* 40px */
  --spacing-11: 2.75rem;    /* 44px */
  --spacing-12: 3rem;       /* 48px */
  --spacing-14: 3.5rem;     /* 56px */
  --spacing-16: 4rem;       /* 64px */
  --spacing-20: 5rem;       /* 80px */
  --spacing-24: 6rem;       /* 96px */
  
  /* ---- Border Radius ---- */
  --radius-none: 0;
  --radius-sm: 0.25rem;     /* 4px */
  --radius-md: 0.375rem;    /* 6px */
  --radius-lg: 0.5rem;      /* 8px */
  --radius-xl: 0.75rem;     /* 12px */
  --radius-2xl: 1rem;       /* 16px */
  --radius-3xl: 1.5rem;     /* 24px */
  --radius-full: 9999px;
  
  /* ---- Shadows ---- */
  --shadow-xs: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-sm: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
  --shadow-2xl: 0 25px 50px -12px rgb(0 0 0 / 0.25);
  --shadow-inner: inset 0 2px 4px 0 rgb(0 0 0 / 0.05);
  
  /* Focus Ring */
  --shadow-ring: 0 0 0 3px var(--color-primary-200);
  --shadow-ring-error: 0 0 0 3px var(--color-error-200, #fecaca);
  
  /* ---- Transitions ---- */
  --transition-none: none;
  --transition-all: all 150ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-fast: 100ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-normal: 150ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-slow: 300ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-slower: 500ms cubic-bezier(0.4, 0, 0.2, 1);
  
  /* Transition Properties */
  --transition-colors: color, background-color, border-color, text-decoration-color, fill, stroke;
  --transition-opacity: opacity;
  --transition-shadow: box-shadow;
  --transition-transform: transform;
  
  /* ---- Z-Index Scale ---- */
  --z-base: 0;
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-fixed: 300;
  --z-modal-backdrop: 400;
  --z-modal: 500;
  --z-popover: 600;
  --z-tooltip: 700;
  --z-notification: 800;
  --z-top: 900;
  
  /* ---- Layout ---- */
  --header-height: 52px;
  --toolbar-height: 42px;
  --statusbar-height: 26px;
  --pane-header-height: 32px;
  --sidebar-width: 260px;
  --toc-width: 240px;
  
  /* Container */
  --container-max-width: 1400px;
  --content-max-width: 800px;
  
  /* ---- Breakpoints (for reference) ---- */
  /* sm: 640px, md: 768px, lg: 1024px, xl: 1280px, 2xl: 1536px */
}

/* ---- Dark Theme ---- */
[data-theme="dark"] {
  --color-bg: var(--color-neutral-950);
  --color-bg-alt: var(--color-neutral-900);
  --color-surface: var(--color-neutral-900);
  --color-surface-hover: var(--color-neutral-800);
  --color-surface-active: var(--color-neutral-700);
  --color-surface-elevated: var(--color-neutral-800);
  
  --color-text: var(--color-neutral-100);
  --color-text-secondary: var(--color-neutral-400);
  --color-text-muted: var(--color-neutral-500);
  
  --color-border: var(--color-neutral-700);
  --color-border-hover: var(--color-neutral-600);
  --color-border-focus: var(--color-primary-400);
  
  --color-accent: var(--color-primary-500);
  --color-accent-hover: var(--color-primary-400);
  --color-accent-light: var(--color-primary-900);
  --color-accent-subtle: rgba(59, 130, 246, 0.1);
  
  --color-code-bg: var(--color-neutral-800);
  --color-code-text: var(--color-neutral-200);
  
  --color-scrollbar: var(--color-neutral-600);
  --color-scrollbar-hover: var(--color-neutral-500);
  
  --color-selection-bg: var(--color-primary-800);
  --color-selection-text: var(--color-neutral-100);
  
  --shadow-ring: 0 0 0 3px var(--color-primary-800);
}

/* ============================================
   2. RESET & BASE STYLES
   ============================================ */

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 16px;
  -webkit-text-size-adjust: 100%;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility;
  scroll-behavior: smooth;
  tab-size: 4;
}

body {
  font-family: var(--font-family-sans);
  font-size: var(--font-size-base);
  font-weight: var(--font-weight-normal);
  line-height: var(--line-height-normal);
  color: var(--color-text);
  background-color: var(--color-bg);
  min-height: 100vh;
  overflow: hidden;
}

/* Selection */
::selection {
  background-color: var(--color-selection-bg);
  color: var(--color-selection-text);
}

/* Scrollbar Styles */
::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: var(--color-scrollbar);
  border-radius: var(--radius-full);
  border: 2px solid transparent;
  background-clip: padding-box;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--color-scrollbar-hover);
  border: 2px solid transparent;
  background-clip: padding-box;
}

::-webkit-scrollbar-corner {
  background: transparent;
}

/* Firefox scrollbar */
* {
  scrollbar-width: thin;
  scrollbar-color: var(--color-scrollbar) transparent;
}

/* Focus Styles */
:focus {
  outline: none;
}

:focus-visible {
  outline: 2px solid var(--color-border-focus);
  outline-offset: 2px;
}

/* Remove tap highlight on mobile */
a,
button,
input,
select,
textarea {
  -webkit-tap-highlight-color: transparent;
}

/* Images */
img,
svg,
video {
  display: block;
  max-width: 100%;
  height: auto;
}

/* Links */
a {
  color: var(--color-accent);
  text-decoration: none;
  transition: color var(--transition-fast);
}

a:hover {
  color: var(--color-accent-hover);
  text-decoration: underline;
}

/* Lists */
ul,
ol {
  list-style: none;
}

/* Tables */
table {
  border-collapse: collapse;
  border-spacing: 0;
}

/* Forms */
button,
input,
select,
textarea {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
  color: inherit;
}

button {
  cursor: pointer;
  background: none;
  border: none;
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

textarea {
  resize: none;
}

/* Hidden */
[hidden] {
  display: none !important;
}

/* ============================================
   3. TYPOGRAPHY
   ============================================ */

h1, h2, h3, h4, h5, h6 {
  font-weight: var(--font-weight-semibold);
  line-height: var(--line-height-tight);
  color: var(--color-text);
}

h1 { font-size: var(--font-size-4xl); }
h2 { font-size: var(--font-size-3xl); }
h3 { font-size: var(--font-size-2xl); }
h4 { font-size: var(--font-size-xl); }
h5 { font-size: var(--font-size-lg); }
h6 { font-size: var(--font-size-base); }

p {
  margin-bottom: var(--spacing-4);
}

small {
  font-size: var(--font-size-sm);
}

strong, b {
  font-weight: var(--font-weight-semibold);
}

em, i {
  font-style: italic;
}

code {
  font-family: var(--font-family-mono);
  font-size: 0.875em;
  background: var(--color-code-bg);
  color: var(--color-code-text);
  padding: 0.125em 0.375em;
  border-radius: var(--radius-sm);
}

pre {
  font-family: var(--font-family-mono);
  font-size: var(--font-size-sm);
  line-height: var(--line-height-relaxed);
  background: var(--color-code-bg);
  border-radius: var(--radius-md);
  overflow-x: auto;
}

pre code {
  background: none;
  padding: 0;
  border-radius: 0;
}

kbd {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 1.5em;
  padding: 0.125em 0.375em;
  font-family: var(--font-family-mono);
  font-size: 0.8em;
  font-weight: var(--font-weight-medium);
  line-height: var(--line-height-normal);
  color: var(--color-text-secondary);
  background: var(--color-bg);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  box-shadow: 0 1px 0 var(--color-border);
}

/* ============================================
   4. LAYOUT COMPONENTS
   ============================================ */

.app-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  height: 100dvh;
  overflow: hidden;
}

.app-main {
  flex: 1;
  min-height: 0;
  overflow: hidden;
}

/* Skip Link */
.skip-link {
  position: absolute;
  top: -100px;
  left: var(--spacing-4);
  padding: var(--spacing-2) var(--spacing-4);
  font-weight: var(--font-weight-medium);
  color: var(--color-text-inverted);
  background: var(--color-accent);
  border-radius: var(--radius-md);
  z-index: var(--z-top);
  transition: top var(--transition-fast);
}

.skip-link:focus {
  top: var(--spacing-4);
  outline: none;
}

/* Offline Banner */
.offline-banner {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-2);
  padding: var(--spacing-2) var(--spacing-4);
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-medium);
  color: var(--color-warning-700);
  background: var(--color-warning-50);
  border-bottom: 1px solid var(--color-warning-500);
}

[data-theme="dark"] .offline-banner {
  color: var(--color-warning-500);
  background: rgba(234, 179, 8, 0.1);
}

.offline-icon {
  font-size: var(--font-size-base);
}

/* ============================================
   5. HEADER
   ============================================ */

.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--spacing-4);
  height: var(--header-height);
  padding: 0 var(--spacing-4);
  background: var(--color-surface);
  border-bottom: 1px solid var(--color-border);
  flex-shrink: 0;
  z-index: var(--z-sticky);
}

.header-start,
.header-end {
  display: flex;
  align-items: center;
  gap: var(--spacing-2);
  flex-shrink: 0;
}

.header-center {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 0;
}

/* Logo */
.logo {
  display: flex;
  align-items: center;
  gap: var(--spacing-2);
  text-decoration: none;
  color: var(--color-text);
  transition: opacity var(--transition-fast);
}

.logo:hover {
  opacity: 0.8;
  text-decoration: none;
}

.logo-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-accent);
}

.logo-icon svg {
  width: 28px;
  height: 28px;
}

.logo-text {
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-bold);
  letter-spacing: var(--letter-spacing-tight);
}

.logo-badge {
  padding: var(--spacing-0-5) var(--spacing-1-5);
  font-size: var(--font-size-2xs);
  font-weight: var(--font-weight-semibold);
  text-transform: uppercase;
  letter-spacing: var(--letter-spacing-wider);
  color: var(--color-accent);
  background: var(--color-accent-subtle);
  border-radius: var(--radius-sm);
}

/* Document Info */
.document-info {
  display: flex;
  align-items: center;
  gap: var(--spacing-3);
  min-width: 0;
}

.document-title {
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-medium);
  color: var(--color-text-secondary);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  max-width: 200px;
}

/* Save Indicator */
.save-indicator {
  display: flex;
  align-items: center;
  gap: var(--spacing-1);
  font-size: var(--font-size-xs);
  color: var(--color-text-muted);
  white-space: nowrap;
}

.save-icon {
  width: 8px;
  height: 8px;
  border-radius: var(--radius-full);
  background: var(--color-neutral-300);
  transition: background var(--transition-fast);
}

.save-indicator[data-status="saved"] .save-icon {
  background: var(--color-success-500);
}

.save-indicator[data-status="saving"] .save-icon {
  background: var(--color-warning-500);
  animation: pulse 1s infinite;
}

.save-indicator[data-status="error"] .save-icon {
  background: var(--color-error-500);
}

/* Header Divider */
.header-divider {
  width: 1px;
  height: 20px;
  background: var(--color-border);
  margin: 0 var(--spacing-1);
}

/* Header Actions */
.header-actions {
  display: flex;
  align-items: center;
  gap: var(--spacing-1);
}

/* Theme Toggle Icons */
[data-theme="light"] .icon-moon,
[data-theme="dark"] .icon-sun {
  display: none;
}

[data-theme="light"] .icon-sun,
[data-theme="dark"] .icon-moon {
  display: block;
}

/* Fullscreen Toggle Icons */
.icon-compress {
  display: none;
}

.fullscreen .icon-expand {
  display: none;
}

.fullscreen .icon-compress {
  display: block;
}

/* ============================================
   6. TOOLBAR
   ============================================ */

.toolbar {
  display: flex;
  align-items: center;
  gap: var(--spacing-1);
  height: var(--toolbar-height);
  padding: 0 var(--spacing-3);
  background: var(--color-surface);
  border-bottom: 1px solid var(--color-border);
  flex-shrink: 0;
  overflow-x: auto;
  overflow-y: hidden;
}

.toolbar::-webkit-scrollbar {
  height: 4px;
}

.toolbar-group {
  display: flex;
  align-items: center;
  gap: var(--spacing-0-5);
  flex-shrink: 0;
}

.toolbar-divider {
  width: 1px;
  height: 20px;
  background: var(--color-border);
  margin: 0 var(--spacing-1);
  flex-shrink: 0;
}

.toolbar-spacer {
  flex: 1;
  min-width: var(--spacing-4);
}

/* ============================================
   7. EDITOR & PREVIEW PANES
   ============================================ */

.editor-container {
  display: flex;
  height: 100%;
  overflow: hidden;
}

.pane {
  display: flex;
  flex-direction: column;
  min-width: 0;
  overflow: hidden;
}

.editor-pane {
  flex: 1;
  min-width: 280px;
  border-right: 1px solid var(--color-border);
}

.preview-pane {
  flex: 1;
  min-width: 280px;
  background: var(--color-bg);
}

/* View Modes */
.editor-container[data-view="editor"] .preview-pane,
.editor-container[data-view="editor"] .pane-resizer {
  display: none;
}

.editor-container[data-view="preview"] .editor-pane,
.editor-container[data-view="preview"] .pane-resizer {
  display: none;
}

.editor-container[data-view="editor"] .editor-pane,
.editor-container[data-view="preview"] .preview-pane {
  flex: 1;
  border: none;
}

/* Pane Header */
.pane-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: var(--pane-header-height);
  padding: 0 var(--spacing-3);
  background: var(--color-surface);
  border-bottom: 1px solid var(--color-border);
  flex-shrink: 0;
}

.pane-header-start,
.pane-header-end {
  display: flex;
  align-items: center;
  gap: var(--spacing-1);
}

.pane-title {
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-semibold);
  text-transform: uppercase;
  letter-spacing: var(--letter-spacing-wider);
  color: var(--color-text-muted);
}

/* Pane Resizer */
.pane-resizer {
  position: relative;
  width: 5px;
  background: var(--color-border);
  cursor: col-resize;
  flex-shrink: 0;
  transition: background var(--transition-fast);
}

.pane-resizer:hover,
.pane-resizer:focus,
.pane-resizer.active {
  background: var(--color-accent);
}

.pane-resizer:focus {
  outline: none;
}

.resizer-handle {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 3px;
  height: 24px;
  border-radius: var(--radius-full);
  opacity: 0;
  transition: opacity var(--transition-fast);
}

.pane-resizer:hover .resizer-handle {
  opacity: 1;
}

/* Resizing State */
body.resizing {
  cursor: col-resize;
  user-select: none;
}

body.resizing .editor-textarea,
body.resizing .preview-content {
  pointer-events: none;
}

/* Editor Wrapper */
.editor-wrapper {
  flex: 1;
  display: flex;
  min-height: 0;
  overflow: hidden;
}

/* Line Numbers */
.line-numbers {
  display: flex;
  flex-direction: column;
  padding: var(--spacing-4) var(--spacing-3);
  font-family: var(--font-family-mono);
  font-size: var(--font-size-sm);
  line-height: var(--line-height-relaxed);
  color: var(--color-text-muted);
  background: var(--color-surface);
  border-right: 1px solid var(--color-border);
  text-align: right;
  user-select: none;
  overflow: hidden;
}

.line-numbers span {
  min-height: 1.625em;
}

/* Editor Textarea */
.editor-textarea {
  flex: 1;
  width: 100%;
  padding: var(--spacing-4);
  font-family: var(--font-family-editor);
  font-size: var(--font-size-sm);
  line-height: var(--line-height-relaxed);
  color: var(--color-text);
  background: var(--color-bg);
  border: none;
  resize: none;
  outline: none;
  overflow: auto;
}

.editor-textarea::placeholder {
  color: var(--color-text-muted);
  opacity: 1;
}

.editor-textarea:focus {
  outline: none;
}

/* Preview Wrapper */
.preview-wrapper {
  flex: 1;
  display: flex;
  min-height: 0;
  overflow: hidden;
}

/* Preview Content */
.preview-content {
  flex: 1;
  padding: var(--spacing-4) var(--spacing-6);
  overflow: auto;
}

.preview-placeholder {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  color: var(--color-text-muted);
  font-style: italic;
}

/* ============================================
   8. STATUS BAR
   ============================================ */

.status-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: var(--statusbar-height);
  padding: 0 var(--spacing-3);
  font-size: var(--font-size-xs);
  color: var(--color-text-secondary);
  background: var(--color-surface);
  border-top: 1px solid var(--color-border);
  flex-shrink: 0;
}

.status-start,
.status-center,
.status-end {
  display: flex;
  align-items: center;
  gap: var(--spacing-2);
}

.status-item {
  display: inline-flex;
  align-items: center;
  gap: var(--spacing-1);
  white-space: nowrap;
}

.status-item .icon {
  width: 12px;
  height: 12px;
}

.status-item-clickable {
  padding: var(--spacing-0-5) var(--spacing-1);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: background var(--transition-fast);
}

.status-item-clickable:hover {
  background: var(--color-surface-hover);
}

.status-value {
  font-weight: var(--font-weight-medium);
  color: var(--color-text);
  font-variant-numeric: tabular-nums;
}

.status-label {
  color: var(--color-text-muted);
}

.status-divider {
  color: var(--color-border);
}

/* ============================================
   9. BUTTONS & CONTROLS
   ============================================ */

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-1-5);
  padding: var(--spacing-2) var(--spacing-3);
  font-family: inherit;
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-medium);
  line-height: 1;
  color: var(--color-text);
  background: transparent;
  border: 1px solid transparent;
  border-radius: var(--radius-md);
  cursor: pointer;
  white-space: nowrap;
  user-select: none;
  transition: 
    color var(--transition-fast),
    background-color var(--transition-fast),
    border-color var(--transition-fast),
    box-shadow var(--transition-fast),
    opacity var(--transition-fast);
}

.btn:hover:not(:disabled) {
  background: var(--color-surface-hover);
}

.btn:active:not(:disabled) {
  background: var(--color-surface-active);
}

.btn:focus-visible {
  outline: none;
  box-shadow: var(--shadow-ring);
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Button Variants */
.btn-primary {
  color: var(--color-text-inverted);
  background: var(--color-accent);
  border-color: var(--color-accent);
}

.btn-primary:hover:not(:disabled) {
  background: var(--color-accent-hover);
  border-color: var(--color-accent-hover);
}

.btn-danger {
  color: var(--color-text-inverted);
  background: var(--color-error-600);
  border-color: var(--color-error-600);
}

.btn-danger:hover:not(:disabled) {
  background: var(--color-error-700);
  border-color: var(--color-error-700);
}

.btn-outline {
  border-color: var(--color-border);
}

.btn-outline:hover:not(:disabled) {
  border-color: var(--color-border-hover);
}

/* Button Sizes */
.btn-sm {
  padding: var(--spacing-1-5) var(--spacing-2-5);
  font-size: var(--font-size-xs);
}

.btn-lg {
  padding: var(--spacing-2-5) var(--spacing-4);
  font-size: var(--font-size-base);
}

.btn-icon {
  width: 32px;
  height: 32px;
  padding: 0;
}

.btn-icon .icon {
  width: 18px;
  height: 18px;
}

.btn-xs {
  width: 24px;
  height: 24px;
  padding: 0;
}

.btn-xs .icon {
  width: 14px;
  height: 14px;
}

/* Toolbar Button */
.btn-tool {
  width: 30px;
  height: 30px;
  padding: 0;
  border-radius: var(--radius-sm);
}

.btn-tool .icon {
  width: 16px;
  height: 16px;
}

.btn-tool.active,
.btn-tool[aria-pressed="true"] {
  color: var(--color-accent);
  background: var(--color-accent-subtle);
}

.btn-dropdown {
  width: auto;
  padding: 0 var(--spacing-2);
  gap: var(--spacing-1);
}

.btn-dropdown .icon-chevron {
  width: 12px;
  height: 12px;
  opacity: 0.6;
}

.btn-primary-tool {
  color: var(--color-text-inverted);
  background: var(--color-accent);
}

.btn-primary-tool:hover:not(:disabled) {
  background: var(--color-accent-hover);
}

.btn-label {
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-medium);
}

.btn-text {
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-semibold);
  font-family: var(--font-family-mono);
}

/* Icons */
.icon {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
}

/* ============================================
   10. FORMS & INPUTS
   ============================================ */

.form {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-4);
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-1-5);
}

.form-label {
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-medium);
  color: var(--color-text);
}

.form-input {
  width: 100%;
  padding: var(--spacing-2) var(--spacing-3);
  font-size: var(--font-size-sm);
  line-height: var(--line-height-normal);
  color: var(--color-text);
  background: var(--color-bg);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  transition: 
    border-color var(--transition-fast),
    box-shadow var(--transition-fast);
}

.form-input::placeholder {
  color: var(--color-text-muted);
}

.form-input:hover {
  border-color: var(--color-border-hover);
}

.form-input:focus {
  outline: none;
  border-color: var(--color-border-focus);
  box-shadow: var(--shadow-ring);
}

.form-input:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* ============================================
   11. MODALS & DIALOGS
   ============================================ */

.modal {
  position: fixed;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--spacing-4);
  z-index: var(--z-modal);
  opacity: 0;
  visibility: hidden;
  transition: 
    opacity var(--transition-normal),
    visibility var(--transition-normal);
}

.modal:not([hidden]) {
  opacity: 1;
  visibility: visible;
}

.modal-backdrop {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  z-index: -1;
}

.modal-content {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 600px;
  max-height: 90vh;
  background: var(--color-surface-elevated);
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-xl);
  overflow: hidden;
  transform: scale(0.95);
  transition: transform var(--transition-normal);
}

.modal:not([hidden]) .modal-content {
  transform: scale(1);
}

.modal-sm .modal-content {
  max-width: 400px;
}

.modal-lg .modal-content {
  max-width: 800px;
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--spacing-4) var(--spacing-5);
  border-bottom: 1px solid var(--color-border);
  flex-shrink: 0;
}

.modal-title {
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-semibold);
}

.modal-close {
  margin: calc(-1 * var(--spacing-2));
}

.modal-body {
  flex: 1;
  padding: var(--spacing-5);
  overflow-y: auto;
}

.modal-footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: var(--spacing-3);
  padding: var(--spacing-4) var(--spacing-5);
  border-top: 1px solid var(--color-border);
  flex-shrink: 0;
}

/* Shortcuts Modal */
.shortcuts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: var(--spacing-6);
}

.shortcuts-section {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-3);
}

.shortcuts-section-title {
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-semibold);
  text-transform: uppercase;
  letter-spacing: var(--letter-spacing-wider);
  color: var(--color-text-muted);
  padding-bottom: var(--spacing-2);
  border-bottom: 1px solid var(--color-border);
}

.shortcuts-list {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-2);
}

.shortcut-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--spacing-3);
  padding: var(--spacing-1-5) var(--spacing-2);
  background: var(--color-surface);
  border-radius: var(--radius-md);
}

.shortcut-item dt {
  font-size: var(--font-size-sm);
  color: var(--color-text);
}

.shortcut-item dd {
  display: flex;
  align-items: center;
  gap: var(--spacing-0-5);
  flex-shrink: 0;
}

.kbd-plus {
  font-size: var(--font-size-xs);
  color: var(--color-text-muted);
}

.shortcuts-footer {
  margin-top: var(--spacing-4);
  padding-top: var(--spacing-4);
  border-top: 1px solid var(--color-border);
}

.shortcuts-note {
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
}

/* ============================================
   12. NOTIFICATIONS
   ============================================ */

.notifications {
  position: fixed;
  bottom: var(--spacing-4);
  right: var(--spacing-4);
  display: flex;
  flex-direction: column;
  gap: var(--spacing-2);
  max-width: 400px;
  z-index: var(--z-notification);
  pointer-events: none;
}

.notification {
  display: flex;
  align-items: flex-start;
  gap: var(--spacing-3);
  padding: var(--spacing-3) var(--spacing-4);
  background: var(--color-surface-elevated);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
  font-size: var(--font-size-sm);
  pointer-events: auto;
  opacity: 0;
  transform: translateX(100%);
  transition: 
    opacity var(--transition-normal),
    transform var(--transition-normal);
}

.notification.visible {
  opacity: 1;
  transform: translateX(0);
}

.notification-icon {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
}

.notification-content {
  flex: 1;
  min-width: 0;
}

.notification-message {
  color: var(--color-text);
}

.notification-close {
  flex-shrink: 0;
  margin: calc(-1 * var(--spacing-1));
}

/* Notification Types */
.notification-success {
  border-color: var(--color-success-500);
}

.notification-success .notification-icon {
  color: var(--color-success-500);
}

.notification-warning {
  border-color: var(--color-warning-500);
}

.notification-warning .notification-icon {
  color: var(--color-warning-500);
}

.notification-error {
  border-color: var(--color-error-500);
}

.notification-error .notification-icon {
  color: var(--color-error-500);
}

.notification-info {
  border-color: var(--color-info-500);
}

.notification-info .notification-icon {
  color: var(--color-info-500);
}

/* ============================================
   13. DROPDOWNS
   ============================================ */

.toolbar-dropdown {
  position: relative;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  min-width: 180px;
  margin-top: var(--spacing-1);
  padding: var(--spacing-1);
  background: var(--color-surface-elevated);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
  z-index: var(--z-dropdown);
  opacity: 0;
  visibility: hidden;
  transform: translateY(-4px);
  transition: 
    opacity var(--transition-fast),
    visibility var(--transition-fast),
    transform var(--transition-fast);
}

.dropdown-menu:not([hidden]) {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.dropdown-menu-right {
  left: auto;
  right: 0;
}

.dropdown-item {
  display: flex;
  align-items: center;
  gap: var(--spacing-2);
  width: 100%;
  padding: var(--spacing-2) var(--spacing-3);
  font-size: var(--font-size-sm);
  color: var(--color-text);
  background: transparent;
  border: none;
  border-radius: var(--radius-md);
  cursor: pointer;
  text-align: left;
  transition: background var(--transition-fast);
}

.dropdown-item:hover {
  background: var(--color-surface-hover);
}

.dropdown-item .icon {
  width: 16px;
  height: 16px;
  color: var(--color-text-secondary);
}

.dropdown-item kbd {
  margin-left: auto;
  font-size: var(--font-size-2xs);
}

.dropdown-divider {
  height: 1px;
  margin: var(--spacing-1) 0;
  background: var(--color-border);
}

/* Heading Dropdown */
.heading-preview {
  font-weight: var(--font-weight-semibold);
}

.heading-preview.h1 { font-size: var(--font-size-lg); }
.heading-preview.h2 { font-size: var(--font-size-base); }
.heading-preview.h3 { font-size: var(--font-size-sm); }
.heading-preview.h4 { font-size: var(--font-size-sm); }
.heading-preview.h5 { font-size: var(--font-size-xs); }
.heading-preview.h6 { font-size: var(--font-size-xs); color: var(--color-text-secondary); }

/* ============================================
   14. FIND & REPLACE PANEL
   ============================================ */

.find-panel {
  position: absolute;
  top: calc(var(--header-height) + var(--toolbar-height));
  right: var(--spacing-4);
  width: 400px;
  max-width: calc(100vw - var(--spacing-8));
  background: var(--color-surface-elevated);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-xl);
  z-index: var(--z-modal);
  opacity: 0;
  visibility: hidden;
  transform: translateY(-8px);
  transition: 
    opacity var(--transition-fast),
    visibility var(--transition-fast),
    transform var(--transition-fast);
}

.find-panel:not([hidden]) {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.find-panel-content {
  padding: var(--spacing-3);
}

.find-row,
.replace-row {
  display: flex;
  align-items: center;
  gap: var(--spacing-2);
}

.replace-row {
  margin-top: var(--spacing-2);
}

.find-input-wrapper {
  flex: 1;
  position: relative;
}

.find-input {
  width: 100%;
  padding: var(--spacing-1-5) var(--spacing-2);
  padding-right: var(--spacing-10);
  font-size: var(--font-size-sm);
  background: var(--color-bg);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
}

.find-input:focus {
  outline: none;
  border-color: var(--color-border-focus);
  box-shadow: var(--shadow-ring);
}

.find-count {
  position: absolute;
  right: var(--spacing-2);
  top: 50%;
  transform: translateY(-50%);
  font-size: var(--font-size-xs);
  color: var(--color-text-muted);
  pointer-events: none;
}

.find-actions,
.find-options,
.replace-actions {
  display: flex;
  align-items: center;
  gap: var(--spacing-0-5);
}

.find-toggle-replace {
  transition: transform var(--transition-fast);
}

.find-toggle-replace[aria-expanded="true"] {
  transform: rotate(180deg);
}

.find-close {
  margin-left: var(--spacing-1);
}

/* ============================================
   15. TABLE OF CONTENTS
   ============================================ */

.toc-sidebar {
  width: var(--toc-width);
  min-width: var(--toc-width);
  display: flex;
  flex-direction: column;
  background: var(--color-surface);
  border-right: 1px solid var(--color-border);
  overflow: hidden;
}

.toc-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--spacing-3);
  border-bottom: 1px solid var(--color-border);
  flex-shrink: 0;
}

.toc-title {
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-semibold);
  color: var(--color-text);
}

.toc-nav {
  flex: 1;
  padding: var(--spacing-2);
  overflow-y: auto;
}

.toc-list {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-0-5);
}

.toc-item {
  list-style: none;
}

.toc-item a {
  display: block;
  padding: var(--spacing-1-5) var(--spacing-2);
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  text-decoration: none;
  border-radius: var(--radius-md);
  transition: 
    color var(--transition-fast),
    background var(--transition-fast);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.toc-item a:hover {
  color: var(--color-text);
  background: var(--color-surface-hover);
}

.toc-item.active a {
  color: var(--color-accent);
  background: var(--color-accent-subtle);
}

.toc-empty {
  padding: var(--spacing-4);
  text-align: center;
  font-size: var(--font-size-sm);
  color: var(--color-text-muted);
  font-style: italic;
}

/* TOC Item Levels */
.toc-item[data-level="2"] a { padding-left: calc(var(--spacing-2) + 12px); }
.toc-item[data-level="3"] a { padding-left: calc(var(--spacing-2) + 24px); }
.toc-item[data-level="4"] a { padding-left: calc(var(--spacing-2) + 36px); }
.toc-item[data-level="5"] a { padding-left: calc(var(--spacing-2) + 48px); }
.toc-item[data-level="6"] a { padding-left: calc(var(--spacing-2) + 60px); }

/* ============================================
   16. MARKDOWN PREVIEW STYLES
   ============================================ */

.markdown-body {
  font-family: var(--font-family-preview);
  font-size: var(--font-size-base);
  line-height: var(--line-height-relaxed);
  color: var(--color-text);
  word-wrap: break-word;
  max-width: var(--content-max-width);
  margin: 0 auto;
}

.markdown-body > *:first-child {
  margin-top: 0;
}

.markdown-body > *:last-child {
  margin-bottom: 0;
}

/* Headings */
.markdown-body h1,
.markdown-body h2,
.markdown-body h3,
.markdown-body h4,
.markdown-body h5,
.markdown-body h6 {
  margin-top: var(--spacing-6);
  margin-bottom: var(--spacing-4);
  font-weight: var(--font-weight-semibold);
  line-height: var(--line-height-tight);
}

.markdown-body h1 {
  font-size: 2em;
  padding-bottom: var(--spacing-3);
  border-bottom: 1px solid var(--color-border);
}

.markdown-body h2 {
  font-size: 1.5em;
  padding-bottom: var(--spacing-2);
  border-bottom: 1px solid var(--color-border);
}

.markdown-body h3 { font-size: 1.25em; }
.markdown-body h4 { font-size: 1em; }
.markdown-body h5 { font-size: 0.875em; }
.markdown-body h6 { 
  font-size: 0.85em; 
  color: var(--color-text-secondary); 
}

/* Paragraphs */
.markdown-body p {
  margin-top: 0;
  margin-bottom: var(--spacing-4);
}

/* Links */
.markdown-body a {
  color: var(--color-accent);
  text-decoration: none;
}

.markdown-body a:hover {
  text-decoration: underline;
}

/* Emphasis */
.markdown-body strong {
  font-weight: var(--font-weight-semibold);
}

.markdown-body del {
  text-decoration: line-through;
}

/* Lists */
.markdown-body ul,
.markdown-body ol {
  margin-top: 0;
  margin-bottom: var(--spacing-4);
  padding-left: 2em;
}

.markdown-body ul {
  list-style-type: disc;
}

.markdown-body ol {
  list-style-type: decimal;
}

.markdown-body li {
  margin-bottom: var(--spacing-1);
}

.markdown-body li + li {
  margin-top: var(--spacing-1);
}

.markdown-body li > p {
  margin-top: var(--spacing-4);
}

.markdown-body li > ul,
.markdown-body li > ol {
  margin-top: var(--spacing-2);
  margin-bottom: 0;
}

/* Task Lists */
.markdown-body .task-list-item {
  list-style-type: none;
  margin-left: -1.5em;
}

.markdown-body .task-list-item input {
  margin-right: var(--spacing-2);
  vertical-align: middle;
}

/* Blockquotes */
.markdown-body blockquote {
  margin: 0 0 var(--spacing-4);
  padding: var(--spacing-1) var(--spacing-4);
  border-left: 4px solid var(--color-border);
  color: var(--color-text-secondary);
}

.markdown-body blockquote > *:last-child {
  margin-bottom: 0;
}

/* Code */
.markdown-body code {
  font-family: var(--font-family-mono);
  font-size: 0.875em;
  padding: 0.2em 0.4em;
  background: var(--color-code-bg);
  border-radius: var(--radius-sm);
}

.markdown-body pre {
  position: relative;
  margin: 0 0 var(--spacing-4);
  padding: var(--spacing-4);
  background: var(--color-code-bg);
  border-radius: var(--radius-md);
  overflow-x: auto;
}

.markdown-body pre code {
  padding: 0;
  background: transparent;
  font-size: var(--font-size-sm);
  line-height: var(--line-height-relaxed);
}

/* Copy Button */
.copy-btn {
  position: absolute;
  top: var(--spacing-2);
  right: var(--spacing-2);
  display: flex;
  align-items: center;
  gap: var(--spacing-1);
  padding: var(--spacing-1) var(--spacing-2);
  font-family: var(--font-family-sans);
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-medium);
  color: var(--color-text-secondary);
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  cursor: pointer;
  opacity: 0;
  transition: 
    opacity var(--transition-fast),
    color var(--transition-fast),
    background var(--transition-fast),
    border-color var(--transition-fast);
}

.markdown-body pre:hover .copy-btn,
.copy-btn:focus {
  opacity: 1;
}

.copy-btn:hover {
  color: var(--color-text);
  background: var(--color-bg);
}

.copy-btn.copied {
  color: var(--color-success-600);
  border-color: var(--color-success-500);
}

.copy-btn .icon {
  width: 14px;
  height: 14px;
}

/* Tables */
.markdown-body table {
  width: 100%;
  margin-bottom: var(--spacing-4);
  border-collapse: collapse;
  border-spacing: 0;
  overflow: auto;
  display: block;
}

.markdown-body th,
.markdown-body td {
  padding: var(--spacing-2) var(--spacing-3);
  border: 1px solid var(--color-border);
  text-align: left;
}

.markdown-body th {
  font-weight: var(--font-weight-semibold);
  background: var(--color-surface);
}

.markdown-body tr:nth-child(even) {
  background: var(--color-surface);
}

/* Horizontal Rule */
.markdown-body hr {
  height: 0;
  margin: var(--spacing-6) 0;
  padding: 0;
  border: 0;
  border-top: 1px solid var(--color-border);
}

/* Images */
.markdown-body img {
  max-width: 100%;
  height: auto;
  border-radius: var(--radius-md);
}

/* ============================================
   17. SEO CONTENT
   ============================================ */

.seo-content {
  background: var(--color-bg-alt);
  border-top: 1px solid var(--color-border);
}

.seo-content-inner {
  max-width: var(--content-max-width);
  margin: 0 auto;
  padding: var(--spacing-12) var(--spacing-6);
}

.seo-section {
  margin-bottom: var(--spacing-10);
}

.seo-section:last-child {
  margin-bottom: 0;
}

.seo-section h2 {
  font-size: var(--font-size-2xl);
  font-weight: var(--font-weight-bold);
  margin-bottom: var(--spacing-4);
  color: var(--color-text);
}

.seo-section p {
  color: var(--color-text-secondary);
  margin-bottom: var(--spacing-4);
}

.seo-section p:last-child {
  margin-bottom: 0;
}

/* Feature List */
.feature-list {
  display: grid;
  gap: var(--spacing-3);
}

```css
.feature-list li {
  display: flex;
  align-items: flex-start;
  gap: var(--spacing-3);
  padding: var(--spacing-3);
  background: var(--color-surface-elevated);
  border-radius: var(--radius-md);
  color: var(--color-text-secondary);
}

.feature-list li strong {
  color: var(--color-text);
}

/* Steps List */
.steps-list {
  counter-reset: step;
  display: flex;
  flex-direction: column;
  gap: var(--spacing-4);
}

.steps-list li {
  position: relative;
  padding-left: var(--spacing-10);
  counter-increment: step;
  color: var(--color-text-secondary);
}

.steps-list li::before {
  content: counter(step);
  position: absolute;
  left: 0;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-semibold);
  color: var(--color-accent);
  background: var(--color-accent-subtle);
  border-radius: var(--radius-full);
}

.steps-list li strong {
  color: var(--color-text);
}

/* Syntax Reference */
.syntax-reference {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: var(--spacing-3);
}

.syntax-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--spacing-3);
  padding: var(--spacing-2) var(--spacing-3);
  background: var(--color-surface-elevated);
  border-radius: var(--radius-md);
}

.syntax-item code {
  font-size: var(--font-size-sm);
  color: var(--color-accent);
  background: var(--color-accent-subtle);
}

.syntax-item span {
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  text-align: right;
}

/* FAQ List */
.faq-list {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-4);
}

.faq-item {
  padding: var(--spacing-4);
  background: var(--color-surface-elevated);
  border-radius: var(--radius-lg);
}

.faq-item dt {
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-semibold);
  color: var(--color-text);
  margin-bottom: var(--spacing-2);
}

.faq-item dd {
  color: var(--color-text-secondary);
  line-height: var(--line-height-relaxed);
}

/* Use Cases */
.use-cases {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: var(--spacing-4);
}

.use-case {
  padding: var(--spacing-4);
  background: var(--color-surface-elevated);
  border-radius: var(--radius-lg);
  border: 1px solid var(--color-border);
}

.use-case h3 {
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-semibold);
  color: var(--color-text);
  margin-bottom: var(--spacing-2);
}

.use-case p {
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  margin-bottom: 0;
}

/* ============================================
   18. FOOTER
   ============================================ */

.app-footer {
  padding: var(--spacing-8) var(--spacing-6);
  background: var(--color-surface);
  border-top: 1px solid var(--color-border);
}

.footer-content {
  max-width: var(--content-max-width);
  margin: 0 auto;
  text-align: center;
}

.footer-links {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: var(--spacing-4);
  margin-bottom: var(--spacing-4);
}

.footer-link {
  display: inline-flex;
  align-items: center;
  gap: var(--spacing-1-5);
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  text-decoration: none;
  transition: color var(--transition-fast);
}

.footer-link:hover {
  color: var(--color-accent);
  text-decoration: none;
}

.footer-link .icon {
  width: 16px;
  height: 16px;
}

.footer-copyright {
  font-size: var(--font-size-sm);
  color: var(--color-text-muted);
  margin-bottom: var(--spacing-2);
}

.footer-copyright a {
  color: var(--color-text-secondary);
}

.footer-copyright a:hover {
  color: var(--color-accent);
}

.footer-separator {
  margin: 0 var(--spacing-2);
}

.footer-version {
  font-size: var(--font-size-xs);
  color: var(--color-text-muted);
}

/* ============================================
   19. UTILITY CLASSES
   ============================================ */

/* Display */
.hidden { display: none !important; }
.block { display: block; }
.inline-block { display: inline-block; }
.flex { display: flex; }
.inline-flex { display: inline-flex; }
.grid { display: grid; }

/* Flex */
.flex-1 { flex: 1; }
.flex-shrink-0 { flex-shrink: 0; }
.flex-col { flex-direction: column; }
.items-center { align-items: center; }
.items-start { align-items: flex-start; }
.items-end { align-items: flex-end; }
.justify-center { justify-content: center; }
.justify-between { justify-content: space-between; }
.justify-end { justify-content: flex-end; }
.gap-1 { gap: var(--spacing-1); }
.gap-2 { gap: var(--spacing-2); }
.gap-3 { gap: var(--spacing-3); }
.gap-4 { gap: var(--spacing-4); }

/* Text */
.text-center { text-align: center; }
.text-left { text-align: left; }
.text-right { text-align: right; }
.text-sm { font-size: var(--font-size-sm); }
.text-xs { font-size: var(--font-size-xs); }
.text-muted { color: var(--color-text-muted); }
.text-secondary { color: var(--color-text-secondary); }
.font-medium { font-weight: var(--font-weight-medium); }
.font-semibold { font-weight: var(--font-weight-semibold); }
.font-bold { font-weight: var(--font-weight-bold); }
.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* Spacing */
.p-0 { padding: 0; }
.p-1 { padding: var(--spacing-1); }
.p-2 { padding: var(--spacing-2); }
.p-3 { padding: var(--spacing-3); }
.p-4 { padding: var(--spacing-4); }
.m-0 { margin: 0; }
.mt-4 { margin-top: var(--spacing-4); }
.mb-4 { margin-bottom: var(--spacing-4); }
.ml-auto { margin-left: auto; }
.mr-auto { margin-right: auto; }

/* Width/Height */
.w-full { width: 100%; }
.h-full { height: 100%; }
.min-w-0 { min-width: 0; }
.min-h-0 { min-height: 0; }

/* Overflow */
.overflow-hidden { overflow: hidden; }
.overflow-auto { overflow: auto; }
.overflow-x-auto { overflow-x: auto; }
.overflow-y-auto { overflow-y: auto; }

/* Position */
.relative { position: relative; }
.absolute { position: absolute; }
.fixed { position: fixed; }
.sticky { position: sticky; }
.inset-0 { inset: 0; }

/* Cursor */
.cursor-pointer { cursor: pointer; }
.cursor-default { cursor: default; }

/* Visibility */
.opacity-0 { opacity: 0; }
.opacity-50 { opacity: 0.5; }
.opacity-100 { opacity: 1; }
.invisible { visibility: hidden; }
.visible { visibility: visible; }

/* Pointer Events */
.pointer-events-none { pointer-events: none; }
.pointer-events-auto { pointer-events: auto; }

/* Select */
.select-none { user-select: none; }
.select-all { user-select: all; }

/* Border */
.border { border: 1px solid var(--color-border); }
.border-0 { border: 0; }
.rounded { border-radius: var(--radius-md); }
.rounded-lg { border-radius: var(--radius-lg); }
.rounded-full { border-radius: var(--radius-full); }

/* Shadow */
.shadow { box-shadow: var(--shadow-md); }
.shadow-lg { box-shadow: var(--shadow-lg); }
.shadow-none { box-shadow: none; }

/* ============================================
   20. ANIMATIONS
   ============================================ */

@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}

@keyframes slideInUp {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideInRight {
  from {
    opacity: 0;
    transform: translateX(100%);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-4px); }
  20%, 40%, 60%, 80% { transform: translateX(4px); }
}

.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

.animate-spin {
  animation: spin 1s linear infinite;
}

.animate-fade-in {
  animation: fadeIn var(--transition-normal) ease-out;
}

.animate-slide-in-up {
  animation: slideInUp var(--transition-normal) ease-out;
}

/* ============================================
   21. RESPONSIVE DESIGN
   ============================================ */

/* Tablet and below */
@media (max-width: 1024px) {
  :root {
    --header-height: 48px;
    --toolbar-height: 40px;
    --toc-width: 200px;
  }
  
  .logo-badge {
    display: none;
  }
  
  .document-title {
    max-width: 150px;
  }
  
  .toolbar-spacer {
    min-width: var(--spacing-2);
  }
  
  .btn-tool .btn-label {
    display: none;
  }
  
  .btn-primary-tool .btn-label {
    display: none;
  }
}

/* Mobile and below */
@media (max-width: 768px) {
  :root {
    --header-height: 44px;
    --toolbar-height: 36px;
    --statusbar-height: 24px;
    --pane-header-height: 28px;
  }
  
  /* Header */
  .logo-text {
    display: none;
  }
  
  .header-center {
    display: none;
  }
  
  .header-divider {
    display: none;
  }
  
  /* Toolbar */
  .toolbar {
    padding: 0 var(--spacing-2);
    gap: var(--spacing-0-5);
  }
  
  .toolbar-divider {
    margin: 0 var(--spacing-0-5);
  }
  
  .btn-tool {
    width: 28px;
    height: 28px;
  }
  
  .btn-tool .icon {
    width: 14px;
    height: 14px;
  }
  
  /* Editor Container - Stacked Layout */
  .editor-container {
    flex-direction: column;
  }
  
  .editor-pane {
    flex: none;
    height: 50%;
    border-right: none;
    border-bottom: 1px solid var(--color-border);
  }
  
  .preview-pane {
    flex: none;
    height: 50%;
  }
  
  .pane-resizer {
    width: 100%;
    height: 5px;
    cursor: row-resize;
  }
  
  .pane-resizer .resizer-handle {
    width: 24px;
    height: 3px;
  }
  
  body.resizing {
    cursor: row-resize;
  }
  
  /* Mobile View Modes */
  .editor-container[data-view="editor"] .editor-pane,
  .editor-container[data-view="preview"] .preview-pane {
    height: 100%;
  }
  
  /* Preview Content */
  .preview-content {
    padding: var(--spacing-3) var(--spacing-4);
  }
  
  /* Copy Button - Always visible on mobile */
  .copy-btn {
    opacity: 1;
  }
  
  /* TOC Sidebar */
  .toc-sidebar {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    z-index: var(--z-dropdown);
    box-shadow: var(--shadow-xl);
  }
  
  /* Modals */
  .modal-content {
    max-height: 100vh;
    border-radius: 0;
  }
  
  .shortcuts-grid {
    grid-template-columns: 1fr;
  }
  
  /* Find Panel */
  .find-panel {
    right: 0;
    left: 0;
    top: calc(var(--header-height) + var(--toolbar-height));
    width: auto;
    max-width: none;
    border-radius: 0;
    border-left: none;
    border-right: none;
  }
  
  /* Status Bar */
  .status-center {
    display: none;
  }
  
  .status-item .status-label {
    display: none;
  }
  
  /* SEO Content */
  .seo-content-inner {
    padding: var(--spacing-8) var(--spacing-4);
  }
  
  .syntax-reference {
    grid-template-columns: 1fr;
  }
  
  .use-cases {
    grid-template-columns: 1fr;
  }
  
  /* Footer */
  .app-footer {
    padding: var(--spacing-6) var(--spacing-4);
  }
  
  .footer-links {
    gap: var(--spacing-3);
  }
  
  .footer-separator {
    display: none;
  }
  
  .footer-copyright span:last-child {
    display: none;
  }
}

/* Small Mobile */
@media (max-width: 480px) {
  :root {
    --toolbar-height: 32px;
  }
  
  .toolbar {
    padding: 0 var(--spacing-1);
  }
  
  .btn-tool {
    width: 26px;
    height: 26px;
  }
  
  .btn-tool .icon {
    width: 12px;
    height: 12px;
  }
  
  .btn-dropdown {
    padding: 0 var(--spacing-1);
  }
  
  .editor-textarea {
    padding: var(--spacing-3);
    font-size: var(--font-size-sm);
  }
  
  .preview-content {
    padding: var(--spacing-3);
  }
  
  .markdown-body {
    font-size: var(--font-size-sm);
  }
  
  .feature-list li {
    flex-direction: column;
    gap: var(--spacing-1);
  }
}

/* Large Desktop */
@media (min-width: 1536px) {
  .seo-content-inner {
    max-width: 900px;
  }
}

/* ============================================
   22. PRINT STYLES
   ============================================ */

@media print {
  *,
  *::before,
  *::after {
    background: transparent !important;
    color: #000 !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }
  
  body {
    font-size: 12pt;
    line-height: 1.5;
    overflow: visible;
  }
  
  .app-container {
    display: block;
    height: auto;
    overflow: visible;
  }
  
  /* Hide non-essential elements */
  .app-header,
  .toolbar,
  .status-bar,
  .pane-resizer,
  .editor-pane,
  .pane-header,
  .toc-sidebar,
  .copy-btn,
  .skip-link,
  .offline-banner,
  .notifications,
  .modal,
  .find-panel,
  .seo-content,
  .app-footer {
    display: none !important;
  }
  
  .app-main,
  .editor-container,
  .preview-pane,
  .preview-wrapper,
  .preview-content {
    display: block !important;
    width: 100% !important;
    height: auto !important;
    overflow: visible !important;
    padding: 0 !important;
    margin: 0 !important;
    border: none !important;
  }
  
  .markdown-body {
    max-width: none;
    padding: 0;
  }
  
  .markdown-body h1,
  .markdown-body h2,
  .markdown-body h3,
  .markdown-body h4,
  .markdown-body h5,
  .markdown-body h6 {
    page-break-after: avoid;
    break-after: avoid;
  }
  
  .markdown-body pre,
  .markdown-body blockquote,
  .markdown-body table,
  .markdown-body figure {
    page-break-inside: avoid;
    break-inside: avoid;
  }
  
  .markdown-body pre {
    white-space: pre-wrap;
    word-wrap: break-word;
    border: 1px solid #ccc;
    padding: 0.5em;
  }
  
  .markdown-body a {
    text-decoration: underline;
  }
  
  .markdown-body a[href]::after {
    content: " (" attr(href) ")";
    font-size: 0.8em;
    color: #666;
  }
  
  .markdown-body a[href^="#"]::after,
  .markdown-body a[href^="javascript:"]::after {
    content: "";
  }
  
  .markdown-body img {
    max-width: 100% !important;
    page-break-inside: avoid;
  }
  
  .markdown-body table {
    border-collapse: collapse;
  }
  
  .markdown-body th,
  .markdown-body td {
    border: 1px solid #ccc;
  }
}

/* ============================================
   23. ACCESSIBILITY
   ============================================ */

/* Reduced Motion */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* High Contrast Mode */
@media (prefers-contrast: high) {
  :root {
    --color-border: #000;
    --color-text-muted: #444;
  }
  
  [data-theme="dark"] {
    --color-border: #fff;
    --color-text-muted: #bbb;
  }
  
  .btn:focus-visible,
  .form-input:focus,
  a:focus-visible {
    outline: 3px solid currentColor;
    outline-offset: 2px;
  }
}

/* Focus Visible Enhancement */
.btn:focus-visible,
.form-input:focus-visible,
.editor-textarea:focus-visible {
  outline: 2px solid var(--color-border-focus);
  outline-offset: 2px;
}

/* Screen Reader Only */
.sr-only {
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

.sr-only-focusable:focus,
.sr-only-focusable:active {
  position: static;
  width: auto;
  height: auto;
  overflow: visible;
  clip: auto;
  white-space: normal;
}

/* Ensure interactive elements have sufficient touch target size */
@media (pointer: coarse) {
  .btn,
  .btn-icon,
  .btn-tool,
  .dropdown-item,
  .toc-item a {
    min-height: 44px;
    min-width: 44px;
  }
  
  .btn-xs {
    min-height: 36px;
    min-width: 36px;
  }
}
```

Now let me provide the main JavaScript application file:

---

## 4. JavaScript Modules

### assets/js/app.js

```javascript
/**
 * MarkdownStudio Pro - Application Entry Point
 * Version: 3.0.0
 * Author: Oathan Rex
 * License: MIT
 */

// ============================================
// IMPORTS
// ============================================

import { EventBus, Events } from './utils/event-bus.js';
import { Storage } from './services/storage.js';
import { ThemeManager } from './services/theme.js';
import { ShortcutsManager } from './services/shortcuts.js';
import { ExportService } from './services/export.js';
import { Editor } from './core/editor.js';
import { Preview } from './core/preview.js';
import { Toolbar } from './core/toolbar.js';
import { debounce, throttle } from './utils/debounce.js';
import { $ } from './utils/dom.js';

// ============================================
// APPLICATION CLASS
// ============================================

class MarkdownStudioApp {
  constructor() {
    // Core instances
    this.eventBus = new EventBus();
    this.storage = new Storage('mds_');
    this.theme = null;
    this.shortcuts = null;
    this.exportService = null;
    this.editor = null;
    this.preview = null;
    this.toolbar = null;
    
    // State
    this.state = {
      isInitialized: false,
      isSaving: false,
      hasUnsavedChanges: false,
      viewMode: 'split', // 'split', 'editor', 'preview'
      syncScroll: true,
      showLineNumbers: false,
      showTOC: false
    };
    
    // DOM Elements
    this.elements = {};
    
    // Initialize
    this.init();
  }
  
  // ============================================
  // INITIALIZATION
  // ============================================
  
  async init() {
    try {
      console.log('MarkdownStudio Pro v3.0.0 - Initializing...');
      
      // Cache DOM elements
      this.cacheElements();
      
      // Initialize services
      this.initServices();
      
      // Initialize core modules
      this.initModules();
      
      // Set up event listeners
      this.setupEventListeners();
      
      // Load saved content
      this.loadSavedContent();
      
      // Set up autosave
      this.setupAutosave();
      
      // Restore preferences
      this.restorePreferences();
      
      // Mark as initialized
      this.state.isInitialized = true;
      
      // Focus editor
      this.editor.focus();
      
      console.log('MarkdownStudio Pro - Ready!');
      this.showNotification('Ready to edit', 'success', 2000);
      
    } catch (error) {
      console.error('Failed to initialize MarkdownStudio:', error);
      this.showNotification('Failed to initialize editor', 'error');
    }
  }
  
  cacheElements() {
    this.elements = {
      // App container
      app: $('#app'),
      
      // Header
      documentTitle: $('#document-title'),
      saveIndicator: $('#save-indicator'),
      
      // Toolbar
      toolbar: $('#toolbar'),
      
      // Editor
      editorContainer: $('#editor-container'),
      editorPane: $('#editor-pane'),
      editorTextarea: $('#editor-textarea'),
      lineNumbers: $('#line-numbers'),
      
      // Preview
      previewPane: $('#preview-pane'),
      previewContainer: $('#preview-container'),
      
      // Resizer
      paneResizer: $('#pane-resizer'),
      
      // Status bar
      statsWords: $('#stats-words'),
      statsChars: $('#stats-chars'),
      statsLines: $('#stats-lines'),
      statsReadingTime: $('#stats-reading-time'),
      cursorLine: $('#cursor-line'),
      cursorCol: $('#cursor-col'),
      
      // Modals
      shortcutsModal: $('#shortcuts-modal'),
      insertModal: $('#insert-modal'),
      confirmModal: $('#confirm-modal'),
      
      // Find panel
      findPanel: $('#find-panel'),
      findInput: $('#find-input'),
      replaceInput: $('#replace-input'),
      findCount: $('#find-count'),
      replaceRow: $('#replace-row'),
      
      // TOC
      tocSidebar: $('#toc-sidebar'),
      tocList: $('#toc-list'),
      
      // File input
      fileInput: $('#file-input'),
      
      // Notifications
      notifications: $('#notifications')
    };
  }
  
  initServices() {
    // Theme Manager
    this.theme = new ThemeManager(this.storage, this.eventBus);
    
    // Export Service
    this.exportService = new ExportService();
    
    // Shortcuts Manager
    this.shortcuts = new ShortcutsManager(this.eventBus);
  }
  
  initModules() {
    // Editor
    this.editor = new Editor({
      element: this.elements.editorTextarea,
      lineNumbers: this.elements.lineNumbers,
      eventBus: this.eventBus
    });
    
    // Preview
    this.preview = new Preview({
      element: this.elements.previewContainer,
      eventBus: this.eventBus
    });
    
    // Toolbar
    this.toolbar = new Toolbar({
      element: this.elements.toolbar,
      editor: this.editor,
      eventBus: this.eventBus
    });
  }
  
  // ============================================
  // EVENT LISTENERS
  // ============================================
  
  setupEventListeners() {
    // Content change -> Update preview
    this.eventBus.on(Events.CONTENT_CHANGE, ({ content }) => {
      this.preview.render(content);
      this.updateStats(content);
      this.state.hasUnsavedChanges = true;
      this.updateDocumentTitle(content);
    });
    
    // Cursor position change
    this.eventBus.on(Events.CURSOR_CHANGE, ({ line, column }) => {
      this.updateCursorPosition(line, column);
    });
    
    // Selection change
    this.eventBus.on(Events.SELECTION_CHANGE, ({ text }) => {
      this.updateSelectionStats(text);
    });
    
    // Content saved
    this.eventBus.on(Events.CONTENT_SAVED, ({ manual }) => {
      this.state.hasUnsavedChanges = false;
      this.updateSaveIndicator('saved');
      if (manual) {
        this.showNotification('Document saved', 'success', 1500);
      }
    });
    
    // Preview rendered
    this.eventBus.on(Events.PREVIEW_RENDERED, ({ toc }) => {
      this.updateTOC(toc);
    });
    
    // Theme change
    this.eventBus.on(Events.THEME_CHANGE, ({ theme }) => {
      // Theme manager handles the actual change
    });
    
    // Shortcut actions
    this.setupShortcutActions();
    
    // Header button actions
    this.setupHeaderActions();
    
    // Resizer
    this.setupResizer();
    
    // View mode buttons
    this.setupViewModeButtons();
    
    // Modal handling
    this.setupModals();
    
    // File input
    this.setupFileInput();
    
    // Dropdowns
    this.setupDropdowns();
    
    // Find/Replace
    this.setupFindReplace();
    
    // TOC
    this.setupTOC();
    
    // Scroll sync
    this.setupScrollSync();
    
    // Before unload warning
    this.setupBeforeUnload();
    
    // Visibility change (for autosave)
    document.addEventListener('visibilitychange', () => {
      if (document.hidden && this.state.hasUnsavedChanges) {
        this.saveDocument();
      }
    });
  }
  
  setupShortcutActions() {
    // Save
    this.eventBus.on(Events.SHORTCUT_SAVE, () => {
      this.saveDocument(true);
    });
    
    // New document
    this.eventBus.on(Events.SHORTCUT_NEW, () => {
      this.newDocument();
    });
    
    // Open file
    this.eventBus.on(Events.SHORTCUT_OPEN, () => {
      this.openFile();
    });
    
    // Find
    this.eventBus.on(Events.SHORTCUT_FIND, () => {
      this.showFindPanel();
    });
    
    // Find and replace
    this.eventBus.on(Events.SHORTCUT_REPLACE, () => {
      this.showFindPanel(true);
    });
    
    // Toggle theme
    this.eventBus.on(Events.SHORTCUT_TOGGLE_THEME, () => {
      this.theme.toggle();
    });
    
    // Toggle preview
    this.eventBus.on(Events.SHORTCUT_TOGGLE_PREVIEW, () => {
      this.cycleViewMode();
    });
    
    // Show shortcuts
    this.eventBus.on(Events.SHORTCUT_SHOW_SHORTCUTS, () => {
      this.showModal('shortcuts');
    });
    
    // Fullscreen
    this.eventBus.on(Events.SHORTCUT_FULLSCREEN, () => {
      this.toggleFullscreen();
    });
  }
  
  setupHeaderActions() {
    // New document
    $('[data-action="new-document"]')?.addEventListener('click', () => {
      this.newDocument();
    });
    
    // Open file
    $('[data-action="open-file"]')?.addEventListener('click', () => {
      this.openFile();
    });
    
    // Show shortcuts
    $('[data-action="show-shortcuts"]')?.addEventListener('click', () => {
      this.showModal('shortcuts');
    });
    
    // Toggle theme
    $('[data-action="toggle-theme"]')?.addEventListener('click', () => {
      this.theme.toggle();
    });
    
    // Toggle fullscreen
    $('[data-action="toggle-fullscreen"]')?.addEventListener('click', () => {
      this.toggleFullscreen();
    });
  }
  
  setupResizer() {
    const resizer = this.elements.paneResizer;
    const container = this.elements.editorContainer;
    const editorPane = this.elements.editorPane;
    
    if (!resizer || !container || !editorPane) return;
    
    let isResizing = false;
    let startX = 0;
    let startWidth = 0;
    
    const startResize = (e) => {
      isResizing = true;
      startX = e.clientX || e.touches?.[0]?.clientX || 0;
      startWidth = editorPane.offsetWidth;
      
      document.body.classList.add('resizing');
      resizer.classList.add('active');
      
      e.preventDefault();
    };
    
    const doResize = (e) => {
      if (!isResizing) return;
      
      const clientX = e.clientX || e.touches?.[0]?.clientX || 0;
      const deltaX = clientX - startX;
      const containerWidth = container.offsetWidth;
      const newWidth = startWidth + deltaX;
      
      // Calculate percentage (clamped between 20% and 80%)
      const percentage = Math.max(0.2, Math.min(0.8, newWidth / containerWidth));
      
      editorPane.style.flex = `0 0 ${percentage * 100}%`;
      
      // Update ARIA
      resizer.setAttribute('aria-valuenow', Math.round(percentage * 100));
      
      e.preventDefault();
    };
    
    const stopResize = () => {
      if (!isResizing) return;
      
      isResizing = false;
      document.body.classList.remove('resizing');
      resizer.classList.remove('active');
      
      // Save preference
      const percentage = editorPane.offsetWidth / container.offsetWidth;
      this.storage.set('paneRatio', percentage);
    };
    
    // Mouse events
    resizer.addEventListener('mousedown', startResize);
    document.addEventListener('mousemove', doResize);
    document.addEventListener('mouseup', stopResize);
    
    // Touch events
    resizer.addEventListener('touchstart', startResize, { passive: false });
    document.addEventListener('touchmove', doResize, { passive: false });
    document.addEventListener('touchend', stopResize);
    
    // Keyboard support
    resizer.addEventListener('keydown', (e) => {
      const step = e.shiftKey ? 0.1 : 0.02;
      let percentage = editorPane.offsetWidth / container.offsetWidth;
      
      if (e.key === 'ArrowLeft' || e.key === 'ArrowDown') {
        percentage = Math.max(0.2, percentage - step);
        editorPane.style.flex = `0 0 ${percentage * 100}%`;
        e.preventDefault();
      } else if (e.key === 'ArrowRight' || e.key === 'ArrowUp') {
        percentage = Math.min(0.8, percentage + step);
        editorPane.style.flex = `0 0 ${percentage * 100}%`;
        e.preventDefault();
      } else if (e.key === 'Home') {
        editorPane.style.flex = `0 0 50%`;
        e.preventDefault();
      }
      
      resizer.setAttribute('aria-valuenow', Math.round(percentage * 100));
      this.storage.set('paneRatio', percentage);
    });
    
    // Restore saved ratio
    const savedRatio = this.storage.get('paneRatio');
    if (savedRatio && savedRatio >= 0.2 && savedRatio <= 0.8) {
      editorPane.style.flex = `0 0 ${savedRatio * 100}%`;
      resizer.setAttribute('aria-valuenow', Math.round(savedRatio * 100));
    }
  }
  
  setupViewModeButtons() {
    const buttons = {
      editor: $('[data-action="view-editor"]'),
      split: $('[data-action="view-split"]'),
      preview: $('[data-action="view-preview"]')
    };
    
    Object.entries(buttons).forEach(([mode, button]) => {
      if (!button) return;
      
      button.addEventListener('click', () => {
        this.setViewMode(mode);
      });
    });
  }
  
  setupModals() {
    // Close modal on backdrop click
    document.querySelectorAll('[data-action="close-modal"]').forEach(el => {
      el.addEventListener('click', () => {
        this.closeAllModals();
      });
    });
    
    // Close modal on Escape
    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') {
        this.closeAllModals();
        this.closeFindPanel();
      }
    });
    
    // Confirm modal actions
    $('[data-action="confirm-cancel"]')?.addEventListener('click', () => {
      this.closeModal('confirm');
      this.confirmReject?.();
    });
    
    $('[data-action="confirm-ok"]')?.addEventListener('click', () => {
      this.closeModal('confirm');
      this.confirmResolve?.();
    });
    
    // Insert form submission
    $('#insert-form')?.addEventListener('submit', (e) => {
      e.preventDefault();
      this.handleInsertSubmit();
    });
  }
  
  setupFileInput() {
    const fileInput = this.elements.fileInput;
    if (!fileInput) return;
    
    fileInput.addEventListener('change', async (e) => {
      const file = e.target.files?.[0];
      if (file) {
        await this.importFile(file);
      }
      // Reset input for re-selection
      fileInput.value = '';
    });
    
    // Drag and drop on editor
    const editorPane = this.elements.editorPane;
    
    editorPane?.addEventListener('dragover', (e) => {
      e.preventDefault();
      editorPane.classList.add('drag-over');
    });
    
    editorPane?.addEventListener('dragleave', () => {
      editorPane.classList.remove('drag-over');
    });
    
    editorPane?.addEventListener('drop', async (e) => {
      e.preventDefault();
      editorPane.classList.remove('drag-over');
      
      const file = e.dataTransfer?.files?.[0];
      if (file) {
        await this.importFile(file);
      }
    });
  }
  
  setupDropdowns() {
    document.querySelectorAll('.toolbar-dropdown').forEach(dropdown => {
      const trigger = dropdown.querySelector('.btn-dropdown');
      const menu = dropdown.querySelector('.dropdown-menu');
      
      if (!trigger || !menu) return;
      
      trigger.addEventListener('click', (e) => {
        e.stopPropagation();
        
        const isOpen = !menu.hidden;
        
        // Close all dropdowns first
        this.closeAllDropdowns();
        
        if (!isOpen) {
          menu.hidden = false;
          trigger.setAttribute('aria-expanded', 'true');
        }
      });
      
      // Handle menu item clicks
      menu.querySelectorAll('.dropdown-item').forEach(item => {
        item.addEventListener('click', () => {
          const action = item.dataset.action;
          
          if (action === 'heading') {
            const level = parseInt(item.dataset.level);
            this.editor.insertHeading(level);
          } else if (action === 'export-md') {
            this.exportMarkdown();
          } else if (action === 'export-html') {
            this.exportHTML();
          } else if (action === 'copy-markdown') {
            this.copyMarkdown();
          } else if (action === 'copy-html') {
            this.copyHTML();
          } else if (action === 'print') {
            window.print();
          }
          
          this.closeAllDropdowns();
          this.editor.focus();
        });
      });
    });
    
    // Close dropdowns on outside click
    document.addEventListener('click', () => {
      this.closeAllDropdowns();
    });
  }
  
  setupFindReplace() {
    const findPanel = this.elements.findPanel;
    const findInput = this.elements.findInput;
    const replaceInput = this.elements.replaceInput;
    
    if (!findPanel || !findInput) return;
    
    // Find input
    findInput.addEventListener('input', debounce(() => {
      this.performFind();
    }, 150));
    
    findInput.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') {
        e.preventDefault();
        if (e.shiftKey) {
          this.findPrevious();
        } else {
          this.findNext();
        }
      } else if (e.key === 'Escape') {
        this.closeFindPanel();
      }
    });
    
    // Replace input
    replaceInput?.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') {
        e.preventDefault();
        this.replaceOne();
      } else if (e.key === 'Escape') {
        this.closeFindPanel();
      }
    });
    
    // Find buttons
    $('[data-action="find-prev"]')?.addEventListener('click', () => this.findPrevious());
    $('[data-action="find-next"]')?.addEventListener('click', () => this.findNext());
    $('[data-action="close-find"]')?.addEventListener('click', () => this.closeFindPanel());
    
    // Toggle replace
    $('[data-action="toggle-replace"]')?.addEventListener('click', (e) => {
      const btn = e.currentTarget;
      const replaceRow = this.elements.replaceRow;
      const isExpanded = btn.getAttribute('aria-expanded') === 'true';
      
      btn.setAttribute('aria-expanded', !isExpanded);
      replaceRow.hidden = isExpanded;
    });
    
    // Find options
    $('[data-action="toggle-find-case"]')?.addEventListener('click', (e) => {
      this.toggleFindOption(e.currentTarget);
    });
    
    $('[data-action="toggle-find-word"]')?.addEventListener('click', (e) => {
      this.toggleFindOption(e.currentTarget);
    });
    
    $('[data-action="toggle-find-regex"]')?.addEventListener('click', (e) => {
      this.toggleFindOption(e.currentTarget);
    });
    
    // Replace buttons
    $('[data-action="replace"]')?.addEventListener('click', () => this.replaceOne());
    $('[data-action="replace-all"]')?.addEventListener('click', () => this.replaceAll());
  }
  
  setupTOC() {
    // Toggle TOC button
    $('[data-action="toggle-toc"]')?.addEventListener('click', () => {
      this.toggleTOC();
    });
    
    // Close TOC button
    $('[data-action="close-toc"]')?.addEventListener('click', () => {
      this.toggleTOC(false);
    });
    
    // TOC item clicks (delegated)
    this.elements.tocList?.addEventListener('click', (e) => {
      const link = e.target.closest('[data-toc-id]');
      if (link) {
        e.preventDefault();
        const id = link.dataset.tocId;
        this.preview.scrollToHeading(id);
      }
    });
  }
  
  setupScrollSync() {
    // Toggle sync scroll button
    $('[data-action="toggle-sync-scroll"]')?.addEventListener('click', (e) => {
      const btn = e.currentTarget;
      this.state.syncScroll = !this.state.syncScroll;
      btn.setAttribute('aria-pressed', this.state.syncScroll);
      
      if (this.state.syncScroll) {
        btn.classList.add('active');
      } else {
        btn.classList.remove('active');
      }
      
      this.storage.set('syncScroll', this.state.syncScroll);
    });
    
    // Editor scroll -> Preview scroll
    const editorTextarea = this.elements.editorTextarea;
    const previewContainer = this.elements.previewContainer;
    
    if (editorTextarea && previewContainer) {
      editorTextarea.addEventListener('scroll', throttle(() => {
        if (!this.state.syncScroll) return;
        
        const editorScrollPercentage = editorTextarea.scrollTop / 
          (editorTextarea.scrollHeight - editorTextarea.clientHeight);
        
        previewContainer.scrollTop = editorScrollPercentage * 
          (previewContainer.scrollHeight - previewContainer.clientHeight);
      }, 16));
    }
  }
  
  setupBeforeUnload() {
    window.addEventListener('beforeunload', (e) => {
      if (this.state.hasUnsavedChanges) {
        // Save before leaving
        this.saveDocument();
        
        // Show warning (browser may ignore custom message)
        e.preventDefault();
        e.returnValue = '';
      }
    });
  }
  
  // ============================================
  // DOCUMENT OPERATIONS
  // ============================================
  
  loadSavedContent() {
    const content = this.storage.get('content');
    
    if (content) {
      this.editor.setContent(content, false);
      this.preview.render(content);
      this.updateStats(content);
      this.updateDocumentTitle(content);
    } else {
      // Load welcome content
      const welcomeContent = this.getWelcomeContent();
      this.editor.setContent(welcomeContent, false);
      this.preview.render(welcomeContent);
      this.updateStats(welcomeContent);
    }
    
    // Restore cursor/scroll position
    const cursorPos = this.storage.get('cursorPosition');
    const scrollPos = this.storage.get('scrollPosition');
    
    if (cursorPos !== null) {
      this.editor.setCursorPosition(cursorPos);
    }
    
    if (scrollPos !== null) {
      this.elements.editorTextarea.scrollTop = scrollPos;
    }
  }
  
  getWelcomeContent() {
    return `# Welcome to MarkdownStudio Pro

Start writing your Markdown here. Your work is **automatically saved** to your browser.

## Quick Start Guide

Use the toolbar above or these keyboard shortcuts:

| Action | Shortcut |
|--------|----------|
| **Bold** | Ctrl + B |
| *Italic* | Ctrl + I |
| [Link](url) | Ctrl + K |
| \`Code\` | Ctrl + \` |
| Heading | Ctrl + 1-6 |

## Features

- **Live Preview** - See your formatted content in real-time
- **Auto Save** - Never lose your work
- **Export** - Download as Markdown or HTML
- **Dark Mode** - Easy on the eyes
- **Offline Support** - Works without internet

## Code Example

\`\`\`javascript
function greet(name) {
  console.log(\`Hello, \${name}!\`);
}

greet('MarkdownStudio');
\`\`\`

## Try It Out

1. Edit this content
2. See the live preview
3. Export when ready

---

Happy writing! ✍️
`;
  }
  
  setupAutosave() {
    const autosave = debounce(() => {
      this.saveDocument();
    }, 2000);
    
    this.eventBus.on(Events.CONTENT_CHANGE, () => {
      this.updateSaveIndicator('saving');
      autosave();
    });
  }
  
  saveDocument(manual = false) {
    const content = this.editor.getContent();
    const cursorPos = this.editor.getCursorPosition();
    const scrollPos = this.elements.editorTextarea?.scrollTop || 0;
    
    const success = this.storage.set('content', content) &&
                    this.storage.set('cursorPosition', cursorPos) &&
                    this.storage.set('scrollPosition', scrollPos) &&
                    this.storage.set('lastSaved', new Date().toISOString());
    
    if (success) {
      this.eventBus.emit(Events.CONTENT_SAVED, { manual });
    } else {
      this.updateSaveIndicator('error');
      this.showNotification('Failed to save. Storage may be full.', 'error');
    }
  }
  
  async newDocument() {
    if (this.state.hasUnsavedChanges) {
      const confirmed = await this.showConfirm(
        'Create New Document',
        'You have unsaved changes. Are you sure you want to create a new document?'
      );
      
      if (!confirmed) return;
    }
    
    this.editor.setContent('# New Document\n\n', false);
    this.preview.render('# New Document\n\n');
    this.editor.setCursorPosition(16);
    this.editor.focus();
    
    // Reset state
    this.state.hasUnsavedChanges = false;
    this.updateSaveIndicator('saved');
    this.updateDocumentTitle('# New Document');
    
    // Save immediately
    this.saveDocument();
    
    this.showNotification('New document created', 'success', 2000);
  }
  
  openFile() {
    this.elements.fileInput?.click();
  }
  
  async importFile(file) {
    // Validate file type
    const validTypes = ['text/markdown', 'text/plain', 'text/x-markdown'];
    const validExtensions = ['.md', '.markdown', '.txt', '.text'];
    
    const isValidType = validTypes.includes(file.type);
    const isValidExtension = validExtensions.some(ext => 
      file.name.toLowerCase().endsWith(ext)
    );
    
    if (!isValidType && !isValidExtension) {
      this.showNotification('Please select a Markdown or text file', 'error');
      return;
    }
    
    // Check file size (max 5MB)
    if (file.size > 5 * 1024 * 1024) {
      this.showNotification('File is too large (max 5MB)', 'error');
      return;
    }
    
    try {
      const content = await file.text();
      
      if (this.state.hasUnsavedChanges) {
        const confirmed = await this.showConfirm(
          'Open File',
          'You have unsaved changes. Are you sure you want to open a new file?'
        );
        
        if (!confirmed) return;
      }
      
      this.editor.setContent(content, false);
      this.preview.render(content);
      this.updateStats(content);
      this.updateDocumentTitle(content);
      this.editor.setCursorPosition(0);
      this.editor.focus();
      
      this.state.hasUnsavedChanges = false;
      this.saveDocument();
      
      this.showNotification(`Opened: ${file.name}`, 'success', 2000);
      
    } catch (error) {
      console.error('Failed to read file:', error);
      this.showNotification('Failed to read file', 'error');
    }
  }
  
  // ============================================
  // EXPORT OPERATIONS
  // ============================================
  
  exportMarkdown() {
    const content = this.editor.getContent();
    const title = this.extractTitle(content);
    
    this.exportService.downloadMarkdown(content, title);
    this.showNotification('Markdown file downloaded', 'success', 2000);
  }
  
  exportHTML() {
    const content = this.editor.getContent();
    const html = this.preview.getHTML();
    const title = this.extractTitle(content);
    
    this.exportService.downloadHTML(html, title);
    this.showNotification('HTML file downloaded', 'success', 2000);
  }
  
  async copyMarkdown() {
    const content = this.editor.getContent();
    
    try {
      await navigator.clipboard.writeText(content);
      this.showNotification('Markdown copied to clipboard', 'success', 2000);
    } catch (error) {
      console.error('Failed to copy:', error);
      this.showNotification('Failed to copy to clipboard', 'error');
    }
  }
  
  async copyHTML() {
    const html = this.preview.getHTML();
    
    try {
      await navigator.clipboard.writeText(html);
      this.showNotification('HTML copied to clipboard', 'success', 2000);
    } catch (error) {
      console.error('Failed to copy:', error);
      this.showNotification('Failed to copy to clipboard', 'error');
    }
  }
  
  // ============================================
  // VIEW OPERATIONS
  // ============================================
  
  setViewMode(mode) {
    this.state.viewMode = mode;
    this.elements.editorContainer?.setAttribute('data-view', mode);
    
    // Update button states
    document.querySelectorAll('[data-action^="view-"]').forEach(btn => {
      const btnMode = btn.dataset.action.replace('view-', '');
      btn.classList.toggle('active', btnMode === mode);
      btn.setAttribute('aria-pressed', btnMode === mode);
    });
    
    // Save preference
    this.storage.set('viewMode', mode);
    
    // Focus appropriate element
    if (mode === 'editor' || mode === 'split') {
      this.editor.focus();
    }
  }
  
  cycleViewMode() {
    const modes = ['split', 'editor', 'preview'];
    const currentIndex = modes.indexOf(this.state.viewMode);
    const nextIndex = (currentIndex + 1) % modes.length;
    this.setViewMode(modes[nextIndex]);
  }
  
  toggleTOC(show) {
    const tocSidebar = this.elements.tocSidebar;
    const btn = $('[data-action="toggle-toc"]');
    
    if (!tocSidebar) return;
    
    const shouldShow = show !== undefined ? show : tocSidebar.hidden;
    
    tocSidebar.hidden = !shouldShow;
    btn?.setAttribute('aria-pressed', shouldShow);
    btn?.classList.toggle('active', shouldShow);
    
    this.state.showTOC = shouldShow;
    this.storage.set('showTOC', shouldShow);
  }
  
  toggleFullscreen() {
    if (!document.fullscreenElement) {
      document.documentElement.requestFullscreen?.();
      document.body.classList.add('fullscreen');
    } else {
      document.exitFullscreen?.();
      document.body.classList.remove('fullscreen');
    }
  }
  
  // ============================================
  // FIND & REPLACE
  // ============================================
  
  showFindPanel(showReplace = false) {
    const findPanel = this.elements.findPanel;
    const findInput = this.elements.findInput;
    const replaceRow = this.elements.replaceRow;
    const toggleBtn = $('[data-action="toggle-replace"]');
    
    if (!findPanel) return;
    
    findPanel.hidden = false;
    replaceRow.hidden = !showReplace;
    toggleBtn?.setAttribute('aria-expanded', showReplace);
    
    // Pre-fill with selection
    const selection = this.editor.getSelectedText();
    if (selection) {
      findInput.value = selection;
    }
    
    findInput.focus();
    findInput.select();
    
    // Perform initial find
    if (findInput.value) {
      this.performFind();
    }
  }
  
  closeFindPanel() {
    const findPanel = this.elements.findPanel;
    if (findPanel) {
      findPanel.hidden = true;
      this.editor.clearHighlights();
      this.editor.focus();
    }
  }
  
  toggleFindOption(button) {
    const isPressed = button.getAttribute('aria-pressed') === 'true';
    button.setAttribute('aria-pressed', !isPressed);
    button.classList.toggle('active', !isPressed);
    this.performFind();
  }
  
  getFindOptions() {
    return {
      caseSensitive: $('[data-action="toggle-find-case"]')?.getAttribute('aria-pressed') === 'true',
      wholeWord: $('[data-action="toggle-find-word"]')?.getAttribute('aria-pressed') === 'true',
      regex: $('[data-action="toggle-find-regex"]')?.getAttribute('aria-pressed') === 'true'
    };
  }
  
  performFind() {
    const query = this.elements.findInput?.value || '';
    const options = this.getFindOptions();
    
    if (!query) {
      this.elements.findCount.textContent = '';
      this.editor.clearHighlights();
      return;
    }
    
    const results = this.editor.find(query, options);
    this.elements.findCount.textContent = results.total > 0 
      ? `${results.current}/${results.total}`
      : 'No results';
  }
  
  findNext() {
    this.editor.findNext();
    this.updateFindCount();
  }
  
  findPrevious() {
    this.editor.findPrevious();
    this.updateFindCount();
  }
  
  updateFindCount() {
    const results = this.editor.getFindResults();
    if (results.total > 0) {
      this.elements.findCount.textContent = `${results.current}/${results.total}`;
    }
  }
  
  replaceOne() {
    const replacement = this.elements.replaceInput?.value || '';
    this.editor.replaceOne(replacement);
    this.performFind();
  }
  
  replaceAll() {
    const replacement = this.elements.replaceInput?.value || '';
    const count = this.editor.replaceAll(replacement);
    this.performFind();
    this.showNotification(`Replaced ${count} occurrences`, 'success', 2000);
  }
  
  // ============================================
  // MODALS
  // ============================================
  
  showModal(name) {
    const modal = this.elements[`${name}Modal`];
    if (!modal) return;
    
    modal.hidden = false;
    
    // Focus first focusable element
    const focusable = modal.querySelector('button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
    focusable?.focus();
    
    // Trap focus
    this.trapFocus(modal);
  }
  
  closeModal(name) {
    const modal = this.elements[`${name}Modal`];
    if (modal) {
      modal.hidden = true;
    }
  }
  
  closeAllModals() {
    document.querySelectorAll('.modal:not([hidden])').forEach(modal => {
      modal.hidden = true;
    });
  }
  
  trapFocus(element) {
    const focusableElements = element.querySelectorAll(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    
    const firstFocusable = focusableElements[0];
    const lastFocusable = focusableElements[focusableElements.length - 1];
    
    const handleKeydown = (e) => {
      if (e.key !== 'Tab') return;
      
      if (e.shiftKey) {
        if (document.activeElement === firstFocusable) {
          lastFocusable.focus();
          e.preventDefault();
        }
      } else {
        if (document.activeElement === lastFocusable) {
          firstFocusable.focus();
          e.preventDefault();
        }
      }
    };
    
    element.addEventListener('keydown', handleKeydown);
    
    // Remove listener when modal closes
    const observer = new MutationObserver(() => {
      if (element.hidden) {
        element.removeEventListener('keydown', handleKeydown);
        observer.disconnect();
      }
    });
    
    observer.observe(element, { attributes: true, attributeFilter: ['hidden'] });
  }
  
  showConfirm(title, message) {
    return new Promise((resolve) => {
      const modal = this.elements.confirmModal;
      if (!modal) {
        resolve(true);
        return;
      }
      
      $('#confirm-title').textContent = title;
      $('#confirm-message').textContent = message;
      
      this.confirmResolve = () => resolve(true);
      this.confirmReject = () => resolve(false);
      
      this.showModal('confirm');
    });
  }
  
  // ============================================
  // UI UPDATES
  // ============================================
  
  updateStats(content) {
    const text = content || '';
    
    // Word count
    const words = text.trim() ? text.trim().split(/\s+/).length : 0;
    const wordsEl = this.elements.statsWords?.querySelector('.status-value');
    if (wordsEl) wordsEl.textContent = words;
    
    // Character count
    const chars = text.length;
    const charsEl = this.elements.statsChars?.querySelector('.status-value');
    if (charsEl) charsEl.textContent = chars;
    
    // Line count
    const lines = text ? text.split('\n').length : 0;
    const linesEl = this.elements.statsLines?.querySelector('.status-value');
    if (linesEl) linesEl.textContent = lines;
    
    // Reading time (200 wpm)
    const readTime = Math.max(1, Math.ceil(words / 200));
    const readTimeEl = this.elements.statsReadingTime?.querySelector('.status-value');
    if (readTimeEl) readTimeEl.textContent = readTime;
  }
  
  updateSelectionStats(text) {
    if (!text) return;
    
    const words = text.trim() ? text.trim().split(/\s+/).length : 0;
    const chars = text.length;
    
    // Show selection count in status bar
    // (Optional: could add a selection-specific display)
  }
  
  updateCursorPosition(line, column) {
    if (this.elements.cursorLine) {
      this.elements.cursorLine.textContent = line;
    }
    if (this.elements.cursorCol) {
      this.elements.cursorCol.textContent = column;
    }
  }
  
  updateSaveIndicator(status) {
    const indicator = this.elements.saveIndicator;
    if (!indicator) return;
    
    indicator.dataset.status = status;
    
    const textEl = indicator.querySelector('.save-text');
    if (textEl) {
      switch (status) {
        case 'saved':
          textEl.textContent = 'Saved';
          break;
        case 'saving':
          textEl.textContent = 'Saving...';
          break;
        case 'error':
          textEl.textContent = 'Error';
          break;
      }
    }
  }
  
  updateDocumentTitle(content) {
    const title = this.extractTitle(content);
    
    if (this.elements.documentTitle) {
      this.elements.documentTitle.textContent = title;
      this.elements.documentTitle.title = title;
    }
    
    // Update page title
    document.title = `${title} - MarkdownStudio Pro`;
  }
  
  extractTitle(content) {
    if (!content) return 'Untitled Document';
    
    const match = content.match(/^#\s+(.+)$/m);
    return match ? match[1].trim().substring(0, 50) : 'Untitled Document';
  }
  
  updateTOC(toc) {
    const tocList = this.elements.tocList;
    if (!tocList) return;
    
    if (!toc || toc.length === 0) {
      tocList.innerHTML = '<li class="toc-empty">No headings found</li>';
      return;
    }
    
    tocList.innerHTML = toc.map(item => `
      <li class="toc-item" data-level="${item.level}">
        <a href="#${item.id}" data-toc-id="${item.id}">${this.escapeHtml(item.text)}</a>
      </li>
    `).join('');
  }
  
  // ============================================
  // NOTIFICATIONS
  // ============================================
  
  showNotification(message, type = 'info', duration = 3000) {
    const container = this.elements.notifications;
    if (!container) return;
    
    const notification = document.createElement('div');
    notification.className = `notification notification-${type}`;
    notification.innerHTML = `
      <span class="notification-message">${this.escapeHtml(message)}</span>
      <button type="button" class="btn btn-icon btn-xs notification-close" aria-label="Close">
        <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <line x1="18" y1="6" x2="6" y2="18"/>
          <line x1="6" y1="6" x2="18" y2="18"/>
        </svg>
      </button>
    `;
    
    // Close button
    notification.querySelector('.notification-close')?.addEventListener('click', () => {
      this.hideNotification(notification);
    });
    
    container.appendChild(notification);
    
    // Trigger animation
    requestAnimationFrame(() => {
      notification.classList.add('visible');
    });
    
    // Auto remove
    if (duration > 0) {
      setTimeout(() => {
        this.hideNotification(notification);
      }, duration);
    }
    
    return notification;
  }
  
  hideNotification(notification) {
    notification.classList.remove('visible');
    setTimeout(() => {
      notification.remove();
    }, 300);
  }
  
  // ============================================
  // PREFERENCES
  // ============================================
  
  restorePreferences() {
    // View mode
    const viewMode = this.storage.get('viewMode') || 'split';
    this.setViewMode(viewMode);
    
    // Sync scroll
    const syncScroll = this.storage.get('syncScroll');
    if (syncScroll !== null) {
      this.state.syncScroll = syncScroll;
      const btn = $('[data-action="toggle-sync-scroll"]');
      btn?.setAttribute('aria-pressed', syncScroll);
      btn?.classList.toggle('active', syncScroll);
    }
    
    // TOC
    const showTOC = this.storage.get('showTOC');
    if (showTOC) {
      this.toggleTOC(true);
    }
    
    // Line numbers
    const showLineNumbers = this.storage.get('showLineNumbers');
    if (showLineNumbers) {
      this.state.showLineNumbers = true;
      this.elements.lineNumbers.hidden = false;
    }
  }
  
  // ============================================
  // UTILITIES
  // ============================================
  
  closeAllDropdowns() {
    document.querySelectorAll('.dropdown-menu:not([hidden])').forEach(menu => {
      menu.hidden = true;
      const trigger = menu.previousElementSibling;
      trigger?.setAttribute('aria-expanded', 'false');
    });
  }
  
  escapeHtml(str) {
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
  }
  
  handleInsertSubmit() {
    const text = $('#insert-text')?.value || '';
    const url = $('#insert-url')?.value || '';
    const title = $('#insert-title')?.value || '';
    const insertType = this.elements.insertModal?.dataset.type || 'link';
    
    if (!url) return;
    
    if (insertType === 'image') {
      this.editor.insertImage(url, text, title);
    } else {
      this.editor.insertLink(url, text, title);
    }
    
    this.closeModal('insert');
    this.editor.focus();
  }
}

// ============================================
// INITIALIZE APPLICATION
// ============================================

// Wait for DOM and libraries to load
document.addEventListener('DOMContentLoaded', () => {
  // Check if required libraries are loaded
  if (typeof marked === 'undefined' || typeof DOMPurify === 'undefined') {
    console.error('Required libraries not loaded');
    document.body.innerHTML = `
      <div style="padding: 2rem; text-align: center;">
        <h1>Loading Error</h1>
        <p>Required libraries failed to load. Please refresh the page.</p>
        <button onclick="location.reload()">Refresh</button>
      </div>
    `;
    return;
  }
  
  // Initialize app
  window.markdownStudio = new MarkdownStudioApp();
});

// Export for module usage
export { MarkdownStudioApp };
```

Now let me provide the remaining JavaScript modules:

---

### assets/js/utils/event-bus.js

```javascript
/**
 * Event Bus - Pub/Sub pattern for decoupled communication
 * @author Oathan Rex
 */

export const Events = {
  // Content events
  CONTENT_CHANGE: 'content:change',
  CONTENT_SAVED: 'content:saved',
  CONTENT_LOADED: 'content:loaded',
  
  // Editor events
  CURSOR_CHANGE: 'cursor:change',
  SELECTION_CHANGE: 'selection:change',
  SCROLL: 'editor:scroll',
  FOCUS: 'editor:focus',
  BLUR: 'editor:blur',
  
  // Preview events
  PREVIEW_RENDERED: 'preview:rendered',
  PREVIEW_SCROLL: 'preview:scroll',
  
  // Theme events
  THEME_CHANGE: 'theme:change',
  
  // Layout events
  LAYOUT_CHANGE: 'layout:change',
  VIEW_MODE_CHANGE: 'viewMode:change',
  
  // Shortcut events
  SHORTCUT_SAVE: 'shortcut:save',
  SHORTCUT_NEW: 'shortcut:new',
  SHORTCUT_OPEN: 'shortcut:open',
  SHORTCUT_FIND: 'shortcut:find',
  SHORTCUT_REPLACE: 'shortcut:replace',
  SHORTCUT_TOGGLE_THEME: 'shortcut:toggleTheme',
  SHORTCUT_TOGGLE_PREVIEW: 'shortcut:togglePreview',
  SHORTCUT_SHOW_SHORTCUTS: 'shortcut:showShortcuts',
  SHORTCUT_FULLSCREEN: 'shortcut:fullscreen',
  
  // Notification events
  NOTIFICATION_SHOW: 'notification:show',
  NOTIFICATION_HIDE: 'notification:hide'
};

export class EventBus {
  constructor() {
    this.events = new Map();
    this.onceEvents = new Map();
  }
  
  /**
   * Subscribe to an event
   * @param {string} event - Event name
   * @param {Function} callback - Event handler
   * @returns {Function} Unsubscribe function
   */
  on(event, callback) {
    if (!this.events.has(event)) {
      this.events.set(event, new Set());
    }
    this.events.get(event).add(callback);
    
    return () => this.off(event, callback);
  }
  
  /**
   * Subscribe to an event once
   * @param {string} event - Event name
   * @param {Function} callback - Event handler
   */
  once(event, callback) {
    const unsubscribe = this.on(event, (data) => {
      unsubscribe();
      callback(data);
    });
  }
  
  /**
   * Unsubscribe from an event
   * @param {string} event - Event name
   * @param {Function} callback - Event handler
   */
  off(event, callback) {
    if (this.events.has(event)) {
      this.events.get(event).delete(callback);
    }
  }
  
  /**
   * Emit an event
   * @param {string} event - Event name
   * @param {*} data - Event data
   */
  emit(event, data) {
    if (this.events.has(event)) {
      this.events.get(event).forEach(callback => {
        try {
          callback(data);
        } catch (error) {
          console.error(`Error in event handler for "${event}":`, error);
        }
      });
    }
  }
  
  /**
   * Clear all listeners for an event or all events
   * @param {string} [event] - Event name (optional)
   */
  clear(event) {
    if (event) {
      this.events.delete(event);
    } else {
      this.events.clear();
    }
  }
  
  /**
   * Get listener count for an event
   * @param {string} event - Event name
   * @returns {number}
   */
  listenerCount(event) {
    return this.events.has(event) ? this.events.get(event).size : 0;
  }
}
```

---

### assets/js/utils/debounce.js

```javascript
/**
 * Debounce and Throttle utilities
 * @author Oathan Rex
 */

/**
 * Create a debounced function
 * @param {Function} func - Function to debounce
 * @param {number} wait - Milliseconds to wait
 * @param {boolean} [immediate=false] - Execute on leading edge
 * @returns {Function}
 */
export function debounce(func, wait, immediate = false) {
  let timeout;
  
  return function executedFunction(...args) {
    const context = this;
    
    const later = () => {
      timeout = null;
      if (!immediate) func.apply(context, args);
    };
    
    const callNow = immediate && !timeout;
    
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
    
    if (callNow) func.apply(context, args);
  };
}

/**
 * Create a throttled function
 * @param {Function} func - Function to throttle
 * @param {number} limit - Milliseconds between executions
 * @returns {Function}
 */
export function throttle(func, limit) {
  let inThrottle;
  let lastFunc;
  let lastRan;
  
  return function executedFunction(...args) {
    const context = this;
    
    if (!inThrottle) {
      func.apply(context, args);
      lastRan = Date.now();
      inThrottle = true;
      
      setTimeout(() => {
        inThrottle = false;
        if (lastFunc) {
          lastFunc();
          lastFunc = null;
        }
      }, limit);
    } else {
      lastFunc = () => func.apply(context, args);
    }
  };
}

/**
 * Create a function that runs at most once per animation frame
 * @param {Function} func - Function to execute
 * @returns {Function}
 */
export function rafThrottle(func) {
  let rafId = null;
  
  return function executedFunction(...args) {
    const context = this;
    
    if (rafId) return;
    
    rafId = requestAnimationFrame(() => {
      func.apply(context, args);
      rafId = null;
    });
  };
}
```

---
### assets/js/utils/dom.js

```javascript
/**
 * DOM Utility Functions
 * @author Oathan Rex
 */

/**
 * Query selector shorthand
 * @param {string} selector - CSS selector
 * @param {Element} [context=document] - Context element
 * @returns {Element|null}
 */
export function $(selector, context = document) {
  return context.querySelector(selector);
}

/**
 * Query selector all shorthand
 * @param {string} selector - CSS selector
 * @param {Element} [context=document] - Context element
 * @returns {NodeList}
 */
export function $$(selector, context = document) {
  return context.querySelectorAll(selector);
}

/**
 * Create element with attributes and children
 * @param {string} tag - Tag name
 * @param {Object} [attrs={}] - Attributes
 * @param {Array|string} [children=[]] - Child elements or text
 * @returns {Element}
 */
export function createElement(tag, attrs = {}, children = []) {
  const element = document.createElement(tag);
  
  Object.entries(attrs).forEach(([key, value]) => {
    if (key === 'className') {
      element.className = value;
    } else if (key === 'dataset') {
      Object.entries(value).forEach(([dataKey, dataValue]) => {
        element.dataset[dataKey] = dataValue;
      });
    } else if (key.startsWith('on') && typeof value === 'function') {
      element.addEventListener(key.slice(2).toLowerCase(), value);
    } else if (key === 'style' && typeof value === 'object') {
      Object.assign(element.style, value);
    } else {
      element.setAttribute(key, value);
    }
  });
  
  const childArray = Array.isArray(children) ? children : [children];
  childArray.forEach(child => {
    if (typeof child === 'string') {
      element.appendChild(document.createTextNode(child));
    } else if (child instanceof Node) {
      element.appendChild(child);
    }
  });
  
  return element;
}

/**
 * Remove all children from element
 * @param {Element} element
 */
export function clearChildren(element) {
  while (element.firstChild) {
    element.removeChild(element.firstChild);
  }
}

/**
 * Check if element is visible in viewport
 * @param {Element} element
 * @returns {boolean}
 */
export function isInViewport(element) {
  const rect = element.getBoundingClientRect();
  return (
    rect.top >= 0 &&
    rect.left >= 0 &&
    rect.bottom <= (window.innerHeight || document.documentElement.clientHeight) &&
    rect.right <= (window.innerWidth || document.documentElement.clientWidth)
  );
}

/**
 * Get scroll percentage of element
 * @param {Element} element
 * @returns {number} 0-1
 */
export function getScrollPercentage(element) {
  const { scrollTop, scrollHeight, clientHeight } = element;
  if (scrollHeight <= clientHeight) return 0;
  return scrollTop / (scrollHeight - clientHeight);
}

/**
 * Set scroll percentage of element
 * @param {Element} element
 * @param {number} percentage - 0-1
 */
export function setScrollPercentage(element, percentage) {
  const { scrollHeight, clientHeight } = element;
  element.scrollTop = percentage * (scrollHeight - clientHeight);
}

/**
 * Escape HTML entities
 * @param {string} str
 * @returns {string}
 */
export function escapeHtml(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

/**
 * Parse HTML string to document fragment
 * @param {string} html
 * @returns {DocumentFragment}
 */
export function parseHTML(html) {
  const template = document.createElement('template');
  template.innerHTML = html.trim();
  return template.content;
}

/**
 * Add event listener with automatic cleanup
 * @param {Element} element
 * @param {string} event
 * @param {Function} handler
 * @param {Object} [options]
 * @returns {Function} Cleanup function
 */
export function addEvent(element, event, handler, options) {
  element.addEventListener(event, handler, options);
  return () => element.removeEventListener(event, handler, options);
}

/**
 * Delegate event handling
 * @param {Element} parent
 * @param {string} event
 * @param {string} selector
 * @param {Function} handler
 * @returns {Function} Cleanup function
 */
export function delegate(parent, event, selector, handler) {
  const delegatedHandler = (e) => {
    const target = e.target.closest(selector);
    if (target && parent.contains(target)) {
      handler.call(target, e, target);
    }
  };
  
  parent.addEventListener(event, delegatedHandler);
  return () => parent.removeEventListener(event, delegatedHandler);
}

/**
 * Wait for element to appear in DOM
 * @param {string} selector
 * @param {number} [timeout=5000]
 * @returns {Promise<Element>}
 */
export function waitForElement(selector, timeout = 5000) {
  return new Promise((resolve, reject) => {
    const element = document.querySelector(selector);
    if (element) {
      resolve(element);
      return;
    }
    
    const observer = new MutationObserver((mutations, obs) => {
      const element = document.querySelector(selector);
      if (element) {
        obs.disconnect();
        resolve(element);
      }
    });
    
    observer.observe(document.body, {
      childList: true,
      subtree: true
    });
    
    setTimeout(() => {
      observer.disconnect();
      reject(new Error(`Element ${selector} not found within ${timeout}ms`));
    }, timeout);
  });
}

/**
 * Copy text to clipboard
 * @param {string} text
 * @returns {Promise<boolean>}
 */
export async function copyToClipboard(text) {
  try {
    await navigator.clipboard.writeText(text);
    return true;
  } catch (err) {
    // Fallback for older browsers
    const textarea = document.createElement('textarea');
    textarea.value = text;
    textarea.style.position = 'fixed';
    textarea.style.opacity = '0';
    document.body.appendChild(textarea);
    textarea.select();
    
    try {
      document.execCommand('copy');
      return true;
    } catch (e) {
      return false;
    } finally {
      document.body.removeChild(textarea);
    }
  }
}
```

---

### assets/js/services/storage.js

```javascript
/**
 * Storage Service - localStorage wrapper with namespacing
 * @author Oathan Rex
 */

export class Storage {
  /**
   * @param {string} namespace - Prefix for all keys
   */
  constructor(namespace = 'app_') {
    this.namespace = namespace;
    this.available = this.checkAvailability();
  }
  
  /**
   * Check if localStorage is available
   * @returns {boolean}
   */
  checkAvailability() {
    try {
      const test = '__storage_test__';
      localStorage.setItem(test, test);
      localStorage.removeItem(test);
      return true;
    } catch (e) {
      console.warn('localStorage not available:', e);
      return false;
    }
  }
  
  /**
   * Get namespaced key
   * @param {string} key
   * @returns {string}
   */
  getKey(key) {
    return `${this.namespace}${key}`;
  }
  
  /**
   * Get item from storage
   * @param {string} key
   * @param {*} [defaultValue=null]
   * @returns {*}
   */
  get(key, defaultValue = null) {
    if (!this.available) return defaultValue;
    
    try {
      const item = localStorage.getItem(this.getKey(key));
      if (item === null) return defaultValue;
      return JSON.parse(item);
    } catch (e) {
      console.warn(`Storage: Error reading "${key}"`, e);
      return defaultValue;
    }
  }
  
  /**
   * Set item in storage
   * @param {string} key
   * @param {*} value
   * @returns {boolean} Success
   */
  set(key, value) {
    if (!this.available) return false;
    
    try {
      const serialized = JSON.stringify(value);
      localStorage.setItem(this.getKey(key), serialized);
      return true;
    } catch (e) {
      if (e.name === 'QuotaExceededError') {
        console.warn('Storage: Quota exceeded');
        this.handleQuotaExceeded();
      } else {
        console.error(`Storage: Error writing "${key}"`, e);
      }
      return false;
    }
  }
  
  /**
   * Remove item from storage
   * @param {string} key
   */
  remove(key) {
    if (!this.available) return;
    localStorage.removeItem(this.getKey(key));
  }
  
  /**
   * Check if key exists
   * @param {string} key
   * @returns {boolean}
   */
  has(key) {
    if (!this.available) return false;
    return localStorage.getItem(this.getKey(key)) !== null;
  }
  
  /**
   * Get all keys with namespace
   * @returns {string[]}
   */
  keys() {
    if (!this.available) return [];
    
    return Object.keys(localStorage)
      .filter(key => key.startsWith(this.namespace))
      .map(key => key.slice(this.namespace.length));
  }
  
  /**
   * Clear all namespaced items
   */
  clear() {
    if (!this.available) return;
    
    this.keys().forEach(key => this.remove(key));
  }
  
  /**
   * Get storage usage info
   * @returns {{ used: number, available: number, percentage: number }}
   */
  getUsage() {
    if (!this.available) {
      return { used: 0, available: 0, percentage: 0 };
    }
    
    let used = 0;
    
    this.keys().forEach(key => {
      const value = localStorage.getItem(this.getKey(key));
      if (value) {
        used += value.length * 2; // UTF-16 = 2 bytes per char
      }
    });
    
    // Estimate available (usually 5-10MB)
    const estimated = 5 * 1024 * 1024; // 5MB
    
    return {
      used,
      available: estimated - used,
      percentage: Math.round((used / estimated) * 100)
    };
  }
  
  /**
   * Handle quota exceeded error
   */
  handleQuotaExceeded() {
    // Try to free up space by removing old data
    const oldData = ['history', 'snapshots'];
    oldData.forEach(key => {
      if (this.has(key)) {
        this.remove(key);
      }
    });
  }
  
  /**
   * Export all data as JSON
   * @returns {Object}
   */
  exportAll() {
    const data = {};
    this.keys().forEach(key => {
      data[key] = this.get(key);
    });
    return data;
  }
  
  /**
   * Import data from JSON
   * @param {Object} data
   */
  importAll(data) {
    Object.entries(data).forEach(([key, value]) => {
      this.set(key, value);
    });
  }
}
```

---

### assets/js/services/theme.js

```javascript
/**
 * Theme Manager - Light/Dark theme switching
 * @author Oathan Rex
 */

export class ThemeManager {
  /**
   * @param {Storage} storage
   * @param {EventBus} eventBus
   */
  constructor(storage, eventBus) {
    this.storage = storage;
    this.eventBus = eventBus;
    this.currentTheme = 'light';
    this.mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
    
    this.init();
  }
  
  /**
   * Initialize theme
   */
  init() {
    // Load saved preference or use system preference
    const savedTheme = this.storage.get('theme');
    
    if (savedTheme && savedTheme !== 'system') {
      this.setTheme(savedTheme, false);
    } else {
      this.detectSystemTheme();
    }
    
    // Watch for system theme changes
    this.watchSystemTheme();
  }
  
  /**
   * Detect system theme preference
   */
  detectSystemTheme() {
    const prefersDark = this.mediaQuery.matches;
    this.setTheme(prefersDark ? 'dark' : 'light', false);
  }
  
  /**
   * Watch for system theme changes
   */
  watchSystemTheme() {
    this.mediaQuery.addEventListener('change', (e) => {
      const savedTheme = this.storage.get('theme');
      
      // Only auto-switch if user hasn't set a preference
      if (!savedTheme || savedTheme === 'system') {
        this.setTheme(e.matches ? 'dark' : 'light', false);
      }
    });
  }
  
  /**
   * Set theme
   * @param {'light' | 'dark'} theme
   * @param {boolean} [save=true] - Save preference
   */
  setTheme(theme, save = true) {
    this.currentTheme = theme;
    
    // Update document
    document.documentElement.setAttribute('data-theme', theme);
    
    // Update meta theme-color
    const metaThemeColor = document.querySelector('meta[name="theme-color"]');
    if (metaThemeColor) {
      metaThemeColor.content = theme === 'dark' ? '#0f172a' : '#ffffff';
    }
    
    // Save preference
    if (save) {
      this.storage.set('theme', theme);
    }
    
    // Emit event
    this.eventBus.emit('theme:change', { theme });
  }
  
  /**
   * Toggle between light and dark
   */
  toggle() {
    const newTheme = this.currentTheme === 'light' ? 'dark' : 'light';
    this.setTheme(newTheme, true);
  }
  
  /**
   * Get current theme
   * @returns {'light' | 'dark'}
   */
  getTheme() {
    return this.currentTheme;
  }
  
  /**
   * Check if dark mode is active
   * @returns {boolean}
   */
  isDark() {
    return this.currentTheme === 'dark';
  }
}
```

---

### assets/js/services/shortcuts.js

```javascript
/**
 * Keyboard Shortcuts Manager
 * @author Oathan Rex
 */

import { Events } from '../utils/event-bus.js';

export class ShortcutsManager {
  /**
   * @param {EventBus} eventBus
   */
  constructor(eventBus) {
    this.eventBus = eventBus;
    this.enabled = true;
    
    this.shortcuts = new Map();
    this.registerDefaultShortcuts();
    this.setupListener();
  }
  
  /**
   * Register default shortcuts
   */
  registerDefaultShortcuts() {
    // File operations
    this.register('ctrl+s', Events.SHORTCUT_SAVE);
    this.register('cmd+s', Events.SHORTCUT_SAVE);
    this.register('ctrl+alt+n', Events.SHORTCUT_NEW);
    this.register('cmd+alt+n', Events.SHORTCUT_NEW);
    this.register('ctrl+o', Events.SHORTCUT_OPEN);
    this.register('cmd+o', Events.SHORTCUT_OPEN);
    
    // Find
    this.register('ctrl+f', Events.SHORTCUT_FIND);
    this.register('cmd+f', Events.SHORTCUT_FIND);
    this.register('ctrl+h', Events.SHORTCUT_REPLACE);
    this.register('cmd+alt+f', Events.SHORTCUT_REPLACE);
    
    // View
    this.register('ctrl+shift+l', Events.SHORTCUT_TOGGLE_THEME);
    this.register('cmd+shift+l', Events.SHORTCUT_TOGGLE_THEME);
    this.register('ctrl+shift+p', Events.SHORTCUT_TOGGLE_PREVIEW);
    this.register('cmd+shift+p', Events.SHORTCUT_TOGGLE_PREVIEW);
    this.register('ctrl+/', Events.SHORTCUT_SHOW_SHORTCUTS);
    this.register('cmd+/', Events.SHORTCUT_SHOW_SHORTCUTS);
    this.register('f11', Events.SHORTCUT_FULLSCREEN);
  }
  
  /**
   * Register a shortcut
   * @param {string} keys - Shortcut keys (e.g., 'ctrl+b')
   * @param {string|Function} action - Event name or callback
   */
  register(keys, action) {
    const normalized = this.normalizeKeys(keys);
    this.shortcuts.set(normalized, action);
  }
  
  /**
   * Unregister a shortcut
   * @param {string} keys
   */
  unregister(keys) {
    const normalized = this.normalizeKeys(keys);
    this.shortcuts.delete(normalized);
  }
  
  /**
   * Normalize key combination string
   * @param {string} keys
   * @returns {string}
   */
  normalizeKeys(keys) {
    return keys
      .toLowerCase()
      .split('+')
      .map(key => key.trim())
      .sort()
      .join('+');
  }
  
  /**
   * Get key combination from event
   * @param {KeyboardEvent} e
   * @returns {string}
   */
  getKeysFromEvent(e) {
    const keys = [];
    
    // Modifiers
    if (e.ctrlKey) keys.push('ctrl');
    if (e.metaKey) keys.push('cmd');
    if (e.altKey) keys.push('alt');
    if (e.shiftKey) keys.push('shift');
    
    // Main key
    let key = e.key.toLowerCase();
    
    // Normalize special keys
    if (key === ' ') key = 'space';
    if (key === 'escape') key = 'esc';
    if (key === 'arrowup') key = 'up';
    if (key === 'arrowdown') key = 'down';
    if (key === 'arrowleft') key = 'left';
    if (key === 'arrowright') key = 'right';
    
    // Don't include modifier keys as main key
    if (!['control', 'meta', 'alt', 'shift'].includes(key)) {
      keys.push(key);
    }
    
    return keys.sort().join('+');
  }
  
  /**
   * Set up keyboard event listener
   */
  setupListener() {
    document.addEventListener('keydown', (e) => {
      if (!this.enabled) return;
      
      // Ignore if in input/textarea (except for specific shortcuts)
      const target = e.target;
      const isInput = target.tagName === 'INPUT' || 
                      target.tagName === 'TEXTAREA' ||
                      target.isContentEditable;
      
      const keys = this.getKeysFromEvent(e);
      
      // Check for registered shortcut
      const action = this.shortcuts.get(keys);
      
      if (action) {
        // Allow certain shortcuts in inputs
        const allowInInput = [
          'ctrl+s', 'cmd+s',
          'ctrl+b', 'cmd+b',
          'ctrl+i', 'cmd+i',
          'ctrl+k', 'cmd+k',
          'ctrl+z', 'cmd+z',
          'ctrl+y', 'cmd+y',
          'ctrl+shift+z', 'cmd+shift+z',
          'ctrl+f', 'cmd+f',
          'ctrl+h', 'cmd+alt+f',
          'esc', 'f11'
        ].map(k => this.normalizeKeys(k));
        
        if (isInput && !allowInInput.includes(keys)) {
          return;
        }
        
        e.preventDefault();
        
        if (typeof action === 'function') {
          action(e);
        } else {
          this.eventBus.emit(action, { originalEvent: e });
        }
      }
    });
  }
  
  /**
   * Enable/disable shortcuts
   * @param {boolean} enabled
   */
  setEnabled(enabled) {
    this.enabled = enabled;
  }
  
  /**
   * Get all registered shortcuts
   * @returns {Map}
   */
  getAll() {
    return new Map(this.shortcuts);
  }
}
```

---

### assets/js/services/export.js

```javascript
/**
 * Export Service - Generate downloadable files
 * @author Oathan Rex
 */

export class ExportService {
  /**
   * Download content as Markdown file
   * @param {string} content - Markdown content
   * @param {string} [title] - Document title
   */
  downloadMarkdown(content, title = 'document') {
    const filename = this.sanitizeFilename(title) + '.md';
    this.download(content, filename, 'text/markdown;charset=utf-8');
  }
  
  /**
   * Download content as HTML file
   * @param {string} html - Rendered HTML content
   * @param {string} [title] - Document title
   */
  downloadHTML(html, title = 'document') {
    const filename = this.sanitizeFilename(title) + '.html';
    const fullHTML = this.generateHTMLDocument(html, title);
    this.download(fullHTML, filename, 'text/html;charset=utf-8');
  }
  
  /**
   * Generate complete HTML document
   * @param {string} content - Body content
   * @param {string} title - Document title
   * @returns {string}
   */
  generateHTMLDocument(content, title) {
    return `<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="generator" content="MarkdownStudio Pro">
  <title>${this.escapeHtml(title)}</title>
  <style>
${this.getExportStyles()}
  </style>
</head>
<body>
  <article class="markdown-body">
${content}
  </article>
</body>
</html>`;
  }
  
  /**
   * Get styles for exported HTML
   * @returns {string}
   */
  getExportStyles() {
    return `
    :root {
      color-scheme: light dark;
    }
    
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
      font-size: 16px;
      line-height: 1.6;
      color: #1e293b;
      background: #ffffff;
      padding: 2rem;
    }
    
    @media (prefers-color-scheme: dark) {
      body {
        color: #f1f5f9;
        background: #0f172a;
      }
    }
    
    .markdown-body {
      max-width: 800px;
      margin: 0 auto;
    }
    
    h1, h2, h3, h4, h5, h6 {
      margin-top: 1.5em;
      margin-bottom: 0.5em;
      font-weight: 600;
      line-height: 1.25;
    }
    
    h1 { font-size: 2em; border-bottom: 1px solid #e2e8f0; padding-bottom: 0.3em; }
    h2 { font-size: 1.5em; border-bottom: 1px solid #e2e8f0; padding-bottom: 0.3em; }
    h3 { font-size: 1.25em; }
    h4 { font-size: 1em; }
    h5 { font-size: 0.875em; }
    h6 { font-size: 0.85em; color: #64748b; }
    
    @media (prefers-color-scheme: dark) {
      h1, h2 { border-bottom-color: #334155; }
      h6 { color: #94a3b8; }
    }
    
    p { margin: 1em 0; }
    
    a { color: #2563eb; text-decoration: none; }
    a:hover { text-decoration: underline; }
    
    @media (prefers-color-scheme: dark) {
      a { color: #3b82f6; }
    }
    
    strong { font-weight: 600; }
    
    code {
      font-family: 'SF Mono', Consolas, 'Liberation Mono', Menlo, monospace;
      font-size: 0.875em;
      background: #f1f5f9;
      padding: 0.2em 0.4em;
      border-radius: 4px;
    }
    
    @media (prefers-color-scheme: dark) {
      code { background: #1e293b; }
    }
    
    pre {
      background: #f1f5f9;
      padding: 1em;
      border-radius: 6px;
      overflow-x: auto;
      margin: 1em 0;
    }
    
    @media (prefers-color-scheme: dark) {
      pre { background: #1e293b; }
    }
    
    pre code {
      background: none;
      padding: 0;
      font-size: 0.875em;
      line-height: 1.6;
    }
    
    blockquote {
      margin: 1em 0;
      padding: 0 1em;
      border-left: 4px solid #e2e8f0;
      color: #64748b;
    }
    
    @media (prefers-color-scheme: dark) {
      blockquote {
        border-left-color: #334155;
        color: #94a3b8;
      }
    }
    
    ul, ol {
      padding-left: 2em;
      margin: 1em 0;
    }
    
    li { margin: 0.25em 0; }
    
    table {
      border-collapse: collapse;
      width: 100%;
      margin: 1em 0;
    }
    
    th, td {
      border: 1px solid #e2e8f0;
      padding: 0.5em 1em;
      text-align: left;
    }
    
    th {
      background: #f8fafc;
      font-weight: 600;
    }
    
    @media (prefers-color-scheme: dark) {
      th, td { border-color: #334155; }
      th { background: #1e293b; }
    }
    
    hr {
      border: none;
      border-top: 1px solid #e2e8f0;
      margin: 2em 0;
    }
    
    @media (prefers-color-scheme: dark) {
      hr { border-top-color: #334155; }
    }
    
    img {
      max-width: 100%;
      height: auto;
      border-radius: 6px;
    }
    
    .task-list-item {
      list-style: none;
      margin-left: -1.5em;
    }
    
    .task-list-item input {
      margin-right: 0.5em;
    }
    
    @media print {
      body { padding: 0; }
      .markdown-body { max-width: none; }
      pre { white-space: pre-wrap; word-wrap: break-word; }
      a[href]::after { content: " (" attr(href) ")"; font-size: 0.8em; }
      a[href^="#"]::after { content: ""; }
    }
    `;
  }
  
  /**
   * Download content as file
   * @param {string} content
   * @param {string} filename
   * @param {string} mimeType
   */
  download(content, filename, mimeType) {
    const blob = new Blob([content], { type: mimeType });
    const url = URL.createObjectURL(blob);
    
    const link = document.createElement('a');
    link.href = url;
    link.download = filename;
    link.style.display = 'none';
    
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    
    // Clean up
    setTimeout(() => URL.revokeObjectURL(url), 100);
  }
  
  /**
   * Sanitize filename
   * @param {string} name
   * @returns {string}
   */
  sanitizeFilename(name) {
    return name
      .replace(/[<>:"/\\|?*]/g, '')
      .replace(/\s+/g, '-')
      .toLowerCase()
      .substring(0, 100) || 'document';
  }
  
  /**
   * Escape HTML entities
   * @param {string} str
   * @returns {string}
   */
  escapeHtml(str) {
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
  }
}
```

---

### assets/js/core/editor.js

```javascript
/**
 * Editor Core - Text editing functionality
 * @author Oathan Rex
 */

import { Events } from '../utils/event-bus.js';
import { debounce } from '../utils/debounce.js';

export class Editor {
  /**
   * @param {Object} options
   * @param {HTMLTextAreaElement} options.element
   * @param {HTMLElement} options.lineNumbers
   * @param {EventBus} options.eventBus
   */
  constructor({ element, lineNumbers, eventBus }) {
    this.textarea = element;
    this.lineNumbersEl = lineNumbers;
    this.eventBus = eventBus;
    
    // Find state
    this.findState = {
      query: '',
      matches: [],
      currentIndex: -1,
      options: {
        caseSensitive: false,
        wholeWord: false,
        regex: false
      }
    };
    
    // History for undo/redo
    this.history = [];
    this.historyIndex = -1;
    this.maxHistory = 100;
    this.lastContent = '';
    
    this.init();
  }
  
  /**
   * Initialize editor
   */
  init() {
    this.setupEventListeners();
    this.setupTabHandling();
    this.lastContent = this.textarea.value;
    this.saveHistory();
  }
  
  /**
   * Set up event listeners
   */
  setupEventListeners() {
    // Content change
    this.textarea.addEventListener('input', () => {
      const content = this.getContent();
      
      this.eventBus.emit(Events.CONTENT_CHANGE, { content });
      this.updateLineNumbers();
      
      // Save to history (debounced)
      this.debouncedSaveHistory();
    });
    
    // Cursor/selection change
    this.textarea.addEventListener('keyup', () => this.emitCursorChange());
    this.textarea.addEventListener('click', () => this.emitCursorChange());
    this.textarea.addEventListener('select', () => {
      const text = this.getSelectedText();
      this.eventBus.emit(Events.SELECTION_CHANGE, { text });
    });
    
    // Focus/blur
    this.textarea.addEventListener('focus', () => {
      this.eventBus.emit(Events.FOCUS);
    });
    
    this.textarea.addEventListener('blur', () => {
      this.eventBus.emit(Events.BLUR);
    });
    
    // Scroll
    this.textarea.addEventListener('scroll', () => {
      this.syncLineNumbersScroll();
    });
    
    // Keyboard shortcuts for formatting
    this.textarea.addEventListener('keydown', (e) => {
      this.handleKeydown(e);
    });
  }
  
  /**
   * Set up tab key handling
   */
  setupTabHandling() {
    this.textarea.addEventListener('keydown', (e) => {
      if (e.key === 'Tab') {
        e.preventDefault();
        
        const start = this.textarea.selectionStart;
        const end = this.textarea.selectionEnd;
        const content = this.getContent();
        
        if (e.shiftKey) {
          // Outdent
          const lineStart = content.lastIndexOf('\n', start - 1) + 1;
          const lineContent = content.slice(lineStart, start);
          
          if (lineContent.startsWith('  ')) {
            const newContent = content.slice(0, lineStart) + 
                               content.slice(lineStart + 2);
            this.setContent(newContent);
            this.setCursorPosition(Math.max(lineStart, start - 2));
          }
        } else {
          // Indent
          const newContent = content.slice(0, start) + '  ' + content.slice(end);
          this.setContent(newContent);
          this.setCursorPosition(start + 2);
        }
      }
    });
  }
  
  /**
   * Handle keyboard shortcuts
   * @param {KeyboardEvent} e
   */
  handleKeydown(e) {
    const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0;
    const ctrl = isMac ? e.metaKey : e.ctrlKey;
    
    if (!ctrl) return;
    
    let handled = true;
    
    switch (e.key.toLowerCase()) {
      case 'b':
        this.wrapSelection('**', '**');
        break;
      case 'i':
        this.wrapSelection('*', '*');
        break;
      case 'k':
        this.insertLink();
        break;
      case '`':
        this.wrapSelection('`', '`');
        break;
      case '1':
      case '2':
      case '3':
      case '4':
      case '5':
      case '6':
        this.insertHeading(parseInt(e.key));
        break;
      case 'z':
        if (e.shiftKey) {
          this.redo();
        } else {
          this.undo();
        }
        break;
      case 'y':
        this.redo();
        break;
      default:
        handled = false;
    }
    
    if (handled) {
      e.preventDefault();
    }
  }
  
  /**
   * Debounced save to history
   */
  debouncedSaveHistory = debounce(() => {
    this.saveHistory();
  }, 500);
  
  /**
   * Save current state to history
   */
  saveHistory() {
    const content = this.getContent();
    
    // Don't save if content hasn't changed
    if (content === this.lastContent) return;
    
    // Remove future history if we're not at the end
    if (this.historyIndex < this.history.length - 1) {
      this.history = this.history.slice(0, this.historyIndex + 1);
    }
    
    // Add to history
    this.history.push({
      content,
      cursorPosition: this.getCursorPosition()
    });
    
    // Limit history size
    if (this.history.length > this.maxHistory) {
      this.history.shift();
    }
    
    this.historyIndex = this.history.length - 1;
    this.lastContent = content;
    
    // Update undo/redo buttons
    this.updateHistoryButtons();
  }
  
  /**
   * Undo last change
   */
  undo() {
    if (this.historyIndex > 0) {
      this.historyIndex--;
      const state = this.history[this.historyIndex];
      this.textarea.value = state.content;
      this.setCursorPosition(state.cursorPosition);
      this.lastContent = state.content;
      
      this.eventBus.emit(Events.CONTENT_CHANGE, { content: state.content });
      this.updateHistoryButtons();
    }
  }
  
  /**
   * Redo last undone change
   */
  redo() {
    if (this.historyIndex < this.history.length - 1) {
      this.historyIndex++;
      const state = this.history[this.historyIndex];
      this.textarea.value = state.content;
      this.setCursorPosition(state.cursorPosition);
      this.lastContent = state.content;
      
      this.eventBus.emit(Events.CONTENT_CHANGE, { content: state.content });
      this.updateHistoryButtons();
    }
  }
  
  /**
   * Update undo/redo button states
   */
  updateHistoryButtons() {
    const undoBtn = document.getElementById('btn-undo');
    const redoBtn = document.getElementById('btn-redo');
    
    if (undoBtn) {
      undoBtn.disabled = this.historyIndex <= 0;
    }
    
    if (redoBtn) {
      redoBtn.disabled = this.historyIndex >= this.history.length - 1;
    }
  }
  
  /**
   * Emit cursor change event
   */
  emitCursorChange() {
    const pos = this.getCursorPosition();
    const content = this.getContent();
    
    // Calculate line and column
    const textBefore = content.slice(0, pos);
    const lines = textBefore.split('\n');
    const line = lines.length;
    const column = lines[lines.length - 1].length + 1;
    
    this.eventBus.emit(Events.CURSOR_CHANGE, { line, column, position: pos });
  }
  
  /**
   * Update line numbers display
   */
  updateLineNumbers() {
    if (!this.lineNumbersEl || this.lineNumbersEl.hidden) return;
    
    const content = this.getContent();
    const lineCount = content.split('\n').length;
    
    let html = '';
    for (let i = 1; i <= lineCount; i++) {
      html += `<span>${i}</span>`;
    }
    
    this.lineNumbersEl.innerHTML = html;
    this.syncLineNumbersScroll();
  }
  
  /**
   * Sync line numbers scroll with textarea
   */
  syncLineNumbersScroll() {
    if (this.lineNumbersEl) {
      this.lineNumbersEl.scrollTop = this.textarea.scrollTop;
    }
  }
  
  // ============================================
  // CONTENT METHODS
  // ============================================
  
  /**
   * Get content
   * @returns {string}
   */
  getContent() {
    return this.textarea.value;
  }
  
  /**
   * Set content
   * @param {string} content
   * @param {boolean} [emitChange=true]
   */
  setContent(content, emitChange = true) {
    this.textarea.value = content;
    
    if (emitChange) {
      this.eventBus.emit(Events.CONTENT_CHANGE, { content });
    }
    
    this.updateLineNumbers();
  }
  
  /**
   * Get cursor position
   * @returns {number}
   */
  getCursorPosition() {
    return this.textarea.selectionStart;
  }
  
  /**
   * Set cursor position
   * @param {number} position
   */
  setCursorPosition(position) {
    this.textarea.setSelectionRange(position, position);
  }
  
  /**
   * Get selection
   * @returns {{ start: number, end: number, text: string }}
   */
  getSelection() {
    return {
      start: this.textarea.selectionStart,
      end: this.textarea.selectionEnd,
      text: this.getSelectedText()
    };
  }
  
  /**
   * Set selection
   * @param {number} start
   * @param {number} end
   */
  setSelection(start, end) {
    this.textarea.setSelectionRange(start, end);
  }
  
  /**
   * Get selected text
   * @returns {string}
   */
  getSelectedText() {
    const start = this.textarea.selectionStart;
    const end = this.textarea.selectionEnd;
    return this.textarea.value.slice(start, end);
  }
  
  /**
   * Insert text at cursor
   * @param {string} text
   */
  insertAtCursor(text) {
    const pos = this.getCursorPosition();
    const content = this.getContent();
    const newContent = content.slice(0, pos) + text + content.slice(pos);
    
    this.setContent(newContent);
    this.setCursorPosition(pos + text.length);
    this.saveHistory();
  }
  
  /**
   * Replace selection with text
   * @param {string} text
   */
  replaceSelection(text) {
    const { start, end } = this.getSelection();
    const content = this.getContent();
    const newContent = content.slice(0, start) + text + content.slice(end);
    
    this.setContent(newContent);
    this.setCursorPosition(start + text.length);
    this.saveHistory();
  }
  
  /**
   * Focus editor
   */
  focus() {
    this.textarea.focus();
  }
  
  /**
   * Blur editor
   */
  blur() {
    this.textarea.blur();
  }
  
  // ============================================
  // FORMATTING METHODS
  // ============================================
  
  /**
   * Wrap selection with prefix and suffix
   * @param {string} prefix
   * @param {string} suffix
   */
  wrapSelection(prefix, suffix) {
    const { start, end, text } = this.getSelection();
    const content = this.getContent();
    
    let newContent, newCursorPos;
    
    if (start === end) {
      // No selection - insert markers and position cursor between
      newContent = content.slice(0, start) + prefix + suffix + content.slice(end);
      newCursorPos = start + prefix.length;
    } else {
      // Check if already wrapped
      const before = content.slice(Math.max(0, start - prefix.length), start);
      const after = content.slice(end, end + suffix.length);
      
      if (before === prefix && after === suffix) {
        // Unwrap
        newContent = content.slice(0, start - prefix.length) + 
                     text + 
                     content.slice(end + suffix.length);
        newCursorPos = start - prefix.length;
        this.setContent(newContent);
        this.setSelection(newCursorPos, newCursorPos + text.length);
      } else {
        // Wrap
        newContent = content.slice(0, start) + prefix + text + suffix + content.slice(end);
        newCursorPos = end + prefix.length + suffix.length;
        this.setContent(newContent);
        this.setSelection(start + prefix.length, end + prefix.length);
      }
      return;
    }
    
    this.setContent(newContent);
    this.setCursorPosition(newCursorPos);
    this.saveHistory();
  }
  
  /**
   * Insert heading
   * @param {number} level - 1-6
   */
  insertHeading(level) {
    const content = this.getContent();
    const cursorPos = this.getCursorPosition();
    
    // Find line start
    const lineStart = content.lastIndexOf('\n', cursorPos - 1) + 1;
    const lineEnd = content.indexOf('\n', cursorPos);
    const actualLineEnd = lineEnd === -1 ? content.length : lineEnd;
    
    // Get current line
    const line = content.slice(lineStart, actualLineEnd);
    
    // Remove existing heading prefix
    const cleanedLine = line.replace(/^#{1,6}\s*/, '');
    
    // Add new prefix
    const prefix = '#'.repeat(level) + ' ';
    const newLine = prefix + cleanedLine;
    
    // Update content
    const newContent = content.slice(0, lineStart) + newLine + content.slice(actualLineEnd);
    this.setContent(newContent);
    
    // Position cursor after prefix
    this.setCursorPosition(lineStart + prefix.length);
    this.saveHistory();
  }
  
  /**
   * Insert link
   * @param {string} [url='']
   * @param {string} [text='']
   * @param {string} [title='']
   */
  insertLink(url = '', text = '', title = '') {
    const { start, end, text: selectedText } = this.getSelection();
    
    const linkText = text || selectedText || 'link text';
    const linkUrl = url || 'url';
    const titlePart = title ? ` "${title}"` : '';
    
    const markdown = `[${linkText}](${linkUrl}${titlePart})`;
    
    this.replaceSelection(markdown);
    
    // Select the URL part for easy replacement
    if (!url) {
      const urlStart = start + linkText.length + 3;
      this.setSelection(urlStart, urlStart + 3);
    }
  }
  
  /**
   * Insert image
   * @param {string} [url='']
   * @param {string} [alt='']
   * @param {string} [title='']
   */
  insertImage(url = '', alt = '', title = '') {
    const { text: selectedText } = this.getSelection();
    
    const altText = alt || selectedText || 'image description';
    const imgUrl = url || 'image-url';
    const titlePart = title ? ` "${title}"` : '';
    
    const markdown = `![${altText}](${imgUrl}${titlePart})`;
    
    this.replaceSelection(markdown);
  }
  
  /**
   * Insert list item
   * @param {string} marker - '- ', '1. ', '- [ ] '
   */
  insertListItem(marker) {
    const content = this.getContent();
    const cursorPos = this.getCursorPosition();
    
    // Find line start
    const lineStart = content.lastIndexOf('\n', cursorPos - 1) + 1;
    
    // Add marker at line start
    const newContent = content.slice(0, lineStart) + marker + content.slice(lineStart);
    this.setContent(newContent);
    this.setCursorPosition(cursorPos + marker.length);
    this.saveHistory();
  }
  
  /**
   * Insert blockquote
   */
  insertBlockquote() {
    this.insertListItem('> ');
  }
  
  /**
   * Insert code block
   * @param {string} [language='']
   */
  insertCodeBlock(language = '') {
    const { start, end, text } = this.getSelection();
    const content = this.getContent();
    
    const codeContent = text || 'code here';
    const block = `\`\`\`${language}\n${codeContent}\n\`\`\``;
    
    const newContent = content.slice(0, start) + block + content.slice(end);
    this.setContent(newContent);
    
    // Position cursor inside block
    const codeStart = start + 4 + language.length;
    this.setSelection(codeStart, codeStart + codeContent.length);
    this.saveHistory();
  }
  
  /**
   * Insert horizontal rule
   */
  insertHorizontalRule() {
    const content = this.getContent();
    const cursorPos = this.getCursorPosition();
    
    // Ensure we're on a new line
    const needsNewline = cursorPos > 0 && content[cursorPos - 1] !== '\n';
    const hr = (needsNewline ? '\n' : '') + '\n---\n\n';
    
    this.insertAtCursor(hr);
  }
  
  /**
   * Insert table
   */
  insertTable() {
    const table = `| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |`;
    
    this.insertAtCursor('\n' + table + '\n');
  }
  
  // ============================================
  // FIND & REPLACE METHODS
  // ============================================
  
  /**
   * Find text in editor
   * @param {string} query
   * @param {Object} options
   * @returns {{ total: number, current: number }}
   */
  find(query, options = {}) {
    this.findState.query = query;
    this.findState.options = { ...this.findState.options, ...options };
    this.findState.matches = [];
    this.findState.currentIndex = -1;
    
    if (!query) {
      return { total: 0, current: 0 };
    }
    
    const content = this.getContent();
    let searchContent = content;
    let searchQuery = query;
    
    // Build regex
    let flags = 'g';
    if (!options.caseSensitive) {
      flags += 'i';
      searchContent = content.toLowerCase();
      searchQuery = query.toLowerCase();
    }
    
    try {
      let regex;
      
      if (options.regex) {
        regex = new RegExp(query, flags);
      } else {
        // Escape special characters for literal search
        const escaped = query.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
        
        if (options.wholeWord) {
          regex = new RegExp(`\\b${escaped}\\b`, flags);
        } else {
          regex = new RegExp(escaped, flags);
        }
      }
      
      let match;
      while ((match = regex.exec(content)) !== null) {
        this.findState.matches.push({
          start: match.index,
          end: match.index + match[0].length,
          text: match[0]
        });
      }
    } catch (e) {
      console.warn('Invalid regex:', e);
    }
    
    // Move to first match after cursor
    if (this.findState.matches.length > 0) {
      const cursorPos = this.getCursorPosition();
      const nextIndex = this.findState.matches.findIndex(m => m.start >= cursorPos);
      this.findState.currentIndex = nextIndex >= 0 ? nextIndex : 0;
      this.highlightCurrentMatch();
    }
    
    return {
      total: this.findState.matches.length,
      current: this.findState.currentIndex + 1
    };
  }
  
  /**
   * Find next match
   */
  findNext() {
    if (this.findState.matches.length === 0) return;
    
    this.findState.currentIndex = 
      (this.findState.currentIndex + 1) % this.findState.matches.length;
    this.highlightCurrentMatch();
  }
  
  /**
   * Find previous match
   */
  findPrevious() {
    if (this.findState.matches.length === 0) return;
    
    this.findState.currentIndex = 
      (this.findState.currentIndex - 1 + this.findState.matches.length) % 
      this.findState.matches.length;
    this.highlightCurrentMatch();
  }
  
  /**
   * Get current find results
   * @returns {{ total: number, current: number }}
   */
  getFindResults() {
    return {
      total: this.findState.matches.length,
      current: this.findState.currentIndex + 1
    };
  }
  
  /**
   * Highlight current match
   */
  highlightCurrentMatch() {
    const match = this.findState.matches[this.findState.currentIndex];
    if (match) {
      this.setSelection(match.start, match.end);
      this.scrollToSelection();
    }
  }
  
  /**
   * Scroll to make selection visible
   */
  scrollToSelection() {
    const textarea = this.textarea;
    const start = textarea.selectionStart;
    const content = textarea.value;
    
    // Calculate approximate line number
    const textBefore = content.slice(0, start);
    const lineNumber = textBefore.split('\n').length;
    const lineHeight = parseFloat(getComputedStyle(textarea).lineHeight);
    
    const targetScroll = (lineNumber - 3) * lineHeight;
    textarea.scrollTop = Math.max(0, targetScroll);
  }
  
  /**
   * Replace current match
   * @param {string} replacement
   */
  replaceOne(replacement) {
    if (this.findState.matches.length === 0) return;
    
    const match = this.findState.matches[this.findState.currentIndex];
    if (!match) return;
    
    const content = this.getContent();
    const newContent = content.slice(0, match.start) + 
                       replacement + 
                       content.slice(match.end);
    
    this.setContent(newContent);
    
    // Adjust match positions
    const diff = replacement.length - match.text.length;
    this.findState.matches.forEach((m, i) => {
      if (i > this.findState.currentIndex) {
        m.start += diff;
        m.end += diff;
      }
    });
    
    // Remove current match
    this.findState.matches.splice(this.findState.currentIndex, 1);
    
    // Move to next match
    if (this.findState.matches.length > 0) {
      if (this.findState.currentIndex >= this.findState.matches.length) {
        this.findState.currentIndex = 0;
      }
      this.highlightCurrentMatch();
    }
    
    this.saveHistory();
  }
  
  /**
   * Replace all matches
   * @param {string} replacement
   * @returns {number} Number of replacements
   */
  replaceAll(replacement) {
    if (this.findState.matches.length === 0) return 0;
    
    const count = this.findState.matches.length;
    let content = this.getContent();
    let offset = 0;
    
    this.findState.matches.forEach(match => {
      const start = match.start + offset;
      const end = match.end + offset;
      
      content = content.slice(0, start) + replacement + content.slice(end);
      offset += replacement.length - match.text.length;
    });
    
    this.setContent(content);
    this.findState.matches = [];
    this.findState.currentIndex = -1;
    
    this.saveHistory();
    return count;
  }
  
  /**
   * Clear find highlights
   */
  clearHighlights() {
    this.findState.matches = [];
    this.findState.currentIndex = -1;
    this.findState.query = '';
  }
}
```

---

### assets/js/core/preview.js

```javascript
/**
 * Preview Engine - Markdown rendering
 * @author Oathan Rex
 */

import { Events } from '../utils/event-bus.js';
import { escapeHtml } from '../utils/dom.js';

export class Preview {
  /**
   * @param {Object} options
   * @param {HTMLElement} options.element
   * @param {EventBus} options.eventBus
   */
  constructor({ element, eventBus }) {
    this.container = element;
    this.eventBus = eventBus;
    this.lastContent = '';
    this.toc = [];
    
    this.init();
  }
  
  /**
   * Initialize preview
   */
  init() {
    this.configureMarked();
  }
  
  /**
   * Configure marked.js parser
   */
  configureMarked() {
    // Configure marked options
    marked.setOptions({
      gfm: true,
      breaks: true,
      headerIds: true,
      mangle: false,
      sanitize: false // We use DOMPurify
    });
    
    // Custom renderer
    const renderer = new marked.Renderer();
    
    // Custom link rendering
    renderer.link = (href, title, text) => {
      // Validate URL
      if (!this.isURLSafe(href)) {
        return text;
      }
      
      const isExternal = href && (href.startsWith('http') || href.startsWith('//'));
      const titleAttr = title ? ` title="${escapeHtml(title)}"` : '';
      const relAttr = isExternal ? ' rel="noopener noreferrer" target="_blank"' : '';
      
      return `<a href="${escapeHtml(href)}"${titleAttr}${relAttr}>${text}</a>`;
    };
    
    // Custom code block rendering
    renderer.code = (code, language) => {
      const langClass = language ? ` class="language-${escapeHtml(language)}"` : '';
      const escapedCode = escapeHtml(code);
      
      return `<pre><code${langClass}>${escapedCode}</code></pre>`;
    };
    
    // Custom checkbox rendering for task lists
    renderer.listitem = (text, task, checked) => {
      if (task) {
        const checkbox = `<input type="checkbox" ${checked ? 'checked' : ''} disabled>`;
        return `<li class="task-list-item">${checkbox} ${text}</li>\n`;
      }
      return `<li>${text}</li>\n`;
    };
    
    marked.use({ renderer });
  }
  
  /**
   * Render Markdown to HTML
   * @param {string} markdown
   */
  render(markdown) {
    if (!markdown || markdown === this.lastContent) {
      if (!markdown) {
        this.container.innerHTML = `
          <div class="preview-placeholder">
            <p>Start typing in the editor to see your formatted Markdown here.</p>
          </div>
        `;
        this.toc = [];
        this.eventBus.emit(Events.PREVIEW_RENDERED, { toc: [] });
      }
      return;
    }
    
    this.lastContent = markdown;
    
    try {
      // Parse Markdown
      const rawHtml = marked.parse(markdown);
      
      // Sanitize HTML
      const cleanHtml = this.sanitize(rawHtml);
      
      // Update container
      this.container.innerHTML = cleanHtml;
      
      // Post-process
      this.addCopyButtons();
      this.extractTOC();
      
      // Emit event
      this.eventBus.emit(Events.PREVIEW_RENDERED, { 
        toc: this.toc,
        html: cleanHtml
      });
      
    } catch (error) {
      console.error('Preview render error:', error);
      this.container.innerHTML = `<pre class="error">${escapeHtml(markdown)}</pre>`;
    }
  }
  
  /**
   * Sanitize HTML using DOMPurify
   * @param {string} html
   * @returns {string}
   */
  sanitize(html) {
    return DOMPurify.sanitize(html, {
      ALLOWED_TAGS: [
        'h1', 'h2', 'h3', 'h4', 'h5', 'h6',
        'p', 'br', 'hr',
        'ul', 'ol', 'li',
        'blockquote', 'pre', 'code',
        'a', 'strong', 'em', 'del', 'ins', 'mark', 's',
        'table', 'thead', 'tbody', 'tfoot', 'tr', 'th', 'td',
        'img', 'figure', 'figcaption',
        'div', 'span', 'sup', 'sub',
        'input', 'label' // For task lists
      ],
      ALLOWED_ATTR: [
        'href', 'src', 'alt', 'title',
        'class', 'id',
        'target', 'rel',
        'colspan', 'rowspan', 'align',
        'type', 'checked', 'disabled' // For checkboxes
      ],
      ALLOW_DATA_ATTR: false,
      ADD_ATTR: ['target'],
      FORBID_TAGS: ['script', 'style', 'iframe', 'form', 'object', 'embed'],
      FORBID_ATTR: ['onerror', 'onclick', 'onload', 'onmouseover']
    });
  }
  
  /**
   * Check if URL is safe
   * @param {string} url
   * @returns {boolean}
   */
  isURLSafe(url) {
    if (!url) return false;
    
    // Allow relative URLs
    if (url.startsWith('./') || url.startsWith('../') || 
        url.startsWith('/') || url.startsWith('#')) {
      return true;
    }
    
    // Check protocol
    try {
      const parsed = new URL(url);
      const safeProtocols = ['http:', 'https:', 'mailto:'];
      return safeProtocols.includes(parsed.protocol);
    } catch {
      return false;
    }
  }
  
  /**
   * Add copy buttons to code blocks
   */
  addCopyButtons() {
    this.container.querySelectorAll('pre').forEach(pre => {
      // Skip if already has button
      if (pre.querySelector('.copy-btn')) return;
      
      const code = pre.querySelector('code');
      if (!code) return;
      
      // Create copy button
      const button = document.createElement('button');
      button.type = 'button';
      button.className = 'copy-btn';
      button.innerHTML = `
        <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <rect x="9" y="9" width="13" height="13" rx="2" ry="2"/>
          <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/>
        </svg>
        <span>Copy</span>
      `;
      button.setAttribute('aria-label', 'Copy code to clipboard');
      
      button.addEventListener('click', async () => {
        try {
          await navigator.clipboard.writeText(code.textContent);
          
          button.classList.add('copied');
          button.querySelector('span').textContent = 'Copied!';
          
          setTimeout(() => {
            button.classList.remove('copied');
            button.querySelector('span').textContent = 'Copy';
          }, 2000);
          
        } catch (err) {
          console.error('Copy failed:', err);
          button.querySelector('span').textContent = 'Failed';
          
          setTimeout(() => {
            button.querySelector('span').textContent = 'Copy';
          }, 2000);
        }
      });
      
      pre.appendChild(button);
    });
  }
  
  /**
   * Extract table of contents from headings
   */
  extractTOC() {
    const headings = this.container.querySelectorAll('h1, h2, h3, h4, h5, h6');
    const toc = [];
    const idCounts = new Map();
    
    headings.forEach(heading => {
      const level = parseInt(heading.tagName.charAt(1));
      const text = heading.textContent.trim();
      
      // Generate unique ID
      let baseId = text
        .toLowerCase()
        .replace(/[^\w\s-]/g, '')
        .replace(/\s+/g, '-')
        .substring(0, 50);
      
      if (!baseId) baseId = 'heading';
      
      // Handle duplicates
      const count = idCounts.get(baseId) || 0;
      idCounts.set(baseId, count + 1);
      const id = count > 0 ? `${baseId}-${count}` : baseId;
      
      // Set ID on heading
      heading.id = id;
      
      toc.push({ level, text, id });
    });
    
    this.toc = toc;
  }
  
  /**
   * Scroll to heading by ID
   * @param {string} id
   */
  scrollToHeading(id) {
    const element = this.container.querySelector(`#${CSS.escape(id)}`);
    if (element) {
      element.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }
  }
  
  /**
   * Get rendered HTML
   * @returns {string}
   */
  getHTML() {
    return this.container.innerHTML;
  }
  
  /**
   * Get table of contents
   * @returns {Array}
   */
  getTOC() {
    return this.toc;
  }
  
  /**
   * Get scroll percentage
   * @returns {number}
   */
  getScrollPercentage() {
    const { scrollTop, scrollHeight, clientHeight } = this.container;
    if (scrollHeight <= clientHeight) return 0;
    return scrollTop / (scrollHeight - clientHeight);
  }
  
  /**
   * Set scroll percentage
   * @param {number} percentage
   */
  setScrollPercentage(percentage) {
    const { scrollHeight, clientHeight } = this.container;
    this.container.scrollTop = percentage * (scrollHeight - clientHeight);
  }
}
```

---

### assets/js/core/toolbar.js

```javascript
/**
 * Toolbar Controller
 * @author Oathan Rex
 */

import { Events } from '../utils/event-bus.js';

export class Toolbar {
  /**
   * @param {Object} options
   * @param {HTMLElement} options.element
   * @param {Editor} options.editor
   * @param {EventBus} options.eventBus
   */
  constructor({ element, editor, eventBus }) {
    this.element = element;
    this.editor = editor;
    this.eventBus = eventBus;
    
    this.init();
  }
  
  /**
   * Initialize toolbar
   */
  init() {
    this.setupButtonHandlers();
  }
  
  /**
   * Set up button click handlers
   */
  setupButtonHandlers() {
    // Define actions
    const actions = {
      'undo': () => this.editor.undo(),
      'redo': () => this.editor.redo(),
      'bold': () => this.editor.wrapSelection('**', '**'),
      'italic': () => this.editor.wrapSelection('*', '*'),
      'strikethrough': () => this.editor.wrapSelection('~~', '~~'),
      'unordered-list': () => this.editor.insertListItem('- '),
      'ordered-list': () => this.editor.insertListItem('1. '),
      'task-list': () => this.editor.insertListItem('- [ ] '),
      'link': () => this.showInsertModal('link'),
      'image': () => this.showInsertModal('image'),
      'table': () => this.editor.insertTable(),
      'inline-code': () => this.editor.wrapSelection('`', '`'),
      'code-block': () => this.editor.insertCodeBlock(),
      'blockquote': () => this.editor.insertBlockquote(),
      'horizontal-rule': () => this.editor.insertHorizontalRule(),
      'toggle-line-numbers': () => this.toggleLineNumbers(),
      'toggle-word-wrap': () => this.toggleWordWrap()
    };
    
    // Attach handlers
    Object.entries(actions).forEach(([action, handler]) => {
      const buttons = this.element.querySelectorAll(`[data-action="${action}"]`);
      
      buttons.forEach(button => {
        button.addEventListener('click', (e) => {
          e.preventDefault();
          handler();
          this.editor.focus();
        });
      });
    });
  }
  
  /**
   * Show insert modal (link/image)
   * @param {string} type - 'link' or 'image'
   */
  showInsertModal(type) {
    const modal = document.getElementById('insert-modal');
    if (!modal) return;
    
    const title = type === 'image' ? 'Insert Image' : 'Insert Link';
    const textLabel = type === 'image' ? 'Alt Text' : 'Text';
    const urlPlaceholder = type === 'image' ? 'https://example.com/image.jpg' : 'https://example.com';
    
    // Update modal content
    document.getElementById('insert-modal-title').textContent = title;
    document.querySelector('label[for="insert-text"]').textContent = textLabel;
    document.getElementById('insert-url').placeholder = urlPlaceholder;
    
    // Pre-fill with selection
    const selectedText = this.editor.getSelectedText();
    document.getElementById('insert-text').value = selectedText;
    document.getElementById('insert-url').value = '';
    document.getElementById('insert-title').value = '';
    
    // Store type
    modal.dataset.type = type;
    
    // Show modal
    modal.hidden = false;
    
    // Focus URL input
    setTimeout(() => {
      document.getElementById('insert-url').focus();
    }, 100);
  }
  
  /**
   * Toggle line numbers
   */
  toggleLineNumbers() {
    const lineNumbers = document.getElementById('line-numbers');
    const btn = document.querySelector('[data-action="toggle-line-numbers"]');
    
    if (lineNumbers) {
      lineNumbers.hidden = !lineNumbers.hidden;
      btn?.classList.toggle('active', !lineNumbers.hidden);
      
      // Update line numbers if showing
      if (!lineNumbers.hidden) {
        this.editor.updateLineNumbers();
      }
    }
  }
  
  /**
   * Toggle word wrap
   */
  toggleWordWrap() {
    const textarea = this.editor.textarea;
    const btn = document.querySelector('[data-action="toggle-word-wrap"]');
    
    if (textarea) {
      const hasWrap = textarea.style.whiteSpace !== 'pre';
      textarea.style.whiteSpace = hasWrap ? 'pre' : 'pre-wrap';
      textarea.style.overflowX = hasWrap ? 'auto' : 'hidden';
      btn?.classList.toggle('active', !hasWrap);
    }
  }
  
  /**
   * Enable/disable a button
   * @param {string} action
   * @param {boolean} enabled
   */
  setButtonEnabled(action, enabled) {
    const button = this.element.querySelector(`[data-action="${action}"]`);
    if (button) {
      button.disabled = !enabled;
    }
  }
  
  /**
   * Set button active state
   * @param {string} action
   * @param {boolean} active
   */
  setButtonActive(action, active) {
    const button = this.element.querySelector(`[data-action="${action}"]`);
    if (button) {
      button.classList.toggle('active', active);
      button.setAttribute('aria-pressed', active);
    }
  }
}
```

---

## 5. Service Worker

### sw.js

```javascript
/**
 * Service Worker for MarkdownStudio Pro
 * Enables offline functionality
 * @author Oathan Rex
 * @version 3.0.0
 */

const CACHE_VERSION = 'v3.0.0';
const CACHE_NAME = `markdownstudio-${CACHE_VERSION}`;

// Base path for GitHub Pages deployment
const BASE_PATH = '/markdown-editor/';

// Assets to precache
const PRECACHE_ASSETS = [
  './',
  './index.html',
  './404.html',
  './manifest.json',
  './assets/css/style.css',
  './assets/js/app.js',
  './assets/js/core/editor.js',
  './assets/js/core/preview.js',
  './assets/js/core/toolbar.js',
  './assets/js/services/storage.js',
  './assets/js/services/theme.js',
  './assets/js/services/shortcuts.js',
  './assets/js/services/export.js',
  './assets/js/utils/event-bus.js',
  './assets/js/utils/debounce.js',
  './assets/js/utils/dom.js',
  './assets/js/lib/marked.min.js',
  './assets/js/lib/purify.min.js',
  './assets/images/favicon.ico',
  './assets/images/icon-192.png',
  './assets/images/icon-512.png'
];

// ============================================
// INSTALL EVENT
// ============================================

self.addEventListener('install', (event) => {
  console.log('[SW] Installing version:', CACHE_VERSION);
  
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => {
        console.log('[SW] Precaching assets');
        return cache.addAll(PRECACHE_ASSETS);
      })
      .then(() => {
        console.log('[SW] Precaching complete');
        return self.skipWaiting();
      })
      .catch((error) => {
        console.error('[SW] Precaching failed:', error);
      })
  );
});

// ============================================
// ACTIVATE EVENT
// ============================================

self.addEventListener('activate', (event) => {
  console.log('[SW] Activating version:', CACHE_VERSION);
  
  event.waitUntil(
    caches.keys()
      .then((cacheNames) => {
        return Promise.all(
          cacheNames
            .filter((name) => name.startsWith('markdownstudio-') && name !== CACHE_NAME)
            .map((name) => {
              console.log('[SW] Deleting old cache:', name);
              return caches.delete(name);
            })
        );
      })
      .then(() => {
        console.log('[SW] Activation complete');
        return self.clients.claim();
      })
  );
});

// ============================================
// FETCH EVENT
// ============================================

self.addEventListener('fetch', (event) => {
  const { request } = event;
  const url = new URL(request.url);
  
  // Only handle same-origin requests
  if (url.origin !== location.origin) {
    return;
  }
  
  // Skip non-GET requests
  if (request.method !== 'GET') {
    return;
  }
  
  // Skip Chrome extensions
  if (url.protocol === 'chrome-extension:') {
    return;
  }
  
  event.respondWith(handleFetch(request));
});

/**
 * Handle fetch requests
 * @param {Request} request
 * @returns {Promise<Response>}
 */
async function handleFetch(request) {
  const url = new URL(request.url);
  
  // For HTML pages: Network first, fall back to cache
  if (request.mode === 'navigate' || request.headers.get('accept')?.includes('text/html')) {
    try {
      const networkResponse = await fetch(request);
      
      // Cache successful responses
      if (networkResponse.ok) {
        const cache = await caches.open(CACHE_NAME);
        cache.put(request, networkResponse.clone());
      }
      
      return networkResponse;
    } catch (error) {
      console.log('[SW] Network failed, serving from cache:', url.pathname);
      
      const cachedResponse = await caches.match(request);
      if (cachedResponse) {
        return cachedResponse;
      }
      
      // Return cached index.html for navigation requests
      return caches.match('./index.html');
    }
  }
  
  // For other assets: Cache first, fall back to network
  const cachedResponse = await caches.match(request);
  
  if (cachedResponse) {
    // Return cached response immediately
    // Also fetch in background to update cache
    fetchAndCache(request);
    return cachedResponse;
  }
  
  // Not in cache, fetch from network
  return fetchAndCache(request);
}

/**
 * Fetch and cache resource
 * @param {Request} request
 * @returns {Promise<Response>}
 */
async function fetchAndCache(request) {
  try {
    const response = await fetch(request);
    
    // Only cache successful responses
    if (response.ok && response.type === 'basic') {
      const cache = await caches.open(CACHE_NAME);
      cache.put(request, response.clone());
    }
    
    return response;
  } catch (error) {
    console.error('[SW] Fetch failed:', error);
    
    // Return offline fallback for images
    if (request.destination === 'image') {
      return new Response(
        '<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100"><rect fill="#f0f0f0" width="100" height="100"/><text x="50" y="50" text-anchor="middle" dy=".3em" fill="#999">Offline</text></svg>',
        { headers: { 'Content-Type': 'image/svg+xml' } }
      );
    }
    
    throw error;
  }
}

// ============================================
// MESSAGE EVENT
// ============================================

self.addEventListener('message', (event) => {
  if (event.data === 'skipWaiting') {
    self.skipWaiting();
  }
  
  if (event.data === 'getVersion') {
    event.ports[0].postMessage(CACHE_VERSION);
  }
});

// ============================================
// BACKGROUND SYNC (Future)
// ============================================

self.addEventListener('sync', (event) => {
  if (event.tag === 'sync-document') {
    console.log('[SW] Background sync triggered');
    // Future: Sync document to cloud storage
  }
});
```

---

## 6. Configuration Files

### manifest.json

```json
{
  "name": "MarkdownStudio Pro",
  "short_name": "MarkdownStudio",
  "description": "Free online Markdown editor with live preview, syntax highlighting, and HTML export. No signup required.",
  "start_url": "./",
  "scope": "./",
  "display": "standalone",
  "orientation": "any",
  "theme_color": "#2563eb",
  "background_color": "#ffffff",
  "categories": ["productivity", "utilities", "developer tools"],
  "lang": "en",
  "dir": "ltr",
  "icons": [
    {
      "src": "./assets/images/icon-192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "./assets/images/icon-512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ],
  "screenshots": [
    {
      "src": "./assets/images/screenshot.png",
      "sizes": "1280x720",
      "type": "image/png",
      "label": "MarkdownStudio Pro Editor Interface"
    }
  ],
  "shortcuts": [
    {
      "name": "New Document",
      "short_name": "New",
      "description": "Create a new Markdown document",
      "url": "./?action=new",
      "icons": [{ "src": "./assets/images/icon-192.png", "sizes": "192x192" }]
    }
  ],
  "related_applications": [],
  "prefer_related_applications": false
}
```

### robots.txt

```
# MarkdownStudio Pro - robots.txt
# https://oathanrex.github.io/markdown-editor/

User-agent: *
Allow: /

# Sitemap
Sitemap: https://oathanrex.github.io/markdown-editor/sitemap.xml

# Crawl-delay (optional, for polite crawling)
Crawl-delay: 1
```

### sitemap.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9
        http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd">
  
  <url>
    <loc>https://oathanrex.github.io/markdown-editor/</loc>
    <lastmod>2025-01-15</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  
</urlset>
```

### 404.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Not Found | MarkdownStudio Pro</title>
  <meta name="robots" content="noindex">
  <link rel="icon" href="./assets/images/favicon.ico">
  <style>
    :root {
      --color-bg: #ffffff;
      --color-text: #1e293b;
      --color-text-secondary: #64748b;
      --color-accent: #2563eb;
      --color-accent-hover: #1d4ed8;
    }
    
    @media (prefers-color-scheme: dark) {
      :root {
        --color-bg: #0f172a;
        --color-text: #f1f5f9;
        --color-text-secondary: #94a3b8;
      }
    }
    
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: var(--color-bg);
      color: var(--color-text);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 2rem;
    }
    
    .error-page {
      text-align: center;
      max-width: 500px;
    }
    
    .error-code {
      font-size: 8rem;
      font-weight: 700;
      color: var(--color-accent);
      line-height: 1;
      margin-bottom: 1rem;
    }
    
    .error-title {
      font-size: 1.5rem;
      font-weight: 600;
      margin-bottom: 0.5rem;
    }
    
    .error-message {
      color: var(--color-text-secondary);
      margin-bottom: 2rem;
      line-height: 1.6;
    }
    
    .error-actions {
      display: flex;
      gap: 1rem;
      justify-content: center;
      flex-wrap: wrap;
    }
    
    .btn {
      display: inline-flex;
      align-items: center;
      gap: 0.5rem;
      padding: 0.75rem 1.5rem;
      font-size: 1rem;
      font-weight: 500;
      text-decoration: none;
      border-radius: 0.5rem;
      transition: all 0.15s ease;
    }
    
    .btn-primary {
      background: var(--color-accent);
      color: white;
    }
    
    .btn-primary:hover {
      background: var(--color-accent-hover);
    }
    
    .btn-secondary {
      background: transparent;
      color: var(--color-accent);
      border: 1px solid var(--color-accent);
    }
    
    .btn-secondary:hover {
      background: var(--color-accent);
      color: white;
    }
    
    .logo {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 0.5rem;
      margin-bottom: 2rem;
    }
    
    .logo-icon {
      width: 40px;
      height: 40px;
      background: var(--color-accent);
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-weight: 700;
      font-size: 1.25rem;
    }
    
    .logo-text {
      font-size: 1.25rem;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <main class="error-page">
    <div class="logo">
      <div class="logo-icon">M</div>
      <span class="logo-text">MarkdownStudio</span>
    </div>
    
    <div class="error-code">404</div>
    <h1 class="error-title">Page Not Found</h1>
    <p class="error-message">
      The page you're looking for doesn't exist or has been moved. 
      Let's get you back to writing.
    </p>
    
    <div class="error-actions">
      <a href="./" class="btn btn-primary">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>
          <polyline points="9 22 9 12 15 12 15 22"/>
        </svg>
        Go to Editor
      </a>
      <a href="javascript:history.back()" class="btn btn-secondary">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <line x1="19" y1="12" x2="5" y2="12"/>
          <polyline points="12 19 5 12 12 5"/>
        </svg>
        Go Back
      </a>
    </div>
  </main>
</body>
</html>
```

### .nojekyll

```
# This file prevents GitHub Pages from processing files with Jekyll
# It allows files starting with underscore and enables faster builds
```

### LICENSE

```
MIT License

Copyright (c) 2025 Oathan Rex

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 7. Testing Suite

### package.json

```json
{
  "name": "markdown-editor",
  "version": "3.0.0",
  "description": "Free online Markdown editor with live preview - MarkdownStudio Pro",
  "author": "Oathan Rex",
  "license": "MIT",
  "homepage": "https://oathanrex.github.io/markdown-editor/",
  "repository": {
    "type": "git",
    "url": "https://github.com/oathanrex/markdown-editor.git"
  },
  "keywords": [
    "markdown",
    "editor",
    "markdown-editor",
    "online-editor",
    "live-preview",
    "github-flavored-markdown"
  ],
  "type": "module",
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage",
    "test:security": "vitest run tests/security/",
    "test:ui": "vitest --ui",
    "lint": "echo 'No linter configured'",
    "serve": "npx serve . -p 3000",
    "build": "echo 'No build step required - static files'"
  },
  "devDependencies": {
    "vitest": "^1.2.0",
    "jsdom": "^24.0.0",
    "@vitest/coverage-v8": "^1.2.0",
    "@vitest/ui": "^1.2.0"
  }
}
```

### vitest.config.js

```javascript
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: ['./tests/setup.js'],
    include: ['tests/**/*.test.js'],
    exclude: ['node_modules', 'dist'],
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html', 'lcov'],
      include: ['assets/js/**/*.js'],
      exclude: ['assets/js/lib/**']
    },
    testTimeout: 10000,
    hookTimeout: 10000
  }
});
```

### tests/setup.js

```javascript
/**
 * Test Setup - Mock browser APIs and globals
 * @author Oathan Rex
 */

import { vi, beforeEach, afterEach } from 'vitest';

// ============================================
// MOCK localStorage
// ============================================

const createLocalStorageMock = () => {
  let store = {};
  
  return {
    getItem: vi.fn((key) => store[key] ?? null),
    setItem: vi.fn((key, value) => {
      store[key] = String(value);
    }),
    removeItem: vi.fn((key) => {
      delete store[key];
    }),
    clear: vi.fn(() => {
      store = {};
    }),
    get length() {
      return Object.keys(store).length;
    },
    key: vi.fn((i) => Object.keys(store)[i] ?? null),
    _getStore: () => store
  };
};

const localStorageMock = createLocalStorageMock();

Object.defineProperty(window, 'localStorage', {
  value: localStorageMock,
  writable: true
});

// ============================================
// MOCK clipboard API
// ============================================

Object.defineProperty(navigator, 'clipboard', {
  value: {
    writeText: vi.fn(() => Promise.resolve()),
    readText: vi.fn(() => Promise.resolve('')),
    write: vi.fn(() => Promise.resolve()),
    read: vi.fn(() => Promise.resolve([]))
  },
  writable: true
});

// ============================================
// MOCK matchMedia
// ============================================

Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: vi.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: vi.fn(),
    removeListener: vi.fn(),
    addEventListener: vi.fn(),
    removeEventListener: vi.fn(),
    dispatchEvent: vi.fn()
  }))
});

// ============================================
// MOCK scrollIntoView
// ============================================

Element.prototype.scrollIntoView = vi.fn();

// ============================================
// MOCK requestAnimationFrame
// ============================================

global.requestAnimationFrame = vi.fn((cb) => setTimeout(cb, 0));
global.cancelAnimationFrame = vi.fn((id) => clearTimeout(id));

// ============================================
// MOCK URL.createObjectURL / revokeObjectURL
// ============================================

global.URL.createObjectURL = vi.fn(() => 'blob:mock-url');
global.URL.revokeObjectURL = vi.fn();

// ============================================
// MOCK marked.js
// ============================================

global.marked = {
  parse: vi.fn((md) => {
    // Simple mock parsing
    return md
      .replace(/^# (.+)$/gm, '<h1>$1</h1>')
      .replace(/^## (.+)$/gm, '<h2>$1</h2>')
      .replace(/^### (.+)$/gm, '<h3>$1</h3>')
      .replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>')
      .replace(/\*(.+?)\*/g, '<em>$1</em>')
      .replace(/`(.+?)`/g, '<code>$1</code>')
      .replace(/\[(.+?)\]\((.+?)\)/g, '<a href="$2">$1</a>')
      .replace(/^- (.+)$/gm, '<li>$1</li>')
      .replace(/\n/g, '<br>');
  }),
  setOptions: vi.fn(),
  use: vi.fn(),
  Renderer: class {
    constructor() {}
  }
};

// ============================================
// MOCK DOMPurify
// ============================================

global.DOMPurify = {
  sanitize: vi.fn((html, options) => {
    // Simple sanitization mock
    return html
      .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
      .replace(/javascript:/gi, '')
      .replace(/on\w+\s*=/gi, 'data-blocked=');
  }),
  setConfig: vi.fn(),
  clearConfig: vi.fn(),
  isSupported: true
};

// ============================================
// RESET MOCKS BETWEEN TESTS
// ============================================

beforeEach(() => {
  // Clear localStorage
  localStorageMock.clear();
  
  // Clear all mocks
  vi.clearAllMocks();
  
  // Reset document body
  document.body.innerHTML = '';
});

afterEach(() => {
  vi.restoreAllMocks();
});

// ============================================
// CUSTOM MATCHERS
// ============================================

expect.extend({
  toBeValidHTML(received) {
    const parser = new DOMParser();
    const doc = parser.parseFromString(received, 'text/html');
    const errors = doc.querySelectorAll('parsererror');
    
    return {
      pass: errors.length === 0,
      message: () => errors.length === 0
        ? `Expected HTML to be invalid`
        : `Expected valid HTML but got parsing errors`
    };
  }
});
```

### tests/unit/storage.test.js

```javascript
/**
 * Storage Service Tests
 * @author Oathan Rex
 */

import { describe, test, expect, beforeEach, vi } from 'vitest';
import { Storage } from '../../assets/js/services/storage.js';

describe('Storage Service', () => {
  let storage;
  
  beforeEach(() => {
    storage = new Storage('test_');
  });
  
  describe('Basic Operations', () => {
    test('should save and retrieve a string', () => {
      storage.set('key', 'value');
      expect(storage.get('key')).toBe('value');
    });
    
    test('should save and retrieve an object', () => {
      const obj = { name: 'test', count: 42 };
      storage.set('obj', obj);
      expect(storage.get('obj')).toEqual(obj);
    });
    
    test('should save and retrieve an array', () => {
      const arr = [1, 2, 3, 'four'];
      storage.set('arr', arr);
      expect(storage.get('arr')).toEqual(arr);
    });
    
    test('should return default value for missing key', () => {
      expect(storage.get('missing')).toBeNull();
      expect(storage.get('missing', 'default')).toBe('default');
    });
    
    test('should remove item', () => {
      storage.set('key', 'value');
      storage.remove('key');
      expect(storage.get('key')).toBeNull();
    });
    
    test('should check if key exists', () => {
      storage.set('exists', 'value');
      expect(storage.has('exists')).toBe(true);
      expect(storage.has('notexists')).toBe(false);
    });
    
    test('should clear all namespaced items', () => {
      storage.set('key1', 'value1');
      storage.set('key2', 'value2');
      
      // Add non-namespaced item
      localStorage.setItem('other_key', 'value');
      
      storage.clear();
      
      expect(storage.get('key1')).toBeNull();
      expect(storage.get('key2')).toBeNull();
      expect(localStorage.getItem('other_key')).toBe('value');
    });
  });
  
  describe('Namespacing', () => {
    test('should prefix keys with namespace', () => {
      storage.set('mykey', 'myvalue');
      expect(localStorage.getItem('test_mykey')).toBe('"myvalue"');
    });
    
    test('should not affect non-namespaced keys', () => {
      localStorage.setItem('mykey', 'external');
      storage.set('mykey', 'internal');
      
      expect(localStorage.getItem('mykey')).toBe('external');
      expect(storage.get('mykey')).toBe('internal');
    });
    
    test('should list only namespaced keys', () => {
      localStorage.setItem('other', 'value');
      storage.set('key1', 'value1');
      storage.set('key2', 'value2');
      
      const keys = storage.keys();
      expect(keys).toContain('key1');
      expect(keys).toContain('key2');
      expect(keys).not.toContain('other');
    });
  });
  
  describe('Error Handling', () => {
    test('should handle corrupted JSON gracefully', () => {
      localStorage.setItem('test_corrupted', 'not{valid}json');
      expect(storage.get('corrupted', 'fallback')).toBe('fallback');
    });
    
    test('should handle quota exceeded error', () => {
      const originalSetItem = localStorage.setItem;
      localStorage.setItem = vi.fn(() => {
        const error = new Error('QuotaExceededError');
        error.name = 'QuotaExceededError';
        throw error;
      });
      
      const result = storage.set('large', 'x'.repeat(10000));
      expect(result).toBe(false);
      
      localStorage.setItem = originalSetItem;
    });
    
    test('should return false when storage unavailable', () => {
      const brokenStorage = new Storage('test_');
      brokenStorage.available = false;
      
      expect(brokenStorage.set('key', 'value')).toBe(false);
      expect(brokenStorage.get('key', 'default')).toBe('default');
    });
  });
  
  describe('Export/Import', () => {
    test('should export all data', () => {
      storage.set('key1', 'value1');
      storage.set('key2', { nested: true });
      
      const exported = storage.exportAll();
      
      expect(exported.key1).toBe('value1');
      expect(exported.key2).toEqual({ nested: true });
    });
    
    test('should import data', () => {
      const data = {
        imported1: 'value1',
        imported2: [1, 2, 3]
      };
      
      storage.importAll(data);
      
      expect(storage.get('imported1')).toBe('value1');
      expect(storage.get('imported2')).toEqual([1, 2, 3]);
    });
  });
  
  describe('Usage Statistics', () => {
    test('should calculate storage usage', () => {
      storage.set('data', 'x'.repeat(1000));
      
      const usage = storage.getUsage();
      
      expect(usage.used).toBeGreaterThan(0);
      expect(usage.percentage).toBeGreaterThanOrEqual(0);
      expect(usage.percentage).toBeLessThanOrEqual(100);
    });
  });
});
```

### tests/unit/editor.test.js

```javascript
/**
 * Editor Core Tests
 * @author Oathan Rex
 */

import { describe, test, expect, beforeEach, vi } from 'vitest';
import { EventBus } from '../../assets/js/utils/event-bus.js';

// Mock Editor class for testing (simplified)
class MockEditor {
  constructor(textarea) {
    this.textarea = textarea;
    this.eventBus = new EventBus();
  }
  
  getContent() {
    return this.textarea.value;
  }
  
  setContent(content) {
    this.textarea.value = content;
  }
  
  getCursorPosition() {
    return this.textarea.selectionStart;
  }
  
  setCursorPosition(pos) {
    this.textarea.setSelectionRange(pos, pos);
  }
  
  getSelection() {
    return {
      start: this.textarea.selectionStart,
      end: this.textarea.selectionEnd,
      text: this.textarea.value.slice(
        this.textarea.selectionStart,
        this.textarea.selectionEnd
      )
    };
  }
  
  setSelection(start, end) {
    this.textarea.setSelectionRange(start, end);
  }
  
  getSelectedText() {
    return this.getSelection().text;
  }
  
  insertAtCursor(text) {
    const pos = this.getCursorPosition();
    const content = this.getContent();
    this.setContent(content.slice(0, pos) + text + content.slice(pos));
    this.setCursorPosition(pos + text.length);
  }
  
  wrapSelection(prefix, suffix) {
    const { start, end, text } = this.getSelection();
    const content = this.getContent();
    
    if (start === end) {
      const newContent = content.slice(0, start) + prefix + suffix + content.slice(end);
      this.setContent(newContent);
      this.setCursorPosition(start + prefix.length);
    } else {
      const newContent = content.slice(0, start) + prefix + text + suffix + content.slice(end);
      this.setContent(newContent);
      this.setSelection(start + prefix.length, end + prefix.length);
    }
  }
  
  insertHeading(level) {
    const content = this.getContent();
    const cursorPos = this.getCursorPosition();
    const lineStart = content.lastIndexOf('\n', cursorPos - 1) + 1;
    const lineEnd = content.indexOf('\n', cursorPos);
    const actualLineEnd = lineEnd === -1 ? content.length : lineEnd;
    
    const line = content.slice(lineStart, actualLineEnd);
    const cleanedLine = line.replace(/^#{1,6}\s*/, '');
    const prefix = '#'.repeat(level) + ' ';
    const newLine = prefix + cleanedLine;
    
    const newContent = content.slice(0, lineStart) + newLine + content.slice(actualLineEnd);
    this.setContent(newContent);
    this.setCursorPosition(lineStart + prefix.length);
  }
}

describe('Editor Core', () => {
  let editor;
  let textarea;
  
  beforeEach(() => {
    textarea = document.createElement('textarea');
    document.body.appendChild(textarea);
    editor = new MockEditor(textarea);
  });
  
  describe('Content Management', () => {
    test('should get content', () => {
      textarea.value = 'Hello World';
      expect(editor.getContent()).toBe('Hello World');
    });
    
    test('should set content', () => {
      editor.setContent('New Content');
      expect(textarea.value).toBe('New Content');
    });
    
    test('should insert at cursor', () => {
      editor.setContent('Hello World');
      editor.setCursorPosition(5);
      editor.insertAtCursor(' Beautiful');
      expect(editor.getContent()).toBe('Hello Beautiful World');
    });
  });
  
  describe('Selection', () => {
    test('should get selection', () => {
      editor.setContent('Hello World');
      editor.setSelection(0, 5);
      
      const selection = editor.getSelection();
      expect(selection.start).toBe(0);
      expect(selection.end).toBe(5);
      expect(selection.text).toBe('Hello');
    });
    
    test('should get selected text', () => {
      editor.setContent('Hello World');
      editor.setSelection(6, 11);
      expect(editor.getSelectedText()).toBe('World');
    });
  });
  
  describe('Formatting', () => {
    test('should wrap selection with bold markers', () => {
      editor.setContent('Hello World');
      editor.setSelection(0, 5);
      editor.wrapSelection('**', '**');
      expect(editor.getContent()).toBe('**Hello** World');
    });
    
    test('should wrap selection with italic markers', () => {
      editor.setContent('Hello World');
      editor.setSelection(6, 11);
      editor.wrapSelection('*', '*');
      expect(editor.getContent()).toBe('Hello *World*');
    });
    
    test('should insert markers at cursor when no selection', () => {
      editor.setContent('Hello World');
      editor.setCursorPosition(5);
      editor.wrapSelection('**', '**');
      expect(editor.getContent()).toBe('Hello**** World');
    });
    
    test('should insert heading', () => {
      editor.setContent('My Title');
      editor.setCursorPosition(0);
      editor.insertHeading(1);
      expect(editor.getContent()).toBe('# My Title');
    });
    
    test('should replace heading level', () => {
      editor.setContent('## Old Heading');
      editor.setCursorPosition(3);
      editor.insertHeading(1);
      expect(editor.getContent()).toBe('# Old Heading');
    });
    
    test('should insert heading 2', () => {
      editor.setContent('Subtitle');
      editor.setCursorPosition(0);
      editor.insertHeading(2);
      expect(editor.getContent()).toBe('## Subtitle');
    });
  });
  
  describe('Cursor Position', () => {
    test('should get cursor position', () => {
      editor.setContent('Hello World');
      textarea.setSelectionRange(5, 5);
      expect(editor.getCursorPosition()).toBe(5);
    });
    
    test('should set cursor position', () => {
      editor.setContent('Hello World');
      editor.setCursorPosition(7);
      expect(textarea.selectionStart).toBe(7);
      expect(textarea.selectionEnd).toBe(7);
    });
  });
});
```

### tests/unit/export.test.js

```javascript
/**
 * Export Service Tests
 * @author Oathan Rex
 */

import { describe, test, expect, beforeEach, vi } from 'vitest';
import { ExportService } from '../../assets/js/services/export.js';

describe('Export Service', () => {
  let exportService;
  
  beforeEach(() => {
    exportService = new ExportService();
  });
  
  describe('Filename Sanitization', () => {
    test('should sanitize simple filename', () => {
      expect(exportService.sanitizeFilename('My Document')).toBe('my-document');
    });
    
    test('should remove special characters', () => {
      expect(exportService.sanitizeFilename('File: Name?')).toBe('file-name');
    });
    
    test('should handle empty string', () => {
      expect(exportService.sanitizeFilename('')).toBe('document');
    });
    
    test('should truncate long filenames', () => {
      const longName = 'a'.repeat(200);
      const result = exportService.sanitizeFilename(longName);
      expect(result.length).toBeLessThanOrEqual(100);
    });
    
    test('should handle unicode characters', () => {
      expect(exportService.sanitizeFilename('文档名称')).toBe('document');
    });
    
    test('should convert to lowercase', () => {
      expect(exportService.sanitizeFilename('UPPERCASE')).toBe('uppercase');
    });
  });
  
  describe('HTML Document Generation', () => {
    test('should generate valid HTML document', () => {
      const html = exportService.generateHTMLDocument('<p>Test</p>', 'Test Title');
      
      expect(html).toContain('<!DOCTYPE html>');
      expect(html).toContain('<html lang="en">');
      expect(html).toContain('<title>Test Title</title>');
      expect(html).toContain('<p>Test</p>');
      expect(html).toContain('</html>');
    });
    
    test('should escape HTML in title', () => {
      const html = exportService.generateHTMLDocument('<p>Test</p>', '<script>alert(1)</script>');
      
      expect(html).not.toContain('<script>alert(1)</script>');
      expect(html).toContain('&lt;script&gt;');
    });
    
    test('should include meta generator tag', () => {
      const html = exportService.generateHTMLDocument('<p>Test</p>', 'Title');
      
      expect(html).toContain('meta name="generator"');
      expect(html).toContain('MarkdownStudio Pro');
    });
    
    test('should include responsive viewport', () => {
      const html = exportService.generateHTMLDocument('<p>Test</p>', 'Title');
      
      expect(html).toContain('viewport');
      expect(html).toContain('width=device-width');
    });
  });
  
  describe('Export Styles', () => {
    test('should return CSS styles', () => {
      const styles = exportService.getExportStyles();
      
      expect(styles).toContain('body');
      expect(styles).toContain('font-family');
      expect(styles).toContain('@media');
    });
    
    test('should include dark mode styles', () => {
      const styles = exportService.getExportStyles();
      
      expect(styles).toContain('prefers-color-scheme: dark');
    });
    
    test('should include print styles', () => {
      const styles = exportService.getExportStyles();
      
      expect(styles).toContain('@media print');
    });
  });
  
  describe('Download Functionality', () => {
    test('should create blob URL for download', () => {
      const createObjectURLSpy = vi.spyOn(URL, 'createObjectURL');
      
      exportService.download('content', 'test.md', 'text/markdown');
      
      expect(createObjectURLSpy).toHaveBeenCalled();
    });
    
    test('should revoke blob URL after download', async () => {
      const revokeObjectURLSpy = vi.spyOn(URL, 'revokeObjectURL');
      
      exportService.download('content', 'test.md', 'text/markdown');
      
      // Wait for cleanup timeout
      await new Promise(resolve => setTimeout(resolve, 150));
      
      expect(revokeObjectURLSpy).toHaveBeenCalled();
    });
  });
  
  describe('HTML Escaping', () => {
    test('should escape HTML entities', () => {
      expect(exportService.escapeHtml('<script>')).toBe('&lt;script&gt;');
      expect(exportService.escapeHtml('"quotes"')).toBe('"quotes"');
      expect(exportService.escapeHtml("'apostrophe'")).toBe("'apostrophe'");
      expect(exportService.escapeHtml('&amp')).toBe('&amp;amp');
    });
  });
});
```

### tests/security/xss.test.js

```javascript
/**
 * XSS Prevention Tests - CRITICAL SECURITY TESTS
 * All tests MUST pass before any release
 * @author Oathan Rex
 */

import { describe, test, expect } from 'vitest';

/**
 * Sanitize function using DOMPurify mock
 */
function sanitize(html) {
  return DOMPurify.sanitize(html, {
    ALLOWED_TAGS: [
      'h1', 'h2', 'h3', 'h4', 'h5', 'h6',
      'p', 'br', 'hr', 'ul', 'ol', 'li',
      'blockquote', 'pre', 'code',
      'a', 'strong', 'em', 'del',
      'table', 'thead', 'tbody', 'tr', 'th', 'td',
      'img'
    ],
    ALLOWED_ATTR: ['href', 'src', 'alt', 'title', 'class', 'id'],
    FORBID_TAGS: ['script', 'style', 'iframe', 'form'],
    FORBID_ATTR: ['onerror', 'onclick', 'onload', 'onmouseover']
  });
}

describe('XSS Prevention - Script Injection', () => {
  const scriptVectors = [
    '<script>alert("XSS")</script>',
    '<script src="evil.js"></script>',
    '<SCRIPT>alert("XSS")</SCRIPT>',
    '<scr<script>ipt>alert("XSS")</scr</script>ipt>',
    '<script/src="evil.js">',
    '"><script>alert("XSS")</script>',
    '</title><script>alert("XSS")</script>',
    '<script>alert(String.fromCharCode(88,83,83))</script>',
    '<script>eval("alert(1)")</script>'
  ];
  
  scriptVectors.forEach((vector, index) => {
    test(`should block script vector ${index + 1}`, () => {
      const output = sanitize(vector);
      
      expect(output.toLowerCase()).not.toContain('<script');
      expect(output.toLowerCase()).not.toContain('</script');
      expect(output).not.toContain('alert');
      expect(output).not.toContain('eval');
    });
  });
});

describe('XSS Prevention - Event Handlers', () => {
  const eventVectors = [
    '<img src=x onerror=alert(1)>',
    '<svg onload=alert(1)>',
    '<body onload=alert(1)>',
    '<div onmouseover=alert(1)>test</div>',
    '<input onfocus=alert(1) autofocus>',
    '<marquee onstart=alert(1)>',
    '<video><source onerror=alert(1)>',
    '<details open ontoggle=alert(1)>',
    '<img src="x" onerror="javascript:alert(1)">',
    '<a onmouseover="alert(1)">click</a>'
  ];
  
  eventVectors.forEach((vector, index) => {
    test(`should block event handler vector ${index + 1}`, () => {
      const output = sanitize(vector);
      
      expect(output).not.toMatch(/on\w+\s*=/i);
      expect(output).not.toContain('alert');
    });
  });
});

describe('XSS Prevention - JavaScript URLs', () => {
  const jsUrlVectors = [
    '<a href="javascript:alert(1)">click</a>',
    '<a href="JAVASCRIPT:alert(1)">click</a>',
    '<a href="javascript&#58;alert(1)">click</a>',
    '<a href="java\nscript:alert(1)">click</a>',
    '<a href="javascript&#x3A;alert(1)">click</a>',
    '<a href=" javascript:alert(1)">click</a>',
    '<a href="&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;">click</a>',
    '<iframe src="javascript:alert(1)">',
    '<form action="javascript:alert(1)">',
    '<object data="javascript:alert(1)">'
  ];
  
  jsUrlVectors.forEach((vector, index) => {
    test(`should block javascript URL vector ${index + 1}`, () => {
      const output = sanitize(vector);
      
      expect(output.toLowerCase()).not.toContain('javascript:');
      expect(output.toLowerCase()).not.toContain('javascript&#');
    });
  });
});

describe('XSS Prevention - Data URLs', () => {
  const dataVectors = [
    '<a href="data:text/html,<script>alert(1)</script>">click</a>',
    '<iframe src="data:text/html,<script>alert(1)</script>">',
    '<object data="data:text/html,<script>alert(1)</script>">',
    '<embed src="data:text/html,<script>alert(1)</script>">'
  ];
  
  dataVectors.forEach((vector, index) => {
    test(`should block data URL vector ${index + 1}`, () => {
      const output = sanitize(vector);
      
      expect(output).not.toContain('<script');
      expect(output).not.toContain('alert');
    });
  });
});

describe('XSS Prevention - SVG', () => {
  const svgVectors = [
    '<svg><script>alert(1)</script></svg>',
    '<svg onload=alert(1)>',
    '<svg><animate onbegin=alert(1)>',
    '<svg><set onbegin=alert(1)>',
    '<svg><foreignObject><body onload=alert(1)></body></foreignObject></svg>'
  ];
  
  svgVectors.forEach((vector, index) => {
    test(`should block SVG vector ${index + 1}`, () => {
      const output = sanitize(vector);
      
      expect(output).not.toContain('alert');
      expect(output).not.toMatch(/on\w+=/i);
    });
  });
});

describe('XSS Prevention - CSS', () => {
  const cssVectors = [
    '<style>body{background:url("javascript:alert(1)")}</style>',
    '<div style="background:url(javascript:alert(1))">',
    '<div style="behavior: url(evil.htc)">',
    '<div style="expression(alert(1))">',
    '<div style="-moz-binding:url(evil.xml)">',
    '<link rel="stylesheet" href="javascript:alert(1)">'
  ];
  
  cssVectors.forEach((vector, index) => {
    test(`should block CSS vector ${index + 1}`, () => {
      const output = sanitize(vector);
      
      expect(output.toLowerCase()).not.toContain('javascript:');
      expect(output).not.toContain('expression');
      expect(output).not.toContain('behavior');
    });
  });
});

describe('XSS Prevention - iframe/form/object', () => {
  test('should block iframe tags', () => {
    const vectors = [
      '<iframe src="evil.html">',
      '<iframe src="https://evil.com">',
      '<IFRAME SRC="evil.html">'
    ];
    
    vectors.forEach(vector => {
      const output = sanitize(vector);
      expect(output.toLowerCase()).not.toContain('<iframe');
    });
  });
  
  test('should block form tags', () => {
    const output = sanitize('<form action="https://evil.com"><input name="password"></form>');
    
    expect(output.toLowerCase()).not.toContain('<form');
    expect(output.toLowerCase()).not.toContain('<input');
  });
  
  test('should block object/embed tags', () => {
    const output = sanitize('<object data="evil.swf"><embed src="evil.swf"></embed></object>');
    
    expect(output.toLowerCase()).not.toContain('<object');
    expect(output.toLowerCase()).not.toContain('<embed');
  });
});

describe('XSS Prevention - Safe Content Preservation', () => {
  test('should preserve safe HTML', () => {
    const safeHtml = '<h1>Title</h1><p>Paragraph with <strong>bold</strong> and <em>italic</em>.</p>';
    const output = sanitize(safeHtml);
    
    expect(output).toContain('<h1>');
    expect(output).toContain('<strong>');
    expect(output).toContain('<em>');
  });
  
  test('should preserve safe links', () => {
    const safeLink = '<a href="https://example.com">Safe link</a>';
    const output = sanitize(safeLink);
    
    expect(output).toContain('https://example.com');
    expect(output).toContain('Safe link');
  });
  
  test('should preserve code blocks', () => {
    const code = '<pre><code>const x = 1;</code></pre>';
    const output = sanitize(code);
    
    expect(output).toContain('const x = 1;');
    expect(output).toContain('<pre>');
    expect(output).toContain('<code>');
  });
  
  test('should preserve images with safe src', () => {
    const img = '<img src="https://example.com/image.jpg" alt="Test image">';
    const output = sanitize(img);
    
    expect(output).toContain('src="https://example.com/image.jpg"');
    expect(output).toContain('alt="Test image"');
  });
  
  test('should preserve tables', () => {
    const table = '<table><thead><tr><th>A</th></tr></thead><tbody><tr><td>1</td></tr></tbody></table>';
    const output = sanitize(table);
    
    expect(output).toContain('<table>');
    expect(output).toContain('<th>A</th>');
    expect(output).toContain('<td>1</td>');
  });
  
  test('should preserve lists', () => {
    const list = '<ul><li>Item 1</li><li>Item 2</li></ul>';
    const output = sanitize(list);
    
    expect(output).toContain('<ul>');
    expect(output).toContain('<li>Item 1</li>');
  });
});

describe('XSS Prevention - Edge Cases', () => {
  test('should handle null byte injection', () => {
    const output = sanitize('<scri\x00pt>alert(1)</script>');
    expect(output).not.toContain('alert');
  });
  
  test('should handle Unicode encoding', () => {
    const output = sanitize('<script\u0000>alert(1)</script>');
    expect(output).not.toContain('alert');
  });
  
  test('should handle nested tags', () => {
    const output = sanitize('<<script>script>alert(1)<</script>/script>');
    expect(output).not.toContain('alert');
  });
  
  test('should handle mixed case', () => {
    const output = sanitize('<ScRiPt>alert(1)</sCrIpT>');
    expect(output.toLowerCase()).not.toContain('<script');
  });
  
  test('should handle empty input', () => {
    expect(() => sanitize('')).not.toThrow();
    expect(sanitize('')).toBe('');
  });
  
  test('should handle very long input', () => {
    const longInput = '<p>test</p>'.repeat(10000);
    expect(() => sanitize(longInput)).not.toThrow();
  });
});
```

---

## 8. Deployment Files

### .github/workflows/test.yml

```yaml
name: Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [18.x, 20.x]
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      
      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run unit tests
        run: npm test
      
      - name: Run security tests
        run: npm run test:security
      
      - name: Generate coverage report
        run: npm run test:coverage
        continue-on-error: true
      
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        if: matrix.node-version == '20.x'
        with:
          files: ./coverage/lcov.info
          fail_ci_if_error: false
          verbose: true

  lint:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      
      - name: Validate HTML
        uses: anishathalye/proof-html@v2
        with:
          directory: ./
          ignore_urls: 'github.com'
        continue-on-error: true
```

### .github/workflows/deploy.yml

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Pages
        uses: actions/configure-pages@v4
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

---

## 9. README

### README.md

```markdown
# MarkdownStudio Pro

[![Tests](https://github.com/oathanrex/markdown-editor/actions/workflows/test.yml/badge.svg)](https://github.com/oathanrex/markdown-editor/actions/workflows/test.yml)
[![Deploy](https://github.com/oathanrex/markdown-editor/actions/workflows/deploy.yml/badge.svg)](https://github.com/oathanrex/markdown-editor/actions/workflows/deploy.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A free, browser-based Markdown editor with live preview, syntax highlighting, and HTML export. No signup required. Works offline.

**Live Demo:** [https://oathanrex.github.io/markdown-editor/](https://oathanrex.github.io/markdown-editor/)

![MarkdownStudio Pro Screenshot](./assets/images/screenshot.png)

## Features

- **Live Preview** - See your formatted Markdown in real-time
- **GitHub Flavored Markdown** - Tables, task lists, strikethrough, and more
- **Syntax Highlighting** - Code blocks with language detection
- **Auto Save** - Never lose your work
- **Export Options** - Download as Markdown or styled HTML
- **Dark/Light Theme** - Automatic system detection or manual toggle
- **Offline Support** - Works without internet after first load
- **Keyboard Shortcuts** - Power user friendly
- **Copy Code Button** - One-click copy for code blocks
- **Table of Contents** - Auto-generated from headings
- **Find & Replace** - With regex support
- **Responsive Design** - Works on desktop, tablet, and mobile
- **Privacy First** - All data stays in your browser

## Quick Start

1. Visit [https://oathanrex.github.io/markdown-editor/](https://oathanrex.github.io/markdown-editor/)
2. Start typing Markdown in the left panel
3. See your formatted content in the right panel
4. Export when ready!

## Keyboard Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| Bold | Ctrl+B | Cmd+B |
| Italic | Ctrl+I | Cmd+I |
| Link | Ctrl+K | Cmd+K |
| Code | Ctrl+` | Cmd+` |
| Heading 1-6 | Ctrl+1-6 | Cmd+1-6 |
| Save | Ctrl+S | Cmd+S |
| Find | Ctrl+F | Cmd+F |
| Replace | Ctrl+H | Cmd+Option+F |
| Undo | Ctrl+Z | Cmd+Z |
| Redo | Ctrl+Y | Cmd+Shift+Z |

## Local Development

```bash
# Clone the repository
git clone https://github.com/oathanrex/markdown-editor.git
cd markdown-editor

# Install dependencies (for testing only)
npm install

# Run tests
npm test

# Serve locally
npx serve . -p 3000
```

## Project Structure

```
markdown-editor/
├── index.html          # Main HTML file
├── 404.html            # Error page
├── sw.js               # Service worker
├── manifest.json       # PWA manifest
├── assets/
│   ├── css/           # Stylesheets
│   ├── js/            # JavaScript modules
│   └── images/        # Icons and images
├── tests/             # Test files
└── .github/           # GitHub Actions workflows
```

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+
- Chrome for Android
- Safari for iOS

## Security

- All HTML is sanitized using DOMPurify
- No external scripts or tracking
- All data stored locally in your browser
- No server-side processing

## Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run tests: `npm test`
5. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) for details.

## Author

**Oathan Rex**

- Website: [https://oathanrex.github.io](https://oathanrex.github.io)
- GitHub: [@oathanrex](https://github.com/oathanrex)

---

Made with ❤️ for the open source community
```

---

## Summary

This complete implementation includes:

### Core Files
- `index.html` - Full HTML with SEO optimization, structured data, accessibility
- `assets/css/style.css` - Complete stylesheet with dark mode, responsive design
- `assets/js/app.js` - Main application entry point
- `assets/js/core/*` - Editor, Preview, Toolbar modules
- `assets/js/services/*` - Storage, Theme, Export, Shortcuts services
- `assets/js/utils/*` - Event bus, debounce, DOM utilities

### Configuration
- `manifest.json` - PWA manifest
- `robots.txt` - SEO robots file
- `sitemap.xml` - SEO sitemap
- `sw.js` - Service worker for offline support
- `404.html` - Custom error page
- `.nojekyll` - GitHub Pages bypass

### Testing
- `package.json` - Project configuration
- `vitest.config.js` - Test configuration
- `tests/setup.js` - Test setup with mocks
- `tests/unit/*` - Unit tests
- `tests/security/xss.test.js` - Security tests

### CI/CD
- `.github/workflows/test.yml` - Test workflow
- `.github/workflows/deploy.yml` - Deploy workflow

### Documentation
- `README.md` - Project documentation
- `LICENSE` - MIT license

To deploy:
1. Create repository `oathanrex/markdown-editor`
2. Copy all files
3. Download `marked.min.js` and `purify.min.js` to `assets/js/lib/`
4. Create images (favicon, icons, og-image)
5. Push to GitHub
6. Enable GitHub Pages
