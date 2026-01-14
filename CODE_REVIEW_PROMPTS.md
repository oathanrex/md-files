# AI Code Review & Debugging Prompt Guide

> A comprehensive collection of professional prompts for AI-assisted code review, debugging, and optimization.

---

## Table of Contents

- [Full Code Review Prompt](#full-code-review-prompt)
- [Quick Security Review](#quick-security-review)
- [Debugging Guide](#debugging-guide)
- [Debugging Prompt Templates](#debugging-prompt-templates)
- [Code Rewrite Prompt](#code-rewrite-prompt)
- [SEO Optimization Prompt](#seo-optimization-prompt)
- [Master Review Prompt](#master-review-prompt)

---

## Full Code Review Prompt

```text
Please perform a full, exhaustive, ultra-detailed technical review of my code.
Analyze correctness, logic, edge cases, error handling, time and space complexity, 
performance bottlenecks, memory usage, security vulnerabilities, architecture quality, 
coding style, naming conventions, readability, maintainability, scalability, and test coverage.

Identify all potential bugs and real-world failure scenarios.

After the review, provide:
1. A detailed explanation of every issue
2. Step-by-step recommendations for improvement
3. An optimized, clean, rewritten version of the code following modern best practices
4. Optional advanced enhancements or alternative implementations
```

---

## Quick Security Review

```text
Review this code for security vulnerabilities, edge cases, and performance bottlenecks. 
Suggest improvements.
```

---

## Debugging Guide

### Why "This Doesn't Work" Is Not Enough

If you want AI to correctly fix your code, simply writing *"this is not working"* is not sufficient.
AI needs **context** and **clues** in order to identify and correct errors effectively.

---

### Step 1: Explain the Error in Detail

You must clearly tell the AI **where** and **how** the problem occurs.

**Include the following:**

- **Copy-paste the exact error message:**
  ```text
  I am getting this error: `[Error Message Here]`
  ```

- **Expected vs. actual behavior:**
  ```text
  I expected [what should have happened], but instead [what is actually happening]
  ```

- **Describe the behavior:**
  ```text
  The code runs but produces incorrect output
  ```
  OR
  ```text
  The code crashes during execution
  ```

---

### Step 2: Use Chain-of-Thought Prompting

Ask the AI to think step-by-step. This helps it identify logical mistakes more accurately.

```text
There is a bug in this code. First, analyze the logic step by step to find where 
it might be wrong. Then fix it and explain what you changed.
```

---

### Step 3: Self-Correction Prompt

If the AI keeps making the same mistake, use this:

```text
Review your previous response. Check if there are any syntax errors or logical flaws. 
Double-check everything and provide a working version of the code.
```

---

## Debugging Prompt Templates

| Situation | Prompt |
|-----------|--------|
| **Wrong Logic** | `This code runs without errors, but the output is incorrect. Please re-check the logic and provide a corrected version.` |
| **Error Message Appears** | `When I run this code, I get [Error Name]. Please fix it and explain why this error occurs.` |
| **Code Is Slow** | `This code works, but it is very slow. Please optimize it and rewrite it for better performance.` |
| **Need a Modern Approach** | `Is there a cleaner or more modern way to write this code? Please update it using best practices.` |

---

### Example of a Good Debugging Prompt

```text
I tried the Python code you provided, but I'm getting an `IndexError` on line 15. 
The loop boundary might be incorrect. Please fix it and provide the full updated code below.
```

---

## Code Rewrite Prompt

```text
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

[Paste your code here]
```

---

## SEO Optimization Prompt

```text
I am sharing my HTML file for an online tool. I need to update the text content 
and SEO meta-tags for better Google ranking and AdSense approval.

Please follow these strict rules:

1. SEO Optimization: Use a human-like, helpful tone.
2. AdSense Friendly: Add a clear FAQ section and use-case descriptions 
   in simple, conversational English.
3. DO NOT TOUCH THE CODE: Do not modify any JavaScript logic, CSS styles, 
   or the core canvas functionality. Only update the HTML tags 
   (Title, Meta, H1, H2, P, and Info sections).
4. Humanize Content: Ensure the text doesn't sound robotic or AI-generated. 
   Use a 'problem-solver' approach for the user.

Here is my current file code:

[Paste your code here]
```

---

## Master Review Prompt

```text
Please perform a full, exhaustive, professional-level technical review of the following code.

ANALYZE:
- Correctness and logic
- Edge cases and real-world failure scenarios
- Error handling and robustness
- Time and space complexity
- Performance bottlenecks
- Memory usage
- Security vulnerabilities
- Code structure and architecture
- Readability, maintainability, and scalability

AFTER THE REVIEW, PROVIDE:
1. A clear summary of all identified issues.
2. Detailed explanations of each issue.
3. Step-by-step recommendations for improvement.
4. A fully rewritten, optimized version of the code following modern best practices.
5. Optional advanced improvements or alternative approaches.

STRICT RULES:
- The final code must be complete, correct, and directly runnable.

OUTPUT FORMAT:
- Short explanation (max 5 lines)
- Complete updated HTML, CSS, and JavaScript code
- No extra commentary

[Paste your code here]
```

---

## Quick Copy Prompts

### For Python Code Review
```text
Review this Python code for bugs, performance issues, and Pythonic best practices. 
Provide an optimized version.
```

### For JavaScript Code Review
```text
Review this JavaScript code for bugs, memory leaks, async issues, and ES6+ best practices. 
Provide an optimized version.
```

### For API/Backend Review
```text
Review this API code for security vulnerabilities, input validation, error handling, 
and RESTful best practices. Suggest improvements.
```

### For Database Query Review
```text
Review this SQL/database query for performance, indexing opportunities, 
N+1 problems, and security (SQL injection). Optimize the query.
```

### For Frontend/UI Code Review
```text
Review this frontend code for accessibility, responsive design, performance, 
and cross-browser compatibility. Suggest improvements.
```

---

## Pro Tips

### Tip 1: Be Specific About Language/Framework
```text
Review this [Python/JavaScript/React/Node.js] code...
```

### Tip 2: Mention Your Environment
```text
This code runs on [Node.js v18 / Python 3.11 / Browser]. 
Please consider environment-specific issues.
```

### Tip 3: Ask for Explanations
```text
Explain each change you make so I can learn from it.
```

### Tip 4: Request Test Cases
```text
Also provide unit test cases for the critical functions.
```

---

## Usage Instructions

1. **Copy** any prompt from the code blocks above
2. **Paste** into your AI assistant (ChatGPT, Claude, Gemini, etc.)
3. **Add** your code where indicated
4. **Submit** and receive professional-level feedback

---

## Contributing

Feel free to add more prompts or improve existing ones via Pull Requests.

---

## License

This prompt guide is free to use for personal and commercial purposes.

---

> **Created for developers who want professional-level AI code reviews.**
