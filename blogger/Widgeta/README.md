---

# Widgeta — AI-Powered Blogger Content & SEO Toolkit

Widgeta is a **lightweight JavaScript widget + visual editor SaaS** designed for **Blogger (Blogspot)** users.
It helps creators generate **clean, SEO-optimized HTML** using a modern editor and enhances published posts using a single external script.

The project is built using a **Product Manager–driven, AI-assisted workflow**, enabling rapid development without deep manual coding.

---

## 🚀 What This Project Does

Widgeta consists of **two main parts**:

### 1. Widget Script (Engine)

A standalone JavaScript SDK (`widget.js`) that users add **once** to their Blogger layout.
It automatically enhances posts with:

* 📑 Auto Table of Contents (H2 / H3)
* 🧩 JSON-LD Schema Injection (FAQ + Article)
* 🔗 External link formatting (`nofollow`, `noopener`, `_blank`)
* ⚡ Lightweight, dependency-free execution

---

### 2. Visual Editor (SaaS Frontend)

A **Notion-style WYSIWYG editor** built with Next.js and Tiptap that allows users to:

* Design structured content visually
* Insert SEO-friendly blocks (FAQ, CTA, Pros/Cons)
* Export **clean semantic HTML**
* Copy everything needed to publish in one click

---

## 🧠 Project Philosophy

* Think like a **Product Manager**
* Treat AI (Cursor / Claude / ChatGPT) as a **Developer**
* Build in **small, well-defined chunks**
* Keep output **clean, portable, and platform-agnostic**

This tool is intentionally designed to work **without WordPress**, plugins, or heavy frameworks.

---

## 🧩 Project Structure

```
/widget-cdn
  └── widget.js        # Lightweight Blogger Widget SDK

/editor-app
  ├── Next.js app
  ├── Tiptap editor
  ├── Custom blocks
  └── Export + copy logic
```

---

## 🛠️ Step-by-Step Build Process

### Step 1 — Project Planning & Prompts

**File:** `Prompts.md`

* Defines the complete product vision
* Breaks development into AI-friendly chunks
* Serves as the foundation for all features

---

### Step 2 — Widget Engine (Core SDK)

**File:** `Lightweight Blogger Widget SDK (widget.js).md`

The `widget.js` script:

* Runs on `window.onload`
* Scans `<h2>` and `<h3>` tags to generate a TOC
* Injects FAQ & Article schema into `<head>`
* Enhances external links inside `.post-body`
* Uses **vanilla JavaScript only** (no jQuery)

This script is hosted on a CDN and reused across all user sites.

---

### Step 3 — Editor Base Setup

**File:** `Notion-Style WYSIWYG Editor with Next.js, Tailwind CSS & Tiptap.md`

* Built with **Next.js** and **Tailwind CSS**
* Uses **Tiptap** for a modern editing experience
* Supports headings, lists, bold, and italic formatting
* Acts as the primary content creation interface

---

### Step 4 — Custom Content Blocks

**File:** `Custom Tiptap Extensions: CTA, FAQ, and Pros-Cons Blocks.md`

Custom blocks include:

* **Call-to-Action (CTA)** block with button
* **FAQ block** with repeatable Q&A pairs
* **Pros / Cons block** with two-column layout

These blocks are designed to export **semantic, widget-friendly HTML**.

---

### Step 5 — Clean HTML Export

**File:** `Clean HTML Export with Semantic Classes.md`

Critical design rule:

> ❌ No inline styles
> ✅ Only clean HTML + semantic classes

Export rules:

* FAQs → `<div class="faq-block" data-question="" data-answer=""></div>`
* TOC → `<div id="my-tool-toc"></div>`

All styling and behavior is handled later by `widget.js`.

---

### Step 6 — Real-Time Stats Sidebar

**File:** `Real-Time Stats Sidebar Integration.md`

* Keyword density calculation
* Reading time estimation
* Updates live as the user types
* Logic reused from older scripts and integrated into Next.js

---

### Step 7 — Copy to Clipboard (Final UX)

**File:** `Copy to Clipboard with Script Tag & Instructions.md`

A single button copies:

```html
<!-- Add once to Layout / Head -->
<script src="https://cdn.yourdomain.com/widget.js"></script>

<!-- Paste inside the post -->
<div id="my-tool-toc"></div>
<div class="faq-block" data-question="..." data-answer="..."></div>
```

This removes all confusion for non-technical users.

---

## 🌍 Deployment Strategy

### Widget Script

* Host on **Cloudflare R2**, **AWS S3**, or **jsDelivr**
* Public access enabled
* Versioned via tags (e.g. `v1.0.0`)

### Editor App

* Deploy on **Vercel**
* Free plan is sufficient for MVP
* No backend required initially

---

## 👤 Intended User Flow

1. User opens the editor
2. Designs content visually
3. Clicks **Copy Code**
4. Pastes script into Blogger layout (once)
5. Pastes HTML into post body
6. Publishes

No plugins. No setup headaches.

---

## ⚠️ What This Project Intentionally Avoids (For Now)

* Authentication
* Payments
* Dashboards
* Analytics
* User accounts

The focus is **validation and usability first**.

---

## 🎯 Current Status

* ✅ Core widget logic defined
* ✅ Editor architecture finalized
* ✅ Clean HTML export strategy implemented
* ✅ Copy-paste UX polished
* 🔜 User testing & iteration

---

## 📌 Final Note

This README reflects a **production-grade MVP architecture**.
The same structure can later support:

* Paid plans
* WordPress / Shopify versions
* Advanced schema types
* Team collaboration features

---
