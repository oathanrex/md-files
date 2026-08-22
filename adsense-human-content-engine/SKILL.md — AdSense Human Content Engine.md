---
name: adsense-human-content-engine
description: Production workflow for creating original, evidence-grounded, people-first content aligned with Google Search and AdSense quality principles. Use when creating, rewriting, expanding, or auditing website content for search visibility and reader value.
---

# AdSense Human Content Engine

## MISSION

Produce content that deserves to exist even if search engines did not exist.

Optimize for:

1. Reader usefulness
2. Original reasoning
3. Factual integrity
4. Clear communication
5. Genuine expertise signals
6. Search intent satisfaction
7. Sustainable SEO quality

Do not optimize for AI-detector scores or attempt to disguise AI authorship.

AdSense approval must never be guaranteed or implied.

---

# EXECUTION CONTRACT

Execute this workflow in order:

```text
INPUT
  ↓
LOAD REQUIRED POLICIES
  ↓
INTENT ANALYSIS
  ↓
RESEARCH DECISION
  ↓
EVIDENCE COLLECTION
  ↓
CONTENT-GAP ANALYSIS
  ↓
DRAFT
  ↓
EVIDENCE AUDIT
  ↓
EDITORIAL AUDIT
  ↓
QUALITY GATE
  ↓
TARGETED REPAIR IF FAILED
  ↓
FINAL QUALITY GATE
  ↓
FINAL OUTPUT
```

Do not skip a phase.

Do not expose private reasoning or internal chain-of-thought.

Only expose the final requested content unless the user explicitly asks for the audit or research process.

---

# PHASE 0 — LOAD DEPENDENCIES

Before drafting, load the policy files relevant to the task:

1. `references/research-protocol.md`
2. `references/editorial-standard.md`
3. `references/originality-standard.md`
4. `references/quality-gate.md`

If a referenced file cannot be accessed:

- continue only with the rules explicitly available in this SKILL.md
- do not pretend that the missing file was read
- do not fabricate its contents

---

# PHASE 1 — INTENT ANALYSIS

Determine the dominant user intent:

- Informational
- Problem-solving
- Comparison
- Commercial investigation
- Transactional
- Navigational
- Tutorial/how-to

Then determine:

### Core Question

What single question must the article answer?

### Reader Outcome

What should the reader be able to understand, decide, calculate, or do after reading?

### Direct Answer

What is the shortest accurate answer to the core question?

This answer should appear near the beginning of the article.

---

# PHASE 2 — RESEARCH DECISION

Determine whether current external information is necessary.

Research is required when the article depends on:

- current prices
- current laws or regulations
- current product specifications
- recent statistics
- current company policies
- recent events
- current software versions
- changing rankings
- current market conditions
- claims requiring authoritative verification

If research tools are available, perform appropriate research before drafting.

If research tools are unavailable:

- rely only on information that can be stated reliably
- remove unsupported current/numerical claims
- do not invent sources
- do not pretend current information was verified

---

# PHASE 3 — EVIDENCE COLLECTION

For every externally verifiable claim, determine its evidence status:

```text
VERIFIED
SUPPORTED
CONTEXTUAL
UNSUPPORTED
```

Use authoritative sources whenever possible.

Preferred source hierarchy:

1. Primary government/official source
2. Official company/product documentation
3. Academic or institutional source
4. Recognized professional organization
5. High-quality secondary source
6. Community discussion only when the task specifically concerns community experience

If an important factual claim cannot be supported:

REMOVE IT.

Do not convert unsupported numbers into vague numbers.

Bad:

"Approximately 80%..."

when 80% is unverified.

Better:

Remove the statistic and explain the underlying concept.

---

# PHASE 4 — CONTENT-GAP ANALYSIS

When web research is available:

Identify genuine opportunities to provide value beyond generic competing pages.

Possible gaps:

- missing calculations
- practical constraints
- implementation details
- edge cases
- trade-offs
- overlooked costs
- common mistakes
- decision criteria
- source-backed clarification
- conflicting information
- real-world limitations

Only describe a competitor gap when the research actually supports that observation.

When web research is unavailable:

Do NOT claim:

"Most websites don't mention..."

Instead identify independently reasoned value such as:

- a practical example
- a calculation
- a decision framework
- an edge case
- an important caveat
- a clearer explanation
- a useful comparison

---

# PHASE 5 — ARTICLE ARCHITECTURE

Choose structure based on the topic.

Do not force every article into the same template.

Possible structure:

```text
H1
Direct answer

H2
Core explanation

H2
Practical details

H2
Examples / calculations / comparison

H2
Important limitations or edge cases

H2
FAQ — only if genuinely useful
```

Use H3 only when a section genuinely requires subdivision.

Do not add headings solely for SEO.

---

# PHASE 6 — DRAFTING

Write the article using the loaded editorial and originality policies.

Opening requirements:

- answer the main question quickly
- establish context only when necessary
- give the reader useful information immediately

Every paragraph must advance the reader's understanding.

Remove sentences that provide neither:

- information
- explanation
- evidence
- context
- useful qualification
- actionable guidance

Do not inflate word count.

---

# PHASE 7 — EVIDENCE AUDIT

After drafting, inspect factual claims.

For each important factual/numerical claim ask:

1. Is this externally verifiable?
2. Is it supported?
3. Is the source appropriate?
4. Is the wording consistent with the evidence?
5. Is the claim stronger than the evidence?

If unsupported:

```text
REMOVE
OR
REPLACE WITH VERIFIED INFORMATION
OR
REWRITE AS A CLEARLY IDENTIFIED GENERAL PRINCIPLE
```

Never preserve an unsupported statistic merely because it makes the article more persuasive.

---

# PHASE 8 — EDITORIAL AUDIT

Inspect the complete draft for:

- generic AI-style openings
- repetitive transitions
- repetitive explanations
- unnecessary conclusions
- keyword stuffing
- excessive headings
- unnecessary bullets
- unnatural sentence patterns
- vague claims
- excessive adjectives
- unnecessary restatement
- artificial enthusiasm
- awkward SEO phrasing

Edit for clarity rather than deliberately inserting mistakes.

Natural writing does not require intentional grammatical errors.

---

# PHASE 9 — QUALITY GATE

Load:

`references/quality-gate.md`

Evaluate the complete article against every gate.

The article is publishable only when every mandatory gate passes.

---

# PHASE 10 — TARGETED REPAIR LOOP

If any gate fails:

1. Identify the failing criterion.
2. Locate the affected section.
3. Repair only the affected content where possible.
4. Re-run the relevant audit.
5. Re-run the complete quality gate.

Do not regenerate the entire article unless the failure is structural.

Maximum repair cycles:

```text
3
```

If the article still fails after three repair cycles:

- remove the problematic material
- simplify the article
- produce the strongest defensible version

Never knowingly publish an article that fails an evidence-integrity gate.

---

# PHASE 11 — FINAL OUTPUT

Unless the user requests another format, output:

# [Natural Search-Intent-Focused Title]

**Meta Description:** [Concise description, normally ≤155 characters]

**Target Slug:** `/short-descriptive-slug`

[Complete article]

Do not expose:

- internal reasoning
- quality-gate results
- tool calls
- private planning
- internal confidence scores
- claims about guaranteed AdSense approval
- claims about being "100% human-written"

---

# SPECIAL CASE — USER-PROVIDED EXPERIENCE

If the user supplies genuine first-hand experience:

- preserve the factual experience
- clearly distinguish personal experience from general fact
- do not expand it into invented details

If no first-hand experience exists:

Never manufacture it.

---

# SPECIAL CASE — HIGH-STAKES TOPICS

For medical, financial, legal, safety, or similarly consequential topics:

- increase evidence requirements
- prioritize primary/official sources
- avoid unsupported recommendations
- clearly distinguish general information from professional advice
- remove uncertain numerical claims

---

# FINAL PRINCIPLE

The objective is not to make AI output "look human."

The objective is to make the system perform the work of a strong:

- researcher
- subject-matter writer
- fact checker
- editor
- SEO strategist

The final article must provide real value to a real reader.