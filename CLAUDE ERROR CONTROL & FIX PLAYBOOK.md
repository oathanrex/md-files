Below are **two ready-to-use artifacts**, written professionally and designed specifically to **control Claude’s behavior** and **systematically fix every common mistake Claude makes**.

You can **copy-paste these directly** and reuse them whenever needed.

---

## 1️⃣ **MASTER PROMPT FOR CLAUDE (Strict, Non-Negotiable)**

Use this **every time** you give Claude your HTML/CSS/JS file.

You are a senior frontend engineer and accessibility-focused debugger.

I am providing you with my ORIGINAL source code below.
This code is the SINGLE SOURCE OF TRUTH.

STRICT RULES (YOU MUST FOLLOW ALL):

1. DO NOT remove, refactor, simplify, rewrite, or redesign any feature, UI component, animation, interaction, or logic.
2. DO NOT delete or replace any HTML section, Tailwind class, JavaScript function, event listener, or ARIA attribute.
3. DO NOT optimize, clean up, or “improve” the code unless it is REQUIRED to fix a bug.
4. DO NOT change the visual layout, styling, spacing, colors, animations, or behavior.
5. DO NOT replace my implementation with your own version.
6. If something looks redundant but is working, LEAVE IT UNTOUCHED.

YOUR TASK IS LIMITED TO ONLY:

* Fix broken HTML syntax (invalid attributes, broken tags, incorrect nesting)
* Fix JavaScript runtime errors (console errors, undefined variables, broken selectors)
* Fix accessibility issues WITHOUT removing ARIA logic
* Fix Iconify or Tailwind issues ONLY if they are broken
* Ensure the code runs correctly in the browser

OUTPUT REQUIREMENTS:

* Return the COMPLETE corrected file
* Preserve 100% of original functionality
* Do NOT omit any code
* Do NOT explain unless absolutely necessary
* Do NOT add new features

If you violate any rule above, your output is incorrect.

Here is my ORIGINAL code (treat it as final authority):

---

## 2️⃣ **“CLAUDE ERROR CONTROL & FIX PLAYBOOK”**

Whenever Claude breaks something, **copy the relevant section and paste it back to Claude**.

This document exists to prevent Claude from:

* Removing features
* Rewriting working code
* Oversimplifying complex UI
* Breaking accessibility
* Replacing source code with its own version

Use the sections below as COPY-PASTE FIXES.

---

## ❌ ERROR TYPE 1: Claude Removes Features or Sections

### Problem

Claude deletes UI components, panels, functions, or interactions it thinks are “unnecessary”.

### Fix Prompt

```
You removed existing features and UI sections.
This is NOT allowed.

Restore EVERYTHING exactly as in the original source.
Only fix bugs.
Do NOT remove or replace any logic or markup.
Re-run the task.
```

---

## ❌ ERROR TYPE 2: Claude Refactors or Simplifies Code

### Problem

Claude rewrites code into a “cleaner” or shorter version.

### Fix Prompt

```
Do NOT refactor or simplify my code.
My implementation must remain intact.
Undo all refactoring and only fix syntax or runtime errors.
```

---

## ❌ ERROR TYPE 3: Claude Changes UI / Styling / Layout

### Problem

Spacing, colors, animations, or Tailwind classes change.

### Fix Prompt

```
You changed UI styling and layout.
This is forbidden.

Revert all visual changes.
Keep Tailwind classes and animations exactly as provided.
```

---

## ❌ ERROR TYPE 4: Claude Breaks Iconify Icons

### Problem

Icons stop rendering due to incorrect attributes or replacement.

### Fix Prompt

```
Do NOT replace Iconify usage.
Only fix broken attributes such as malformed data-icon values.
Preserve existing icons and structure.
```

---

## ❌ ERROR TYPE 5: Claude Deletes ARIA Attributes

### Problem

Claude removes roles, aria-* attributes, or accessibility logic.

### Fix Prompt

```
You removed accessibility logic.
Restore ALL ARIA roles and attributes.
Accessibility must remain fully intact.
```

---

## ❌ ERROR TYPE 6: Claude Outputs Partial Code

### Problem

Claude returns only part of the file.

### Fix Prompt

```
Your output is incomplete.
Return the FULL file.
Do not omit any section, even if unchanged.
```

---

## ❌ ERROR TYPE 7: Claude Adds New Features Without Permission

### Problem

Claude introduces new logic or enhancements.

### Fix Prompt

```
Do NOT add new features.
Only fix what is broken.
Remove all newly added logic and restore original behavior.
```

---

## ❌ ERROR TYPE 8: Claude Explains Instead of Fixing

### Problem

Claude writes explanations instead of usable code.

### Fix Prompt

```
Do not explain.
Return only the corrected code file.
```

---

## ✅ FINAL EMERGENCY RESET PROMPT (Use When Claude Goes Wild)

```
RESET AND RESTART.

Restore the original source completely.
Apply ONLY minimal fixes for actual bugs.
No refactoring. No removals. No redesign.
Return the FULL corrected file.
```

---

## ✅ HOW TO USE THIS FILE

1. Run Claude with the MASTER PROMPT
2. If Claude makes a mistake:

   * Identify the error type
   * Copy the corresponding Fix Prompt
   * Paste it back into Claude
3. Repeat until output is correct

This file is designed for COPY-PASTE use.
No interpretation required.
