Role: Principal Web Engineer conducting a 10/10 production audit of perspectivefix.app (Static HTML/CSS/JS architecture — no backend/API).

CRITICAL EXECUTION CONSTRAINTS (Zero Token Waste, Zero Hallucination):
- Zero Narration: Start directly with Part 1. No greetings, no explanations.
- Single-Pass Parsing: Read every file exactly once. Correlate DOM/CSS/JS across files in memory.
- Missing Files: If a referenced file is missing, write "MISSING: [filename]" in the table and skip. Never guess content.
- Strict Diff Protocol: NEVER output full files. Output ONLY SEARCH/REPLACE diff blocks.
- Verbatim Match Rule: Text inside <<< SEARCH must be character-for-character, whitespace-for-whitespace identical to the source file. Do not reformat indentation or line breaks.
- Structural Anchor (Fail-safe): Do NOT guess line numbers. Alongside each file name, give the closest unique structural anchor (parent <div id="...">, wrapping function name, or unique CSS class/selector). If no unique anchor exists nearby (e.g. a generic CSS rule), quote the exact preceding sibling selector instead.

REQUIRED DIFF FORMAT:
​```text
🛠️ File: [filename] (Anchor: [nearest unique ID/class/function/sibling])
<<< SEARCH
[exact existing buggy code, 2 lines context above/below]
===
REPLACE
[corrected code, same indentation style]
>>>
​```

AUDIT CATEGORIES (Priority Order):
1. Functional Bugs: JS errors, broken handlers, undo/redo/theme-toggle state failures
2. DOM/Layout: <nav> strictly precedes <h1> and is sticky; header/footer consistent across ALL pages; zero duplicate IDs
3. Responsive/Mobile: Overflow at breakpoints, touch targets ≥44px
4. Accessibility: alt text, contrast ≥4.5:1, keyboard nav, ARIA on icon-only buttons, sequential heading hierarchy
5. Performance: Image lazy-loading + explicit width/height, zero render-blocking head scripts
6. SEO: Canonical/robots/sitemap consistency, one instance per tag, matching URL forms
7. Security: rel="noopener noreferrer" on target="_blank", zero inline secrets
8. Legal/Trust: Privacy/Terms/Disclaimer/Contact linked in footer on EVERY page

OUTPUT SEQUENCE:
Part 1: Master table (File | Category | Status (PASS/FAIL) | Issue | Fix Summary)
Part 2: SEARCH/REPLACE diffs (ONLY for FAIL statuses)
Part 3: 3-5 bullet high-leverage subjective design suggestions (opinion only, DO NOT apply in Part 2)
Part 4: Exactly one line: "X files audited, Y bugs fixed, Z suggestions flagged."

Begin audit now.
