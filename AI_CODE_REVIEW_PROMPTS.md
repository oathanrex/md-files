# AI Code Review & Debugging Prompt Guide

A professional, reusable prompt collection designed to help you get high-quality code reviews, bug fixes, and optimized implementations from AI.

This repository contains Beginner and Advanced prompt templates that you can use for:
- Learning programming
- Debugging issues
- Improving performance
- Refactoring projects
- Building clean portfolio-ready code

---

## Why This Exists

Simply telling AI "this code doesn’t work" is not enough.

To fix code correctly, AI needs:
- Context
- Error details
- Expected vs actual behavior
- Clear instructions

These prompts follow prompt-engineering best practices to ensure:
- Accurate debugging
- Clean implementations
- Professional-quality output

---

## Beginner Code Review & Fix Prompt

Use this prompt when you are:
- Learning programming
- Working on small to medium projects
- Trying to understand logic and errors
- New to debugging

### Prompt Template

```
Please review my code and help me fix it.

Tasks:
1. Explain in simple terms what this code is supposed to do.
2. Identify any errors, bugs, or logical mistakes.
3. Explain why the problem is happening.
4. Fix the code step by step.
5. Provide a clean, working version of the full code.

Guidelines:
- Use simple and clear explanations.
- Avoid advanced jargon unless necessary.
- Keep the code readable and easy to understand.
- Use only HTML, CSS, and JavaScript.
- The output must be fully working and directly runnable.

Here is my code:
[PASTE YOUR CODE HERE]

The problem I am facing:
[DESCRIBE THE ISSUE OR ERROR MESSAGE]
```

---

## Advanced / Professional Code Review Prompt

Use this prompt when you want:
- Production-quality code
- Performance optimization
- Security checks
- Scalable architecture
- Portfolio-level refactoring

### Prompt Template

```
Please perform a full, exhaustive, professional-level technical review of the following code.

Analyze:
- Correctness and logic
- Edge cases and real-world failure scenarios
- Error handling and robustness
- Time and space complexity
- Performance bottlenecks
- Memory usage
- Security vulnerabilities
- Code structure and architecture
- Readability, maintainability, and scalability

After the review, provide:
1. A clear summary of all identified issues.
2. Detailed explanations of each issue.
3. Step-by-step recommendations for improvement.
4. A fully rewritten, optimized version of the code following modern best practices.
5. Optional advanced improvements or alternative approaches.

Strict Rules:
- Do NOT add themes, styling enhancements, emojis, or visual effects.
- Keep CSS and JS functional only.
- Write clean, minimal, modular, production-quality code.
- Use only HTML, CSS, and JavaScript.
- The final code must be complete, correct, and directly runnable.

Output Format:
- Short explanation (max 5 lines)
- Complete updated HTML, CSS, and JavaScript code
- No extra commentary

Here is the code:
[PASTE YOUR CODE HERE]
```

---

## Recommended Debugging Workflow

1. Copy the relevant prompt (Beginner or Advanced)
2. Paste your full code
3. Include exact error messages if any
4. Describe expected vs actual behavior
5. Run the improved code provided by AI
6. Compare changes to learn better patterns

---

## Pro Tip: Self-Correction Prompt

```
Review your previous response carefully.
Check for any logical mistakes, syntax errors, or incorrect assumptions.
Double-check everything and provide a fully working corrected version of the code.
```

---

## License

Free to use for learning, personal projects, professional development, and portfolio work.
