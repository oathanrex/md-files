# Binary Quality Gate

The article must PASS every mandatory gate before publication.

Each criterion is binary:

`PASS` or `FAIL`

There is no "mostly passes."

---

## GATE 1 — INTENT VALUE

PASS when:

- the article clearly answers the intended query
- the main answer appears early
- the reader can understand the core solution without reading unnecessary background

FAIL when:

- the article delays the answer
- the article answers a different question
- the article contains substantial irrelevant material

---

## GATE 2 — EVIDENCE INTEGRITY

PASS when:

- factual claims are supported appropriately
- important numerical claims are verified
- current claims were researched when necessary
- no source, statistic, quote, expert, or experience was invented

FAIL when:

- an important claim is unsupported
- a statistic is guessed
- a citation is fabricated
- outdated information is presented as current
- invented experience is presented as real

This gate is NON-NEGOTIABLE.

---

## GATE 3 — ORIGINALITY

PASS when:

- structure is independently constructed
- explanations are independently reasoned
- useful value exists beyond simple paraphrasing

FAIL when:

- the article resembles a source too closely
- it is essentially a synonym rewrite
- it contributes no meaningful independent value

---

## GATE 4 — PRACTICAL VALUE

PASS when the topic permits at least one useful enhancement such as:

- example
- calculation
- decision rule
- practical constraint
- trade-off
- edge case
- implementation detail
- clarification

FAIL when the article consists primarily of generic statements that could apply to almost any website.

For topics where these enhancements genuinely do not apply, judge based on the strongest useful form appropriate to the topic.

---

## GATE 5 — EDITORIAL QUALITY

PASS when:

- paragraphs advance understanding
- language is natural
- headings improve navigation
- formatting improves comprehension
- unnecessary repetition is removed
- filler is removed

FAIL when:

- the article contains obvious AI-style filler
- sections repeat each other
- headings are excessive
- formatting exists primarily for SEO

---

## GATE 6 — SEO INTEGRITY

PASS when:

- search intent is satisfied
- terminology is natural
- title accurately represents content
- meta description accurately represents the page
- there is no keyword stuffing
- content is written primarily for people

FAIL when:

- keywords are unnaturally repeated
- title is misleading
- content is clearly search-engine-first
- multiple sections exist primarily to target keywords

---

## GATE 7 — TRUST & EXPERIENCE

PASS when:

- expertise claims are honest
- first-hand experience is genuine
- limitations are disclosed where relevant
- factual and opinion statements are distinguishable

FAIL when:

- experience is fabricated
- expertise is falsely implied
- uncertain claims are presented as certain

---

# FINAL DECISION

```text
IF GATE 1 = PASS
AND GATE 2 = PASS
AND GATE 3 = PASS
AND GATE 4 = PASS
AND GATE 5 = PASS
AND GATE 6 = PASS
AND GATE 7 = PASS

THEN:
    QUALITY_GATE = PASS

ELSE:
    QUALITY_GATE = FAIL
```

When FAIL occurs:

1. Identify the failed gate.
2. Locate the affected content.
3. Repair the smallest necessary section.
4. Re-run the relevant validation.
5. Re-run all gates.

Maximum repair cycles: 3.

If evidence integrity still fails after repair:

REMOVE the unsupported material.

Never knowingly output unsupported factual claims merely to preserve article length.