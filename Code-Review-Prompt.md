Here is a **perfect, universal, English prompt** that you can copy-paste anytime you want AI to review your code at a professional level.

---

# **Code Review Prompt**

"Please perform a full, exhaustive, ultra-detailed technical review of my code.
Analyze correctness, logic, edge cases, error handling, time and space complexity, performance bottlenecks, memory usage, security vulnerabilities, architecture quality, coding style, naming conventions, readability, maintainability, scalability, and test coverage.
Identify all potential bugs and real-world failure scenarios.
After the review, provide:

1. A detailed explanation of every issue,
2. Step-by-step recommendations for improvement,
3. An optimized, clean, rewritten version of the code following modern best practices,
4. Optional advanced enhancements or alternative implementations."

---
Below is a **clear, professional version**, suitable for explaining how to ask AI to fix code properly. The meaning and structure are preserved, but the language is polished and natural.

---

## When Asking AI to Fix Code, “This Doesn’t Work” Is Not Enough

If you want AI to correctly fix your code, simply writing *“this is not working”* is not sufficient.
AI needs **context** and **clues** in order to identify and correct errors effectively.

According to prompt-engineering best practices, when your code has a bug, you should follow this **3-step formula**:

---

## 1. Explain the Error in Detail

You must clearly tell the AI **where** and **how** the problem occurs. Instead of writing *“fix this”*, include the following:

* **Copy-paste the exact error message:**
  “I am getting this error: `[Error Message Here]`.”
* **Expected vs. actual behavior:**
  “I expected [what should have happened], but instead [what is actually happening].”
* **Describe the behavior:**
  “The code runs but produces incorrect output” or “The code crashes during execution.”

---

## 2. Use “Chain-of-Thought” Prompting

Ask the AI to think step-by-step. This helps it identify logical mistakes more accurately.

**Example Prompt:**

> “There is a bug in this code. First, analyze the logic step by step to find where it might be wrong. Then fix it and explain what you changed.”

---

## 3. Best Debugging Prompt Templates

You can use the following prompt templates depending on your situation:

| Situation                  | What to Write (Prompt)                                                                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Wrong Logic**            | “This code runs without errors, but the output is incorrect. Please re-check the logic and provide a corrected version.” |
| **Error Message Appears**  | “When I run this code, I get `[Error Name]`. Please fix it and explain why this error occurs.”                           |
| **Code Is Slow**           | “This code works, but it is very slow. Please optimize it and rewrite it for better performance.”                        |
| **Need a Modern Approach** | “Is there a cleaner or more modern way to write this code? Please update it using best practices.”                       |

---

## Pro Tip: Use a “Self-Correction” Prompt

If the AI keeps making the same mistake, say this:

> **“Review your previous response. Check if there are any syntax errors or logical flaws. Double-check everything and provide a working version of the code.”**

---

## Example of a Good Debugging Prompt

> “I tried the Python code you provided, but I’m getting an `IndexError` on line 15. The loop boundary might be incorrect. Please fix it and provide the full updated code below.”
> Review this code for security vulnerabilities, edge cases, and performance bottlenecks. Suggest improvements."

---

**If you have a specific piece of code that is not working right now, you can try this.**

TASK:
Understand the concept, functionality, and behavior of this code,
and then rewrite similar code, but with better implementation and performance
than the original.

STRICT RULES (very important):
1. You will not add any themes, color schemes, UI styles, emojis, icons,
animations, branding, or creative touches of your own.
2. Write only clean, professional, and minimal code.
3. No emojis, decorative comments, or fancy formatting should be used.
4. CSS and JS should be for functionality only, not visual over-design.
5. The code must be modular, readable, and optimized.
6. Use only HTML, CSS, and JavaScript.
7. The output must be exactly working and directly runnable.

IMPROVEMENT REQUIREMENTS:
- The code structure should be cleaner.
- Remove unnecessary repetition.
- Improve performance and readability.
- The logic should be clearly separated.

OUTPUT FORMAT:
- First, provide a short explanation (maximum 5 lines).
- Then, provide the complete HTML, CSS, and JavaScript code.
- Do not include any extra commentary, emojis, or personal text.
---

