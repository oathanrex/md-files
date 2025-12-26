Below is a **domain-specific adaptation** of the guide for **Frontend, Backend, Machine Learning, Python, and JavaScript**.
Each section explains **what context that domain needs** and provides **copy-paste-ready prompt templates** that consistently produce better results from ChatGPT, Claude, or Copilot.

---

# How to Ask AI to Fix Code (Domain-Specific Guide)

---

## 1. Frontend (HTML / CSS / JavaScript / React / Vue)

### What AI Needs for Frontend Bugs

Frontend issues are often **state-, event-, or rendering-related**. AI needs:

* Browser & device
* Framework + version
* Steps to reproduce
* Visual vs functional issue

### Frontend Debugging Prompt Template

```
Frontend issue in [React / Vue / Vanilla JS].

Environment:
- Browser: [Chrome 120 / Safari / Firefox]
- Device: [Desktop / Mobile]
- Framework & version: [React 18, etc.]

Problem:
[Describe the UI or interaction bug]

Expected behavior:
[What should happen visually or functionally]

Actual behavior:
[What actually happens]

Error (if any):
[Console error or warning]

Please:
- Analyze the component logic step by step
- Identify why the UI state is incorrect
- Fix the issue
- Provide the corrected code
- Mention any performance or accessibility improvements
```

### Frontend Performance Prompt

```
This UI works but feels slow or janky.
Please analyze re-renders, event handlers, and state usage.
Optimize it using modern frontend best practices.
```

---

## 2. Backend (APIs / Databases / Auth / Scalability)

### What AI Needs for Backend Bugs

Backend problems are usually **data-, concurrency-, or config-related**. Provide:

* Language + framework
* Endpoint or function
* Input/output samples
* Logs or stack traces

### Backend Debugging Prompt Template

```
Backend issue in [Node.js / Django / Spring Boot].

Tech stack:
- Language & version:
- Framework:
- Database:
- OS (if relevant):

Endpoint / function:
[Paste code]

Problem:
[Describe failure: wrong response, 500 error, timeout, etc.]

Error / logs:
[Paste stack trace or logs]

Expected behavior:
[Correct response / data]

Actual behavior:
[What happens instead]

Please:
- Identify the root cause
- Fix the logic or configuration
- Handle edge cases
- Suggest improvements for reliability and scalability
```

### Backend Security Prompt

```
Review this backend code for security issues
(SQL injection, auth flaws, race conditions).
Fix vulnerabilities and explain the changes.
```

---

## 3. Machine Learning (Training / Inference / Data Issues)

### What AI Needs for ML Bugs

ML bugs are often **silent failures**. Provide:

* Model type
* Dataset shape
* Metrics
* Training behavior

### ML Debugging Prompt Template

```
Machine learning issue.

Model:
[Model type: CNN, Transformer, XGBoost, etc.]

Framework:
[PyTorch / TensorFlow / scikit-learn + version]

Dataset:
- Input shape:
- Output/labels:
- Dataset size:

Problem:
[Loss not decreasing / overfitting / bad predictions]

Expected behavior:
[Target metrics or behavior]

Actual behavior:
[Observed metrics or failure]

Code:
[Paste training or inference code]

Please:
- Analyze the pipeline step by step
- Identify possible data, model, or training issues
- Fix the code
- Suggest hyperparameter or architectural improvements
```

### ML Optimization Prompt

```
This model trains correctly but performance is poor.
Please optimize data loading, model architecture, and training loop.
Explain trade-offs.
```

---

## 4. Python (Scripts / Automation / Data Processing)

### What AI Needs for Python Bugs

Python issues are often **type, index, or dependency related**.

### Python Debugging Prompt Template

```
Python bug.

Python version:
[3.10 / 3.11]

Error message:
[Full traceback]

Code:
[Paste code]

Expected behavior:
[What the script should do]

Actual behavior:
[What happens instead]

Please:
- Trace the error line by line
- Explain why it occurs
- Fix the bug
- Provide the corrected full script
```

### Python Refactor Prompt

```
This Python code works but is hard to read and maintain.
Please refactor it using clean code principles and best practices.
```

---

## 5. JavaScript (Node.js / Browser / Async Bugs)

### What AI Needs for JS Bugs

JS problems are frequently **async, scope, or state related**.

### JavaScript Debugging Prompt Template

```
JavaScript bug.

Runtime:
[Browser / Node.js + version]

Code:
[Paste code]

Problem:
[Describe bug]

Expected behavior:
[Expected output]

Actual behavior:
[Observed output or error]

Notes:
- Is async/await involved? [Yes/No]
- Is this event-driven? [Yes/No]

Please:
- Analyze execution flow step by step
- Identify async or scope issues
- Fix the bug
- Provide the corrected code
```

### JavaScript Performance Prompt

```
This JavaScript code works but is inefficient.
Please optimize execution, memory usage, and async flow.
```

---

## Universal “Senior Engineer” Prompt (Works in Any Domain)

```
Here is my code:
[PASTE CODE]

Context:
[Frontend / Backend / ML / Python / JS]

Environment:
[Versions, framework, runtime]

Problem:
[Describe clearly]

Expected behavior:
[What should happen]

Actual behavior:
[What happens]

Error/logs (if any):
[Paste here]

Please analyze step by step, identify the root cause,
fix the issue, and provide a clean, production-ready solution.
```

---

## Final Rule (Very Important)

The **quality of AI output is directly proportional to the quality of your input**.

Bad input → guess
Good input → debugging
Great input → **senior-level solution**

---
