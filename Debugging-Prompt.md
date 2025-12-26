Below is a **tool-specific adaptation** of your content for **ChatGPT, Claude, and GitHub Copilot**.
Each section explains **how that AI thinks**, **what it expects**, and provides **ready-to-use prompt templates** optimized for that platform.

---

# How to Ask AI to Fix Code (Tool-Specific Guide)

## 1. ChatGPT (Best for Explanation + Refactoring)

### How ChatGPT Works Best

* Strong at **step-by-step reasoning**
* Excellent for **debugging + explanation**
* Performs best when you **explicitly ask it to analyze before fixing**

### Recommended Prompt Style

Use **Chain-of-Thought + explicit instructions**.

### Best ChatGPT Debugging Prompt Template

```
I am getting the following error:
[PASTE ERROR MESSAGE]

Expected behavior:
[WHAT YOU EXPECTED]

Actual behavior:
[WHAT IS HAPPENING]

Please do the following:
1. Analyze the code step by step
2. Identify the root cause of the bug
3. Fix the issue
4. Explain what you changed and why
5. Provide the full corrected code
```

### Optimization / Refactor Prompt (ChatGPT)

```
This code works but is slow / messy / hard to maintain.
Please:
- Analyze performance and logic
- Identify bottlenecks
- Rewrite it using modern best practices
- Explain the improvements
```

### Self-Correction Prompt (Very Effective in ChatGPT)

```
Review your previous response carefully.
Check for any logical flaws, edge cases, or syntax errors.
Provide a corrected and fully working version of the code.
```

---

## 2. Claude (Best for Logic Validation & Safety)

### How Claude Works Best

* Excellent at **reasoning correctness**
* Very good at **spotting edge cases**
* Prefers **clear problem statements**
* Less verbose unless asked

### Recommended Prompt Style

Use **problem framing + verification request**.

### Best Claude Debugging Prompt Template

```
There is a bug in the following code.

Error message:
[ERROR]

What I expected:
[EXPECTED RESULT]

What actually happens:
[ACTUAL RESULT]

Please:
- Identify the logical mistake
- Explain why the error occurs
- Provide a corrected version
- Mention any edge cases I should be aware of
```

### Logic Validation Prompt (Claude-Specific)

```
This code runs without errors, but I am not confident the logic is correct.
Please review it carefully, verify correctness, and point out any hidden bugs or edge cases.
```

### Claude Self-Review Prompt

```
Double-check your solution.
If there are assumptions, limitations, or potential failure cases, list them clearly.
```

---

## 3. GitHub Copilot (Best for Inline Fixes & Refactoring)

### How Copilot Works Best

* Works **inside the editor**
* Relies heavily on **comments and nearby code**
* Not ideal for long explanations
* Best for **incremental fixes**

### Recommended Prompt Style

Use **comments directly above the code**.

### Best Copilot Debugging Comment

```js
// BUG:
// This function throws an IndexError when the array is empty.
// Expected: return null
// Actual: crashes
// Please fix the logic and handle edge cases.
```

### Copilot Optimization Comment

```js
// OPTIMIZE:
// This loop is slow for large inputs.
// Rewrite using a more efficient approach.
// Avoid unnecessary memory allocations.
```

### Copilot Refactor Comment

```js
// REFACTOR:
// Rewrite this function using modern best practices.
// Improve readability and maintainability.
```

### Pro Tip for Copilot

Copilot responds best when:

* The **file is small**
* The **comment is precise**
* You avoid abstract language like “fix this”

---

## Quick Comparison Table

| AI Tool     | Best For                       | What to Emphasize           |
| ----------- | ------------------------------ | --------------------------- |
| **ChatGPT** | Debugging + explanations       | Step-by-step analysis       |
| **Claude**  | Logic correctness & edge cases | Expected vs actual behavior |
| **Copilot** | Inline fixes & refactors       | Clear code comments         |

---

## Universal “Perfect Debug Prompt” (Works Everywhere)

```
Here is my code:
[PASTE CODE]

Problem:
[DESCRIBE ISSUE]

Error (if any):
[ERROR MESSAGE]

Expected behavior:
[EXPECTED]

Actual behavior:
[ACTUAL]

Please analyze the logic, fix the issue, and provide the corrected code.
```

---

### Final Advice (Important)

If you only write:

> “This code is not working”

AI can **guess**, but it cannot **debug reliably**.

If you provide:

* Error
* Context
* Expectation
* Constraints

AI behaves like a **senior engineer**, not a guesser.

---
