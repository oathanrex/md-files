Below you get **EXACT, ready-to-copy prompts** for Claude AI — with **pure English**, **paste-only workflow**, and **zero-error objective**.

⚠️ **IMPORTANT**
**Please change the order of prompts**.
After completing one prompt, use the next one.

---

# 🔐 MASTER CONTROL PROMPT (FIRST MESSAGE TO CLOUDE)

👉 **Paste this first (ONE TIME ONLY)**

```
You are acting as a senior SaaS production engineer.

CRITICAL NON-NEGOTIABLE RULES:

1. I cannot upload files. I paste the code manually.
2. NEVER give partial, truncated, or summarized code.
3. Every code block must be COMPLETE and self-contained.
4. ALWAYS tell me EXACTLY where to paste code using: 
- "PASTE THIS AFTER the line containing: ___" 
- "PASTE THIS BEFORE the function named: ___"
5. NEVER rewrite, delete, or refactor my existing code unless I explicitly ask.
6. If placement is unclear, ASK ME FIRST before answering.
7. Assume I am pasting code manually — no auto merge.
8. One response = one focused change only.
9. Priority order: stability → correctness → performance → accessibility → polish.
10. Goal is ZERO runtime errors and SaaS-level production quality.

At the end of every response, you MUST say:
"Tell me the next improvement to apply."

PROJECT CONTEXT:
This is a large single-file HTML application with embedded CSS,
JavaScript, and a Web Worker string that performs perspective
image correction using homography.
```

---

# 📌 STEP 0: CONTEXT LOAD PROMPT

👉 **Paste this, THEN paste your full code**

```
I am about to paste a very large single-file HTML project.

TASK:
- Analyze and understand the structure.
- Do NOT modify or suggest changes yet.
- Do NOT output any code.
- Just confirm when you fully understand the codebase.

Reply only with:
"I fully understand the code structure."
```

⬇️
**Now paste your full code**

⬇️
Claude reply → next step.

---

# 📌 STEP 1: BASELINE FREEZE PROMPT

```
Before we begin improvements:

RULES:
- Treat the current code as a frozen baseline.
- You are NOT allowed to remove or rewrite existing logic.
- All fixes must be additive, guarded, or defensive.
- No refactors unless I explicitly request them.

Confirm you understand and agree.
```

---

# 🧱 STEP 2: RUNTIME SAFETY (CRASH PROOF)

```
Add a hard runtime safety guard for extremely large images.

REQUIREMENTS:
- Reject images exceeding a safe pixel limit.
- Show a user-friendly error message.
- Do NOT break existing behavior.
- Give me COMPLETE code only.

PLACEMENT RULE:
Tell me EXACTLY where to paste the code
using BEFORE / AFTER line instructions.

Do NOT add anything else.
```

---

# 📐 STEP 3: INVALID QUADRILATERAL VALIDATION

```
Add validation to prevent invalid or self-intersecting
quadrilateral selections.

REQUIREMENTS:
- Detect zero-area or invalid shapes.
- Prevent processing if invalid.
- Fail gracefully with a clear message.
- No refactoring of existing math.

RULES:
-Complete code only
- Exact paste location required
```

---

# ⚙️ STEP 4: WEB WORKER RELIABILITY

```
Add reliability protection for the Web Worker.

REQUIREMENTS:
- Add a timeout watchdog for long-running processing.
- Handle worker failure gracefully.
- Ensure loading states never get stuck.
- Do NOT change the worker math.

RULES:
- Give exact paste locations
- One change only
```

---

# 🧯 STEP 5: GLOBAL ERROR HANDLING

```
Add global error and promise rejection handlers.

REQUIREMENTS:
- Catch unexpected runtime errors.
- Catch unhandled promise rejections.
- Show a safe user-facing error message.
- Log technical details to console only.

Do NOT interfere with normal execution.

Provide exact paste location.
```

---

# ♿ STEP 6: ACCESSIBILITY (KEYBOARD SUPPORT)

```
Add full keyboard accessibility for corner handles.

REQUIREMENTS:
- Arrow keys move corners.
- Shift + arrow moves faster.
- Must not break mouse or touch dragging.
- Must update preview correctly.

RULES:
- Do NOT rewrite existing drag logic.
- Give exact paste instructions.
```

---

#✔ STEP 7: PERFORMANCE SAFETY

```
Improve preview update performance.

REQUIREMENTS:
- Prevent excessive worker calls while dragging.
- Use a safe throttling or animation-frame approach.
- Do NOT change visual output.

RULES:
- Minimal additive code
- Exact paste location required
```

---

# 🧼 STEP 8: FINAL PRODUCTION LOCK

```
Assume the code is now production-locked.

TASK:
- Identify any remaining edge cases that could 
still cause runtime errors.
- Do NOT suggest refactors or new features.
- Do NOT output code unless absolutely required.

Only list potential risks, if any.
```

---

# 🏆 RESULT (If You Follow This Exactly)

✅ No missing code
✅ No Claude truncation
✅ No confusion about paste location
✅ Manual workflow safe
✅ Zero runtime errors
✅ SaaS-level stability
✅ True **10 / 10 production quality**

---
